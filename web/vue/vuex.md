---
title: Vuex
description: 
published: true
date: 2023-03-26T08:07:34.794Z
tags: 
editor: markdown
dateCreated: 2023-02-26T03:50:53.072Z
---

# 

> 在 Vue 中实现集中式状态（数据）管理的一个 Vue 插件，对 vue 应用中多个组件的共享状态进行集中式的管理（读/写），也是一种组件间通信的方式，且适用于任意组件间通信。

![https://raw.githubusercontent.com/cloutp/blog_img/main/202201271444075.png](https://raw.githubusercontent.com/cloutp/blog_img/main/202201271444075.png)

> 使用场景： 多组件数据共享

### 搭建vuex环境

> 创建store文件夹，添加index.js文件

`// store/index.js //引入Vue核心库 import Vue from 'vue' //引入Vuex import Vuex from 'vuex' //应用Vuex插件 Vue.use(Vuex) //准备actions对象——响应组件中用户的动作 const actions = {} //准备mutations对象——修改state中的数据 const mutations = {} //准备state对象——保存具体的数据 const state = {} //创建并暴露store export default new Vuex.Store({     actions,     mutations,     state })`

> 在main文件中配置

`...... //引入store import store from './store' ...... //创建vm new Vue({     el:'#app',     render: h => h(App),     store })`

### 基本使用

> 初始化数据，配置actions、mutation、

`const actions = {     //响应组件中加的动作     jia(context,value){         // console.log('actions中的jia被调用了',miniStore,value)         context.commit('JIA',value)     }, } const mutations = {     //执行加     JIA(state,value){         // console.log('mutations中的JIA被调用了',state,value)         state.sum += value     } } //初始化数据 const state = {    sum:0 } //创建并暴露store export default new Vuex.Store({     actions,     mutations,     state, })`

**组件中读取数据：**​`$store.state.sum`

**组件中修改数据：**​`$store.dispatch('action中的方法名',数据)` 或 `$store.commit('mutations中的方法名',数据)`

> 备注：若没有网络请求或其他业务逻辑，组件中也可以越过 actions，即不写 dispatch，直接编写 commit

### getters使用

> 当 state 中的数据需要经过加工后再使用时，可以使用 getters 加工。

在 `store.js` 中追加 `getters` 配置：

`...... const getters = {     bigSum(state){         return state.sum * 10     } } //创建并暴露store export default new Vuex.Store({     ......     getters })`

**组件中读取数据**：`$store.getters.bigSum`

### 四个map方法使用

### **mapState 方法：**

> 用于帮助我们映射 state 中的数据为计算属性

`computed: {    //借助mapState生成计算属性：sum、school、subject（对象写法）     ...mapState({sum:'sum',school:'school',subject:'subject'}),    //借助mapState生成计算属性：sum、school、subject（数组写法）    ...mapState(['sum','school','subject']), },`

### **mapGetters 方法：**

> 用于帮助我们映射 getters 中的数据为计算属性

`computed: {    //借助mapGetters生成计算属性：bigSum（对象写法）    ...mapGetters({bigSum:'bigSum'}),    //借助mapGetters生成计算属性：bigSum（数组写法）    ...mapGetters(['bigSum']) },`

### **mapActions 方法：**

> 用于帮助我们生成与 actions 对话的方法，即：包含 $store.dispatch(xxx) 的函数

`methods:{    //靠mapActions生成：incrementOdd、incrementWait（对象形式）    ...mapActions({incrementOdd:'jiaOdd',incrementWait:'jiaWait'})    //靠mapActions生成：incrementOdd、incrementWait（数组形式）    ...mapActions(['jiaOdd','jiaWait']) }`

### **mapMutations 方法：**

> 用于帮助我们生成与 mutations 对话的方法，即：包含 $store.commit(xxx) 的函数

`methods:{    //靠mapActions生成：increment、decrement（对象形式）    ...mapMutations({increment:'JIA',decrement:'JIAN'}),    //靠mapMutations生成：JIA、JIAN（对象形式）    ...mapMutations(['JIA','JIAN']), }`

> 备注：mapActions 与 mapMutations 使用时，若需要传递参数需要：在模板中绑定事件时传递好参数，否则参数是事件对象。