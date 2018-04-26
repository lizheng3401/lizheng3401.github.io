---
title: Vue解决echart在element的tab切换时显示不正确
date: 2018-03-30 22:36:06
tags:
  - 项目问题记录
  - Vue
  - Element
categories: Web开发
---
最近在项目中遇到了这种情况，需要在tab控件上渲染多个echart图标，然后切换查看时，发现图表的宽度不正确
<!-- more -->
### 原因：在页面进行加载时，隐藏的图表找不到对应的div大小，所以默认给了一个大小。所以要做的就是在页面加载时，就对图表进行初始化。

网上的解决方案大多都是监听tab的切换事件，然后再根据切换的页面重新渲染echart组件，比较麻烦。如下是个人的解决方法：

### 原理：利用`v-if`属性,当切换至对应的tab时，设置其`v-if`的值为`true`即可，同时设置默认显示的tab

举例如下：

```HTML
<el-tabs type="card" v-model="tabItem">
  <el-tab-pane name="heart">
    <span slot="label"><icon name="heart" scale="2"></icon>心率</span>
    <baseline ref="heart" :chartData="{}" v-if="'heart' === tabItem"></baseline>
  </el-tab-pane>
  <el-tab-pane name="breath">
    <span slot="label"><icon name="breath" scale="2"></icon>呼吸</span>
    <baseline ref="breath" :chartData="{}" v-if="'breath' === tabItem"></baseline>
  </el-tab-pane>
  <el-tab-pane label="体动" name="move">
    <span slot="label"><icon name="move" scale="2"></icon>体动</span>
    <baseline ref="move" :chartData="{}" v-if="'move' === tabItem"></baseline>
  </el-tab-pane>
</el-tabs>
```

这里默认tab为心率tab，当切换时，同一时刻只有一个`v-if`为`true`，当将其设置为`true`时，Vue会重新在页面渲染组件，即完成了组件渲染的步骤。
[欢迎访问我的博客了解更多](http://lizheng3401.github.io/2018/03/30/Vue解决echart在element的tab切换时显示不正确/)