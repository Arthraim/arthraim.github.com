---
comments: true
date: 2010-05-09 14:46:14
layout: post
slug: pageload_an_is_post_back
title: ASP.NET的page_load和IsPostBack
wordpress_id: 425
categories:
- Programing
tags:
- ASP.NET
- debug
- IsPostBack
- page_load
---

为啥碰到好多人问，然后这两天调某个网站的代码的时候我又碰到了这个问题，其实说出来很简单，不过你说你发现个bug，要想到是因为这个原因还真是恼人而且一下子想不到。Arthur同学每次遇到某些症状，比如页面元素无法提交修改，我都要去Google一下，最后都会发现就是这个原因啊…… 其实针对page_load事件，MSDN上解释的很清楚：




> 
	
> 
> To prevent data and mouse click events from being overwritten, any binding code in the Page_Load event handler is placed within a Not IsPostBack conditional block, which prevents the binding code from being called during postbacks.
> 
> 





虽然我难以理解设计师们的考量，但是在我看来从逻辑上说page_load这个事件总是会让人理解为只在load页面的时候发生。我总是忘了ispostback，所以总是碰到无法提交修改的问题。主要是我还真是好久没有写ASP.NET，昨天真是费解了一把，然后突然想起来很久以前貌似也被这个问题困惑过不止一次。




贴个代码：



    
    protected void Page_Load(object sender, EventArgs e)
    {
        User loginUser = Session["user"] as User;
        this.TextboxID.Text = loginUser.ID;
        if (!Page.IsPostBack)
        {
            this.TextboxName.Text = loginUser.Name;
        }
    }




这样写page_load，那如果页面上有一个按钮提交Textbox里修改好的内容，那id是不会被提交的，name则会。




写个文章加深印象，不要再忘记了啊 T T




以上……
