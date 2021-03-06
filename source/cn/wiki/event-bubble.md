---
title: 事件冒泡
type: wiki
group: 事件
order: 4.2
version: 2.1
---

# 事件冒泡 <span class="api-version">v0.13+</span>

Weex 2.0 实现了 W3C 标准的事件冒泡机制。

### 使用

```html
<template>
  <div class="root" @click="rootClick" bubble="true">
    <text style="font-size: 40px;">{{rootText}}</text>
    <div class="outer" @click="parentClick">
      <div>
        <text style="font-size: 40px;">{{parentText}}</text>
      </div>
      <text class="inner" @click="click">{{innerText}}</text>
    </div>
  </div>
</template>

<script>
  module.exports = {
    data: {
      innerText: 'click me',
      parentText: '',
      rootText: ''
    },
    methods: {
      click: function(e) {
        this.innerText = 'inner bubble'
      },
      parentClick: function(e) {
        this.parentText = 'parent bubble'
      },
      rootClick: function(e) {
        this.rootText = 'root bubble'
      }
    }
  }
</script>

<style scoped>
  .inner {
    font-size: 40px;
    text-align: center;
    background-color: #7a9b1b;
    padding: 40px;
  }

  .outer {
    font-size: 40px;
    text-align: center;
    background-color: #9b7a1b;
    padding: 120px;
  }

  .root {
    font-size: 40px;
    text-align: center;
    background-color: #7a1b9b;
    padding: 80px;
  }
</style>
```

[try it](http://dotwe.org/vue/fa2957ce3e9eb47ad9ae1da22d845e95)

运行以上代码，用客户端打开，点击中间的元素，可以看到事件向上传播，依次触发。

### 注意

需要注意的是: ** 为了兼容之前的版本，Weex 默认不会开启事件冒泡机制。需要在根元素的属性上，添加 `bubble="true"` 来开启冒泡机制。否则，将不会向上传播事件，保持与之前版本的效果相同。 **

### stopPropagation

在事件处理函数中，可以使用 `e.stopPropagation()` 方法，来阻止本次事件向上的传递过程。注意，`e.stopPropagation()` 与 `bubble="true"` 不同，前者只会影响当前元素以及父元素的传播，不会影响子元素的传播；后者是为了版本兼容而增加的开关机制，会全局关闭或者开启冒泡机制，两者可以共同存在使用，如下：

```html
<template>
  <div class="root" @click="rootClick" bubble="true">
    <text style="font-size: 40px;">{{rootText}}</text>
    <div class="outer" @click="parentClick">
      <div>
        <text style="font-size: 40px;">{{parentText}}</text>
      </div>
      <text class="inner" @click="click">{{innerText}}</text>
    </div>
  </div>
</template>

<script>
  module.exports = {
    data: {
      innerText: 'click me',
      parentText: '',
      rootText: ''
    },
    methods: {
      click: function(e) {
        this.innerText = 'inner bubble'
      },
      parentClick: function(e) {
        this.parentText = 'parent bubble'
        e.stopPropagation()
      },
      rootClick: function(e) {
        this.rootText = 'root bubble'
        // e.stopPropagation()
      }
    }
  }
</script>

<style scoped>
  .inner {
    font-size: 40px;
    text-align: center;
    background-color: #7a9b1b;
    padding: 40px;
  }

  .outer {
    font-size: 40px;
    text-align: center;
    background-color: #9b7a1b;
    padding: 120px;
  }

  .root {
    font-size: 40px;
    text-align: center;
    background-color: #7a1b9b;
    padding: 80px;
  }
</style>
```

[try it](http://dotwe.org/vue/2cc80e19c9b2430fb780234628065a69)

运行以上代码，用客户端打开，点击中间的元素，可以看到事件向上传播到父元素被终止，不再继续往根元素传播。
