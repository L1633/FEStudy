## setState 是同步的还是异步的

setState 并不是真正的异步函数，它实际上是通过队列延迟执行操作实现的，通过 isBatchingUpdates 来判断 setState 是先存进 state 队列还是直接更新。值为 true 则执行异步操作，false 则直接同步更新。

在 onClick、onFocus 等事件中，由于合成事件封装了一层，所以可以将 isBatchingUpdates 的状态更新为 true；在 React 的生命周期函数中，同样可以将 isBatchingUpdates 的状态更新为 true。那么在 React 自己的生命周期事件和合成事件中，可以拿到 isBatchingUpdates 的控制权，将状态放进队列，控制执行节奏。而在外部的原生事件中，并没有外层的封装与拦截，无法更新 isBatchingUpdates 的状态为 true。这就造成 isBatchingUpdates 的状态只会为 false，且立即执行。所以在 addEventListener&nbsp;、setTimeout、setInterval 这些原生事件中都会同步更新。


### 总结
setState 并非真异步，只是看上去像异步。在源码中，通过 isBatchingUpdates 来判断setState 是先存进 state 队列还是直接更新，如果值为 true 则执行异步操作，为 false 则直接更新。

那么什么情况下 isBatchingUpdates 会为 true 呢？在 React 可以控制的地方，就为 true，比如在 React 生命周期事件和合成事件中，都会走合并操作，延迟更新的策略。

但在 React 无法控制的地方，比如原生事件，具体就是在 addEventListener、setTimeout、setInterval 等事件中，就只能同步更新。

一般认为，做异步设计是为了性能优化、减少渲染次数，React 团队还补充了两点。
1 保持内部一致性。如果将 state 改为同步更新，那尽管 state 的更新是同步的，但是 props不是。

2 启用并发更新，完成异步渲染。


 setState 本身的方法调用是同步的，但是调用了 setState 并不表示 react 的 state 立马就更新，根据当前执行环境的上下文来判断，如果处于批量更新的环境，那么 state 就不是立马更新，如果不处于批量更新的环境 ， 就有可能使立马更新

比如之前的合成事件，由于你是setTimeout(_ => { this.setState()}, 0)是在try代码块中,当你try代码块执行到setTimeout的时候，把它丢到列队里，并没有去执行，而是先执行的finally代码块，等finally执行完了，isBatchingUpdates又变为了false，导致最后去执行队列里的setState时候，requestWork走的是和原生事件一样的expirationTime === Syncif分支，所以表现就会和原生事件一样，可以同步拿到最新的state值

