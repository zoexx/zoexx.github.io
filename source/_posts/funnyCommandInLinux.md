title: 有趣的linux（Debian）命令
date: 2017-11-28 16:26:21
tags: linux
---

## 本 APT 具有超级牛力是什么鬼？

>  apt-get is the command-line tool for handling packages, and may be considered the user's "back-end" to other tools using the APT library. Several "front-end" interfaces exist, such as aptitude(8), synaptic(8) and wajig(1).

给系统做了汉化之后，Sean发现执行apt-get命令时，command说明的最后一行出现了“本 APT 具有超级牛力”。英文是"This APT has Super Cow Powers."。

![本 APT 具有超级牛力](http://cdn.zoeservers.com/blog/apt_20171128163821.png)

有趣的是[aptitude](https://baike.baidu.com/item/aptitude/6849487?fr=aladdin)的说明中也出现了牛力。

```
$ aptitude --help
```

“这个 aptitude 没有超级牛力。”

"This aptitude does not have Super Cow Powers."

好吧，在此之前，我其实也有用过一个有趣的小命令，叫 cowsay 。

```
pi@raspberrypi:~ $ cowsay hello world
 _____________
< hello world >
 -------------
        \   ^__^
         \  (oo)\_______
            (__)\       )\/\
                ||----w |
                ||     ||

```

那么，为什么这一堆程序员这么偏爱牛呢？🐮🐮🐮🐮

#### What's the story behind Super Cow Powers?

> Apt started its life around 1997 and entered Debian officially around 1999. During its early days, Jason Gunthorpe was its main maintainer/developer. Well, apparently Jason liked cows. I don't know if he still does. :-) Anyway, I think the apt-get moo thing was added by him as a joke. The corresponding aptitude easter eggs (see below) were added later by Daniel Burrows as a homage, I think.

> If there is more to the story, Jason is probably the person to ask. He has (likely in response to this question) written a post on Google+. A small bit of it:

>> Once a long time ago a developer was known for announcing his presence on IRC with a simple, to the point 'Moo'. As with cows in pasture others would often Moo back in greeting. This led to a certain range of cow based jokes.

以上是来自[unix.stackexchange.com](https://unix.stackexchange.com/questions/92185/whats-the-story-behind-super-cow-powers)的高票回答。

吼，原来是[IRC](https://baike.baidu.com/item/IRC/10410?fr=aladdin)( Internet Relay Chat )时代的故事。

脑补对话:

- zoe: moo.    
- <small>( anyone else? )</small>
- sean: moo~   
- <small>( yes,here. )</small>
- della: moo~  
- <small>( here I am. )</small>

#### 更多牛力请尝试👇

万圣节彩蛋？
```
$ aptitude moo
$ aptitude -v moo
$ aptitude -vv moo
$ aptitude -vvv moo
$ aptitude -vvvv moo
$ aptitude -vvvvv moo
$ aptitude -vvvvvv moo
```

Have you mooed today?
```
$ apt-get moo
$ apt-get moo moo
$ apt-get moo moo moo
```

#### 其它有趣的命令

##### 1. sl大家应该都知道，手误的时候可以看火车呜呜呜的经过
##### 2. cmatrix 屏保🙂
##### 3. fortune
 
美国中餐馆的最后一道菜，往往是小甜饼，叫做"幸运饼"（fortune cookie）。里面有一张纸条，写着人生格言。

![fortune cookie](https://timgsa.baidu.com/timg?image&quality=80&size=b9999_10000&sec=1512466632&di=b22654281d39ca66b2085d11de46b2aa&imgtype=jpg&er=1&src=http%3A%2F%2Ffarm1.staticflickr.com%2F53%2F120157809_d3a7ee3718.jpg)

这种形式的格言非常受欢迎，1979年，就有人写了一个叫做 fortune 的小程序。

```
pi@raspberrypi:~ $ fortune
If one cannot enjoy reading a book over and over again, there is no use
in reading it at all.
		-- Oscar Wilde
```

还有中文版的 fortune-zh 。

```
pi@raspberrypi:~ $ fortune-zh
《赠内人》
作者：张祜
禁门宫树月痕过，媚眼惟看宿鹭巢。
斜拔玉钗灯影畔，剔开红焰救飞蛾。
```
