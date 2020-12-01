Vue：渐进式JavaScript框架，提供基础服务为主
         库的话就是API的支持

#### 指令
- 本质就是自定义属性
- 指令的格式：以v-开始
- 
1、v-cloak指令用法        //插值表达式存在的问题：“闪动”用v-cloak解决
   提供样式
   [v-cloak] {
       display:none;
   }
   在插值表达式所在的标签中添加v-cloak指令
   <div v-cloak>{{msg}}</div>

   背后的原理：先通过样式隐藏内容，然后再内现存中进行值得替换，替换好之后再显示最终的结果

2、数据绑定指令
   v-text 填充纯文本：相比插值表达式更加简洁

   v-html 填充HTML片段：存在安全问题，本网站内部可以使用，来自第三方的数据不可以用

   v-pre 填充原始信心：显示原始信息，跳过编译过程

3、数据响应式
   如何理解响应式：@html5中的响应式（屏幕尺寸的变化导致样式的变化）@数据的响应式（数据的变化导致页面内容的变化）

   什么是数据绑定：@将数据填充到标签中

   v-once只编译一次：@显示内容之后不再具有响应式功能

4、双向数据绑定   v-model
   主要体现在表单输入域中：用户更改数据，数据有影响页面也变化了

   MVVM设计思想：Model、View、View-

5、事件绑定
   v-on：<input type="button"  v-on:click='num++'/>
   简写：<input type='button' @click='num++'/>

   事件函数的调用方式
   第一种：直接绑定函数名称： <button @click='handel'>双击</button>
                默认会认为传递事件对象作为事件函数的第一个参数
   第二种：调用函数： <button @click='handel( )'>双击</button>
                事件对象必须作为最后一个参数显示传递，并且事件对象的名称必须是$event
```js
var vm = new Vue ({
    el:'#app',
    data:{
        num:0
    },
    methods:{   //函数必须定义在methods里面，里面
        handel:function(){
            //这里this是Vue的实例对象
            console.log(this === vm)
            this.num++;
        }
    }
});
```
   事件函数参数传递：<button @click='handel("hi",$event )'>双击</button>

6、事件修饰符
   .stop阻止冒泡
   <a v-on:click.stop="handle">跳转</a>
   .prevent阻止默认行为
   <a v-onclick.prevent="handel">跳转</a>

7、按键修饰符
   .enter回车键 <input v-on:keyup.enter='submit'>
   .delete删除键 <input v-on:keyup.delete='handle'>

8、自定义按键修饰符
   .全局config.keyCodes对象   config.keyCodes .f1 = 112  

9、属性绑定
   v-bind指令：<a v-bind:href='url'>跳转</a>
                        <a :href='url'>跳转</a>

10、样式绑定
   class样式处理：<div v-bind:class="{active: isActive}"></div>
          数组语法：<div v-bind:class="{activeClass , errorClass}"></div>
   style样式处理：<div v-bind:style='{borderStyle,width:widthStyle}'>

11、分支循环结构
   v-if  控制元素是否渲染到页面
   v-else
   v-else-if
   v-show  控制元素是否显示（已经渲染了页面）

   v-for遍历数组：<li v-for='item in list'>{{item}}</li>
   <li v-for = '(item,index) in list'>{{item}} +'---' +{{index}}</li>

   key的作用：帮助Vue区分不同的元素，从而提高性能
   <li :key='item.id' v-for='(item,index) in list>{{item}} + '---' {{index}} </li> 


#### 案例时间：实现Tab栏切换
实现步骤：
      1.实现静态UI效果：用传统的方式实现标签结构和样式
      2.基于数据重构UI效果：将静态的结构和样式重构为基于Vue模板语法的形式，处理时间和js控制逻辑
      3.声明式编程：模板的结构和最终显示的效果基本一致
```js
<body>
    <div id="app">
        <div class="tab">
            <ul>
                <li v-on:click='change(index)' :class='currentIndex==index?"active":""' :key='item.id' v-for='(item,index) in list'>{{item.title}}</li>
            </ul>
            <div :class='currentIndex==index?"current":""' :key='item.id' v-for='(item, index) in list'>
                <img :src="item.path">
            </div>
        </div>
    </div>
    <script type="text/javascript" src="js/vue.js"></script>
    <script type="text/javascript">
        var vm = new Vue({
            el: '#app',
            data: {
                currentIndex: 0, // 选项卡当前的索引
                list: [{
                    id: 1,
                    title: 'apple',
                    path: 'img/apple.png'
                }, {
                    id: 2,
                    title: 'orange',
                    path: 'img/orange.png'
                }, {
                    id: 3,
                    title: 'lemon',
                    path: 'img/lemon.png'
                }]
            },
            methods: {
                change: function(index) {
                    // 在这里实现选项卡切换操作：本质就是操作类名
                    // 如何操作类名？就是通过currentIndex
                    this.currentIndex = index;
                }
            }
        });
    </script>
</body>
```