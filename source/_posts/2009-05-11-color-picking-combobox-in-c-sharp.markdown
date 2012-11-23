---
comments: true
date: 2009-05-11 21:58:20
layout: post
slug: color-picking-combobox-in-c-sharp
title: C#实现ComboBox选择颜色的控件
wordpress_id: 28
categories:
- Programing
tags:
- C#
- ComboBox
- Winform
---




因为项目需要，要找一个ComboBox选择颜色的控件。于是想自己动手做，一看网上资料很多，很多人很多年前就实现过。不过要不就是我的搜索功力不够，要不就是真的没有C#版本的东西，所以我无聊就把VB版的"翻译"了一下，供大家参考。（VB版本请参照[闵峰4年前的总结](http://blog.csdn.net/hamadou/archive/2005/08/06/447127.aspx)）




在把VB写过来的时候遇到了GetType用法不太一样的问题，VB里GetType(Color)这个语句在C#是不一样的。我尝试过Color.GetType()但是之后就不能再GetProperties了。另外Color必须是一个变量，而不是一个类型。如果用typeof(Color).GetProperties()的话，结果是什么都获取不到。我始终没有得到什么解决的方法，[这里](http://forums.asp.net/p/1143676/1847955.aspx#1847955)有一些关于这个问题老外的讨论。我最终是采取了其他的方法获取，就是System.Enum.GetValues(typeof(KnownColor))，返回一个Array，感觉不是很好，希望得到补充。




另外我加上了一个SelectedColor的属性，可以在自定义控件外得到相应的颜色。不过我想到有一个缺点，就是UserControl上放上ComboBox再处理，那就无法获取SelectedIndexChanged或者SelectedValueChanged等等的方法了，所以不要在UserControl加，直接从ComboBox派生出来更加理想，交给各位看官去修改了。




还看到一个其他的实现方法，类似于Word的颜色选择框，需求和我这个差不多，实现起来要困难一些，放上链接看需要了。[原文](http://blog.csdn.net/ymqpxy/archive/2007/11/29/1907933.aspx) | [详解](http://hi.baidu.com/skynomadism/blog/item/45d1ebcdfe2072560fb345b8.html)（有6篇，链接为第一篇）




Good luck ;)







**实际效果图：**




![](/upload/2009-05-11_ColorComboBox.png)







**另一个我提到的效果：**




![](/upload/2009-05-11_WordTypeColorSelect.png)




**代码：**



    
    public partial class ColorComboBox : UserControl
    {
        private Color _SelectedColor;
        /// <summary>
        /// 已选择颜色
        /// </summary>
        public Color SelectedColor
        {
            get { return _SelectedColor; }
            set { _SelectedColor = value; }
        }
        public ColorComboBox()
        {
            InitializeComponent();
        }
        private void ColorComboBox_Load(object sender, EventArgs e)
        {
            PersonalizeComponent();
        }
        private void PersonalizeComponent()
        {
            this.comboBox1.DrawMode = DrawMode.OwnerDrawFixed;
            this.comboBox1.DropDownStyle = ComboBoxStyle.DropDownList;
            this.comboBox1.ItemHeight = 15;
            this.comboBox1.BeginUpdate();
            this.comboBox1.Items.Clear();
            /*
            //原始代码，期待有心之人的修改
            foreach (PropertyInfo propertyInfo in typeof(System.Drawing.KnownColor).GetProperties())
            {
            this.comboBox1.Items.Add(propertyInfo.Name);
            }
            */
            Array colors = System.Enum.GetValues(typeof(KnownColor));
            foreach (object oneColor in colors)
            {
                this.comboBox1.Items.Add(oneColor.ToString());
            }
            this.comboBox1.EndUpdate();
        }
        private void comboBox1_DrawItem(object sender, DrawItemEventArgs e)
        {
            if (e.Index < 0)    return;
            Rectangle rect = e.Bounds;
            if (e.State == DrawItemState.Selected)
            {
                e.Graphics.FillRectangle(SystemBrushes.Highlight, rect);
            }
            else
            {
                e.Graphics.FillRectangle(SystemBrushes.Window, rect);
            }
            string colorName = comboBox1.Items[e.Index].ToString();
            SolidBrush brush = new SolidBrush(Color.FromName(colorName));
            SelectedColor = brush.Color;
            //缩小选定项区域
            rect.Inflate(-2, -2);
            // 填充颜色
            e.Graphics.FillRectangle(brush, rect);
            // 绘制边框
            e.Graphics.DrawRectangle(Pens.Black, rect);
            Brush brush2;
            // 确定显示的文字的颜色
            if (Convert.ToInt32(brush.Color.R)+Convert.ToInt32(brush.Color.G)+Convert.ToInt32(brush.Color.B)>3*128)
            {
                brush2 = Brushes.Black;
            }
            else
            {
                brush2 = Brushes.White;
            }
            e.Graphics.DrawString(colorName, this.comboBox1.Font, brush2, rect.X, rect.Y);
        }
    }
