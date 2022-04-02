---
layout: post
title: push_to_talk多播APP
date: 2022-04-02
tags: 安卓开发
---



## 介绍

对讲机是这样一种对讲设备，可以在某个无线电频道上收、发语音。典型的设计是有一个调节频道的转纽，一个push-to-talk按钮，讲话前需要把这个按钮摁下去。

同一个多播域中可以使用类似的方法通信。IP多播域中的群组以224.X.Y.Z:W区分（或者叫就叫频道也行），IP设备可以加入某个群组，然后就可以发送、接收数据了。

比如QLSC的WIFI就支持这种多播，使用wireshark能看到224.0.0.251:5353这个频道很忙，有很多名字解析信息在传递。这种多播通信的特点是不需要知道对方的IP地址，只要对方也在QLSC/WIFI热点，就能收到。

使用IP可以多播，使用MAC好像也可以多播，细节不详。

于是有了一个想法，写一个Android APP，借用QLSC/WIFI多播频道通信，实现类似对讲机的效果。

## 设计

### 设计的初稿如下：

![设计1](/images/push_to_talk_imgs/设计1.png)

用林老师的高情商话术说：非常朴实，一看就是自己做的。

实际上代码借鉴了git中其他同学。设计还是自己做的。

### 设计第二稿

为了使这个用户图形界面看起来不这么朴素，我决定采用最简单有效的办法：加个背景。

加背景在实验一飞花令里面使用过了，但是当时的实验报告忘记记录了，我又学了一遍怎么加背景，记录一下：

1. 找一张美观的、尺寸合适的背景图片，放在app\src\main\res\mipmap-hdpi路径下，命名为aaa.png；
2. 在activity_main.xml文件前面layout下加上这一行android:background="@mipmap/aaa"，如下图：

![](/images/push_to_talk_imgs/插入图片.png)



插入图片后的效果如下图：

![](/images/push_to_talk_imgs/设计2.png)

好多了，我很满意了。

## 实现

在MainActivity.java文件的onCreate()方法中加入以下字段：

```java
public  void sender(View view){
        TextView editText1=findViewById(R.id.editText1);
        try {
            byte[] arb=new byte[1024];
            String one=editText1.getText().toString();
            int k=one.length();
            for (int i=0;i<k;i++){
                arb[i]=(byte)one.charAt(i);
            }
            InetAddress inetadress=InetAddress.getByName("224.5.1.7");
            DatagramPacket datagram=new DatagramPacket(arb,arb.length,inetadress,7777);
            MulticastSocket multicastSocket1=new MulticastSocket();
            multicastSocket1.send(datagram);

        } catch (Exception e) {
            // TODO Auto-generated catch block
            e.printStackTrace();
        }
        
    }
```

然后在push_to_talk目录下新建一个类，

```java
public class j extends Thread{
public j(){
    try{
        //加入到Ip为230.0.1，端口为7777的多播组中，并通过创建一个多播套接字来接受多播消息
        MulticastSocket multicastsocket=new MulticastSocket(7777);
        InetAddress inetaddress=InetAddress.getByName("224.5.1.7");
        multicastsocket.joinGroup(inetaddress);

        //无线循环接收来自发送端的消息
        while(true){
            //System.out.print(1);
            byte[] arb=new byte[100];
            DatagramPacket datagrampacket=new DatagramPacket(arb,arb.length);
            //	System.out.print(1);
            multicastsocket.receive(datagrampacket);
            System.out.println("接收到消息");
            //	System.out.print(1);
            System.out.println(new String(arb));
            
        	}
    	}catch(Exception e){}

	}

}
```

## 测试

send是完全没有问题的：

send内容为send：

![](/images/push_to_talk_imgs/send1.png)

结果：

![](/images/push_to_talk_imgs/send结果1.png)

send内容为se：

![](/images/push_to_talk_imgs/send2.png)

结果：

![](/images/push_to_talk_imgs/send结果2.png)

可惜，接收是有问题的。不太能接收到。

## 说明与收获

1. 为什么又不用手机，用这个难看还不好用的Genymotion虚拟机：前两天把我的安卓手机卖掉换成了iPhone……之后如果时间允许我会尝试学习iOS开发。
2. 为什么之前用doc，现在用Markdown写报告了？原因有两点：
   1. 我最近设法并成功使用Github Pages搭建了[我的个人博客首页](https://yorksunsdu.github.io/)，所以使用Markdown写报告更方便将报告上传至我的博客。
   2. 之前有同学在课程群里问实验报告可不可以用Markdown写，老师回复："good"。我觉得大多数人都用doc写，那用Markdown写会显得与众不同。所以我也学习了使用Typora写Markdown，并花了一整个下午完成了我的第一篇博文：[如何用Typora编写Markdown文件](https://yorksunsdu.github.io/2022/04/howToUseTypora/)。
3. 这次的project在压缩之前记住要在build里面clean project了。clean之后项目文件夹的大小只有2.5MB，打成zip包的大小只有550KB，效果十分显著。
4. 用Markdown相比于用doc写报告并上传git，最大的麻烦就是Markdown只能用链接的方式显示图片。所以上传到git的项目包括：图片目录、Markdown报告、源码zip。无法实现老师要求的一次实验两个文件。

## 源码

[push_to_talk源码链接](https://github.com/YorkSunSdu/YorkSunSdu/tree/master/AndroidDevelopment/push_to_talk)

<div style="color: red; font-size:24px">商业转载请联系博主获得授权，非商业转载请注明出处！</div>

分享结束，大家辛苦了。散会！

