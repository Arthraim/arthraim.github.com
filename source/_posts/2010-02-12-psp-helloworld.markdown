---
comments: true
date: 2010-02-12 13:58:15
layout: post
slug: psp-helloworld
title: PSP的hello,world
wordpress_id: 142
categories:
- Programing
tags:
- Hello World
- PSP
- 教程
---




有人记得这个教程不？不知道写得大大到哪里去了，不过问题不大，资料是很多的，要写一个psp的helloworld还不简单？（我记得很久很久以前就用devkitpro写过了）如果你愿意用psptoolchain来写，那么就根据上面那个链接里的教程先搭建一个环境吧。如果是windows的用户，可以去搜索一下devkitpro，很傻瓜的。话说还有一个教程也不错，看看这里吧。额，其实今天只是觉得很久没有更新博客了太不厚道，所以来水一下……




[![](/images/uploads/zb/2010-02-12_psp_helloworld.jpg)](/images/uploads/zb/2010-02-12_psp_helloworld.jpg)




代码如下。




    #include <pspkernel.h>
    #include <pspdebug.h>
    PSP_MODULE_INFO("Hello World", 0, 1, 1);
    #define printf pspDebugScreenPrintf
    /* Exit callback */
    int exit_callback(int arg1, int arg2, void *common) {
        sceKernelExitGame();
        return 0;
    }
    /* Callback thread */
    int CallbackThread(SceSize args, void *argp) {
        int cbid;
        cbid = sceKernelCreateCallback("Exit Callback", exit_callback, NULL);
        sceKernelRegisterExitCallback(cbid);
        sceKernelSleepThreadCB();
        return 0;
    }
    /* Sets up the callback thread and returns its thread id */
    int SetupCallbacks(void) {
        int thid = 0;
        thid = sceKernelCreateThread("update_thread", CallbackThread, 0x11, 0xFA0, 0, 0);
        if(thid >= 0) {
            sceKernelStartThread(thid, 0, 0);
        }
        return thid;
    }
    int main() {
        pspDebugScreenInit();
        SetupCallbacks();
        printf("Hello World");
        sceKernelSleepThread();
        return 0;
    }</pspdebug.h></pspkernel.h>




当然如果要正常编译，还需要一个Makefile文件。如下：




    TARGET = hello
    OBJS = main.o
    CFLAGS = -O2 -G0 -Wall
    CXXFLAGS = $(CFLAGS) -fno-exceptions -fno-rtti
    ASFLAGS = $(CFLAGS)
    EXTRA_TARGETS = EBOOT.PBP
    PSP_EBOOT_TITLE = Hello World
    PSPSDK=$(shell psp-config --pspsdk-path)
    include $(PSPSDK)/lib/build.mak




Makefile决定了产生的EBOOT.PBP文件的一些信息，比如TITLE是Hello World，如果要加上图pic0 pic1那都是在Makefile里写得，不过只是helloworld就淡定一定吧 = = PSPSDK的环境变量当然也需要你事先配置好的（没打算说怎么配置环境）。




以上。
