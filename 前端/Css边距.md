**Css边距**

```html
 padding :10px 20px;              /*垂直方向，水平方向*/
 padding :10px 20px 30px;         /*上 左右 下*/
 padding :10px 20px 30px 40px     /*上 右 下 左*/
 /*以顺时针方向*/
```

**Css盒子**

​	盒子大小

- 空间大小:width+padding+border+margin
- 实际大小:width+padding+border 
- Box-sizing-border-box：包括padding和border
- Box-sizing-content-box：不包括padding和border

**Css定位**

​	position定位 

​	类型 

1. 静态定位 ：static   清除定位   不脱离标准流   偏移量无效
2. 相对定位 ：relative   以自我为中心，占据标准流的位置   不脱  有效
3. 绝对定位 ：absolute  以父元素为中心，不占位置，脱离标准流位置
4. 固定定位 ：fixed     类似于将 position 设置为 absolute，不过其包含块是视窗本身
5. 粘贴定位 ：sticky     在relative和fixed之间切换

​	偏移量

​       top	bottom   left	right

​	**Css元素隐藏**

​		display :none  元素隐藏     不占位

​		Visibility :hidden: 元素隐藏   占位