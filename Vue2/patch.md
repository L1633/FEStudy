# patch
 patch 的整体流程: createComponent -> 子组件初始化 -> 子组件 render -> 子组件 patch

 activeInstance 为当前激活的 vm 实例; vm.$vnode 为组件的占位 vnode; vm._vnode 为组件的 渲染 vnode

 嵌套组件的插入顺序是先子后父