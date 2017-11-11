### 单元素/组件的过渡

Vue 提供了 transition 的封装组件，在下列情形中，可以给任何元素和组件添加 entering/leaving 过渡
* 条件渲染 (使用 v-if)
* 条件展示 (使用 v-show)
* 动态组件
* 组件根节点

代码示例：

 ![](../img/transition.png)
过渡顺序：
 * 当v-if,v-show的值为从false变成true,也就是官网里称之为'进入过渡':
		类名的触发顺序是：enter--->enter-active--->enter-to
 * 当v-if,v-show的值为从true变成false,也就是官网里称之为'离开过渡':
		类名的触发顺序是: leave--->leave-active--->leave-to

各个类名的含义:


```


```