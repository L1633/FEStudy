## ueEffect 与 ueLayoutEffect 
从运用效果上而言，useEffect 与 useLayoutEffect 两者都是用于处理副作用,
不同点是使用场景

大多数场景下可以直接使用 useEffect，但是如果你的代码引起了页面闪烁，也就是引起了组件突然改变位置、颜色及其他效果等的情况下，就推荐使用 useLayoutEffect。

总结起来就是如果有直接操作 DOM 样式或者引起 DOM 样式更新的场景更推荐使用 useLayoutEffect。

### 总结
它们的共同点很简单，底层的函数签名是完全一致的，都是调用的 mountEffectImpl，在使用上也没什么差异，基本可以直接替换，也都是用于处理副作用。

useEffect 在 React 的渲染过程中是被异步调用的，用于绝大多数场景，而 LayoutEffect 会在所有的 DOM 变更之后同步调用，主要用于处理 DOM 操作、调整样式、避免页面闪烁等问题。也正因为是同步处理，所以需要避免在 LayoutEffect 做计算量较大的耗时任务从而造成阻塞。

React 团队的建议非常实用，如果实在分不清，先用 useEffect，一般问题不大；如果页面有异常，再直接替换为 useLayoutEffect 即可。