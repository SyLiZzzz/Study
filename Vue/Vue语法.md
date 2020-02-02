# Vue语法

**{{}}**：模板语法，data中的变量及表达式。

**v-html**：替换标签内的文本，将数据以html格式进行替换（添加**页面元素**）

**v-text**：替换标签内的文本，直接替换（添加**文本元素**）

**v-bind**：动态绑定属性(样式) 。 如 v-bind:id="www"，  简写 :id="www"

```vue
//绑定style
<span :style="{color:error,fontSize:'32px'}">账户不能为空</span>
let vm=new Vue({
data:{
error:"red"
}})
```

```vue
//绑定class
<span :class="{actve:actve,hassError:ture}">密码不能为空</span>
let vm=new Vue({
data:{
actve:true,//类样式的定义
hassError:true
}})
```

```vue
//以数组的形式绑定类样式
<span :class="[actve1,'hassError1']">得得得得得得</span>
let vm=new Vue({
data:{
actve1:"actve1"
}})
```

v-on:click：绑定点击事件

```vue
<button v-on:click="myfunction"></button>
methods:{
myfunction(){
alert(11);
}}
```

点击事件练习

```
//密码不能为空是span标签默认的类样式:actve,点击了以后,span标签的样式由actve变成hasserror
<span :class="{actve:actve,hassError:hassError}" @click="change">密码不能为空</span>
let vm=new Vue({
data:{
actve:true,
hassError:false
},
methods:{
change(){
	this.actve=false,
	this.hassError=true
}}})
```

## 分支结构

v-if：v-if="js表达式"，v-if="flag"    flag:true  data中的变量=true or false

v-else :与v-if成对出现，if真else假

```
<div v-if="flag">真</div>
<div v-else>假</div>
```

v-for：循环   v-for="(值，索引) in 数组"

```
<tr v-for="(obj,index) in users">
		<td>{{obj.name}}</td>
		<td>{{(obj.sex==1)?'男':'女'}}</td>
		<td>{{obj.phone}}</td>
</tr>
```

##### 事件修饰符

.stop：阻止时间的冒泡机制

.prevent ：阻止元素的默认时间

.self 阻止当前元素的冒泡

.once 事件只执行一次

注：.stop是阻止所有的冒泡机制，而self只阻止当前的元素

双向数据绑定

v-model       如：v-model="name" 与data中的属性一致；

##### 过滤器

Vue.filter :过滤器   Vue.filter("过滤器的名字"，function(arge){ 

//执行数据过滤

return 已过滤的数据

})

调用：{{原数据|过滤器名字（传递参数）|过滤器名字}}；

**局部过滤器**的定义：filter:{

dataFormat(arge){

​	//执行数据过滤

Rturn 已过滤的数据

}}

调用：{{原数据|过滤器名字（传递参数）|过滤器名字}}；

##### 自定义指令

全局自定义指令定义：Vue.directive("指令的名字",{

​	bind(el,binding){

​		//el代表绑定的元素，bind是钩子函数，指页面加载该元素时触发。binding是一个对象，binding.value可以获取到外界传过来的参数

}

})

**注意**：如果只需要绑定一个钩子的时候，简写为：Vue.directive（"指令的名字"，function(el,binding){}）;

局部自定义指令	

​		directives:{

​	lilText:{

​	bind(el,binding){},

}

//简写方式

​	lilHtml(el,binding){}

}

**注意**：在定义自定义指令时不需要加入v-符号，但是在调用时一定要加入v-

#### Vue-resource

> 可以向服务器发送请求的异步加载插件，是vue插件系列的一种

**注意**：由于vue-resource请求发送完，返回的是一个promise对象是无法直接解析的，必须通过.then进行数据解析

this.$http.**get**("请求的url",{“参数列表”}).then((result)=>{},(error)=>{});

**注意**：由于post请求服务直接模型form-data数据，所以需要手动转化成表单形式

this.$http.**post**("请求url",{"参数"}，{emulateJSON:true}).then((result)=>{}，(error)=>{});