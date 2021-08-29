# Vue学习笔记

# Vue学习笔记


>     没有一蹴而就的辉煌，只有默不作声的努力。          

### 使用 Vue CLI 
#### 安装Vue CLI

1. 官网地址 ：https://cli.vuejs.org/zh/
2. 命令行安装(-g代表全局模式)
```sh
npm install -g @vue/cli
```
使用命令行不加版本号的话表示使用的最新版（目前最新的就是4.5.13）

![](https://gitee.com/lonercci/picbed/raw/master/img/20210720001323.png)
#### 新建项目
有两种方式：
+ 也支持 CLI2 的创建

    ```sh
    vue init webpack my-project
    ```
+ 从 CLI3 开始，提供了一种可视化的部署方式，操作很简单，不详细介绍。

    ```sh
    vue ui
    ```

+ 使用命令行

    ```sh
    vue create hello-world
    ```
    
    ![](https://gitee.com/lonercci/picbed/raw/master/img/20210720001353.png)
    
    这样可以创建一个空白的项目文件，后面需要用到的可以随时添加。
    
    ![](https://gitee.com/lonercci/picbed/raw/master/img/20210720001410.png)
    
    目录结构大概就长这个样子，很清晰，`main.js`是入口文件，`components`是自定义的组件，`App.vue`是项目的主组件，`package.json`依赖的包环境，`public`为静态资源。
    

### 关于App.vue的一些理解

`App.vue`可以当做网站的首页，是一个`vue`项目的主组件，页面的入口文件，所有的页面都是在`App.vue`下进行切换的。是整个项目的关键，app.vue负责构建定义及页面组件归集。

#### App.vue作为主组件在main.js中被使用
```js
import Vue from 'Vue'
import App from './App.vue'

Vue.config.productionTip = false

new Vue({
render: h => h(App) 
}).$mount('#app')
```
如果把箭头函数和mount按原来的写法是这样的
```js
new Vue({
  el: '#app'
  render: function(h){
    return h(App)
  }
})
//.$mount('#app')的作用等同于el: '#app'
```

这两者的效果等同。

在VueCLI 2.x版本中，使用的一般都是`runtime-compiler`，而3.x 以后使用的都是`untime-only`。这两者是有区别的，后者相对更加轻巧，因为省略了templated到ast的步骤，直接使用render到virdom到UI。随着技术的迭代，后者使用的也更多。上面的例子都是后者的写法，下面示例一个前者的写法：

```js
new Vue({
  el: '#app',
  template: '<App/>',
  components: {APP}
})
```

#### 主组件app.vue中调用其他组件，构建页面

```vue
<template>
  <div id="app">
    <MyTest/>
  </div>
</template>

<script>
//导入其他组件
import MyTest from './components/MyTest.vue'

export default {
  name: 'App',
  components: {
    MyTest
  }
}
</script>

<style>
</style>
```

### vue-router的配置

#### 前提：如何实现前端路由：

1.  URL的hash：更改hash值，在不刷新页面的情况下改变url。（location.hash）

2.  HTML5的history模式：pushState。（使用的是栈结构，先进后出）即压栈操作，栈顶显示的永远是最新的url。

    back()弹栈一层。go()可弹栈多层。

    history.back()   <==>   history.go(-1)

    history.forward   <==>   history.go(1)

3.  HTML5的replace模式：replaceState。没有回退记录，直接进行替换

#### 1、安装vue-router 插件

```npm
npm install vue-router --save
```

导入路由对象，并且调用Vue.use(VueRouter)

在router/index.js中：

```js
import VueRouter from "vue-router";

//1、通过Vue.use(插件)，安装插件【使用任何的插件的时候，都需要使用Vue.use】
Vue.use(VueRouter)
```

**Tips**:对于所有的插件安装，都必须使用 Vue.use() 来注册

#### 2、创建路由实例，并且传入路由映射配置

在router/index.js中：

```js
//2、创建路由(VueRouter)对象,一个映射就是一个对象
const routes = [
  {
    path: '/home',           //路径
    component: Home    //组件   这些东西不能是null ,需要组件，但是组件获取不到，需要import导入
  },
  {
    path: '/about',
    component: About
  },

]


const router = new VueRouter({
  // 配置路由和组件之间的应用关系
  // routes: [] 我们可以把这数组写到前面，通过引用取值(es6写法)。
  routes
})

```
#### 3、在Vue实例中挂载创建的路由实例

在router/index.js中：我们首先要把router导出

```js
export default router
```

然后在main.js中，导入router

```js
import router from './router' 
```

在main.js中，挂载到Vue实例中
```js
new Vue({
  router,  //把router对象传入Vue实例，但是拿不到，所以在index文件里面需要导出，在main.js中导入
  render: function (h) { return h(App) }
}).$mount('#app')
```

#### 4、总结vue-router 的配置方式

就是router/index.js与main.js之间的导入导出与使用。

完整代码：

1.  router/index.js： 

```js
import VueRouter from "vue-router";
import Vue from "vue";

import Home from '../components/Home';
import About from '../components/About';


//1、通过Vue.use(插件)，安装插件【使用任何的插件的时候，都需要使用Vue.use】
Vue.use(VueRouter)

//2、创建路由(VueRouter)对象,一个映射就是一个对象
const routes = [
  {
    path: '/home',           //路径
    component: Home    //组件   这些东西不能是null ,需要组件，但是组件获取不到，需要import导入
  },
  {
    path: '/about',
    component: About
  },

]

const router = new VueRouter({
  // 配置路由和组件之间的应用关系
  // routes: [] 我们可以把这数组写到前面，通过引用取值(es6写法)。
  routes
})

//3、将router对象传入Vue实例，放入main.js中【导出】
export default router
```
2.  main.js
```js
import Vue from 'vue'
import App from './App.vue'
import router from './router' // 如果导入的文件在一个目录中，且里面只有一个index文件，那么可以不用跟后面的东西，省略

Vue.config.productionTip = false

new Vue({
  router,  //把router对象传入Vue实例，但是拿不到，所以在index文件里面需要导出，在main.js中导入
  render: function (h) { return h(App) }
}).$mount('#app')

```

### 使用vue-router 

#### 创建路由组件

首先把App.vue里面不要的删除，把components目录下的组件也删除。弄一个空白的项目文件。

在components目录下创建几个组件。

![](https://gitee.com/lonercci/picbed/raw/master/img/20210720061008.png)

#### 配置路由映射，组件和路径的映射关系

在router/index.js中：

![](https://gitee.com/lonercci/picbed/raw/master/img/20210720061622.png)

#### 使用路由，通过`<router-link>`和`<router-view>`

组件在App.vue中使用
![](https://gitee.com/lonercci/picbed/raw/master/img/20210720062213.png)

`<router-link>`会被渲染成`<a>`标签
`<router-view>`会根据当前的路径，动态渲染不同的组件

在路由切换时，切换的是`<router-view>`挂载的组件，其他内容不会发生变化

![](https://gitee.com/lonercci/picbed/raw/master/img/20210720062800.png)

#### 小细节：设置路由的默认路径

![](https://gitee.com/lonercci/picbed/raw/master/img/20210720063523.png)

#### 小细节：修改为history模式

![](https://gitee.com/lonercci/picbed/raw/master/img/20210720064139.png)

只需要在router/index.js中，创建路由的地方，添加一个mode属性。

![](https://gitee.com/lonercci/picbed/raw/master/img/20210720064345.png)

#### router-link的其他属性

![](https://gitee.com/lonercci/picbed/raw/master/img/20210720065307.png)

#### 使用代码方式实现路由跳转

```js
this.$router.push('/home') //栈跳转，可以进退
this.$router.replace('/home')//不可进退
```

![](https://gitee.com/lonercci/picbed/raw/master/img/20210720070414.png)

