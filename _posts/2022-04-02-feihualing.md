---
layout: post
title: 飞花令APP
date: 2022-04-02
tags: 安卓开发
---

## 介绍

飞花令，每年春节期间举行的“中国诗词大会”有一个飞花令环节，是精彩的环节之一。

从helloworld开始，做一个简本的飞花令，用以检验你的Android studio是否安装就绪，以及练习最简单的apk开发过程。

加一个单行文本输入框，用以存放查询词。加一个多行文本框，存放查询结果。还需要一个按钮。

网上找到一个“唐诗三百首”的文本文件使用即可。

## 实现

### 步骤1：文件和框、按钮

诗文放到这个位置：

.\app\src\main\res\raw\shi300.txt

注：把该文件加入项目中去可以使用拖放，但是用文件角度描述更有利于重现。要描述清楚从哪里拖到哪里，可费劲了，偷懒就用文件角度了，各自可以继续使用自己喜欢的方法。

拖一个plain text进来，id命名为“id_key”;

拖一个text view进来，命名为“id_res”；

拖个按钮进来，现在是这样了：

![](/images/feihualing_imgs/文本框和按钮.png)

### 步骤2：加代码

在activity_main.xml中给按钮加一个事件处理函数：

`android:onClick="onClick_go"`

在main java中加一个函数和oncreate平行即可：

```java
public void onClick_go(View view) {
    EditText key = findViewById(R.id.id_key);
    TextView res = findViewById(R.id.id_res);
    InputStream input = getResources().openRawResource((R.raw.shi300));
    try {
        int k = 0;
        String temp = "";
        String result = "";
        Scanner scan = new Scanner(input);
        int max=100;
        while (scan.hasNext()) {
            String str = scan.nextLine();
            if (str.contains("：")) {
                temp = str;
                continue;
            }
            if (str.contains(key.getText().toString())) {
                k++;
                result += k + "." + str + temp + "\n";
                max--;
                if (max<=0) break;
            }
        }

        Toast.makeText(MainActivity.this, "一共有" + k + "句！", Toast.LENGTH_SHORT).show();
        res.setText(result);
        res.setMovementMethod(ScrollingMovementMethod.getInstance());
        scan.close();
    } catch (Exception e) {
        e.printStackTrace();
    }

}
```

## 思考人生：遇到各种各样的错误并改正

以下为我在过程中遇到的无数个问题：

1. 虚拟手机设置失败：在安装Android Studio虚拟手机的时候，有未能正确安装haxm报错，在论坛查阅了资料后，尝试在bios设置中开启相关选项、尝试在“启用或关闭Windows功能”中开启相关选项等……依然未能成功下载。尝试去intel官网下载该驱动，并没有找到。尝试去第三方下载，成功下载haxm之后，不能进行安装。这才发现自己的电脑是AMD处理器，不能下载intel的haxm驱动……然后在论坛寻找搭载AMD处理器的电脑如何安装Android Studio虚拟机，在尝试若干错误方法后，终于配置好了驱动，在Android Studio的虚拟机跑起来了helloworld。结果下一次开机再打开，又跑不起来了……看了无数方法仍然失败后，决定放弃Android Studio自带的虚拟机，下载了genymotion。安装打开genymotion后，提示需要下载Oracle VM virtualbox。下载了Oracle VM virtualbox后，注册了genymotion账号并登录，添加了虚拟机，终于能跑起来helloworld了。以上为星期五晚上完成，星期天晚上再跑项目的时候，发现虚拟机又跑不起来了，卡在下面这个界面……

   ![](/images/feihualing_imgs/虚拟机卡住.png)

   寻找了一晚上方法后无果。星期一早上找到了方法，那就是在新建虚拟机的时候，应该把下面的NAT(default)改为Bridge。虽然每次用这种方法打开虚拟机都需要十秒以上时间，但是毕竟成功跑起来了。![](/images/feihualing_imgs/解决1.png)

   星期一晚上准备编写飞花令，根据实验报告中的步骤配置好了代码之后，运行了飞花令，结果发现虚拟机不能使用中文输入……把系统语言改为中文也不行，虚拟机根本没有中文输入法……

   <img src="/images/feihualing_imgs/输入法1.png" width="40%"><img src="/images/feihualing_imgs/输入法2.png" width="40%">

   我一开始以为是moto虚拟机的问题，又下了一个华为的虚拟机，结果华为手机也没有中文输入法……最后只能用自己的手机了。 星期五搞了半天也没弄明白怎么用Android Studio连接自己的手机，结果星期一找到的第一个帖子的方法就成功了，手机截图如下：

   <img src="/images/feihualing_imgs/成功1.png" width="40%">

   到此，飞花令运行环境的问题才成功解决。

2. 唐诗文件乱码：在网上下载了诗三百txt，发现在电脑上记事本正确显示，在手机上全是乱码不能查找。查找资料后，首先在Android Studio的设置中修改了编码：

   <img src="/images/feihualing_imgs/修改编码1.png" width="">

   跟我想象的一样，并没有成功……然后下载了notepad++，用notepad++打开这个txt文件，在下面设置encoding为UTF-8，然后保存：

   <img src="/images/feihualing_imgs/修改编码2.png" width="">

   没用，还是乱码。最后用Android Studio打开了这个txt文件，提示是否转换为UTF-8编码，然后转换，终于可以成功显示和搜索了：

   <img src="/images/feihualing_imgs/修改编码3.png" width="40%">

3. 添加背景：被折磨的筋疲力竭之后，不想再折腾复杂的东西了，觉得添加宋词元曲四大名著没什么技术含量。尝试搜索把搜索的关键字标红的方法，发现太麻烦。于是决定给应用添加一个背景。在网上找了张淡雅的手机壁纸图，找了添加背景的教程，很顺利地成功了，效果还不错，见下效果图。<img src="/images/feihualing_imgs/效果图1.png" width="40%"><img src="/images/feihualing_imgs/效果图2.png" width="40%">


## 源码

[飞花令源码链接](https://github.com/YorkSunSdu/YorkSunSdu/tree/master/AndroidDevelopment/feihualing)

<div style="color: red; font-size:24px">商业转载请联系博主获得授权，非商业转载请注明出处！</div>

分享结束，大家辛苦了。散会！

