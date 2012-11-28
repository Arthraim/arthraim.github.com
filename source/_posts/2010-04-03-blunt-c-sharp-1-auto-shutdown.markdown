---
comments: true
date: 2010-04-03 23:24:13
layout: post
slug: blunt-c-sharp-1-auto-shutdown
title: 'Blunt C# (1): 自动关机'
wordpress_id: 148
categories:
- Programing
tags:
- blunt
- C#
- WinAPI
---

开始好好整理毕业设计的代码，在硬盘里找出很多以前写的C#代码，很多自己写的小应用都蛮有趣的，比如自动关机啊，修改系统变量啊，计算周年纪念日啊之类的无聊的小东西~ 想着最近博客也没有什么值得更新的内容，要不就贴一些以前的这些程序的傻傻的代码吧~ 所以这个blunt C-sharp（钝的C锋利LOL）系列的文章就是贴这些代码的，欢迎大家讨论代码的好坏~ 骂我的时候Arthur会加上"被骂的是过去的我"这个前提的~ hoho~




今天是自动关机的那个程序~




还是2009-1-18的时候在学长的电脑上用SharpDevelop写的。当时和他一起租房子住在外面，后来有段时间他快毕业了，出去找工作了，于是我每天一个人看动画看到睡着，因为租房子要省电费，所以电脑开通宵很心疼，所以就自己写了一个自动关机的程序，其他东西都不是关键，关键还是代码。当时是不知道哪里找来了一段关机的代码，是用C#调用winapi的其实~  截图和代码都放在下面吧~




![](/images/uploads/zb/2010-04-03_bluntcsharp_autoshutdown.png)




话说这个程序现在看起来体验很好，因为时间匹配的时候不会直接把电脑关了，而会出来一个提示框，提示框上倒数30秒（足够我从被窝里爬出来去点取消），如果没点取消就关机~ 哈哈。当时不知道怎么想的，这么一个需求还要自己写个程序~ 果然有热情！




    // 这个结构体将会传递给API。使用StructLayout(...特性，确保其中的成员是按顺序排列的，C#编译器不会对其进行调整。
    [StructLayout(LayoutKind.Sequential, Pack = 1)]
    internal struct TokPriv1Luid
    {
        public int Count;
        public long Luid;
        public int Attr;
    }
    // 以下使用DllImport特性导入了所需的Windows API。
    // 导入的方法必须是static extern的，并且没有方法体。调用这些方法就相当于调用Windows API。
    [DllImport("kernel32.dll", ExactSpelling = true)]
    internal static extern IntPtr GetCurrentProcess();
    [DllImport("advapi32.dll", ExactSpelling = true, SetLastError = true)]
    internal static extern bool OpenProcessToken(IntPtr h, int acc, ref IntPtr phtok);
    [DllImport("advapi32.dll", SetLastError = true)]
    internal static extern bool LookupPrivilegeValue(string host, string name, ref long pluid);
    [DllImport("advapi32.dll", ExactSpelling = true, SetLastError = true)]
    internal static extern bool AdjustTokenPrivileges(IntPtr htok, bool disall,
    ref TokPriv1Luid newst, int len, IntPtr prev, IntPtr relen);
    [DllImport("user32.dll", ExactSpelling = true, SetLastError = true)]
    internal static extern bool ExitWindowsEx(int flg, int rea);
    // 以下定义了在调用WinAPI时需要的常数。这些常数通常可以从Platform SDK的包含文件（头文件）中找到
    internal const int SE_PRIVILEGE_ENABLED = 0x00000002;
    internal const int TOKEN_QUERY = 0x00000008;
    internal const int TOKEN_ADJUST_PRIVILEGES = 0x00000020;
    internal const string SE_SHUTDOWN_NAME = "SeShutdownPrivilege";
    internal const int EWX_LOGOFF = 0x00000000;
    internal const int EWX_SHUTDOWN = 0x00000001;
    internal const int EWX_REBOOT = 0x00000002;
    internal const int EWX_FORCE = 0x00000004;
    internal const int EWX_POWEROFF = 0x00000008;
    internal const int EWX_FORCEIFHUNG = 0x00000010;
    // 通过调用WinAPI实现关机，主要代码再最后一行ExitWindowsEx，这调用了同名的WinAPI，正好是关机用的。
    private static void DoExitWin(int flg)
    {
        bool ok;
        TokPriv1Luid tp;
        IntPtr hproc = GetCurrentProcess();
        IntPtr htok = IntPtr.Zero;
        ok = OpenProcessToken(hproc, TOKEN_ADJUST_PRIVILEGES | TOKEN_QUERY, ref htok);
        tp.Count = 1;
        tp.Luid = 0;
        tp.Attr = SE_PRIVILEGE_ENABLED;
        ok = LookupPrivilegeValue(null, SE_SHUTDOWN_NAME, ref tp.Luid);
        ok = AdjustTokenPrivileges(htok, false, ref tp, 0, IntPtr.Zero, IntPtr.Zero);
        ok = ExitWindowsEx(flg, 0);
    }




坦率的说我也不知道是哪里找来的这个代码，如果您看了，知道这个代码的原作者，也请告诉我，并不是我不尊重他，是我年少无知 =  =




以上~
