## 长文本处理

默认-字符太长溢出容器

字符超出部分换行
```css
.wrap {
	overflow-wrap: break-word;
}
```
字符超出部分使用连字符
```css
.hyphens {
	hyphens: auto;
}
```
单行文本超出省略
```css
.ellipsis {
	white-space: nowrap;
	overflow: hidden;
	text-overflow: ellopsis;
}
```
多行文本超出省略
```css
.line-clamp {
	overflow : hidden;
	text-overflow: ellipsis;
	display: -webkit-box;
	-webkit-line-clamp: 2
	-webkit-box-orient: vertical;
}
```

## 消除浏览器默认样式





