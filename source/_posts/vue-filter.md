title: vue-filter
date: 2016-01-08 01:11:17
tags: vue
---
今天利用vue的数据双向绑定重新写了一下列表页的多级联动筛选栏
写完感觉神清气爽，大概两百行代码 取代了之前多又杂的代码
而且可以动态修改数据来修改筛选栏
来一张截图

![vue-filter](http://7xndda.com1.z0.glb.clouddn.com/re_write_filter.png)

由直接操作DOM转为直接操作数据

-------------------------


做小螺管家app时 试图用react重写
发现这个思路很有问题
它试图将 多层级的data转换为扁平的dom
data结构如下，这是后台惯用的多层级结构，而dom的结构是扁平的

```
{
	"code": 0,
	"data": [{
		"code": 44030000,
		"districtInfoList": [{
			"areaInfos": [{
				"code": 44030601,
				"districtCode": 44030600,
				"name": "宝安中心区"
			}],
			"cityCode": 44030000,
			"code": 44030600,
			"name": "宝安区"
		}, {
			"areaInfos": [{
				"code": 44030902,
				"districtCode": 44030900,
				"name": "龙华"
			}],
			"cityCode": 44030000,
			"code": 44030900,
			"name": "龙华新区"
		}, {
			"areaInfos": [{
				"code": 44030509,
				"districtCode": 44030500,
				"name": "蛇口"
			}],
			"cityCode": 44030000,
			"code": 44030500,
			"name": "南山区"
		}],
		"metroLineInfoList": [{
			"cityCode": 44030000,
			"code": 5,
			"metroStationInfoList": [{
				"code": 125,
				"metroLineCode": 5,
				"name": "洪浪北"
			}, {
				"code": 127,
				"metroLineCode": 5,
				"name": "翻身"
			}, {
				"code": 126,
				"metroLineCode": 5,
				"name": "灵芝"
			}, {
				"code": 128,
				"metroLineCode": 5,
				"name": "宝安中心"
			}, {
				"code": 116,
				"metroLineCode": 5,
				"name": "五和"
			}, {
				"code": 117,
				"metroLineCode": 5,
				"name": "民治"
			}],
			"name": "环中线"
		}, {
			"cityCode": 44030000,
			"code": 4,
			"metroStationInfoList": [{
				"code": 93,
				"metroLineCode": 4,
				"name": "上塘"
			}, {
				"code": 94,
				"metroLineCode": 4,
				"name": "红山"
			}, {
				"code": 97,
				"metroLineCode": 4,
				"name": "民乐"
			}],
			"name": "龙华线"
		}, {
			"cityCode": 44030000,
			"code": 2,
			"metroStationInfoList": [{
				"code": 56,
				"metroLineCode": 2,
				"name": "水湾"
			}, {
				"code": 52,
				"metroLineCode": 2,
				"name": "登良"
			}, {
				"code": 51,
				"metroLineCode": 2,
				"name": "后海"
			}, {
				"code": 54,
				"metroLineCode": 2,
				"name": "湾厦"
			}],
			"name": "蛇口线"
		}, {
			"cityCode": 44030000,
			"code": 1,
			"metroStationInfoList": [{
				"code": 25,
				"metroLineCode": 1,
				"name": "宝体"
			}, {
				"code": 27,
				"metroLineCode": 1,
				"name": "西乡"
			}, {
				"code": 20,
				"metroLineCode": 1,
				"name": "大新"
			}, {
				"code": 26,
				"metroLineCode": 1,
				"name": "坪洲"
			}, {
				"code": 18,
				"metroLineCode": 1,
				"name": "深大"
			}, {
				"code": 17,
				"metroLineCode": 1,
				"name": "高新园"
			}, {
				"code": 23,
				"metroLineCode": 1,
				"name": "新安"
			}, {
				"code": 28,
				"metroLineCode": 1,
				"name": "固戍"
			}, {
				"code": 19,
				"metroLineCode": 1,
				"name": "桃园"
			}],
			"name": "罗宝线"
		}, {
			"cityCode": 44030000,
			"code": 0,
			"metroStationInfoList": [{
				"code": 0,
				"metroLineCode": 0,
				"name": "在建地铁"
			}]
		}],
		"name": "深圳市"
	}],
	"msg": "success"
}
```

为了更通用 我将格式固定为如下
"title" 是标题栏
"n" means name 展示默认名称
"a" means active 展示选项内容
"content" 是 筛选的选项
"t" means title
"v" means value
"c" means context 存放下一级的选项

```
{
    "title": [{
        "n": "位置",
        "a": ""
    }, {
        "n": "租金",
        "a": ""
    }, {
        "n": "方式",
        "a": ""
    }, {
        "n": "状态",
        "a": ""
    }],
    "content": [
        [{"t":"区域"},
        {"t":"地铁"}],
        [{
            "t": "不限价格",
            "v": "minPrice=&maxPrice="
        }, {
            "t": "1500以下",
            "v": "maxPrice=1500"
        }, {
            "t": "1500~2000",
            "v": "minPrice=1500&maxPrice=2000"
        }, {
            "t": "2000~3000",
            "v": "minPrice=2000&maxPrice=3000"
        }, {
            "t": "3000~5000",
            "v": "minPrice=3000&maxPrice=5000"
        }, {
            "t": "5000~8000",
            "v": "minPrice=5000&maxPrice=8000"
        }, {
            "t": "8000~12000",
            "v": "minPrice=8000&maxPrice=12000"
        }, {
            "t": "12000以上",
            "v": "minPrice=12000"
        }],
        [{
            "t": "不限",
            "v": "rentType="
        }, {
            "t": "合租",
            "v": "rentType=0"
        }, {
            "t": "整租",
            "v": "rentType=1"
        }],
        [{
            "t": "全部",
            "v": "status="
        },{
            "t": "未发布",
            "v": "status=1"
        }, {
            "t": "销售中",
            "v": "status=2"
        }, {
            "t": "已出租",
            "v": "status=4"
        }]
    ]
}

```

dom

```
<optionBox>
	<container><li>深圳</li><li>广州</li></container>		
	<container><li>深圳南山中心区</li></container>		
	<container><li>广州白云区</li></container>		
</optionBox>>

```


我需要判断数据中是否含有"c(context)"来增加一个container，然而在开发时发现除非把数据拆成扁平的与dom结构一致，无法写成层级较深的递归

联动时 使用了index来标记激活的选项
这样做的坏处也是显而易见的
如果有四级的联动 就需要手动加一个container
：（
下次重构 换种思路


