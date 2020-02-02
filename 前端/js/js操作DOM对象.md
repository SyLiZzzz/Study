# js操作DOM对象

this关键字作为对象函数调用 作用到对象。

this关键字作为构造函数 作用到新创建的对象

this关键字作为函数调用 作用到全局对象

this关键字作为时间传递时  指的是元素本身

### 节点树

根据id获取当前节点的方法：getElementById();

根据标签名获取当前节点的方法：getElementsByTagName();

根据类名获取当前节点的方法：getElementsByClassName();

根据name属性获取当前节点的方法：getElementsByName()；

获取当前节点集合的任意节点：item(索引)；数组[索引]

获取当前节点的属性：getAttribute();

设置当前节点的属性：setAttribute();

删除当前节点的属性：removeAttribute(属性名)；

### Dom节点属性

获取子节点的集合：childNodes; children

获取子节点集合的任意节点：(children) childNodes.item(索引)；

获取父节点：parentNode

获取当前节点的下一个兄弟节点：nextSibling nextElementSibling;

##### 操作节点

创建元素节点：createElement(标签名)

将标签字符串插入到文档：write()

当前节点末尾追加一个子节点：appendChild()

当前节点首项追加子节点：insertBefore()

当前节点中删除一个子节点：removeChild();

删除当前节点：remove()

##### 修改

修改文本内容：innerText=" ";

修改当前元素的所有内容：innerHTML=" ";

修改当前元素的值：value=" ";

##### Dom中操作元素Css样式

当前元素节点.style.属性名=值

##### Dom事件

直接在标签内引用

1.<p onclick=" "></p>

2.Document.getElementById("选择器").onclick=function(){}

##### document对象

可以对文档进行节点的crud

##### Event对象

每个事件定义时都会内置一个event对象，可以拿到触发该事件内的函数时的一些属性

e.pageX,e.pageY,keyCode

##### window对象

对应浏览器的窗口，我们在js中定义的函数，变量，对象，都属性window对象。

window.location.href="url";实现页面跳转

alert();弹出框

Confirm()；确认框

Prompt(对话文本，文本提示)；