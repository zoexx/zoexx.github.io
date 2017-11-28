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

其实cowsay不仅仅🐮say，还有很多小动物  
Show all available cows
```
$ for i in /usr/share/cowsay/cows/*.cow; do cowsay -f $i "$i"; done
```

#### 其它有趣的命令（有一些是没有打包在系统中的需要手动装一下，apt-get install in Debian）

##### 1. sl大家应该都知道，手误的时候可以看火车呜呜呜的经过
##### 2. cmatrix 屏保 🙂 黑客帝国的大门为你打开（假装）
##### 3. fortune
 
美国中餐馆的最后一道菜，往往是小甜饼，叫做"幸运饼"（fortune cookie）。里面有一张纸条，写着人生格言。

![fortune cookie](https://timgsa.baidu.com/timg?image&quality=80&size=b9999_10000&sec=1512466632&di=b22654281d39ca66b2085d11de46b2aa&imgtype=jpg&er=1&src=http%3A%2F%2Ffarm1.staticflickr.com%2F53%2F120157809_d3a7ee3718.jpg)

这种形式的格言非常受欢迎，1979年，就有人写了一个叫做 fortune 的小程序。每一次执行都可以得到一条随机的格言/故事。

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

use fortune with cowsay ~

```
pi@raspberrypi:~ $ fortune | cowsay
 ________________________________________
/ My only love sprung from my only hate! \
| Too early seen unknown, and known too  |
| late!                                  |
|                                        |
| -- William Shakespeare, "Romeo and     |
\ Juliet"                                /
 ----------------------------------------
        \   ^__^
         \  (oo)\_______
            (__)\       )\/\
                ||----w |
                ||     ||

```

Have a random "cow" say a random thing
```
pi@raspberrypi:~ $ fortune | cowsay -f $(ls /usr/share/cowsay/cows/ | shuf -n1)
 ________________________________________
/ It is a wise father that knows his own \
| child.                                 |
|                                        |
| -- William Shakespeare, "The Merchant  |
\ of Venice"                             /
 ----------------------------------------
 \     /\  ___  /\
  \   // \/   \/ \\
     ((    O O    ))
      \\ /     \ //
       \/  | |  \/ 
        |  | |  |  
        |  | |  |  
        |   o   |  
        | |   | |  
        |m|   |m|  

```

##### 4. ddate

> ddate prints the date in Discordian date format.

是否厌倦了千百年不变的[ Gregorian Calendar（罗马教历）](https://en.wikipedia.org/wiki/Gregorian_calendar)？准备好乱入了吗？试试输入“ddate”。这样会把当前日历以[Discordian Calendar（不和教历）](https://en.wikipedia.org/wiki/Discordian_calendar)的方式显示出来。你会遇见这样的语句：

“今天是Sweetmorn（甜美的清晨），3181年Discord（不和）季的第18天。”

##### 5. adventure 文字冒险游戏

```
pi@raspberrypi:~ $ adventure 

Welcome to Adventure!!  Would you like instructions?
yes

Somewhere nearby is Colossal Cave, where others have found fortunes in
treasure and gold, though it is rumored that some who enter are never
seen again.  Magic is said to work in the cave.  I will be your eyes
and hands.  Direct me with commands of 1 or 2 words.  I should warn
you that I look at only the first five letters of each word, so you'll
have to enter "northeast" as "ne" to distinguish it from "north".
(Should you get stuck, type "help" for some general hints.  For
information on how to end your adventure, etc., type "info".)
			      - - -
This program was originally developed by Will Crowther.  Most of the
features of the current program were added by Don Woods.  Address
complaints about the UNIX version to Jim Gillogly (jim@rand.org).

You are standing at the end of a road before a small brick building.
Around you is a forest.  A small stream flows out of the building and
down a gully.
up

You have walked up a hill, still in the forest.  The road slopes back
down the other side of the hill.  There is a building in the distance.
.......
```

这个游戏可以通过安装 bsdgames 得到。[bsdgames](http://wiki.linuxquestions.org/wiki/BSD_games)包含了adventure和其它经典文字游戏，都可以尝试一下哦。

都在 /usr/games/ 目录里。


### 更多更多有趣&实用的命令👉[http://www.commandlinefu.com](http://www.commandlinefu.com)