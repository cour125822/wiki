---
title: V3配置
description: 
published: true
date: 2023-03-06T04:32:40.107Z
tags: 
editor: markdown
dateCreated: 2023-02-26T03:46:34.065Z
---

### setup

> 在vue3中新增了一个配置项setup，同时setup是所有compositionAPI的舞台 ，组件中的属性和方法都可以定义在setup中，vue3中兼容vue2中的data、methods等配置项，但是不推进使用，并且vue2和vue3方法中对数据的操作也不兼容，在冲突时，以setup为主。

### **setup的两种返回值**

> 如果setup返回的是一个Promise实例，需要其他组件配合

1. 若返回一个对象，则对象中的属性、方法, 在模板中均可以直接使用。（重点关注！）
2. 若返回一个渲染函数：则可以自定义渲染内容。（了解）

### **setup的两个注意点**

* setup执行的时机
* 在beforeCreate之前执行一次，this是undefined。
* setup的参数
* props：值为对象，包含：组件外部传递过来，且组件内部声明接收了的属性。
* context：上下文对象

  * attrs: 值为对象，包含：组件外部传递过来，但没有在props配置中声明的属性, 相当于 `this.$attrs`。
  * slots: 收到的插槽内容, 相当于 `this.$slots`。
  * emit: 分发自定义事件的函数, 相当于 `this.$emit`。
  
  ### ref函数

> 定义一个基本类型的响应式的数据

* 语法: `const xxx = ref(initValue)`
* 创建一个包含响应式数据的**引用对象（reference对象，简称ref对象）**。
* JS中操作数据： `xxx.value`
* 模板中读取数据: 不需要.value，直接：`<div>{{xxx}}</div>`
* 备注：
* 接收的数据可以是：基本类型、也可以是对象类型。
* 基本类型的数据：响应式依然是靠`Object.defineProperty()`的`get`与`set`完成的。
* 对象类型的数据：内部 ***“ 求助 ”*** 了Vue3.0中的一个新函数—— `reactive`函数。

### reactive函数

> 定义一个对象类型的响应式数据（基本类型不要用它，要用ref函数）

* 语法：`const 代理对象= reactive(源对象)`接收一个对象（或数组），返回一个**代理对象（Proxy的实例对象，简称proxy对象）**
* reactive定义的响应式数据是“深层次的”。
* 内部基于 ES6 的 Proxy 实现，通过代理对象操作源对象内部数据进行操作。

### reactive对比ref

* 从定义数据角度对比：
* ref用来定义：**基本类型数据**。
* reactive用来定义：**对象（或数组）类型数据**。
* 备注：ref也可以用来定义**对象（或数组）类型数据**, 它内部会自动通过`reactive`转为**代理对象**。
* 从原理角度对比：
* ref通过`Object.defineProperty()`的`get`与`set`来实现响应式（数据劫持）。
* reactive通过使用**Proxy**来实现响应式（数据劫持）, 并通过**Reflect**操作**源对象**内部的数据。
* 从使用角度对比：
* ref定义的数据：操作数据**需要**​`.value`，读取数据时模板中直接读取**不需要**​`.value`。
* reactive定义的数据：操作数据与读取数据：**均不需要**​`.value`。
  
  
  
  
  ### **computed函数**

* 与Vue2.x中computed配置功能一致
* 写法

`import {computed} from 'vue' setup(){     ...   //计算属性——简写     let fullName = computed(()=>{         return person.firstName + '-' + person.lastName     })     //计算属性——完整     let fullName = computed({         get(){             return person.firstName + '-' + person.lastName         },         set(value){             const nameArr = value.split('-')             person.firstName = nameArr[0]             person.lastName = nameArr[1]         }     }) }`
  
  
  
  
  ### **watch函数**

* 与Vue2.x中watch配置功能一致
* 两个小“坑”：
* 监视reactive定义的响应式数据时：oldValue无法正确获取、强制开启了深度监视（deep配置失效）。

  * 监视reactive定义的响应式数据中某个属性时：deep配置有效。

`//情况一：监视ref定义的响应式数据 watch(sum,(newValue,oldValue)=>{   console.log('sum变化了',newValue,oldValue) },{immediate:true}) //情况二：监视多个ref定义的响应式数据 watch([sum,msg],(newValue,oldValue)=>{   console.log('sum或msg变化了',newValue,oldValue) })  /* 情况三：监视reactive定义的响应式数据           若watch监视的是reactive定义的响应式数据，则无法正确获得oldValue！！           若watch监视的是reactive定义的响应式数据，则强制开启了深度监视  */ watch(person,(newValue,oldValue)=>{   console.log('person变化了',newValue,oldValue) },{immediate:true,deep:false}) //此处的deep配置不再奏效 //情况四：监视reactive定义的响应式数据中的某个属性 watch(()=>person.job,(newValue,oldValue)=>{   console.log('person的job变化了',newValue,oldValue) },{immediate:true,deep:true})  //情况五：监视reactive定义的响应式数据中的某些属性 watch([()=>person.job,()=>person.name],(newValue,oldValue)=>{   console.log('person的job变化了',newValue,oldValue) },{immediate:true,deep:true}) //特殊情况 watch(()=>person.job,(newValue,oldValue)=>{     console.log('person的job变化了',newValue,oldValue) },{deep:true}) //此处由于监视的是reactive素定义的对象中的某个属性，所以deep配置有效`

### **watchEffect函数**

* watch的套路是：既要指明监视的属性，也要指明监视的回调。
* watchEffect的套路是：不用指明监视哪个属性，监视的回调中用到哪个属性，那就监视哪个属性。
* watchEffect有点像computed：
* 但computed注重的计算出来的值（回调函数的返回值），所以必须要写返回值。
* 而watchEffect更注重的是过程（回调函数的函数体），所以不用写返回值。

`//watchEffect所指定的回调中用到的数据只要发生变化，则直接重新执行回调。 watchEffect(()=>{     const x1 = sum.value     const x2 = person.age     console.log('watchEffect配置的回调执行了') })`
  
 ### 生命周期

![https://cn.vuejs.org/images/lifecycle.png](https://cn.vuejs.org/images/lifecycle.png)

![https://v3.cn.vuejs.org/images/lifecycle.svg](https://v3.cn.vuejs.org/images/lifecycle.svg)

* Vue3.0中可以继续使用Vue2.x中的生命周期钩子，但有有两个被更名：
* `beforeDestroy`改名为 `beforeUnmount`
* `destroyed`改名为 `unmounted`
* Vue3.0也提供了 Composition API 形式的生命周期钩子，与Vue2.x中钩子对应关系如下：
* `beforeCreate`===>`setup()`
* `created`=======>`setup()`
* `beforeMount` ===>`onBeforeMount`
* `mounted`=======>`onMounted`
* `beforeUpdate`===>`onBeforeUpdate`
* `updated` =======>`onUpdated`
* `beforeUnmount` ==>`onBeforeUnmount`
* `unmounted` =====>`onUnmounted`
  
  
  ### 自定义hook函数

> 本质是一个函数，把setup函数中使用的Composition API进行了封装。

* 类似于vue2.x中的mixin。
* 自定义hook的优势: 复用代码, 让setup中的逻辑更清楚易懂。


### toref
### 

> 创建一个 ref 对象，其value值指向另一个对象中的某个属性。

* 语法：`const name = toRef(person,'name')`
* 应用: 要将响应式对象中的某个属性单独提供给外部使用时。
* 扩展：`toRefs` 与`toRef`功能一致，但可以批量创建多个 ref 对象，语法：`toRefs(person)`
  
  
  
  ### 通讯
  ### provide 与 inject

![https://raw.githubusercontent.com/cloutp/blog_img/main/202201271719692.png](https://raw.githubusercontent.com/cloutp/blog_img/main/202201271719692.png)

* 作用：实现**祖与后代组件间**通信
* 套路：父组件有一个 `provide` 选项来提供数据，后代组件有一个 `inject` 选项来开始使用这些数据
* 具体写法：
* 祖组件中：
  `setup(){      ......       let car = reactive({name:'奔驰',price:'40万'})       provide('car',car)       ......   }`
* 后代组件中：
  `setup(props,context){      ......       const car = inject('car')       return {car}      ......`
  
  ### other
  ### shallowReactive 与 shallowRef

* shallowReactive：只处理对象最外层属性的响应式（浅响应式）。
* shallowRef：只处理基本数据类型的响应式, 不进行对象的响应式处理。
* 什么时候使用?
* 如果有一个对象数据，结构比较深, 但变化时只是外层属性变化 ===> shallowReactive。
* 如果有一个对象数据，后续功能不会修改该对象中的属性，而是生新的对象来替换 ===> shallowRef。

### readonly 与 shallowReadonly

* readonly: 让一个响应式数据变为只读的（深只读）。
* shallowReadonly：让一个响应式数据变为只读的（浅只读）。
* 应用场景: 不希望数据被修改时。

### 3.toRaw 与 markRaw

* toRaw：
* 作用：将一个由`reactive`生成的**响应式对象**转为**普通对象**。
* 使用场景：用于读取响应式对象对应的普通对象，对这个普通对象的所有操作，不会引起页面更新。
* markRaw：
* 作用：标记一个对象，使其永远不会再成为响应式对象。
* 应用场景:

  1. 有些值不应被设置为响应式的，例如复杂的第三方类库等。
  2. 当渲染具有不可变数据源的大列表时，跳过响应式转换可以提高性能。

### 4.customRef

* 作用：创建一个自定义的 ref，并对其依赖项跟踪和更新触发进行显式控制。
* 实现防抖效果：

`<template>   <input type="text" v-model="keyword">   <h3>{{keyword}}</h3> </template> <script>   import {ref,customRef} from 'vue'   export default {       name:'Demo',       setup(){           // let keyword = ref('hello') //使用Vue准备好的内置ref           //自定义一个myRef           function myRef(value,delay){               let timer               //通过customRef去实现自定义               return customRef((track,trigger)=>{                   return{                       get(){                           track() //告诉Vue这个value值是需要被“追踪”的                           return value                       },                       set(newValue){                           clearTimeout(timer)                           timer = setTimeout(()=>{                               value = newValue                               trigger() //告诉Vue去更新界面                           },delay)                       }                   }               })           }           let keyword = myRef('hello',500) //使用程序员自定义的ref           return {               keyword           }       }   } </script>`

### 响应式数据的判断

* isRef: 检查一个值是否为一个 ref 对象
* isReactive: 检查一个对象是否是由 `reactive` 创建的响应式代理
* isReadonly: 检查一个对象是否是由 `readonly` 创建的只读代理
* isProxy: 检查一个对象是否是由 `reactive` 或者 `readonly` 方法创建的代理
  
  
  
  