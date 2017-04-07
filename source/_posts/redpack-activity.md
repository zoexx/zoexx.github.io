title: 红包活动
date: 2017-04-07 18:25:37
tags: php wechat
---

## 年前的红包活动总结

- 公众号的粉丝量并不阻碍有“利”内容在微信中的传播力
- 羊毛党是非常可怕的
- 缺少压力测试
- 尽可能的静态化能大大减少服务器的压力
- 尽可能的记录活动数据
- 出错后的数据回滚
- 顺手做的统计功能很实用

2017年新年的前一周，来了个回馈新老用户发红包的活动，新用户需要关注公众号，老用户点击领取按钮后用公众号下发红包。

这次活动仅在公众号文章中做了宣传，并没有在其它渠道做推广，然而似乎被羊毛党盯上，随着时间的推移，发出红包的速度像一个指数函数，大约6小时后随机红包被薅光，21:20 分左右迎来了一个小高潮，并且发完后的几个小时访问量还在持续增加，第三天才恢复正常。当日的PV是平时的150多倍，领取红包的用户中新用户占98%。

发红包的规则：

| 类型     |名额    | 方式 |金额（元）|随机概率占比 |
|--        |--     |--    |--       |--          |
| 指定老用户| 100   | 固定 | 6.66    |  元        |
| 普通用户  | 20    | 随机 | 5~10    |  36/10000  |   
| 普通用户  | 480   | 随机 | 2~5     |  873/10000 |  
| 普通用户  | 5000  | 随机 | 1~2     |  元        |


目前活动已下架，以下是业务代码。

```php
    // 活动：新年红包 zoe@20170116 
    public function happy_new_year(Request $request) {
		$WechatJSMgr = new WechatJSMgr();
		$ua= $WechatJSMgr->checkBrowser($request);
		$err_msg = '' ;
		// 是否关注了公众号，0未关注，1关注
		$subscription = 0; 
		$has_gift     = 0 ;
		$user 		  = false ;

		if ($ua=="wechat"){
			$user = @session('wechat_user') ;
			$open_id = $user['openid'];
			if ( !$user || empty($open_id) )return redirect('/wechat/auth_rate?url='.urlencode($request->getRequestUri()));

			$sendMsgMgr = new sendMsgMgr();
			$chk_wx_user = $sendMsgMgr->chk_wx_user($open_id);
			if ( $chk_wx_user ){
				$subscription = 1 ;
			}
			// 判断是否领过奖
			$gift = ActivityHappyNewYearModel::where('open_id', $open_id )->first();
			if ( empty($gift) ){
				$gift = new ActivityHappyNewYearModel();
				$gift->open_id = $open_id ;
				$gift->user_name = $user['nickname'] ;
				$rs = $gift->save();
			}else if ( $gift->status == 1 ){
				// 礼物已领取
				$has_gift = 1 ;		
			}
            
		}else{
			$err_msg = "活动需要在微信中打开 >...< ";
		}

		$wechatJSDK  = $WechatJSMgr->get_JS_config($request->getUri());

        return view('activity.happy_new_year',[
				'user'		  => json_encode( $user ) ,
				'wechatJSDK'  => $wechatJSDK['data']['config'] ,
				'err_msg'     => $err_msg ,
				'has_gift'    => $has_gift ,
				'subscription'=> $subscription ,
				'from_subscribe'=> $request->has('from_subscribe') ? 1 : 0 ,
				'webstatic' => 'https://webstatic.xxxxx.com/activity/happy_new_year/' , // CDN静态资源地址
			]);
    }


	// 新年活动 step2 发红包
	// 微信接口文档 https://pay.weixin.qq.com/wiki/doc/api/tools/cash_coupon.php?chapter=13_4&index=3
	public function happy_new_year_gift(Request $request) {
		DB::beginTransaction();
		try{
			// 根据open_id 来发
			$open_id = $request->input('open_id');
			$err_msg = '';
			$err_code = 0;

			if ( !$open_id ){
				$err_msg = '请在微信中打开~' ;
			}else{
				// 判断是否领过奖
				$gift = ActivityHappyNewYearModel::where('open_id', $open_id )->first();

				if ( !empty($gift) && $gift->status == 1 ){
					// 礼物已领取
					$err_msg = "好看的你已经领过红包了~" ;
					$err_code = 99 ;
				}else{
					// 发红包
					if ( empty($gift) ){
						$gift = new ActivityHappyNewYearModel();
						$gift->open_id = $open_id ;
					}
					$money = 0 ;
					
					// 100位老客户给6.66 					
					if ( $gift->isSeedUser() ){
						 $money = 666 ;
					}else{

						 $money = $this->happy_new_year_money();
						 if ( $money == 0) {
							 $err_msg = '红包已经被抢完啦<br>感谢您持续关注<br>健康就是最大的财富';
						 }
						 
					}

					if ( $money ){
						 if ( !$gift->gift_id ) $gift->save() ;

						// 补齐giftid 4位数
						$gift_id_paddding = substr(strval($gift->gift_id + 10000),1,4) ;
						$mch_id     = config('wechat.mch_id') ;
						$mch_billno = $mch_id . date('YmdHis') . $gift_id_paddding ;

						$business  = new Business( config('wechat.app_id'),config('wechat.secret'),config('wechat.mch_id'),config('wechat.mch_key'));

						$cert_path = dirname(__FILE__) .'/../../../../'.config('wechat.cert_path');
						$cert_path = realpath($cert_path) ;

						$key_path = dirname(__FILE__) .'/../../../../'.config('wechat.key_path');
						$key_path = realpath($key_path) ;

						$business->setClientCert( $cert_path );
						$business->setClientKey( $key_path );

						$luckyMoney = new LuckMoney($business);
						$luckyMoneyData = [
							'mch_billno'       => $mch_billno , //mch_id+yyyymmdd+10位
							'send_name'        => 'send_name',
							're_openid'        => $open_id ,
							'total_num'        => 1 ,  			//固定为1，可不传
							'total_amount'     => $money ,      //单位为分
							'wishing'          => 'wishing',
							'act_name'         => 'act_name',
							'remark'           => 'remark',
						];

						$result = $luckyMoney->sendNormal($luckyMoneyData);

						// 成功后修改数据库
						if ( $result['return_code'] == "SUCCESS" && $result['result_code'] == "SUCCESS" ){

							$gift->amount 		 = $result['total_amount'] ;
							$gift->mch_billno    = $result['mch_billno'];
							$gift->wechat_billno = $result['send_listid'];

							$user = $gift->user ;
							if ( $user && isset( $user ) ){
								$gift->user_name = $user->real_name ? $user->real_name : $user->nickname ;
								$gift->mobile 	 = $user->mobile ;
							}else{
								$gift->remark    = '未注册用户';
							}
							$gift->invitation  = $request->input('invitation') || '';
							$gift->firing_time = Carbon::now(); 
							$gift->status = 1 ;

						}else{

							$gift->status = 99 ;
							// 失败时返回失败信息，并将gift的状态标记为99异常
							$err_msg = isset($result['return_msg']) ? 
									'微信接口返回：'.$result['return_msg'] :
									isset($result['err_code']) || isset($result['err_code_des']) ?
									'微信业务接口返回：'.(isset($result['err_code']) ? $result['err_code'] : '' ).(isset($result['err_code_des']) ? $result['err_code_des'] : '' ) :
									'微信接口调用失败' ;
							$gift->remark = $err_msg ;
							// 返回给用户的提示
							$err_msg = '红包已经被抢完啦<br>感谢您持续关注<br>健康就是最大的财富';
						}

						if ( $gift->save() == false ) throw new \Exception('新年红包活动数据保存失败');
					}

					
				}
			}
			DB::commit();
			if ( $err_msg ){
				return json_encode([
					'code' => $err_code ? $err_code : 90001 ,
					'errmsg' => $err_msg ,
					'data' => []
				],JSON_UNESCAPED_UNICODE);
			}else{
				return json_encode([
					'code' => 0 ,
					'data' => []
				],JSON_UNESCAPED_UNICODE);
			}
	    }catch(\Exception $e){
		    	DB::rollBack();

				$open_id = $request->has('open_id') ? $request->open_id : 'no open_id';

				Rizhi::one( 'happy_new_year_gift' , $open_id  , array('Message'=>$e->getMessage(),'TraceAsString'=>$e->getTraceAsString()) );

				return json_encode([
					'code' => 90001 ,
					'errmsg' => '红包已经被抢完啦<br>感谢您持续关注<br>健康就是最大的财富' ,
					'data' => []
				],JSON_UNESCAPED_UNICODE);
        }
	}

	protected function happy_new_year_money (){
		// 20   位普通用户随机 5~10  元 概率占比   36/10000
		// 480  位普通用户随机 2~5   元 概率占比  873/10000
		// 5000 位普通用户随机 1~2   元 概率占比 9091/10000
		$gift = new ActivityHappyNewYearModel() ;
		if ( $gift->count('gift_id') >= 7850 ){
			return 0 ;
		}else{
			$resMoney = 0 ;
			$money = rand( 1 , 10000 );
			// 限定四档 红包的个数 
			$gift_first  = 0 ;
			$gift_second = 0 ;
			$gift_third  = 0 ;

			if       ( $money <= 36    ){
				$gift_first = $gift->where('amount','>', 500 )->where('status','!=',666)->where('amount','<=', 1000 )->where('status',1)->count('gift_id') ;
				if ( $gift_first < 20  ){
					$resMoney = rand( 501 , 1000 );
				}
			}else if ( $money <= 909   ){
				$gift_second = $gift->where('amount','>', 200 )->where('amount','<=', 500 )->where('status',1)->count('gift_id') ;
				if ( $gift_second < 480 ){
					$resMoney = rand( 201 , 500 );
				}
			}else if ( $money <= 10000   ){
				$gift_third = $gift->where('amount','>', 99 )->where('amount','<=', 200 )->where('status',1)->count('gift_id') ;
				if ( $gift_third < 5000 ){
					$resMoney = rand( 101 , 200 );
				}
			}

			// 抽到的档位已经发完的情况
			if ( !$resMoney ){

				if ( !$resMoney && !$gift_third ) $gift_third = $gift->where('amount','>', 99 )->where('amount','<=', 200 )->where('status',1)->count('gift_id') ;
				if ( !$resMoney && $gift_third < 5000 ) $resMoney = rand( 101 , 200 );

				if ( !$resMoney && !$gift_second ) $gift_second = $gift->where('amount','>', 200 )->where('amount','<=', 500 )->where('status',1)->count('gift_id') ;
				if ( !$resMoney && $gift_second < 480 ) $resMoney = rand( 201 , 500 );

				if ( !$resMoney && !$gift_first ) $gift_first = $gift->where('amount','>', 500 )->where('status','!=',666)->where('amount','<=', 1000 )->where('status',1)->count('gift_id') ;
				if ( !$resMoney && $gift_first < 20 ) $resMoney = rand( 501 , 1000 );

			}
			return $resMoney ;

		}

	}

	// 红包发送情况统计
	public function happy_new_year_stats (){

		if( !session('adm_info.adm_id',null) ){
			echo '没有权限';
			return ;
		}


		$gift = new ActivityHappyNewYearModel() ;
		echo '已发放红包总数：' . $gift->where('status',1)->count('gift_id') .'/5600 个 <br>';
		echo '已发放总金额：' . $gift->where('status',1)->sum('amount')/100 . '元 <br>' ;	
		echo '----------------------------------------- <br>';

		$gift_count = $gift->where('amount','!=', 666 )->where('status',1)->count('gift_id') ;
		echo '普通红包已发放：' . $gift_count . '/5500 个<br>';
		echo '剩余普通红包：' . ( 5500 - $gift_count ) . '个<br>' ;
		echo '----------------------------------------<br>';


		$gift_second = $gift->where('amount','>', 99 )->where('amount','<=', 200 )->where('status',1);
		$gift_second_count = $gift_second->count('gift_id') ;
		echo '1~2 元红包已发放：' . $gift_second_count .'/5000<br>';
		$gift_second_amount = $gift_second->sum('amount')/100 ;
		echo '总金额：' . $gift_second_amount .'<br>' ;
		echo '中位值*总数：' . $gift_second_count*1.5 .'<br>' ;

		$gift_third = $gift->where('amount','>', 200 )->where('amount','<=', 500 )->where('status',1);
		$gift_third_count = $gift_third->count('gift_id') ;
		echo '2~5 元红包已发放：' . $gift_third_count .'/480 <br>';
		$gift_third_amount = $gift_third->sum('amount')/100 ;
		echo '总金额：' . $gift_third_amount .'<br>' ;
		echo '中位值*总数：' . $gift_third_count*3.5 .'<br>' ;

		$gift_forth = $gift->where('amount','>', 500 )->where('amount','<=', 1000 )->where('amount','!=', 666 )->where('status',1);
		$gift_forth_count = $gift_forth->count('gift_id');
		echo '5~10 元红包已发放：' . $gift_forth_count .'/20<br>' ;
		$gift_forth_amount = $gift_forth->sum('amount')/100 ;
		echo '总金额：' . $gift_forth_amount .'<br>' ;
		echo '中位值*总数：' . $gift_forth_count*7.5 .'<br>' ;

		echo '----------------------------------------<br>';
		$total = $gift_second_amount + $gift_third_amount + $gift_forth_amount ;
		echo '普通红包已发放总金额：' . $total .'元<br>' ;


		echo '----------------------------------------<br>';
		$seed_count = $gift->where( 'amount', '=' , 666 )->where('status',1)->count('gift_id') ;
        echo '666红包已发放：' . $seed_count . '/100 个<br>' ;
		echo '剩余666红包：' . ( 100 - $seed_count ) . '个<br>' ;
		echo '总金额：' . ( $seed_count*666/100 ) . '元<br>' ;

		echo '-----------------------------------------<br>';
		$gift_error =  ActivityHappyNewYearModel::where('status','=', 99 );
		echo '异常（调用微信接口错误）红包数：' . $gift_error->count() .'<br>';
		echo implode( '<br>' , $gift_error->lists('remark')->toArray() );

		return ;
	}
```
