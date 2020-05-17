### 1 路由的基本概念与原理
#### 1.1 路由
##### 3 实现简易前端路由
- 基于 URL 中的 hash 实现（点击菜单时改变 URL 的 hash，根据 hash 的变化控制组件的切换）；
```
// 监听 window 的 onhashchange 时间，根据获取到的最新的 hash，切换要显示的组件名称
window .onhashchange = function () {
    // 通过 location.hash 获取到最新的 hash 值
}
```

#### 1.2 Vue Router
- 支持 H5 历史模式或 hash 模式
- 支持嵌套路由
- 支持路由参数
- 支持编程式路由
- 支持命名路由

### 2 vue-router 的基本使用
#### 2.1 基本使用
- 先引入 vue，再引入 vue router；

##### 2 添加路由链接
```
<!--router-link 是 vue 中提供的标签，默认会被渲染为 a 标签-->
<!--to 属性默认会被渲染为 href 属性-->
<!--to 属性的值默认会被渲染为 # 开头的 hash 地址-->
<router-link to="/user">User</router-link>
```

##### 3 路由填充位
也叫路由占位符；
将来通过路由规则匹配到的组件，将会被渲染到 router-view 所在的位置；
也就是说，相关的组件的渲染是在此处进行的；
```
<router-view></router-view>
```

##### 4 定义路由组件
```
var User = {
    template: '<div>User</div>'
}

var Register = {
    template: '<div>Register</div>'
}
```

##### 5 配置路由规则并创建路由实例
每个路由规则都是一个配置对象，其中至少包含 <strong><font color=red>path</font></strong> 和 <strong><font color=red>component</font></strong> 两个属性；
- path 表示当前路由规则匹配的 hash 地址；
- component 表示当前路由规则对应要展示的组件；
```
// 创建路由实例对象
var router = new VueRouter({
    // routes 是路由规则数组
    routes: [
        {path: '/user', component: User},
        {path: '/register', component: Register},
    ]
})
```

##### 6 把路由挂载到 Vue 根实例中
```
new Vue({
    el: '#app',
    // 挂载到根实例上
    // 第五步中的 VueRouter 实例对象
    router: router,
})
```

#### 2.2 路由重定向
路由重定向指的是：用户在访问地址 A 的时候，强制用户跳转到地址 C，从而展示特定的组件页面；
通过路由规则的 redirect 属性，指定一个新的路由地址，从而实现重定向；
- path 表示需要被重定向的 <strong><font color=red>原地址</font></strong>，redirect 表示将要被重定向的 <strong><font color=red>新地址</font></strong>；
- 进入 ‘/’ 后，直接自动跳转到 ‘/user’；
```
var router = new VueRouter({
    routes: [
        {path: '/', redirect: '/user'},
        {path: '/user', component: User},
        {path: '/register', component: Register},
    ]
})
```

### 3 vue-router 嵌套路由
#### 3.1 嵌套路由用法
- 点击父级路由链接显示模板内容；
- 模板内容中又有子级路由链接；
- 点击子级路由链接显示子级模板内容；
- <strong>能够在父组件中显示子组件的内容；</strong>

##### 2 父路由组件模板
- 父级路由链接
- 父组件路由填充位

##### 3 子级路由模板
- 子级路由链接；
- 子级路由填充位；
```
const Register = {
    template: `<div>
        <h1>Register 组件</h1>
        <hr/>
        <router-link to="/register/tab1">Tab1</router-link>
        <router-link to="/register/tab2">Tab2</router-link>

        <!-- 子路由填充位 -->
        <router-view/>
    </div>`
}
```

##### 4 定义子组件
```
const Tab1 = {
    template: `<h3>tab1 子组件</h3>`
}

const Tab2 = {
    template: `<h3>tab2 子组件</h3>`
}
```

##### 5 定义子路由规则
```
{
    path: '/register',
    component: Register,
    children: [
        {
            path: '/register/tab1',
            component: Tab1,
        }, {
            path: '/register/tab2',
            component: Tab2,
        }
    ]
}
```

### 4 vue-router 动态路由匹配
#### 4.1 动态匹配路由的基本用法
思考：
```
<!-- 有如下 3 个路由链接 -->
<router-link to="/user/1">User1</router-link>
<router-link to="/user/2">User2</router-link>
<router-link to="/user/3">User3</router-link>
```
<strong>应用场景：通过动态路由参数的模式进行路由匹配；</strong>

- 动态路径参数，以冒号开头；
- 路由组件中通过 $route.params 获取路由参数；

```
const router = new VueRouter({
    routes: [
        {path: '/user/:id', component: User}
    ]
})

const User = {
    template: '<div>User {{$route.params.id}}</div>'
}
```

#### 4.2  路由组件传递参数
$route 与对应路由形成高度耦合，不够灵活，可以使用 props 将组件和路由解耦；
##### 1 props 的值为布尔类型
```
const router = new VueRouter({
    routes: [
        // 路由重定向
        { path: '/', redirect: '/user' },
        {
            path: '/user/:id',
            component: User,
            props: true,
        },
        { path: '/register', component: Register },
    ]
})

const User = {
    props: ['id'],
    template: '<h1>User 组件 --id：{{id}}</h1>'
}
```

##### 2 props 的值为对象类型
```
const router = new VueRouter({
    routes: [
        // 路由重定向
        { path: '/', redirect: '/user' },
        {
            path: '/user/:id',
            component: User,
            // id 不可访问
            props: {
                uname: 'lisi',
                age: 20
            }
        },
        { path: '/register', component: Register },
    ]
})

const User = {
    props: ['uname', 'age'],
    template: '<h1>User 组件 --uname：{{uname}} --age：{{age}}</h1>'
}
```

##### 3 props 的值为函数参数
```
const router = new VueRouter({
    routes: [
        // 路由重定向
        { path: '/', redirect: '/user' },
        {
            path: '/user/:id',
            component: User,
            // props: true,
            // id 不可访问
            props: route => ({
                uname: '25',
                age: 20,
                id: route.params.id
            })
        },
        { path: '/register', component: Register },
    ]
})

const User = {
    props: ['id', 'uname', 'age'],
    template: '<h1>User 组件 --id：{{id}} --uname：{{uname}} --age：{{age}}</h1>'
}
```

### 5 vue-router 命名路由
#### 5.1 命名路由的配置规则
为了方便地表示路由的路径，可以给路由规则起一个别名，即为“命名路由”；
```
<router-link :to="{
    name: 'user',
    params: {id: 3}
}">User3</router-link>

const router = new VueRouter({
    routes: [
        { path: '/', redirect: '/user' },
        {
            // 命名路由
            name: 'user',
            path: '/user/:id',
            component: User,
            props: route => ({
                uname: '25',
                age: 20,
                id: route.params.id
            })
        },
        { path: '/register', component: Register },
    ]
})
```

### 6 vue-router 编程式导航
#### 6.1 页面导航的两种方式
##### 1 概述
- <strong><font color=red>声明式导航</font></strong>：通过<strong><font color=red>点击链接</font></strong>实现导航的方式，叫做声明式导航；
    - 如 a 链接，或 vue 中的 route-link 标签；
- <strong><font color=red>编程式导航</font></strong>：通过<strong><font color=red>调用 JavaScript</font></strong> 形式的 API 实现导航的方式，叫做编程式导航；
    - 如普通网页中的 location.href；

###### 2 常用的编程式导航 API
- this.$router.<strong><font color=red>push</font></strong>('hash地址')
    ```
    const User = {
    props: ['id', 'uname', 'age'],
    template: `
            <div>
                <h1>User 组件 --id：{{id}} --uname：{{uname}} --age：{{age}}</h1>
                <button @click='goRegister'>跳转到注册页面</button>    
            </div>
        `,
        methods: {
            goRegister: function () {
                this.$router.push('/register')
            }
        }
    }
    ```
- this.$router.<strong><font color=red>go</font></strong>(n)
    ```
    const Register = {
        template: `
            <div>
                <h1>Register 组件</h1>
                <button @click='goBack'>后退</button>
            </div>
        `,
        methods: {
            goBack: function () {
                this.$router.go(-1)
            }
        }
    }
    ```

#### 5.2 编程式导航参数规则
router.push() 方法的参数规则
```
// 字符串（路径名称）
router.push('/home')
// 对象
router.push({ path: '/home' })
// 命名路由（传参）
router.push({ name: '/home', params: { userID: 123 }})
// 带查询参数，变成 /register?uname=lisi
router.push({ path: '/register', query: { uname: 'lisi' }})
```