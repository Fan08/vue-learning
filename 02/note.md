<!--
 * @Descripttion: 
 * @version: 
 * @Author: 唐帆
 * @Date: 2020-05-10 10:25:21
 * @LastEditors: 唐帆
 * @LastEditTime: 2020-05-10 22:56:46
 -->
### 5 常用特性
- 表单操作
- 自定义指令
- 计算属性
- 过滤器
- 侦听器
- 生命周期

#### 5.2 表单操作
##### 1 基于 Vue 的表单操作
1 多选框
- 多选框的值绑定在 input 上；
- 且，值是数组类型；
    ```
      <div>
        <span>爱好：</span>
        <input type="checkbox" id="ball" value="1" v-model="hobby">
        <label for="ball">篮球</label>
        <input type="checkbox" id="sing" value="2" v-model="hobby">
        <label for="sing">唱歌</label>
        <input type="checkbox" id="code" value="3" v-model="hobby">
        <label for="code">写代码</label>
      </div>
    ```
2 select 框
- 值绑定在 select 标签上；
- 单选时值为字符串，多选时为数组；
    ```
        <select v-model='occupation'>
            <option value='0'>请选择职业...</option=>
            <option value='1'>教师</option>
            <option value='2'>软件工程师</option>
            <option value='3'>律师</option>
        </select>

        <!-- 多选状态 -->
        <select v-model='occupation' multiple>
          <option value='0'>请选择职业...</option=>
          <option value='1'>教师</option>
          <option value='2'>软件工程师</option>
          <option value='3'>律师</option>
        </select>
    ```

3 表单域修饰符
- number：转化为数值；
    输入为字符串，自动转为数字；
    ```
    <input v-model.number="age" type="number">
    ```
- trim：去掉首尾空格；
- lazy：将 input 事件切换为 change 事件；
    - input 事件是在输入框发生数据变化时立刻触发；
    - change 事件是在失去焦点时触发；
    - 比如注册时的数据验证，输入框失去焦点时进行验证；

#### 5.3 自定义指令
##### 1 为何需要自定义指令？
内置指令不满足需求

##### 2 自定义指令的语法规则（获取元素焦点）
```
Vue.directive('focus' {
    inserted: function(el) {
        // 获取元素的焦点
        el.focus();
    }
})
```

##### 3 自定义指令用法
```
<input type='text' v-focus>
```

##### 4 带参数的自定义指令（改变元素背景色）
可以使用多种钩子函数，inserted 就是其中一种钩子函数；
```
Vue.directive('color', {
    inserted: function(el, binding) {
        el.style.backgroundColor = binding.value.color;
    }
})
```

##### 5 指令的用法
```
<input type="text" v-color='{color: "orange"}'>
```

##### 6 局部指令
```
directives: {
    focus: {
        // 指令的定义
        inserted: function (el) {
            el.focus();
        }
    }
}
```
局部指令只能在该组件内使用，是被限制的；
而之前的是全局指令；

#### 5.4 计算属性
##### 2 计算属性的用法
在 Vue 的实例对象中新增 computed 对象；
```
<!-- 方法使用 -->
<div>{{reverseString}}</div>

computed: {
    reverseString: function () {
        return this.msg.split('').reverse().join('');
    }
}
```

##### 3 计算属性与方法的区别
- 计算属性是基于它们的依赖进行缓存的；
- 方法不存在缓存；
- 对于相同的计算，计算属性只运行一次，而方法则会多次运行；
    - 从而可以实现性能的节省；

#### 5.5 侦听器
数据变化触发侦听器绑定的方法；

##### 1 侦听器应用场景
数据变化时执行异步或开销较大的操作；

##### 2 侦听器的用法
```
// 通过 watch 属性来监听数据的变化
watch: {
    // val 实际上直接获取了 firstName 的最新值
    firstName: function (val) {
        this.fullName = val + ' ' + this.lastName;
    },
    lastName: function (val) {
        this.fullName = this.firstName + ' ' + val;
    }
}
```

##### 案例 验证用户名是否可用
需求：输入框中输入姓名，<strong>失去焦点</strong>时验证是否存在；
- 1 通过 v-model 实现数据绑定；
- 2 需要提供提示信息；
- <strong>3 需要侦听器监听输入信息的变化；</strong>
    - 使用侦听器监听用户名的变化
    - 调用后台接口进行验证
    - 根据验证结果调整提示信息
- 4 修改触发的事件从 input 变 change；

#### 5.6 过滤器
格式化数据；

##### 2 自定义过滤器
```
Vue.filter('过滤器名称', function(value){
    // 过滤器业务逻辑
})
```

##### 3 过滤器使用
```
<!-- 插值表达式使用 -->
<div>{{msg | upper}}</div>
<!-- 级联使用，多个过滤器进行过滤 -->
<div>{{msg | upper | lower}}</div>

<!-- 通过属性绑定的值使用 -->
<div v-bind:id="id | formatId"></div>


Vue.filter('upper', function (val) {
    return val.charAt(0).toUpperCase() + val.slice(1);
})

Vue.filter('lower', function (val) {
    return val.charAt(0).toLowerCase() + val.slice(1);
})

var vm = new Vue({
    el: "#app",
    data: {
        msg: '',
    }
});
```

##### 4 局部过滤器
```
filters: {
    capitalize: function(){
        
    }
}
```

##### 5 带参数的过滤器
第一个 value 是默认值参数，第二个开始才是自定义参数；
```
Vue.filter('format', function(value, arg1){
    // value 就是过滤器传递过来的参数
})
```

##### 6 过滤器的使用
```
<div>{{date | format('yy-MM-dd')}}</div>
```

#### 5.7 生命周期
##### 1 主要阶段
- 挂载（初始化相关属性）
    1 beforeCreate
    2 created
    3 beforeMount
    4 mounted
    - mounted 被触发意味着组件加载完成；
    - 此时页面中已经有相关元素，可以进行数据填充；
- 更新（元素或组件的变更操作）
    1 beforeUpdate
    - 数据更新时调用，发生在虚拟 DOM 打补丁之前；

    2 update
    - 由于数据更改导致的虚拟 DOM 重渲染和打补丁，在这之后调用该钩子；
- 销毁（销毁相关属性）
    1 beforeDestroy
    2 destroyed
    
##### 案例
###### 1 变异方法
将会<strong>触发视图更新</strong>；
- push()
- pop()
- shift()
- unshif()
- splice()
- sort()
- reserve()

###### 2 替换数组
- filter()
- concat()
- slice()

###### 3 修改响应式数据
- Vue.set(vm.items, indexOfItem, newValue)
- vm.$set(vm.items, indexOfItem, newValue)
    - 参数一表示要处理的数组名称
    - 参数二表示要处理的数组的索引
    - 参数三表示要处理的数组的值
```
Vue.set(vm.list, 2, 'lemon');
vm.$set(vm.list, 1, 'lemon')
```

- 修改对象属性
```
var vm = new Vue({
      el: '#app',
      data: {
        fname: '',
        list: ['apple', 'orange', 'banana'],

        info: {
          name: '李四',
          age: 12
        }
      },
})

vm.$set(vm.info, 'gender', 'female')
```