# 什么是 vue

1.构建用户界面

-   用 vue 往 html 页面中填充数据，非常方便

    2.框架

-   是一套现成的解决方案，程序员只能遵守框架的规范，去编写自己的业务功能！

-   要学习 vue,就要学习 vue 框架中的规定用法

-   vue 的指令，组件（对 UI 结构的复用），路由、Vuex，vue 组件库

-   掌握以上内容

# vue 特性

1.数据驱动视图

-   数据的变化**会驱动视图**自动更新

-   好处：程序员只需要把数据维护好，页面结构 vue 就会自动渲染

-   注：是单向数据绑定

    ![image-20220624094012805](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20220624094012805.png)

    2.双向数据绑定

> 在网页中，form 表单负责**采集数据**，Ajax 负责**提交数据**

-   js 数据的变化会自动渲染到页面上
-   页面上表单采集的数据发生变化时，会被 vue 自动获取到，更新到 js 数据中

![image-20220624094513782](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20220624094513782.png)

> 注意：数据驱动视图和双向数据绑定的底层原理是**MVVM**(Model:数据源，View:数据视图--DOM,VM:就是 Vue 的实例)

![image-20220624095827548](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20220624095827548.png)

# Vue 的指令

## 1.内容渲染指令

-   `v-text`（几乎不用）

    > -   功能：例：<p v-text="username"></p> --- 将 usrname 值显示在 p 标签内部
    > -   功能 2：例<p v-text="gender">性别</p>-- gender 值会覆盖掉性别
    > -   缺点：会覆盖元素内部原有的内容！

-   `{{}}`（差值表达式）

    > -   功能：在实际开发中用的最多，只是内容的占位符，不会覆盖原有的内容
    > -   内部可使用 JS 语法 例如：` {{tag?ture:false}}`
    > -   注意：**只能用在内容节点，不能用在属性节点**

-   `v-html`

    > -   功能：把带有标签的字符串渲染成真正的 HTML 内容

## 2.属性绑定指令

注意：差值表达式**只能用在元素的内容节点中，不能用在元素的属性节点！**

> -   在 Vue 中可以使用`v-bind:`指令，为元素的属性动态绑定值
> -   可以简写为英文 `:`
> -   **是单向绑定**，当数据源改变时，视图数据更新，但视图数据修改过后，数据源不变！
>
> *   在使用 v-bind 属性绑定期间，如果绑定内容需要动态拼接，则字符串外面应该包裹单引号 ，例如
>
>     ```xml
>     <div :title="'box'+index">这是一个div</div>
>     ```

## 3.事件绑定指令

-   `v-on`简写为`@`

-   语法格式：

    ```xml
    <button @click="add"></button>

    methods:{
       add(){
       //如果在方法中要修改data中的数据，可以通过this访问到
          this.count ++;
       },
    }
    ```

-   `$event` 的应用场景：如果默认的事件对象 e 被覆盖了，则可以手动传递一个$event

    ```xml
    <button @click="add(n,$event)"></button>

    methods:{
       add(n,e){
       //如果在方法中要修改data中的数据，可以通过this访问到
          this.count ++;
       },
    }
    ```

-   事件修饰符

    -   `.prevent` 阻止默认事件

    ```xml
    <a @click.prevent = "xxx">链接</a>
    ```

    -   `.stop` 阻止冒泡事件

    ```xml
    <button @click.stop="xxx">按钮</button>
    ```

-   按键修饰符

    `@keyup.enter`:回车时调用触发事件

    `@keyup.esc`:按`ESC`键调用触发事件

## 4.双向绑定指令

1.`v-model`适用标签

-   `input`输入框
    -   type="radio"
    -   type="checkbox"
    -   type = "xxx"
-   `textarea`
-   `select`

    2.`v-model`修饰符

-   `.number`:自动将用户的输入值转为数值类型

```xml
<input v-model.number="age" />
```

-   `.trim`:自动过滤用户输入的首位空白字符

```xml
<input v-model.trim="age" />
```

-   `.lazy`:在“change”时而非“input”时更新

```xml
<input v-model.lazy="mag" />
```

## 5.条件渲染指令

1.`v-show`的原理是：动态为元素添加或移除`display:none`,来实现元素的显示和隐藏

-   如果要频繁的切换元素的显示状态，用 v-show 的性能会更好

    2.`v-if`的原理是：每次动态创建或移除元素，实现元素的隐藏和显示

-   如果刚进入页面时，某些元素默认不需要被展示，而后期也有可能不需要被展示，用 v-if 性能更好
-   搭配使用：`v-if` `v-else-if` `v-else`

    3.在实际开发中绝大数情况下不用考虑性能问题，**直接使用 v-if 即可**！

## 6.列表渲染指令

`v-for`:语法格式为 （item,index） in list

-   官方建议：只要用到`v-for`指令，就一定要绑定`:key`属性
-   尽量将 id 作为 key 值
-   官方对 key 的类型是有要求的：数字或字符串类型
-   key 的值是千万不能重复的，否则终端（控制台）会报错`Duplicate keys detected`

# 过滤器（vue3 弃）- filters

## 1.私有过滤器

-   过滤器函数，必须定义到`filters`节点之下,其本质为函数

```xml
<p>message的值是：{{message | capi}}</p>

data:(){
   return {
    message:"hello"
  }
},
// 过滤器一定要有返回值
//注意：过滤器函数形参中的val,永远都是"管道符"前面的那个值
filters:{
  capi(val){
     // 字符串中charAt方法，这个方法接收索引值，表示把字符串中把索引对应的字符取出来
     const first = val.charAt(0).toUpperCase();
     // 字符串中的slice方法，可以指定索引，往后截取字符串
     const others = val.slice(1);
     return first+others; //返回首字母大写
   }
},
```

## 2.全局过滤器

-   全局过滤器：独立于每个 vm 实例之外

```xml
//Vue.filer()方法接收两个参数：
// 第一个参数：是全局过滤器的名字
//  第二个参数：是全局过滤器的"处理函数"
Vue.filter('capitalize',(str)=>{
  return str.charAt(0).toUpperCase() + str.slice(1)
})
```

-   **私有过滤器高与全局过滤器**

### 1.格式化时间

-   使用`dayjs`插件 [dayjs 官网](https://dayjs.fenxianglu.cn/category/)

    ```xml
    // 声明格式化时间的全局过滤器
    Vue.filter('dateFormat',function(time){
      //1.对time格式化处理，得到YYYY-MM-DD HH:mm:ss
      //2.把格式化的结果return出去

      //直接调用dayjs()得到的是当前时间
      //dayjs(给定的日期时间)得到指定的日期
      const dtStr = dayjs(time).format('YYYY-MM-DD HH:mm:ss')
      return dtStr

    })
    ```

# watch 监听器-watch

-   作用：`监听数据变化`
-   所有监听器都应该被定义到 watch 节点下
-   监听器的本质是一个函数，要监视哪个数据的变化，就把数据名作为方法名即可

```xml
watch:{
  //newVal:新值，oldVal:旧值
  userName(newVal,oldVal){
   console.log("监听到了username值的变化！");
  }
}
```

-   深度监听

```xml
watch:{
  userName:{
    handler(newVal,oldVal){
       console.log("监听到了username值的变化！");
     },
    //默认值为false,作用是控制监听器是否自动触发一次
    immediate:true,
    // 深度监听
    deep:true,
  }
}
```

-   监听对象的子属性

```xml
watch:{
  "info.userName"(newVal,oldVal){
    console.log("监听子属性");
  }
}
```

# 计算属性 -computed

-   是通过一系列运算后，最终得到的一个属性值
-   所有的计算属性都要定义到`computed`节点之下
-   动态`style`:style={backgroundColor:rgb}

```xml
coumputed:{
   rgb(){
     return `rgb(${this.r},${this.g},${this.b})`
   }
}
```

-   好处

    1.实现了代码复用

    2.只要计算属性中依赖的数据源变化了，则计算属性会自动重新求值

# Axios

> axios 是一个专注于网络请求的库！

1.调用 axios 方法得到的返回值是 Promise 对象

-   基本语法

```xml
axios({
    method:'请求类型',
    url:'请求的 URL 地址',
    //GET 请求参数（URL中的查询参数）
    params:{},
    //请求体参数
    data:{},
}).then((res)=>{
   console.log(res);
})
```

![image-20220701162749515](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20220701162749515.png)

2.解构赋值的时候，用：冒号重命名

-   调用 axios 之后，使用 async/await 进行简化
-   使用解构赋值，从 axios 封装的大对象中，把 data 属性解构出来
-   把解构出来的 data 属性，使用冒号重新命名，一般命名为{data:res}

```xml
const {data:res} = await axios({
                       methods:"GET",
                       url:"XXX",
                       params:{}
                  })
```

3.直接发起 GET/POST 请求

```xml
//GET请求
const {data:res} = await axios.get('url',{
                    //GET请求参数
                     params:{}
                  })
//POST请求
const {data:res} = await axios.post('url',{ /*post请求体数据*/})
```

# Vue-cli

## 1.单页面应用程序

> 英文名：Single Page Application ,简称 SPA,意思是一个 Web 网页中只有唯一的一个 HTML 页面，**所有功能与交互都在这一个页面中**

## 2.Vue-cli

-   概念：

> vue-cli 是 Vue.js 开发的标准工具，他简化了程序员基于 webpack 创建工程化的 Vue 项目的过程

-   安装与使用

> 安装全局命令：`npm install -g @vue/cli`
>
> 创建项目：`vue create 项目名`
>
> 查看版本：`vue -V`

-   创建项目流程

![image-20220707144439809](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20220707144439809.png)

![image-20220707144905534](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20220707144905534.png)

![image-20220707145101105](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20220707145101105.png)

![image-20220707145210529](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20220707145210529.png)

![image-20220707145553419](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20220707145553419.png)

![image-20220707145759392](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20220707145759392.png)

（Manually select features=>自行选择所需内容） ----图二：按空格选中内容

-   Vue 项目中 src 目录的构成

```xml
assets 文件夹：存放项目中用到的静态资源文件，例如：css样式表，图片资源
components 文件夹： 程序员封装的、可复用的组件，都放到此目录下
main.js :项目的入口文件，整个项目的运行，要先执行main.js
App.vue : 是项目的根组件
```

-   Vue 项目运行流程

> 目的：通过 main.js 把 App.vue 渲染到 index.html 的指定区域中

# Vue 组件

> 概念：将可重用的 UI 结构封装为组件，便于开发与项目维护

## 1.Vue 组件的三个组成部分

1. template->组件的模块结构

2. script -> 组件的 JavaScript 行为
3. style ->组件的样式

## 2.使用组件的三个步骤

1.导入需要使用的 .vue 组件

```xml
import Left from '@/components/Left.vue'
```

2.注册组件

```xml
components:{
  Left
}
```

3.以标签形式，使用注册好的组件

```xml
<Left></Left>
```

## 3.注册全局组件

> 在 vue 项目的 main.js 入口文件中，通过 Vue.component()方法，可以注册全局组件

1.导入需要全局注册的组件

```xml
import Count from '@/components/Count.vue'
```

2.注册到 Vue 中

```xml
Vue.component('MyCount',Count) //参数1：组件注册名称；参数2：需要被全局注册的那个组件
```

## 4.组件中 prop 的使用

> `props`是组件的自定义属性，在封装通用组件时，合理的使用 props 可以**极大的提高组件的复用性**

```xml
export default{
   //组件的自定义属性,
   props:["自定义属性A","自定义属性B"...]
  //或
  props:{
     init:{
       default:0, //默认值
       type:Number/String/Object/Array/Boolean, //类型
       required:true,//必填项校验
     }
   },
   //组件的私有数据
   data(){
     return {
      }
    },
}
```

-   props 是只读的，不可修改！

> Vue 中组件中封装的自定义属性是可读的，程序员不可直接修改 props 的值，否则会直接报错：

```xml
Avoid mutating a prop directly...
```

-   要想修改 props 的值，可以将 props 的值转存到 data 中，因为 data 中的数据都是可读可写的

```xml
props:[init],
data(){
 return {
  count:this.init//把this.init转存到count中
  }
}
```

## 5.scoped 的使用和底层实现原理--样式冲突

> 默认情况下，写在.vue 组件中的样式会全局生效，因此很容易造成多个组件之间的样式冲突问题

-   **导致组件之间样式冲突的根本原因**

    1.单页面应用程序中，所有组件的 DOM 结构，都是基于唯一的 index.html 页面进行呈现的

    2.每个组件中的样式，都会影响整个 index.html 中的 DOM 元素

-   实现原理

> 在每个单独的组件中，都会生成唯一的`data-v-xxx`,当样式中：标签 p[id]设定的样式只在当前组件中生效【等同于在 style 中添加 scoped 属性】

```xml
<template>
   <div data-v-001>
       <p data-v-001> Hello</p>
       <span data-v-001>World</span>
   </div>
</template>
<style lang="less">
   p[data-v-001]{
     color:red
    }
</style>
```

## 6.deep 修改子组件样式

> **在父组件中想要修改子组件样式**
>
> （当使用第三方组件库的时候，如果有修改第三方默认样式的需求，需要用到/deep/）

```xml
<style lang="less" scoped>
     /**
    区别：h5=>h5[data-v-0454t3]
         /deep/h5=>[data-v-0454t3] h5(代表父组件下的 子组件h5标签样式设定）
    */
    h5{
      color:red,
    }
    /deep/h5{
      color:yellow,
    }

</style>
```

## 7.Vue 组件的实例化对象

> 解析过程：
>
> `vue`文件---------------->通过`package.json`文件的`vue-template-compiler`组件解析`vue`文件---------------->转化为`Javascript`语法的文件------->浏览器渲染

# Vue 生命周期

> 概念：生命周期（Life Cycle）是指一个组件从创建->运行->销毁的过程，强调的是一个时间段

![image-20220708160230744](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20220708160230744.png)

-   beforeCreate:组件的 props/data/methods 尚未被创建，都处于不可用的状态
-   **created**【发起 ajax 请求拿数据】:组件的 props/data/methods 已创建好，都处于可用的状态，但是组件的模板(DOM)结构尚未生成
-   beforeMount:将内存中编译好的 html 渲染到浏览器中，此时浏览器中还没有当前组件的 DOM 结构
-   **mounted**【最早操作 DOM 元素】:已经将内存中的 html 结构，成功渲染到浏览器之中，此时浏览器中已包含当前的 DOM 结构
-   beforeUpdate【数据改变，视图还未渲染】:将要根据变化之后，最新的数据，重新渲染组件的模板结构（最少执行 0 次）

-   **updated**:【数据改变，视图渲染成功】：已经根据最新的数据，完成了组件 DOM 结构的最新渲染（最少执行 0 次）

<注：关于 updated:当数据变化之后，为了能够操作到最新的 DOM 结构，必须把代码写到 updated 生命周期函数中>

-   beforeDestroy:将要销毁此组件，此时尚未销毁，组件还处于正常工作状态
-   destroyed:组件已经被完全销毁，此组件在浏览器中对应的 DOM 结构已被完全移除（销毁当前组件的数据侦听器、子组件、事件监听）

​

# 组件之间的数据共享

## 1.组件之间的关系

> 最常见的关系
>
> 1.父子关系
>
> 2.兄弟关系

## 2.父组件向子组件共享数据 --自定义属性

```xml
//父组件
<Son :msg="msg" :user = "userinfo"></Son>

data(){
   return {
     msg:"你好"，
     user:{
        age:18
     }
   }
},

//子组件
<template>
 <div>
    <h5>我是子组件</h5>
     <p>父组件闯过来的msg值是{{msg}}</p>
     <p>父组件闯过来的user值是{{user}}</p>
  </div>
</template>

props:["msg","user"]
```

## 3.子组件向父组件共享数据 --自定义事件

```xml
//父组件
<Son  @numchange="getNewCount"></Son>

data(){
   return {
    countFromSon:0;//定义接收子组件
   }
},
methods:{
  getNewCount(val){
	this.countFromSon = val;
 }
},

//子组件
<template>
 <div>
    <h3>子组件Count:{{count}}</h3>
     <button @click = "add">值+1</button>
  </div>
</template>
<script>
    data:{
        return {
       	 count:0
        }
    },
    methods:{
        add(){
       	 this.count +=1;
         // 当数据改变时，通过$emit()触发自定义事件
         this.$emit('numchange',this.count)
        }
    }
</script>
```

## 4.兄弟组件的数据共享--EventBus

> 在 vue2.x 中，兄弟组件之间数据共享的方案是`EventBus`

![image-20220711105233388](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20220711105233388.png)

**使用步骤：**

1. 创建`eventBus.js`模块，并向外共享一个 Vue 的实例对象

2. 在数据发送方，调用 bus.$emit('事件名称'，要发送的数据)方法，触发自定义事件
3. 在数据接收方，调用 bus.$on('事件名称'，事件处理函数)方法，注册一个自定义事件【bus.on 事件实时监听 share 方法】【固定写法】

# Ref 引用

**数据驱动视图**（在 view 中强烈建议大家不要用 jquery）

> ref 用来辅助开发者在不依赖 jQuery 的情况下，获取 DOM 元素或组件的引用
>
> 在 vue 的每个组件实例中，都包含一个`$refs`对象，里面存储这对应的 DOM 元素或组件的引用，默认情况下，组件的`$refs`都**指向一个空对象**
>
> 注意：$开头的都是 vue 内置的成员

## 1.基础使用-直接操作 DOM

```xml
<h3 ref="myh3">学习ref的使用</h3>
<button @click="changeMyH3Color"></button>
<script>
  methods:{
     changeMyH3Color(){
     		console.log(this.$refs.myh3);//此为h3 dom 的标签
    		this.$refs.myh3.style.color = "red";//设定字体颜色为红色
  	  },
    }
</script>
```

## 2.使用 ref 引用页面上的组件实例 - 父调用子组件

> 父组件直接调用子组件的方法或者属性

```xml
//父组件
<button @click = "resetLeftCount"></button>
<Left ref="comLeft"></Left>

methods:{
  resetLeftCount(){
	this.$refs.comLeft.resetCount();
    //或者
    this.$refs.comLeft.count = 0;
 }
}

//子组件
data:{
 return {
	count:0
  }
},
methods:{
 resetCount(){
   this.count = 0;
 }
}
```

## 3.this.$nextTick 的使用

> 当有些代码需要在 DOM 元素更新完后执行，就可以使用`this.$nextTick`
>
> 概念：组件的`$nextTick(cb)`方法，会把 cb 回调`推迟到下一个DOM更新周期之后执行`。通俗的理解是：等组件的 DOM 更新完成之后，再执行 cb 回调函数，从而能保证 cb 回调函数可以操作到最新的 DOM 元素

```xml
<input type="text" v-if ="inputVisible" ref="iptRef"/>
<button @click="showInput"></button>

data:{
  return {
	inputVisible:false
 }
},
methods:{
 showInput(){
   this.inputVisible = true;
   // 让展示出来的文本框，自动聚焦
   this.$refs.iptRef.focus();(×)

  /*  this.$refs.iptRef.focus(); 返回结果报错：focus is undefined
     原因：inputVisible 这个值已经为true,但页面DOM还未渲染完成，导致获取不到iptRef此标签
     解决方案：使用this.$nextTick(()=>{})
  */
  （√）
  this.$nextTick(()=>{
    this.$refs.iptRef.focus();
   })
  }
}
```

-   为什么 updated 不行

> 其实下面这方法是可行的，但由于是 v-if 切换，当值为 false 的时候，也会执行 updated 方法，导致报错，所以才不可行
>
> 解决报错 ：多加一层判断，当有值的时候再进行聚焦即可（`if(this.$refs.iptRef) {}`）

```xml
updated:{
   this.$refs.iptRef.focus()
}
```

# 数组里的方法

## 1.some

> 从数组里面找元素，some 比较合适，因为一旦找到相对应元素，就会停止循环

-   forEach 循环一旦开始，无法在中间被停止-效率比较低

```xml
const arr = ["小红"，"小黄","小绿"];
arr.forEach((item,index)=>{
  if(item==="小黄"){

   }
})
```

-   some 一旦找到相对应元素，就会停止循环

```xml
const arr = ["小红"，"小黄","小绿"];
arr.some((item,index)=>{
  if(item==="小黄"){
    // 找到对应的项后，通过return true固定语法来终止some循环
     return true;
   }
})
```

## 2.every

> 当每一个值都符合条件时，返回 true,否则返回 false

```xml
const arr = [
{id:1,name:"西瓜",state:true},
{id:2,name:"香蕉",state:true},
{id:3,name:"草莓",state:true},
]
const result = arr.every(item=>item.state);
console.log(result);//true
```

## 3.reduce

> 其实是**累加器**，将每次循环的结果累加起来
>
> `arr.filter(item=>item.state).reduce((累加的结果，当前循环项)=>{ },初始值)`

-   基本用法

需求：把购物车数组中，已勾选的水果，总价累加起来

```xml
const arr = [
{id:1,name:"西瓜",state:true,price:10,count:1},
{id:2,name:"香蕉",state:false,price:30,count:4},
{id:3,name:"草莓",state:true,price:50,count:3},
]

***方法1***
let amt = 0;//总价
arr.filter(item=>item.state).forEach(item=>{
  amt += item.price * item.count
})

***方法2***
arr.filter(item=>item.state).reduce((amt,item)=>{
 return amt += item.price * item.count
},0)
```

-   简化写法

> 去除 return 和 花括号{ }

```xml
arr.filter(item=>item.state).reduce((amt,item)=> amt += item.price * item.count,0)
```
