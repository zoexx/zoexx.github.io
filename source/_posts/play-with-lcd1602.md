title: play_with_lcd1602
date: 2018-02-05 17:10:29
tags: raspberry
---

# 给树莓派配一块显示屏 lcd1602

![显示xrpusdt的价格](http://cdn.zoeservers.com/IMG_0991_Lively.gif)

[lcd的说明书](https://baike.baidu.com/item/1602%E5%AD%97%E7%AC%A6%E6%B6%B2%E6%99%B6/6103144?fr=aladdin)百度百科上也写的比较详细，有了wiringPi的封装，我们只需要接好线，调用wiringpi提供的方法即可。

wiringPi官网上有案例指导👉[直达链接](http://wiringpi.com/dev-lib/lcd-library/)。

需要注意的是示意图中的树莓派与3B+的GPIO数量不一样，白线连的是输入电压5V，依次往左推。

![接线示意图](http://cdn.zoeservers.com/lcd4_bb.png)

另外，wiringPi使用的是针脚编号，以下奉上一张对比图。

![树莓派GPIO编号](http://cdn.zoeservers.com/rpi-pins-40-0.png)

电位器是用来调节屏幕对比度，如果像我一样没有电位器的朋友也可以用电阻替代。

我使用了2kΩ的电阻接地👇

V0======2kΩ=======GND

第一次通电之后，lcd1602会显示上面一排黑色下面一排白色，这是它在初始化，莫慌。

|__________________________________________________|

| ▇ ▇ ▇ ▇ ▇ ▇ ▇ ▇ ▇ ▇ ▇ ▇ ▇ ▇ ▇ ▇ |

|__________________________________________________|


接好线之后可以使用wiringpi提供的测试代码跑一跑，出现字母数字之类，就已经算ok了。

以下是一段简单的打印文件内容的c代码。

```c
#include <stdio.h>
#include <stdlib.h>
#include <stdint.h>

#include <wiringPi.h>
#include <lcd.h>

int main (int args, char *argv[])
{
    if (wiringPiSetup () == -1)
    {
       printf("some thing wrong!");
       exit (1) ;
    }
    int fd = lcdInit (2, 16, 4,  11,10 , 0,1,2,3,0,0,0,0) ;
    if (fd == -1)
    {
        printf ("lcdInit 1 failed\n") ;
        return 1 ;
    }
    sleep (1) ; //显示屏初始化

    FILE *fp;
    char Body[32]; 
    
    while(1)
    {
        fp=fopen("./bid.txt","r");
        fgets(Body,32,fp);
        fclose(fp);

        lcdClear (fd);
        lcdPosition (fd, 0, 0) ;
        lcdPuts(fd , Body);
        
        sleep(1);
    }

    return 0;
    
}
```

编译

```bash
gcc lcd_printer.c -o lcd_printer -lwiringPi -lwiringPiDev
```

bid.txt中的内容来自我用python抓回来的数据。

```text
B 0.7970 : 00078S 0.7996 : 5759.0
```