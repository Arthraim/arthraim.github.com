---
comments: true
date: 2009-06-08 22:49:31
layout: post
slug: asp-net-twitter-api-client
title: ASP.NET调用twitter api实现简单的功能
wordpress_id: 59
categories:
- Programing
tags:
- ASP.NET
- C#
- Twitter
---




很让人高兴的是在敏感时期过后，我们可爱的twitter又能上去了。今天更新的tweet特别多，很多人都在欢呼雀跃，不过大多数用着其他渠道发的人还是继续保持步调。于是在一星期前就有过看twitter api的想法，今天还是得以如愿以偿了。不过看着看着我就偷了个懒，在blogengine.net的开源项目里找到了一段.net实现的通过feed方式获取tweets的方法。把代码整理了一下，去掉了blogengine的widget成分，做成个页面……




**[Follow me](http://twitter.com/arthraim/)**




**Links:**




[BlogEngine.net](http://www.dotnetblogengine.net/) | blogEngine.net的官方网站，下载完整博客文件后，查看widget中的twitter部分就可以了。  

	[TwitterFeed-Widget for BlogEngine.NET](http://twitterfeedwidget.codeplex.com/) | 这是在上面那些代码的基础上修改的一个codeplex开源项目，稍微优化了一下，其实读下来差不多。  

	[twitter api 中文文档](http://www.54chen.com/c/591) | twitter api的翻译，来自陈臻（@54chen）。




**前台代码**。加入页面，如Default.aspx。



    
    <asp:HyperLink runat="server" ID="hlTwitterAccount" Text="Follow me on Twitter" rel="me" />
    <asp:Repeater runat="server" ID="repItems" OnItemDataBound="repItems_ItemDataBound">
        <ItemTemplate>
            <img src="twitter.ico" alt="Twitter" />
            <asp:Label runat="server" ID="lblDate" style="color:gray" /><br />
            <asp:Label runat="server" ID="lblItem" /><br /><br />
        </ItemTemplate>
    </asp:Repeater>
    







**后台代码**。



    
    using System;
    using System.Configuration;
    using System.Data;
    using System.Xml;
    using System.Text.RegularExpressions;
    using System.Web;
    using System.Web.Security;
    using System.Web.UI;
    using System.Web.UI.HtmlControls;
    using System.Web.UI.WebControls;
    using System.Web.UI.WebControls.WebParts;
    using System.Collections.Specialized;
    using System.Collections.Generic;
    using System.Net;
    using System.Globalization;
    public partial class _Default : System.Web.UI.Page
    {
        protected void Page_Load(object sender, EventArgs e)
        {
            ShowTweets();
        }
        private static DateTime LastModified = DateTime.MinValue;
        private const string CACHE_KEY = "twits";
        private void ShowTweets()
        {
            //用户名
            string _username = "arthraim";
            //用户的twitter地址
            string _url = "http://twitter.com/" + _username + "/";
            //FEED地址（user_timeline为自己的，可以根据需要修改）
            string _feedurl = "http://twitter.com/statuses/user_timeline/" + _username + ".rss";
            //显示的最大条目数
            int maxItems = 30;
            hlTwitterAccount.NavigateUrl = _url;
            if (_feedurl != null)
            {
                if (HttpRuntime.Cache[CACHE_KEY] != null)
                {
                    string xml = (string)HttpRuntime.Cache[CACHE_KEY];
                    XmlDocument doc = new XmlDocument();
                    doc.LoadXml(xml);
                    BindFeed(doc, maxItems);
                }
                if (DateTime.Now > LastModified.AddMinutes(15))
                {
                    LastModified = DateTime.Now;
                    BeginGetFeed(_feedurl);
                }
            }
        }
        protected void repItems_ItemDataBound(object sender, RepeaterItemEventArgs e)
        {
            Label text = (Label)e.Item.FindControl("lblItem");
            Label date = (Label)e.Item.FindControl("lblDate");
            Twit twit = (Twit)e.Item.DataItem;
            text.Text = twit.Title;
            date.Text = twit.PubDate.ToString("MMMM d. HH:mm");
        }
        private void BeginGetFeed(string url)
        {
            HttpWebRequest request = (HttpWebRequest)HttpWebRequest.Create(url);
            request.Credentials = CredentialCache.DefaultNetworkCredentials;
            request.BeginGetResponse(EndGetResponse, request);
        }
        private void EndGetResponse(IAsyncResult result)
            {
            HttpWebRequest request = (HttpWebRequest)result.AsyncState;
            using (HttpWebResponse response = (HttpWebResponse)request.GetResponse())
            {
                XmlDocument doc = new XmlDocument();
                doc.Load(response.GetResponseStream());
                HttpRuntime.Cache[CACHE_KEY] = doc.OuterXml;
            }
        }
        private void BindFeed(XmlDocument doc, int maxItems)
        {
            XmlNodeList items = doc.SelectNodes("//channel/item");
            List<Twit> twits = new List<Twit>();
            int count = 0;
            for (int i = 0; i < items.Count; i++)
            {
                if (count == maxItems)
                    break;
                XmlNode node = items[i];
                Twit twit = new Twit();
                string title = node.SelectSingleNode("description").InnerText;
                if (title.Contains("@"))
                    continue;
                if (title.Contains(":"))
                {
                    int start = title.IndexOf(":") + 1;
                    title = title.Substring(start);
                }
                twit.Title = ResolveLinks(title);
                twit.PubDate = DateTime.Parse(node.SelectSingleNode("pubDate").InnerText, CultureInfo.InvariantCulture);
                twit.Url = new Uri(node.SelectSingleNode("link").InnerText, UriKind.Absolute);
                twits.Add(twit);
                count++;
            }
            twits.Sort();
            repItems.DataSource = twits;
            repItems.DataBind();
        }
        private static readonly Regex regex = new Regex("((http://|https://|www\.)([A-Z0-9.\-]{1,})\.[0-9A-Z?;~&\(\)#,=\-_\./\+]{2,})", RegexOptions.Compiled | RegexOptions.IgnoreCase);
        private const string link = "<a href="{0}{1}" rel="nofollow">{1}</a>";
        /// <summary>
        /// The event handler that is triggered every time a comment is served to a client.
        /// </summary>
        private string ResolveLinks(string body)
        {
            CultureInfo info = CultureInfo.InvariantCulture;
            foreach (Match match in regex.Matches(body))
            {
                if (!match.Value.Contains("://"))
                {
                    body = body.Replace(match.Value, string.Format(info, link, "http://", match.Value));
                }
                else
                {
                    body = body.Replace(match.Value, string.Format(info, link, string.Empty, match.Value));
                }
            }
            return body;
        }
        private struct Twit : IComparable<Twit>
        {
            public string Title;
            public Uri Url;
            public DateTime PubDate;
            public int CompareTo(Twit other)
            {
                return other.PubDate.CompareTo(this.PubDate);
            }
        }
    }
    




[修改自开源项目blogengine.net]
