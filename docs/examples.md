# 使用案例

## Vue 自定义指令 v-window 的基本使用案例

本案例包含 3 个特性：拖拽移动、调整大小、窗口最大化。

注意：
- `<iframe />`会把移动事件（`touchmove`和`mousemove`）吞掉，造成鼠标进入`<iframe />`区域后便不再响应拖拽移动或是调整大小的功能；`vue-directive-window`已经对这种场景做了相应的处理，现在你可以愉快的在窗口内使用`<iframe />`了。
::: demo

```html
<template>
  <div class="container">
    <div class="window window1" v-show="ifShowWindow" v-window="windowParams">
      <div class="window__header">
        一般窗口
        <button class="maximize-btn" type="button">
          <template v-if="!isMaximize"
            >点这放大</template
          >
          <template v-else
            >点这缩小</template
          >
        </button>
      </div>
      <div class="window__body">
        <iframe height="100%" width="100%" frameborder="0" src="https://array-huang.github.io/vue-directive-window/">
      </div>
    </div>

    <button type="button" @click="ifShowWindow = true" v-if="!ifShowWindow">
      显示窗口
    </button>
    <button type="button" @click="ifShowWindow = false" v-else>隐藏窗口</button>
  </div>
</template>
<script>
  Vue.use(window['vue-directive-window']);

  function maximizeCb(isMaximize) {
    this.isMaximize = isMaximize;
  }

  export default {
    data() {
      return {
        windowParams: {
          minWidth: 10,
          maxWidth: 800,
          minHeight: 100,
          maxHeight: 800,
          customMaximizeHandler: '.maximize-btn',
          maximizeCallback: maximizeCb.bind(this),
        },
        ifShowWindow: false,
        isMaximize: false,
      };
    },
  };
</script>
<style>
  .container {
    padding: 30px;
  }
  .window1 {
    width: 400px;
    position: fixed;
    top: 60px;
    left: 0;
  }
</style>
```

:::

## 函数方式的基本使用案例

::: demo

```html
<template>
  <div class="container">
    <div class="window window2 window--by-function" v-show="ifShowWindow">
      <div class="window__header">
        函数方式生成的窗口
      </div>
      <div class="window__body">
        <p>窗口内容1</p>
        <p>窗口内容2</p>
        <p>窗口内容3</p>
      </div>
    </div>

    <button type="button" @click="show" v-if="!ifShowWindow">显示窗口</button>
    <button type="button" @click="hide" v-else>隐藏窗口</button>
  </div>
</template>
<script>
  function enhanceWindow() {
    const windowElement = document.querySelector('.window--by-function');
    const windowParams = {
      minWidth: 10,
      maxWidth: 400,
      minHeight: 100,
      maxHeight: 400,
    };
    window['vue-directive-window'].enhanceWindow(windowElement, windowParams);
  }
  export default {
    data() {
      return {
        ifShowWindow: false,
        ifInited: false,
      };
    },
    methods: {
      show() {
        this.ifShowWindow = true;
        if (!this.ifInited) {
          enhanceWindow();
        }
      },
      hide() {
        this.ifShowWindow = false;
      },
    },
  };
</script>
<style>
  .container {
    padding: 30px;
  }
  .window2 {
    width: 200px;
    position: fixed;
    top: 60px;
    left: 200px;
  }
</style>
```

:::

## 停用拖拽移动特性，启用部分方向的调整大小

::: demo

```html
<template>
  <div class="container">
    <div class="window window3" v-show="ifShowWindow" v-window="windowParams">
      <div class="window__header">
        部分功能可用的窗口
      </div>
      <div class="window__body">
        <p>窗口内容1</p>
        <p>窗口内容2</p>
        <p>窗口内容3</p>
      </div>
    </div>

    <button type="button" @click="ifShowWindow = true" v-if="!ifShowWindow">
      显示窗口
    </button>
    <button type="button" @click="ifShowWindow = false" v-else>隐藏窗口</button>
  </div>
</template>
<script>
  Vue.use(window['vue-directive-window']);
  export default {
    data() {
      return {
        windowParams: {
          movable: false,
          resizable: ['left', 'left-top'],
        },
        ifShowWindow: false,
      };
    },
  };
</script>
<style>
  .container {
    padding: 30px;
  }
  .window3 {
    width: 200px;
    position: fixed;
    top: 60px;
    left: 400px;
  }
</style>
```

:::

## 指定拖拽移动的 handler

这个例子中，只有拖拽`.window_header`，即窗口的头部，才能拖动窗口

::: demo

```html
<template>
  <div class="container">
    <div class="window window4" v-show="ifShowWindow" v-window="windowParams">
      <div class="window__header">
        指定拖拽移动的handler的窗口
      </div>
      <div class="window__body">
        <p>窗口内容1</p>
        <p>窗口内容2</p>
        <p>窗口内容3</p>
      </div>
    </div>

    <button type="button" @click="ifShowWindow = true" v-if="!ifShowWindow">
      显示窗口
    </button>
    <button type="button" @click="ifShowWindow = false" v-else>隐藏窗口</button>
  </div>
</template>
<script>
  Vue.use(window['vue-directive-window']);
  export default {
    data() {
      return {
        windowParams: {
          customMoveHandler: '.window__header',
        },
        ifShowWindow: false,
      };
    },
  };
</script>
<style>
  .container {
    padding: 30px;
  }
  .window4 {
    width: 200px;
    position: fixed;
    top: 60px;
    left: 600px;
  }
</style>
```

:::
