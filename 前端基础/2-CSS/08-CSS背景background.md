[CSS背景（background）篇\_css background\_何北木的博客-CSDN博客](https://blog.csdn.net/weixin_44908855/article/details/106887629)
#### background-image设置背景图
`background-image:url("imgs/main_bg.jpg")`
#### background-repeat背景图重复
默认情况下，背景图会铺满整个页面，背景图大小不够铺满整个页面时，背景图会在横坐标和纵坐标中进行重复
background-repeat: repeat | no-repeat | repeat-x

**repeat** 横、纵坐标重复（默认值）
**no-repeat** 不重复
**repeat-x repeat-y** 只在x轴重复、y也一样


#### background-size设置背景图的尺寸
**contain** 图片要完整的显示在指定的区域，不能改变图片的比例,可能有部分空白区域
**cover** 图片撑满整个指定区域，而且比例不变，可能会溢出
**100%** 横向撑满，纵向按比例缩放
**100% 100%** 横、纵向撑满，图片比例可能会发生变化
**x y**可以设置数值代表横、纵向的像素尺寸

#### background-position设置背景图位置

预设值： left、bottom、right、top、center（居中）
|center|背景图横、纵向居中|
|center top|横向居中，纵向靠上|
|center bottom|横向居中，纵向靠下|
|left center|横向靠左，纵向居中|

也可以用数值或百分比如background-position：10px 10px 表示横、纵坐标离左边、上边边框的距离；

#### 简写background
- background：url(“imgs/main.jpg”) no-repeat 50% 50%/100% fixed #000  
    顺序为设置图片、不重复、位置、尺寸、视口、背景颜色，因为位置和尺寸都有可能为百分比，所有浏览器规定尺寸必须写在位置后面中间加/隔开。
#### 背景透明(CSS3)
```js
 background: rgba(0,0,0,0.3); 
```
最后一个参数是alpha 透明度 取值范围 0~1之间

注意： 背景半透明是指盒子背景半透明， 盒子里面的内容不受影响。

#### 11. 背景图和`img`属性的区别

1. img元素属于HTML的概念，背景图属于css的概念
2. 当图片属于网页内容时，必须使用img元素
3. 当图片仅用于美化页面时，必须使用背景图