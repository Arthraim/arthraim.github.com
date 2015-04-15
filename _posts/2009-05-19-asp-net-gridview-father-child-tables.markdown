---
comments: true
date: 2009-05-19 18:56:00
layout: post
slug: asp-net-gridview-father-child-tables
title: ASP.NET 用GridView实现主从表
wordpress_id: 38
categories:
- Programing
tags:
- .net
- ASP.NET
---

在做项目的时候碰到了类似的需求，要做一个主从表，本来是用ajax，悬停在某一行的时候显示一个气泡，用来显示具体信息，但是对方要求说要在表里显示这些信息，于是主从表应该是比较合适的。扩展一下的话还可以加上各种控制。







**效果**：




![](/images/uploads/zb/2009-05-19_showtable.png)




虽然这样实现看上去不是很舒服，不过也算是简单的主从表的实现了。大体的思路就是在template列里面嵌套另外一个gridview。看下具体的实现方法……（无视业务相关的部分）







**前台** ---- 先添加一个TemplateField在GridView中。 并且编辑好里面的GridView




    <asp:GridView ID="gvhandle" runat="server" Caption="已检查项目" AutoGenerateColumns="False" OnRowDataBound="gvhandle_RowDataBound">
    <Columns>
    <asp:TemplateField Visible="False">
    <ItemTemplate>
    <asp:Label ID="lbhidden" runat="server" Text='<%#Eval("projID") %>'></asp:Label>
    </ItemTemplate>
    </asp:TemplateField>
    <asp:BoundField DataField="projectName" HeaderText="项目名称"/>
    <asp:BoundField DataField="hostName" HeaderText="主持人" />
    <asp:BoundField DataField="college" HeaderText="所在学院/部门" />
    <asp:BoundField DataField="Rank" HeaderText="学院/部门排名情况" />
    <!--就是这里！！-->
    <asp:TemplateField HeaderText="评审细节">
    <ItemTemplate>
    <asp:GridView ID="gvChild" runat="server" AutoGenerateColumns="False">
    <Columns>
    <asp:BoundField DataField="userName" HeaderText="专家姓名"/>
    <asp:BoundField DataField="judgeLvl" HeaderText="评分"/>
    <asp:BoundField DataField="judgeContent" HeaderText="定性评价"/>
    </Columns>
    </asp:GridView>
    </ItemTemplate>
    </asp:TemplateField>
    <asp:BoundField DataField="result" HeaderText="最终处理" />
    </Columns>
    </asp:GridView>








**后台** ---- 写gvhandle的RowDataBound事件




    protected void gvhandle_RowDataBound(object sender, GridViewRowEventArgs e)
    {
        if (((GridView)e.Row.FindControl("gvChild")) != null)
        {
            // 无论什么乱七八糟的代码，主要是在这里绑定GridView里的那个GridView
            string tProjectID = (e.Row.FindControl("lbhidden") as Label).Text;
            string tMissionID = missionID;
            GridView tempGV = (e.Row.FindControl("gvChild") as GridView);
            tempGV.DataSource = new ProjectOperate().GetComplexChildTable(tMissionID, tProjectID);
            tempGV.DataBind();
        }
    }
