<!--
 * @Author: hcs
 * @Date: 2023-04-17 08:12:05
 * @LastEditTime: 2023-04-18 11:04:28
 * @LastEditors: Do not edit
 * @Description: Modify here please
 * @FilePath: \git_program\FEStudy\CSS\BFC.md
-->
### BFC
  BFC （block formatting context）块级格式化上下文，是⼀个独立的渲染区域，让处于 BFC 内部的元素 与 外部的元素 相互隔离，使内外元素的定位不会相互影响。

### 触发条件
  根元素, 即 HTML 元素

  position: absolute/fixed

  display: inline-block / table / table-row/ table-ceil

  float !== none

  ovevflow !== visible

### 应用
阻止 margin 重叠

块的上外边距 (margin-top)和下外边距 (margin-bottom)有时合并 (折叠) 为单个边距，其大小为单个边距的最大值 (或如果它们相等，则仅为其中一个)，这种行为称为边距折叠。
```
  p {
    width: 100px;
    height: 100px;
    background:#00F0F0;
    margin: 30px;
  }
  /* 触发 BFC */
  div {
    overflow: hidden; 
  }

  <p>left</p>
  <div>
    <p>right</p>
  </div>

```

可以 包含浮动元素 —— 清除内部浮动(清除浮动的原理是两个 div 都位于同⼀个 BFC 区域之中) 防止元素塌陷
```

  div {
    border: 5px solid #F00;
    /* 触发 BFC */
    overflow: hidden;
  }
  
  .box {
    width: 100px;
    height: 100px;
    float: left;
    background:#00F0F0;
  }
  <div>
    <p class="box"></p>
    <p class="box"></p>
  </div>

```

⾃适应两栏布局

可以 阻止元素被浮动元素覆盖 (文字环绕) 

```
 .box {
    width: 100px;
    height: 100px;
    float: left;
    background:#00F0F0;
  }
  .text {
    font-size: 14px;
    overflow: hidden;  /* 触发 BFC */
  }

  <div>
    <p class="box">盒子</p>
    <p class="text">
      文字文字文字文字文字文字文字文字文字文字文字文字
      文字文字文字文字文字文字文字文字文字文字文字文字
      文字文字文字文字文字文字文字文字文字文字文字文字
      文字文字文字文字文字文字文字文字文字文字文字文字
      文字文字文字文字文字文字文字文字文字文字文字文字
      文字文字文字文字文字文字文字文字文字文字文字文字
      文字文字文字文字文字文字文字文字文字文字文字文字
      文字文字文字文字文字文字文字文字文字文字文字文字
      文字文字文字文字文字文字文字文字文字文字文字文字
      文字文字文字文字文字文字文字文字文字文字文字文字
    </p>
  </div>

```