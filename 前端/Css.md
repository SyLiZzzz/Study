#### **Css:重叠样式表。用来布局，美化页面**

**Css组成：**选择器+声明 

1. 声明体是以key/value的形式存在的，每一个属性与属性之间是以分好间隔区分的。
2. **基本选择器**：标签选择器，类选择器，id选择器。
3. **派生选择器**：选择器分组，后代选择器，子元素选择器，相邻兄弟选择器。
4. **伪类**：伪类元素，伪类选择器。

- **标签选择器：**标签本身，作用于所有该标签，权重为：1。

- **类选择器：**.代表类选择，作用所有相同类选择器的标签，权重为：10。

- **id选择器：**#代表id选择器，作用于当前一个标签，权重为：100。

- **选择器分组：**以，间隔区分不同的基本选择器，作用于所有分组选择器都作用到。

- **后代选择器：**以空格间隔区分不同的基本选择器，作用于最后一个选择器。

- **子元素选择器：**以>间隔区分不同的基本选择器，作用于>后面的选择器。

- **相邻兄弟选择器：**以+间隔区分不同的基本选择器，作用于+后面的选择器。

- **伪类元素：** ::before(在元素前面添加内容)，::after(在元素后面添加内容)。

- **锚伪类：**

  ```html
  	a:link{}   未访问的连接    
  	a:visited{}  已访问的连接    
  	a:hover{}  鼠标选中的连接
  	a:active{}  选中的连接
  ```

**Css创建：**外部式<内部式<内联式

**Css特性：**css渲染先后顺序是通过权重值的大小来区分的，若两种样式权重值一样，新样式会冲旧样式，样式是能继承的。

**Css属性定义背景效果:**

1. background-color：背景颜色
2. background-image：背景图片
3. background-repeat：水平或垂直平铺    repeat-x/y
4. background-attachment：设置背景图像是否固定或者随着页面的其余部分滚动.scoll滚动(默认)，fixed固定
5. background-position：改变图片在背景的位置  
6. 简写 ：background color image repeat attacgment position
7. background-size  背景大小

**Css文本**

1. text-decoration:none：删除下划线
2. text-decoration：overline ：文本上的一条线
3. text-decoration：underline：文本下的一条线
4. text-decoration：line-through：定义传过文本下的一条线
5. text-align :对齐方式   left Center right
6. letter-spacing 设置字符间距(中文)
7. word-spacing  设置单词间距(英文)
8. text-shadow :文本阴影                水平偏移量(必选)    垂直偏移量(必选)    阴影模糊度   阴影颜色
9. line-height : 行高
10. vertical-align ：设置文本垂直方向对齐  top  middle  bottom

**Css字体**

    1. font-style：nomal：文本正常
       2. font-style：italic ：文本斜土
       3. font-style：oblique：文本倾斜
       4. font-width : 字体粗细
       5. font-variant ：以小型大写字体或正常字体展示
       简写  font:style variant size weight

**Css列表**

1. ul{list-style-type:circle;}：空心圆
2. ul{list-style-type:square}：实心正方形
3. ul{list-style-type:upper-roman}:次数
4. ul{list-style-type:lower-alpha}：字母显示

**Css链接**

​	a:link 未访问链接										a:visited 已访问链接

​	a:hover 鼠标悬停										a:active  鼠标点击

​		注：a:hover必须放在 a:link 和 a:visited后面，a:active必须放在a:hover后面

**Css边框**

​	Border-color 边框颜色  

​    Border-style 边框类型    solid 实线   dotted 点状  double 双线  dashed虚线