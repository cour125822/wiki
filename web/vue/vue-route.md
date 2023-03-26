---
title: Vue Route
description: 
published: true
date: 2023-03-26T08:07:31.659Z
tags: 
editor: markdown
dateCreated: 2023-02-26T03:55:10.892Z
---

# vueRouter

# 

> 一个路由（route）就是一组映射关系（key - value），多个路由需要路由器（router）进行管理。

### 注意

> 路由参数必须为routes路由在main.js中需要使用router：导入名 ，只有导入名等于router时才可以省略否则会渲染不出来

1. 路由组件通常存放在 `pages` 文件夹，一般组件通常存放在 `components` 文件夹
2. 通过切换，“隐藏”了的路由组件，默认是被销毁掉的，需要的时候再去挂载。
3. 每个组件都有自己的 `$route` 属性，里面存储着自己的路由信息。
4. 整个应用只有一个 router，可以通过组件的 `$router` 属性获取到。

[基本使用](https://www.notion.so/5bcf2345f69b4bf689ce69de33e6e3aa)

[多级路由](https://www.notion.so/f5079556ec7e465a8b625ce1332615df)

[命名路由](https://www.notion.so/68aac46545ec46b8a0f6a4d320db870d)

[路由参数](https://www.notion.so/7c5ab7b378ad4a9aadf034e756d5916d)

[编程式路由导航](https://www.notion.so/19f88fe01618435a99fa0a6922aa58cd)

[缓存路由组件](https://www.notion.so/d9aa1b29d16441dba9b2e73a8a404bf1)

[生命周期钩子](https://www.notion.so/962cac07ca3c4c168b621d82147cbc01)

[路由守卫](https://www.notion.so/cc1f4263ed654bae87b607d176eac60a)

[路由工作模式](https://www.notion.so/edfe6dd16df44ed697b9cb236f25fd40)


## 基本使用
### 

1. 安装vue-router ：`npm install vue-router`
2. 在main中设置`Vue.use(vueRouter)`
3. 新建route文件夹，书写index.js

`//引入VueRouter import VueRouter from 'vue-router' //引入Luyou 组件 import About from '../components/About' import Home from '../components/Home' //创建router实例对象，去管理一组一组的路由规则 const router = new VueRouter({  routes:[      {          path:'/about',          component:About      },      {          path:'/home',          component:Home      }  ] }) //暴露router export default router`

1. 路由跳转

`<router-link active-class="active" to="/about">About</router-link>`

1. 路由展示

`<router-view></router-view>`



```

```

## 多级路由

### 

`routes:[     {         path:'/about',         component:About,     },     {         path:'/home',         component:Home,         children:[ //通过children配置子级路由             {                 path:'news', //此处一定不要写：/news                 component:News             },             {                 path:'message',//此处一定不要写：/message                 component:Message             }         ]     } ]`

> 跳转<router-link to="/home/news">News</router-link>



## 命名路由
### 

> 给路由添加name属性简化路由跳转

### **命名**

`{     path:'/demo',     component:Demo,     children:[         {             path:'test',             component:Test,             children:[                 {                       name:'hello' //给路由命名                     path:'welcome',                     component:Hello,                 }             ]         }     ] }`

### **跳转**

`<!--简化前，需要写完整的路径 --> <router-link to="/demo/test/welcome">跳转</router-link> <!--简化后，直接通过名字跳转 --> <router-link :to="{name:'hello'}">跳转</router-link> <!--简化写法配合传递参数 --> <router-link :to="{ name:'hello', query:{ id:666, title:'你好' } }"

> 跳转</router-link>`



## 路由参数
### 路由的query参数

1. 路由配置`null`
2. 传递参数

`<!-- 跳转并携带query参数，to的字符串写法 --> <router-link :to="/home/message/detail?id=666&title=你好">跳转</router-link> <!-- 跳转并携带query参数，to的对象写法 --> <router-link :to="{ path:'/home/message/detail', query:{ id:666, title:'你好' } }"

> 跳转</router-link>`

1. 接收参数

`$route.query.id $route.query.title`

### 路由的params参数

1. 路由配置

`{  path:'/home',  component:Home,  children:[      {          path:'news',          component:News      },      {          component:Message,          children:[              {                  name:'xiangqing',                  path:'detail/:id/:title', //使用占位符声明接收params参数                  component:Detail              }          ]      }  ] }`

1. 传递参数

`<!-- 跳转并携带params参数，to的字符串写法 --> <router-link :to="/home/message/detail/666/你好">跳转</router-link> <!-- 跳转并携带params参数，to的对象写法 --> <router-link :to="{ name:'xiangqing', params:{ id:666, title:'你好' } }"

> 跳转</router-link>`

> 特别注意：路由携带 params 参数时，若使用 to 的对象写法，则不能使用 path 配置项，必须使用 name 配置！

1. 接收参数

`$route.params.id $route.params.title`

### 路由的props参数

> 让路由组件更见方便接收参数

### **路由配置**

`// /router/index文件 {     name:'xiangqing',     path:'detail/:id',     component:Detail,     //第一种写法：props值为对象，该对象中所有的key-value的组合最终都会通过props传给Detail组件     // props:{a:900}     //第二种写法：props值为布尔值，布尔值为true，则把路由收到的所有params参数通过props传给Detail组件     // props:true     //第三种写法：props值为函数，该函数返回的对象中每一组key-value都会通过props传给Detail组件     props(route){         return {             id:route.query.id,             title:route.query.title         }     } }`

### **路由跳转**

`// 参数传递 <router-link :to="{     name:'xiangqing',     query:{         id:m.id,         title:m.title     } }">     {{m.title}} </router-link>`

### **组件配置**

`//参数接受 props:['id','title'],`

### `<router-link>` 的 replace 属性

1. 作用：控制路由跳转时操作浏览器历史记录的模式
2. 浏览器的历史记录有两种写入方式：分别为 `push` 和 `replace`，`push` 是追加历史记录，`replace` 是替换当前记录。路由跳转时候默认为 `push`
3. 如何开启 `replace` 模式：`<router-link replace .......>News</router-link>`



## 编程式路由导航
### 

> 不借助 <router-link> 实现路由跳转，让路由跳转更加灵活

`//$router的两个API this.$router.push({     name:'xiangqing',         params:{             id:xxx,             title:xxx         } }) this.$router.replace({     name:'xiangqing',         params:{             id:xxx,             title:xxx         } }) this.$router.forward() //前进 this.$router.back() //后退 this.$router.go() //可前进也可后退`
  
  
  ## 缓存路由组件
  ### 

> 让不展示的路由组件保持挂载，不被销毁

`<keep-alive include="News">      <router-view></router-view> </keep-alive>`

## 生命周期钩子
  ### 两个生命周期钩子

> 路由组件所独有的两个钩子，用于捕获路由组件的激活状态，书写在组件内

### **钩子**

1. `activated` 路由组件被激活时触发。
2. `deactivated` 路由组件失活时触发。
  
  
  ## 路由守卫
  ### 

> 对路由进行权限控制

### **全局守卫**

`//全局前置守卫：初始化时执行、每次路由切换前执行 router.beforeEach((to,from,next)=>{     console.log('beforeEach',to,from)     if(to.meta.isAuth){ //判断当前路由是否需要进行权限控制         if(localStorage.getItem('school') === 'atguigu'){ //权限控制的具体规则             next() //放行         }else{             alert('暂无权限查看')             // next({name:'guanyu'})         }     }else{         next() //放行     } }) //全局后置守卫：初始化时执行、每次路由切换后执行 router.afterEach((to,from)=>{     console.log('afterEach',to,from)     if(to.meta.title){          document.title = to.meta.title //修改网页的title     }else{         document.title = 'vue_test'     } })`

### **独享守卫**

`// 单独路由配置 beforeEnter(to,from,next){     console.log('beforeEnter',to,from)     if(to.meta.isAuth){ //判断当前路由是否需要进行权限控制         if(localStorage.getItem('school') === 'atguigu'){             next()         }else{             alert('暂无权限查看')             // next({name:'guanyu'})         }     }else{         next()     } }`

### **组件内守卫**

`//进入守卫：通过路由规则，进入该组件时被调用 beforeRouteEnter (to, from, next) { }, //离开守卫：通过路由规则，离开该组件时被调用 beforeRouteLeave (to, from, next) { }`



  ## 工作模式
  ### 

1. 对于一个 url 来说，什么是 hash 值？—— #及其后面的内容就是 hash 值。
2. hash 值不会包含在 HTTP 请求中，即：hash 值不会带给服务器。
3. hash 模式：
4. 地址中永远带着#号，不美观 。
5. 若以后将地址通过第三方手机 app 分享，若 app 校验严格，则地址会被标记为不合法。
6. 兼容性较好。
7. history 模式：
8. 地址干净，美观 。
9. 兼容性和 hash 模式相比略差。
10. 应用部署上线时需要后端人员支持，解决刷新页面服务端 404 的问题





