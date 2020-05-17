<!--
 * @Descripttion: 
 * @version: 
 * @Author: 唐帆
 * @Date: 2020-05-09 09:15:07
 * @LastEditors: 唐帆
 * @LastEditTime: 2020-05-10 10:25:33
 -->
vue：渐进式 javascript 框架；
声明式渲染 → 组件系统 → 客户端路由 → 集中式状态管理 → 项目构建

### 2 vue 基本使用
#### 2.1 基本使用步骤
1 提供标签用于填充数据；
2 引入 vue.js 库文件；
3 可以使用 vue 的语法做功能了；
4 把 vue 提供的数据填充到标签；

#### 2.2 hello world
```
<body>
    <div id="app">
        {{msg}}
    </div>

    <script>
        var vm = new Vue({
            el: '#app',
            data: {
                msg: 'hello vue',
            }
        });
    </script>
</body>
```

#### 2.3 hello world 细节分析
##### 1 实例参数分析
el：元素的挂载位置（值可以是 css 选择器或者 DOM 元素）；
data：模型数据（值是一个对象）；

##### 2 插值表达式用法
将数据填充到 HTML 标签中；
插值表达式支持基本的计算操作；


### 3 Vue 模板语法
#### 3.1 模板语法概述
- 插值表达式
- 指令
- 事件绑定
- 属性绑定
- 样式绑定
- 分支循环结构

#### 3.2 指令
##### 1 什么是指令
- 什么是自定义属性
- 指令的本质就是自定义属性
- 指令格式：以 v 开始（比如：v-cloak）

##### 2 v-cloak 指令
解决插值表达式带来的闪动问题；
1 提供样式（css）：
```
[v-cloak]{
    display: none;
}
```
2 在插值表达式所在的标签中添加 v-cloak 指令：
```
<div v-cloak>{{msg}}</div>
```
3 原理：
- 先通过样式隐藏内容；
- 在内存中进行值的替换；
- 显示最终结果；

##### 3 数据绑定指令
- v-text 填充文本
    - 相比插值表达式更加简洁；
    - 不存在闪动问题；
    ```
    <div v-text='msg'></div>
    ```
- v-html 填充 HTML 片段
    - 存在安全问题，比较危险；
    - 本网站内部数据可以使用，来自第三方的数据不可用；
    ```
    <div v-html='msg1'></div>

    var vm = new Vue({
        el: '#app',
        data: {
            msg: 'hello vue',
            msg1: '<h1>HTML</h1>'
        }
    });
    ```
- v-pre 填充原始信息
    - 显示原始信息，跳过编译过程；
    - 页面中显示的是 “{{msg}}”
    ```
    <div v-pre>{{msg}}</div>
    ```

##### 4 数据响应式
- 如何理解响应式
    - 在 html 中是屏幕尺寸的改变导致样式的变化；
    - 数据的响应式（数据的变化导致页面的变化）；
- 什么是数据绑定
    - 数据绑定：将数据填充到标签中；
- vue-once 只编译一次
    - 显示内容之后不再具有响应式功能；
    - 应用于显示的信息不再变化的场景，可以提高性能；

#### 3.3 双向数据绑定
##### 2 双向数据绑定分析
- v-model 指令用法
    ```
    <input type='text' v-model='uname' />
    ```
    数据影响页面，页面影响数据；

##### 3 MVVM
Model（M）
View（V）
View-Model（VM）

#### 3.4 事件绑定
##### 1 Vue 如何处理事件？
- v-on 指令用法
    ```
    <input type='button' v-on:click='num++'/>
    ```
- v-on 简写形式
    ```
    <input type='button' @click='num++'/>
    ```

##### 2 事件函数的调用方式
- 直接绑定函数名称
    ```
    <button v-on:click='say'>Hello</button>
    ```
- 调用函数
    ```
    <button v-on:click='say()'>Hello</button>
    ```

- 函数定义
    ```
    var vm = new Vue({
        el: '#app',
        data: {
            num: 0
        },
        methods: {
            // 此处定义方法，this 指向 vm
            handle: function () {
                this.num++;
            }
        }
    })
    ```

##### 3 事件函数参数传递
- 普通参数和事件对象
    ```
    <button v-on:click='say("hi", $event)'>Hello</button>
    ```
- 如果事件直接绑定的是函数名称，那么默认会传递事件对象作为第一个参数；
- 如果事件绑定函数调用，事件对象必须作为 <strong>最后一个参数</strong> 进行显式传递，且事件对象的名称必须是 <strong>$event</strong>;

##### 4 事件修饰符
- .stop 阻止冒泡
    ```
    <a v-on:click.stop='handle'>跳转</a>
    ```
- .prevent 组织默认行为
    ```
    <a v-on:click.prevent='handle'>跳转</a>
    ```
- 可以传链使用，即同时阻止冒泡和默认行为；
    ```
    <a v-on:click.stop.prevent="doThat"></a>

    <!-- 只有修饰符 -->
    <form v-on:submit.prevent></form>
    ```

##### 5 按键修饰符
- .enter 回车键
    ```
    <input v-on:keyup.enter='submit'>
    ```
- .delete 删除键
    ```
    <input v-on:keyup.delete='handle'>
    ```

##### 6 自定义按键修饰符
- 全局 config.keyCodes 对象
    ```
    Vue.config.keyCodes.fl = 112;

    <!-- 按键为 a 时才触发事件 -->
    <input type="text" @keyup.65='handle' v-model='info'>

    Vue.config.keyCodes.aaa = 65;
    <input type="text" @keyup.aaa='handle' v-model='info'>
    ```
- 名字是自定义的，但按键必须是对应的值；

#### 3.5 属性绑定
##### 1 Vue 如何动态处理属性？
- v-bind 指令用法
    ```
    <a v-bind:href='url'>跳转</a>
    ```
- 缩写形式
    ```
    <a :href='url'>跳转</a>
    ```

##### 2 v-model 的底层实现原理
通过 v-bind 和 v-on 组合实现；
分别进行属性的绑定和事件的绑定；
```
<input type="text" v-bind:value="msg" v-on:input="handle">

methods: {
    handle: function (event) {
        // 新数据覆盖原数据
        this.msg = event.target.value;
    }
}
```

#### 3.6 样式绑定
##### 1 class 样式处理
- 对象语法
    ```
    <div v-bind:class="{active:isActive}"></div>
    
    var vm1 = new Vue({
      el: '#app1',
      data: {
        isActive: true,
        isError: false,
      },
      methods: {
        handle: function () {
          // 控制 isActive 的切换
          this.isActive = !this.isActive;
          this.isError = !this.isError;
        }
      }
    });
    ```
    active 是类名；
    isActive 是布尔值，用于判断是否使用该样式；
    ```
    <div v-bind:class="{active:isActive, error: isError}">
    ```
    控制多个样式情况下的使用；
    >
- 数组语法
    ```
    <div v-bind:class="[activeClass, errorClass]"></div>

    var vm = new Vue({
      el: '#app',
      data: {
        activeClass: 'active',
        errorClass: 'error',
      },
      methods: {
        handle: function () {
          this.activeClass = '';
        }
      }
    })
    ```
    activeClass, errorClass 中设置类名；

###### 相关语法细节
1 对象绑定和数组绑定可以结合使用；
```
<div v-bind:class="[activeClass, errorClass, {test: isTest}]">
```

2 class 绑定的值可以简化操作；
- 提高可读性
```
<div v-bind:class='arrClasses'></div>
<div v-bind:class='objClasses'></div>

var vm = new Vue({
    el: '#app',
    data: {
        activeClass: 'active',
        errorClass: 'error',
        isTest: true,

        arrClasses: ['active', 'error'],
        objClasses: {
          active: true,
          error: true,
        }
    },
    methods: {
        handle: function () {
            this.activeClass = '';
        }
    }
})
```

3 默认的 class 如何处理；
```
<div class="base" v-bind:class='objClasses'>测试样式</div>
```
- 保留 base 样式的同时，还有 objClasses 中的样式；

##### 2 style 样式处理
- 对象语法
    ```
    <div v-bind:style="{active:isActive}"></div>
    ```
- 数组语法
    ```
    <div v-bind:style="[activeClass, errorClass]"></div>
    ```

#### 3.7 分支循环结构
##### 1 分支结构
最终渲染出来的只有一个节点；
控制的是元素是否被渲染到页面；
- v-if
- v-else
- v-else-if
    ```
    <div id="app">
        <div v-if='score>=90'>优秀</div>
        <div v-else-if='score<90&&score>=80'>良好</div>
        <div v-else-if='score<80&&score>=60'>一般</div>
        <div v-else>差</div>
    </div>

    ```
- v-show
    当 flag 为 false 时，节点不会显示，是通过 css 来实现的；
    元素节点依然存在；
    适合于频繁切换显示隐藏的元素；
    ```
    <div v-show='flag'>测试 v-show</div>
    ```

##### 3 循环结构
- v-for 遍历数组
    ```
    <li v-for='item in list'>{{item}}</li>

    <li v-for='(item, index) in list'>{{item}} --- {{index}}</li>
    ```
- key，提高性能
    ```
    <li :key='item.id' v-for='(item, index) in list'>{{item}} --- {{index}}</li>
    ```

- v-for 遍历对象
    ```
    <li v-for='(value, key, index) in object'>{{key}} --- {{value}}</li>
    ```
- v-for 和 v-if 结合使用
    ```
    <li v-if='value===2' v-for='(value, key, index) in object'>{{key}} --- {{value}}</li>
    ```

### 4 tab 案例
根据某变量判定是否使用某样式；
```
:class="currentIndex===index?'active':''"
```

