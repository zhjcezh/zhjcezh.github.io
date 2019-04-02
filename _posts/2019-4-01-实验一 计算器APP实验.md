---
layout:     post
title:      "实验一 计算器APP实验"
subtitle:   "实验报告"
date:       2019-04-01 20:00:00
author:     "Cfeng"
header-img: "img/post-sample-image.jpg"
catalog: true
tags:
    - Android
    - 学习之路
---
# 实验一 计算器APP实验
## 一、目的：
1. 熟悉Android程序的生命周期；
2. 掌握Android界面设计和控件使用；
3. 掌握Android事件处理方法。

## 二、内容：
* 设计与实现计算器APP，具有以下功能：
1. 输入两个操作数，输出运算结果；
2. 能进行加减乘除运算；
3. 清空操作数和运算结果；
4. 输入错误时能回退一个字符。
***
## 三、设计与实现:

#### 1：App布局：
计算数字显示部分使用TextView，使用setText()更新运算式子。
```
<TextView
    android:id="@+id/textView"
    android:layout_width="match_parent"
    android:layout_height="wrap_content"
    android:gravity="right"
    android:height="100dp"
    android:textSize="40sp" />
```

按钮部分使用TableLayout表格布局，每一行使用TableRow包含多个Button。
```
<TableLayout
    android:layout_width="match_parent"
    android:layout_height="wrap_content"
    android:stretchColumns="*" >
    <TableRow
        android:id="@+id/row1"
        android:layout_width="match_parent"
        android:layout_height="wrap_content" >
        <Button
            android:id="@+id/button_CE"
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:layout_span="2"
            android:onClick="onClick"
            android:text="清空" />
……
	</TableRow>
</TableLayout>
```
#### 2：App功能实现：
申明四个私有属性，分别代表textview显示的东西，运算式子，运算结果，是否新运算
```
private TextView textView;
private String str;
private double result;
private boolean addNum;
```
在用户进行操作时，先用getText()方法，获取显示界面当前运算式子，并赋值给str
1. 清空
判断用户点击按钮是否为button_CE，如果是则将textView置为空
```
textView.setText("");
```
2. 回退
判断用户点击按钮是否为button_Delete，如果是，先判断str是否为空，若非空，则截取前str.length()-1位
```
if(!str.equals("") && str != null){
    textView.setText(str.substring(0, str.length()-1));
}
```
3. 点击数字或小数点
首先判断addNum的值，若位false,则代表新开始一次运算，我们直接将按钮的值赋给textView。
若位true,我们将按钮的值接在str后，再赋给textView
若没有该操作，我们可能会将新值与前一次运算相连，造成结果错误。
```
 if (addNum) {
    textView.setText(str+((Button)v).getText());
}else{
    textView.setText(((Button)v).getText());
    addNum = true;
}
```
4. 点击运算符号
判断式子钟之前是否有符号，如果有则取消当前操作。
如果没有则直接加在式子后
这里可以进行addNum判断，若果位false则代表用户想根据之前的计算结果继续运算
```
 if (str.contains("+")||str.contains("-")||str.contains("×")||str.contains("÷"))
    return;
else
    textView.setText(str+((Button)v).getText());
if(!addNum)
    addNum = true;
```
5. 计算结果
判断按钮为button_Equal，获取式子钟的操作符，调用自定义getResult(String op)方法
先使用substring获取代表前后两个数字的字符串，之后使用parseDouble将其强制转换成double型进行计算
再通过+""，将其强制转换成字符串，并去掉末尾0
```
String num1 = str.substring(0,str.indexOf(op));
String num2 = str.substring(str.indexOf(op)+1);
try {
    double n1 = Double.parseDouble(num1);
    double n2 = Double.parseDouble(num2);
    if (op.equals("+")) {
        result = n1+n2;
    }else if(op.equals("-")){
        result = n1-n2;
    }else if(op.equals("×")){
        result = n1*n2;
    }else if(op.equals("÷")){
        result = n1/n2;
    }else {
        return;
    }
    String r = result+"";
    if(r.contains(".")&&r.substring(r.length()-1).equals("0")){
        r = r.substring(0,r.indexOf("."));
    }
    textView.setText(r);
    addNum = false;
} catch (Exception e) {
    return;
}
```
***
## 四、遇到的问题
刚开始布局有点困难，所有的按钮都是一个竖排，后来通过TableLayout表格布局实现，其余没有什么大问题，与之前的java计算器作业没有大差别











