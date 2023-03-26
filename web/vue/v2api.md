---
title: V2配置
description: 
published: true
date: 2023-03-26T08:07:26.789Z
tags: 
editor: markdown
dateCreated: 2023-02-26T03:43:59.166Z
---

### 元素选择（el）

1. new vue时传入el属性

`export default new Vue({     el: '#app', })`

1. 创建完实例后进行挂载

`var vm = new Vue({}) vm.$mount('#app')`


### 数据（data）

1. 对象式（非组件使用）

`export default new Vue({     el: '#app',     data: {         age:10,     } })`

1. 函数式（组件使用，**vue3必须使用**）

`export default new Vue({     el: '#app',     data(){         return {             age: 10,         }     } })`


### 方法（methods）

> 由 Vue 管理的函数，一定不要写箭头函数，一旦写了箭头函数，this 就不再是 Vue 实例了。

`export default new Vue({     el: '#app',     data(){         return {             age: 10,         }     },     methods:{         alert_a(){             this.age = 100;         }     } })`

### **nextTick**

1. 语法：`this.$nextTick(回调函数)`
2. 作用：在下一次 DOM 更新结束后执行其指定的回调。
3. 什么时候用：当改变数据后，要基于更新后的新 DOM 进行某些操作时，要在 nextTick 所指定的回调函数中执行。



### **事件**

事件的基本使用：

1. 使用 v-on:xxx 或 @xxx 绑定事件，其中 xxx 是事件名；
2. 事件的回调需要配置在 methods 对象中，最终会在 vm 上；
3. methods 中配置的函数，不要用箭头函数！否则 this 就不是 vm 了；
4. methods 中配置的函数，都是被 Vue 所管理的函数，this 的指向是 vm 或 组件实例对象；
5. @click="demo" 和 @click="demo($event)" 效果一致，但后者可以传参；

`<div id="root">          <h2>欢迎来到{{name}}学习</h2>          <!-- <button v-on:click="showInfo">点我提示信息</button> -->          <button @click="showInfo1">点我提示信息1（不传参）</button>          <button @click="showInfo2($event,66)">点我提示信息2（传参）</button>      </div>  </body>  <script type="text/javascript">      Vue.config.productionTip = false //阻止 vue 在启动时生成生产提示。      const vm = new Vue({          el:'#root',          data:{              name:'尚硅谷',          },          methods:{              showInfo1(event){                  // console.log(event.target.innerText)                  // console.log(this) //此处的this是vm                  alert('同学你好！')              },              showInfo2(event,number){                  console.log(event,number)                  // console.log(event.target.innerText)                  // console.log(this) //此处的this是vm                  alert('同学你好！！')              }          }      })  </script>`

### 计算属性（computer）

`computed:{     //完整写法     fullName:{         //get有什么作用？当有人读取fullName时，get就会被调用，且返回值就作为fullName的值         get(){             //get什么时候调用？1.初次读取fullName时。2.所依赖的数据发生变化时。         },         //set什么时候调用? 当fullName被修改时。         set(value){         }     }     //简写     fullName(){         console.log('get被调用了')         return this.firstName + '-' + this.lastName     } }`

1. 定义：要用的属性不存在，要通过已有属性计算得来。
2. 原理：底层借助了 Objcet.defineproperty 方法提供的 getter 和 setter。
3. get 函数什么时候执行？
4. 初次读取时会执行一次。
5. 当依赖的数据发生改变时会被再次调用。
6. 优势：与 methods 实现相比，内部有缓存机制（复用），效率更高，调试方便。
7. 备注：
8. 计算属性最终会出现在 vm 上，直接读取使用即可。
9. 如果计算属性要被修改，那必须写 set 函数去响应修改，且 set 中要引起计算时依赖的数据发生改变。


### 监视属性（watch）

1. 全局配置

`// 通过 vm.$watch 监视 //正常写法   vm.$watch('isHot',{     immediate:true, //初始化时让handler调用一下     deep:true,//深度监视     handler(newValue,oldValue){       console.log('isHot被修改了',newValue,oldValue)     }   })    //简写   vm.$watch('isHot',(newValue,oldValue)=>{     console.log('isHot被修改了',newValue,oldValue,this)   })`

1. 局部配置

`// new Vue 时传入 watch 配置 watch:{     //正常写法     isHot:{       // immediate:true, //初始化时让handler调用一下       // deep:true,//深度监视       handler(newValue,oldValue){         console.log('isHot被修改了',newValue,oldValue)       }     },      //简写     isHot(newValue,oldValue){       console.log('isHot被修改了',newValue,oldValue,this)     }  }`

### 计算属性和监视属性

### **computed 和 watch 之间的区别：**

1. computed 能完成的功能，watch 都可以完成。
2. watch 能完成的功能，computed 不一定能完成，例如：watch 可以进行异步操作。

### **两个重要的小原则：**

1. 所被 Vue 管理的函数，最好写成普通函数，这样 this 的指向才是 vm 或 组件实例对象。
2. 所有不被 Vue 所管理的函数（定时器的回调函数、ajax 的回调函数等、Promise 的回调函数），最好写成箭头函数，这样 this 的指向才是 vm 或 组件实例对象。



### 过滤器

**定义：**对要显示的数据进行特定格式化后再显示（适用于一些简单逻辑的处理）。

**注册语法**：

1. 全局

`// 注册 Vue.filter(name,callback) Vue.filter('mySlice',function(value){     return value.slice(0,4) })`

1. 局部

`// 注册 new Vue{filters:{}} filters:{     timeFormater(value,str='YYYY年MM月DD日 HH:mm:ss'){     // console.log('@',value)     return dayjs(value).format(str)     } }`

**使用**

`// 使用过滤器：{{ xxx | 过滤器名}}  或  v-bind:属性 = "xxx | 过滤器名"`


# vue生命周期

> 生命周期，又名：生命周期回调函数、生命周期函数、生命周期钩子。是Vue 在关键时刻帮我们调用的一些特殊名称的函数，生命周期函数的名字不可更改，但函数的具体内容是程序员根据需求编写的。 生命周期函数中的 this 指向是 vm 或 组件实例对象。

常见的生命周期使用：

1. mounted: 发送 ajax 请求、启动定时器、绑定自定义事件、订阅消息等【初始化操作】。
2. beforeDestroy: 清除定时器、解绑自定义事件、取消订阅消息等【收尾工作】。

## vue2

> 在vue2中的生命周期函数书写在vue实例身上的配置项中

`beforeCreate() {     console.log('beforeCreate') }, created() {     console.log('created') }, beforeMount() {     console.log('beforeMount') }, mounted() {     console.log('mounted') }, beforeUpdate() {     console.log('beforeUpdate') }, updated() {     console.log('updated') }, beforeDestroy() {     console.log('beforeDestroy') }, destroyed() {     console.log('destroyed') },`

### vue2图示

![https://raw.githubusercontent.com/cloutp/blog_img/main/202201271339125.png](https://raw.githubusercontent.com/cloutp/blog_img/main/202201271339125.png)

### vue3

> vue3中有些许变动，书写时可以和vue2一样书写在配置项中，也可以在setup中书写，在setup中书写需要进行变化


### ref属性

> 被用来给元素或子组件注册引用信息（id 的替代者）
>
> 应用在 html 标签上获取的是真实 DOM 元素，应用在组件标签上是组件实例对象（vc）

**使用方式**：

1. 打标识：`<h1 ref="xxx">.....</h1>` 或 `<School ref="xxx"></School>`
2. 获取：`this.$refs.xxx`

`<template>     <div>         <h1 v-text="msg" ref="title"></h1>         <button ref="btn" @click="showDOM">点我输出上方的DOM元素</button>         <School ref="sch"/>     </div> </template> <script>     //引入School组件     import School from './components/School'     export default {         name:'App',         components:{School},         data() {             return {                 msg:'欢迎学习Vue！'             }           },         methods: {             showDOM(){                 console.log(this.$refs.title) //真实DOM元素                 console.log(this.$refs.btn) //真实DOM元素                 console.log(this.$refs.sch) //School组件的实例对象（vc）                 }             },         } </script>`


### props属性

> 让组件接收外部传过来的数据

**外部组件：**​`<Demo name="xxx"/>`

**接收：**

1. 简单接收`props:['name']`
2. 限制类型`props:{name:String}`
3. 限制类型，限制必要性，指定默认值

`props:{     name:{         type:String, //name的类型是字符串         required:true, //name是必要的     },     age:{         type:Number,         default:99 //默认值     },     sex:{         type:String,         required:true     } }`

**备注：**props 是只读的，Vue 底层会监测你对 props 的修改，如果进行了修改，就会发出警告，若业务需求确实需要修改，那么请复制 props 的内容到 data 中一份，然后去修改 data 中的数据。



### mixins混入

> vue3中使用hooks
>
> 可以把多个组件共用的配置提取成一个混入对象

### **定义**

`//minxin.js export const hunhe = {   methods: {     showName(){       alert(this.name)     }   },   mounted() {     console.log('你好啊！')   }, } export const hunhe2 = {   data() {     return {       x:100,       y:200     }   }, }`

### **使用**

1. 全局配置

`// main.js Vue.mixin(hunhe) Vue.mixin(hunhe2)`

1. 局部配置

`// vue文件 <script>     //引入一个hunhe     import {hunhe,hunhe2} from '../mixin'     export default {           name:'School',           data() {               return {               name:'尚硅谷',               address:'北京',               x:666                 }             },          mixins:[hunhe,hunhe2], } </script>`

### **使用**

> 和正常配置一样使用，只是在冲突时有优先级区分

