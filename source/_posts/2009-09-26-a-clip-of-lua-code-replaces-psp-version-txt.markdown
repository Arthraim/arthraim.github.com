---
comments: true
date: 2009-09-26 00:55:38
layout: post
slug: a-clip-of-lua-code-replaces-psp-version-txt
title: 破解PSP 5.55系统的雷人Lua代码
wordpress_id: 109
categories:
- Programing
tags:
- 破解
- Lua
- PSP
---




今天看到了一段很雷人的代码，是一段lua的脚本，这是一个邪恶的东西，因为TGS上放出了一个MGS和平行者的试玩版，所以PSP玩家当然都涌上去了，但是PSP的破解现在是滞后的，所有的DEMO会查看你的内核版本，必须要求5.55以上（或者5.51以上），但是现在最新的自制固件（破解的内核）是5.50，所以都是没办法玩的。




当然办法不是没有，要说起PSP的flash0（下次有机会一定要好好写写PSP的破解）。大概介绍下，flash0是存放内核的空间，而flash1是存放用户设置的地方。内核的验证也就很简单（不是所有的验证都这样，因为新内核会有新插件、新模块），验证一个txt文本文件 ---- version.txt。名字叫的太高调了。那么现在欺骗的办法就有了，只要改version.txt！但是不得不说一下version.txt，这个东西不是我凭空讲讲写写改个数字就好了，里面有一堆信息，这里我贴个5.55的version.txt。



    
    release:5.55:
    build:3230,0,3,1,0:builder@vsh-build6
    system:52556@release_555,0x05050510:
    vsh:p6434@release_555,v52629@release_555,20090608:
    target:1:WorldWide




很复杂，所谓的5.55只是release的版本号，至于他的其他编译信息非常详细，一般来说（具体情况无从考证）软件都是全部要验证的。




内核更新包只要一出现，非常成熟的解包技术就可以用上了，普通用户都可以做到。解包之后除了各种各样的其他内核文件，当然就有version.txt了，要最正宗的东西，就是它了。只要解包了5.55升级包，然后在其他系统里刷进这个version.txt，那么就达到了欺骗的目的。




另外还有一个背景，就是PSP上的lua解释器，原理也很简单，动态脚本的解释器。当然弊端在于，用解释器来实现破解，方便。但是源代码无疑谁都能看，那么源代码要保密咋办呢？今天看到的雷人之处，就是代码的保护措施，请看下方代码。




代码很长，所以折叠起来了，可以点击"expend source"来展开；  

	另外没有Lua的高亮，所以我用SQL的高亮代替一下，注释看得出就好了。



    
    -- Not edit!!!
    -- DAS IST NUR EINE VORL腢FIGE vERSION DIE oRIGINALLE WIRD BESSER -- ---------------------------------------
    red = Color.new(255,255,255) -- ---------------------------------------
    black = Color.new(0,0,0) -- ---------------------------------------
    -- Hule true make clean -- ---------------------------------------
    while true do -- ---------------------------------------
    -- Learn for school -- ---------------------------------------
    System.memclean() -- ---------------------------------------
    -- make a cooktail -- ---------------------------------------
    CPU = System.setcpuspeed(333) -- ---------------------------------------
    -- Lose your key -- ---------------------------------------
    if System.cfwVersion() > "5.50" then -- ---------------------------------------
    -- Make a Panorama -- ---------------------------------------
    System.message("CGTP is just for 5.50 GEN-B2",0) -- ---------------------------------------
    -- Cookkiing with freiends -- ---------------------------------------
    System.Quit() -- ---------------------------------------
    -- Killer a man killer -- ---------------------------------------
    end -- ---------------------------------------
    -- Ending the story -- ---------------------------------------
    if System.cfwVersion() < "5.50" then -- ---------------------------------------
    -- Not editer because code by Team CGTP -- ---------------------------------------
    System.message("CGTP is just for 5.50 GEN-B2",0) -- ---------------------------------------
    -- MCpsp the Forum -- ---------------------------------------
    System.Quit() -- ---------------------------------------
    -- if system clear then -- ---------------------------------------
    end -- ---------------------------------------
    -- end with sucks -- ---------------------------------------
    if System.powerGetBatteryLifePercent() < 15 then -- ---------------------------------------
    -- make Pandora with vlf -- ---------------------------------------
    System.message("Charge your batterie over 15 %",0) -- ---------------------------------------
    -- Pudinng a joke -- ---------------------------------------
    System.Quit() -- ---------------------------------------
    -- tell you something -- ---------------------------------------
    end -- ---------------------------------------
    -- End the next Story -- ---------------------------------------
    screen:clear(black) -- ---------------------------------------
    -- Do not edit  -- ---------------------------------------
    screen:print(1,1,"5.55 CGTP Installer", red) -- ---------------------------------------
    -- because its my code -- ---------------------------------------
    screen:print(1,20,"By Team CGTP", red) -- ---------------------------------------
    -- its an easy code -- ---------------------------------------
    screen:print(1,50,"Press X to Install 5.55 CGTP", red) -- ---------------------------------------
    -- Installing friends -- ---------------------------------------
    screen:print(1,60,"Press O to Exit", red) -- ---------------------------------------
    -- Love a dog with cookies -- ---------------------------------------
    screen:print(1,30,"www.cngba.com", red) -- ---------------------------------------
    -- and the code makes Pudding -- ---------------------------------------
    screen.flip() -- ---------------------------------------
    --System making piece -- ---------------------------------------
    screen.waitVblankStart(0) -- ---------------------------------------
    -- Wild world maker -- ---------------------------------------
    pad = Controls.read() -- ---------------------------------------
    -- No Joke without Fire -- ---------------------------------------
    X = pad:cross() -- ---------------------------------------
    -- A Womanizer -- ---------------------------------------
    O = pad:circle() -- ---------------------------------------
    -- Codeing this little CFW -- ---------------------------------------
    if O then -- ---------------------------------------
    -- Team Philix see you -- ---------------------------------------
    System.Quit() -- ---------------------------------------
    -- Do nothing without us -- ---------------------------------------
    end -- ---------------------------------------
    -- make some more pudding -- ---------------------------------------
    if X then -- ---------------------------------------
    -- clear to somone -- ---------------------------------------
    function System.EasyAssign(flash) -- ---------------------------------------
    -- definition somethink withoaut some -- ---------------------------------------
    System.unassign("flash"..flash..":") -- ---------------------------------------
    -- Learn for school -- ---------------------------------------
    System.sleep(800) -- ---------------------------------------
    -- Dont be afraid -- ---------------------------------------
    System.assign("flash"..flash..":","lflash0:0,"..flash,"flashfat"..flash..":")
    -- I kill you (not) -- --------------s--------------------------------
    System.sleep(800) -- ---------------f-----fs----------------------------
    -- lua is baaad -- -----------------------------sf---------------------
    end -- ------------------------------fs---------fs-----------------------
    -- Have fun with this tool -- ------s---------fs-----------fs-------------
    System.EasyAssign(0)  -- ----------------------------------fs----------
    -- did you have finished?  -- ---------------------------------------
    screen:print(1,80,"Install 5.55 CGTP...", red) -- --------------
    -- lorning for school -- ----------------------------------sdf----------
    screen:print(1,90,"Do not Shutdown...", red) -- ---------------------
    -- lorning for school -- ---------------------------sdf-----------------
    screen.flip() -- ----------------------------------------------------
    -- lorning for school -- ---------s-------------sfs----------------------
    -- lorning for school -- -----------------------------------sfs--------
    screen.flip() -- ------------------------sfs---------------------------
    -- lorning for school -- ---------------sf---sfsd-------------------------
    screen.waitVblankStart(0) -- ---------------------------------------
    -- lorning for school -- ----------------------gd---------------------
    screen:print(1,100,"Creat Folders...", red) -- ----------------------
    -- lorning for school -- -------------------------------------------
    screen.flip() -- ---------------------------------df------------------
    -- lorning for school -- -------------------------------------------
    screen.waitVblankStart(60) -- -------------dgd-------------------------
    -- lorning for school -- ---------------------------hfh----------------
    kp = System.createDirectory("flash0://kp") -- ----------------------
    -- lorning for school -- -------------------------dhf------------------
    screen.flip() -- ---------------------------------------------------
    -- lorning for school  -- ------------------------------------------
    screen.waitVblankStart(0) -- --------------fhg-------------------------
    -- lorning for school -- -------------------------------------------
    screen:print(140,100,"Done", red) -- -------------------------------
    -- lorning for school -- -------------------------------------------
    screen.flip() -- ---------------------------------------------------
    -- lorning for school -- -------------------------------------------
    screen.waitVblankStart(50) -- --------------------------------------
    -- lorning for school -- -------------------------------------------
    screen.flip() -- ---------------------------------------------------
    -- lorning for school -- -------------------------------------------
    screen.waitVblankStart(0) -- ---------------------------------------
    -- lorning for school -- -------------------------------------------
    screen:print(1,110,"Flashing Files...", red) -- --------------------
    -- lorning for school -- -------------------------------------------
    screen.flip(5) -- --------------------------------------------------
    -- lorning for school -- -------------------------------------------
    System.copyFile("version.txt", "flash0:/vsh/etc/version.txt", 0)
    -- lorning for schoollearning and not edit this code or copy
    screen.flip() -- -------------------------------------------
    -- lorning for schoollearning and not edit this code or copy
    screen.waitVblankStart(0) -- -------------------------------
    -- lorning for schoollearning and not edit this code or copy
    screen:print(140,110,"Done", red) -- -----------------------
    -- lorning for schoollearning and not edit this code or copy
    screen.flip() -- -------------------------------------------
    -- lorning for schoollearning and not edit this code or copy
    screen.waitVblankStart(40) -- -------------------------------
    -- lorning for schoollearning and not edit this code or copy
    screen.flip() -- -------------------------------------------
    -- lorning for schoollearning and not edit this code or copy
    screen.waitVblankStart(0) -- -------------------------------
    -- lorning for schoollearning and not edit this code or copy
    screen:print(1,120,"Shutdown in 3 Seconds", red) -- --------
    -- lorning for schoollearning and not edit this code or copy
    screen.flip() -- -----------------------------------------
    -- lorning for schoollearning and not edit this code or copy
    -- lorning for schoollearning and not edit this code or copy
    -- lorning for schoollearning and not edit this code or copy
    -- lorning for schoollearning and not edit this code or copy
    -- lorning for schoollearning and not edit this code or copy
    -- lorning for schoollearning and not edit this code or copy
    screen.waitVblankStart(120) -- ------------------------------
    -- lorning for schoollearning and not edit this code or copy
    -- lorning for schoollearning and not edit this code or copy
    -- lorning for schoollearning and not edit this code or copy
    screen:clear(black) -- --------------------------------------
    -- lorning for schoollearning and not edit this code or copy
    -- lorning for schoollearning and not edit this code or copy
    -- lorning for schoollearning and not edit this code or copy
    -- lorning for schoollearning and not edit this code or copy
    System.shutdown() -- -----------------------------------------
    -- lorning for schoollearning and not edit this code or copy
    -- lorning for schoollearning and not edit this code or copy
    -- lorning for schoollearning and not edit this code or copy
    -- lorning for schoollearning and not edit this code or copy
    -- lorning for schoollearning and not edit this code or copy
    -- lorning for schoollearning and not edit this code or copy
    -- lorning for schoollearning and not edit this code or copy
    -- lorning for schoollearning and not edit this code or copy
    -- lorning for schoollearning and not edit this code or copy
    -- lorning for schoollearning and not edit this code or copy
    -- lorning for schoollearning and not edit this code or copy
    -- lorning for schoollearning and not edit this code or copy
    -- lorning for schoollearning and not edit this code or copy
    -- lorning for schoollearning and not edit this code or copy
    -- lorning for schoollearning and not edit this code or copy
    -- lorning for schoollearning and not edit this code or copy
    -- lorning for schoollearning and not edit this code or copy
    -- lorning for schoollearning and not edit this code or copy
    -- lorning for schoollearning and not edit this code or copy
    -- lorning for schoollearning and not edit this code or copy
    -- lorning for schoollearning and not edit this code or copy
    end -- -------------------------------------------------------
    -- lorning for schoollearning and not edit this code or copy
    -- lorning for schoollearning and not edit this code or copy
    -- lorning for schoollearning and not edit this code or copy
    -- lorning for schoollearning and not edit this code or copy
    -- lorning for schoollearning and not edit this code or copy
    -- lorning for schoollearning and not edit this code or copy
    -- lorning for schoollearning and not edit this code or copy
    -- lorning for schoollearning and not edit this code or copy
    -- lorning for schoollearning and not edit this code or copy
    -- lorning for schoollearning and not edit this code or copy
    -- lorning for schoollearning and not edit this code or copy
    -- lorning for schoollearning and not edit this code or copy
    -- lorning for schoollearning and not edit this code or copy
    -- lorning for schoollearning and not edit this code or copy
    -- lorning for schoollearning and not edit this code or copy
    end -- ------------------------------------------------------
    -- lorning for schoollearning and not edit this code or copy
    -- lorning for schoollearning and not edit this code or copy
    -- lorning for schoollearning and not edit this code or copy
    -- lorning for schoollearning and not edit this code or copy
    -- lorning for schoollearning and not edit this code or copy
    -- lorning for schoollearning and not edit this code or copy
    -- lorning for schoollearning and not edit this code or copy
    -- lorning for schoollearning and not edit this code or copy
    -- lorning for schoollearning and not edit this code or copy
    -- lorning for schoollearning and not edit this code or copy
    




整理起来是很辛苦的事情，可读性差不过代码还是很简单的，还真有无聊的人去整理了一下（比如说我），整理之后如下。



    
    red = Color.new(255,255,255)
    black = Color.new(0,0,0)
    while true do
        System.memclean()
        CPU = System.setcpuspeed(333)
        if System.cfwVersion() > "5.50" then
            System.message("CGTP is just for 5.50 GEN-B2",0)
            System.Quit()
        end
        if System.cfwVersion() < "5.50" then
            System.message("CGTP is just for 5.50 GEN-B2",0)
            System.Quit()
        end
        if System.powerGetBatteryLifePercent() < 15 then
            System.message("Charge your batterie over 15 %",0)
            System.Quit()
        end
        screen:clear(black)
        screen:print(1,1,"5.55 CGTP Installer", red)
        screen:print(1,20,"By Team CGTP", red)
        screen:print(1,50,"Press X to Install 5.55 CGTP", red)
        screen:print(1,60,"Press O to Exit", red)
        screen:print(1,30,"www.cngba.com", red)
        screen.flip()
        screen.waitVblankStart(0)
        pad = Controls.read()
        X = pad:cross()
        O = pad:circle()
        if O then
            System.Quit()
        end
        if X then
            function System.EasyAssign(flash)
                System.unassign("flash"..flash..":")
                System.sleep(800)
                System.assign("flash"..flash..":","lflash0:0,"..flash,"flashfat"..flash..":")
                System.sleep(800)
            end
            System.EasyAssign(0)
            screen:print(1,80,"Install 5.55 CGTP...", red)
            screen:print(1,90,"Do not Shutdown...", red)
            screen.flip()
            screen.flip()
            screen.waitVblankStart(0)
            screen:print(1,100,"Creat Folders...", red)
            screen.flip()
            screen.waitVblankStart(60)
            kp = System.createDirectory("flash0://kp")
            screen.flip()
            screen.waitVblankStart(0)
            screen:print(140,100,"Done", red)
            screen.flip()
            screen.waitVblankStart(50)
            screen.flip()
            screen.waitVblankStart(0)
            screen:print(1,110,"Flashing Files...", red)
            screen.flip(5)
            System.copyFile("version.txt", "flash0:/vsh/etc/version.txt", 0)
            screen.flip()
            screen.waitVblankStart(0)
            screen:print(140,110,"Done", red)
            screen.flip()
            screen.waitVblankStart(40)
            screen.flip()
            screen.waitVblankStart(0)
            screen:print(1,120,"Shutdown in 3 Seconds", red)
            screen.flip()
            screen.waitVblankStart(120)
            screen:clear(black)
            System.shutdown()
        end
    end




逻辑除去界面和操作，那只有两个意思：复制 version.txt 到 f0:/vsh/etc/ 目录；f0下创建一个kp目录。（目的何在？当然代码作者应该是分析了demo才做的）回头看看前面的干扰的注释是不是好有趣，内容也是颇为有趣~ 娱乐一下而已~




以上。
