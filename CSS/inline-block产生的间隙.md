## CSS 知识点：inline-block产生的间隙
display： inline-block  会产生间隙
  产生的原因： inline-blcok 块之间的不可见符号会被保留父层字体的1/3大小的空间， 比如 父元素的 font-size 为 30px ，那么间隙就是 10px.

### 解决方案
  1. 父元素 font-size：0
  2. 改变代码的书写样式 不进行换行的书写方式， 也就是移除空格
  3. letter-spacing 要在子元素上设置letter-spacing:0
  4. margin-right 负值
    子元素 使用 margin-right 负值，右侧元素移动，自身不受影响