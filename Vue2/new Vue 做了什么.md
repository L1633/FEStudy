# new Vue 做了什么
  init ----- $mount ----- compile ----- render ----- vnode ----- patch ---- DOM

  patch 将 vnode 转换成真是的 DOM

## new Vue
  new Vue({
    el: '#app',
    template: '<div>Test</div>'
  })

  function Vue(options) {
    // a lot of code......
    this._init(options)
  }

## init
 Vue.prototype._init = function(options?: Object) {
  const vm: Component = this;
  
  if (options && options._isComponent) {
  // a lot of code......
  } else {
    vm.$options = mergeOptions(
      resolveComstructorOptions(vm.constructor),
      options || {},
      vm
    )
  }

  vm._self = vm;
  initLifecycle(vm);
  initEvents(vm);
  initRender(vm);
  callHook(vm, 'beforeCreate');
  initInjections(vm);
  initState(vm);
  initProvide(vm);
  callHook(vm, 'created');

  if(vm.$options.el) {
    vm.$mount(vm.$optuons.el);
  }

 }

## mount
// public mount method
Vue.prototype.$mount = function (
  el,
  hydrating
) {
  el = el && inBrowser ? query(el) : undefined;
  return mountComponent(this, el, hydrating)
};

// 缓存一下上面的方法
var mount = Vue.prototype.$mount;

Vue.prototype.$mount = function (
  el,
  hydrating
) {
  el = el && query(el);

  if (el === document.body || el === document.documentElement) {
    process.env.NODE_ENV !== 'production' && warn(
      "Do not mount Vue to <html> or <body> - mount to normal elements instead."
    );
    return this
  }

  var options = this.$options;
  // 讲 模板 / el 转换成  render , Vue 最终只认 render 函数！
  // resolve template/el and convert to render function
  if (!options.render) {
    var template = options.template;
    if (template) {
      if (typeof template === 'string') {
        if (template.charAt(0) === '#') {
          template = idToTemplate(template);
          /* istanbul ignore if */
          if (process.env.NODE_ENV !== 'production' && !template) {
            warn(
              ("Template element not found or is empty: " + (options.template)),
              this
            );
          }
        }
      } else if (template.nodeType) {
        template = template.innerHTML;
      } else {
        if (process.env.NODE_ENV !== 'production') {
          warn('invalid template option:' + template, this);
        }
        return this
      }
    } else if (el) {
      template = getOuterHTML(el);
    }
    // 是否有模板
    if (template) {
      /* istanbul ignore if */
      if (process.env.NODE_ENV !== 'production' && config.performance && mark) {
        mark('compile');
      }

      var ref = compileToFunctions(template, {
        outputSourceRange: process.env.NODE_ENV !== 'production',
        shouldDecodeNewlines: shouldDecodeNewlines,
        shouldDecodeNewlinesForHref: shouldDecodeNewlinesForHref,
        delimiters: options.delimiters,
        comments: options.comments
      }, this);
      var render = ref.render;
      var staticRenderFns = ref.staticRenderFns;
      options.render = render;
      options.staticRenderFns = staticRenderFns;

    }
  }
  // 执行 缓存的 mount 方法
  return mount.call(this, el, hydrating)
};


function mountComponent(){
  // a lot of code.....
  callHook(vm, 'beforeMount')
  if () {
    // a lot of code
  } else {
    // 重点
     updateComponent = () => {
      vm._update(vm._render(), hydrating)
    }
  }
  // 渲染 watcher
  // 构造函数 执行了 updateComponent
  new Watcher(vm, updateComponent, noop, {
    before() {
      if (vm._isMounted && !vm._isDestroyed) {
        callHook(vm, 'beforeUpdate')
      }
    }
  }, true /* isRenderWatcher */ )
  hydrating = false

  // manually mounted instance, call mounted on self
  // mounted is called for render-created child components in its inserted hook
  if (vm.$vnode == null) {
    vm._isMounted = true
    callHook(vm, 'mounted')
  }
  return vm
 }


class Watcher {
  constructor(
    vm: Component,
    expOrFn: string | Function,
    cb: Function,
    options?: ?Object,
    isRenderWatcher?: boolean
  ) {
    // a lot of code......
  // parse expression for getter
  if (typeof expOrFn === 'function') {
      this.getter = expOrFn
  } else {
      this.getter = parsePath(expOrFn)
      if (!this.getter) {
        this.getter = noop
        process.env.NODE_ENV !== 'production' && warn(
          `Failed watching path: "${expOrFn}" ` +
          'Watcher only accepts simple dot-delimited paths. ' +
          'For full control, use a function instead.',
          vm
        )
      }
    }
  this.value = this.lazy
    ? undefined
    : this.get()
  }

  get () {
    pushTarget(this)
    let value
    const vm = this.vm
    try {
      value = this.getter.call(vm, vm)
    } catch (e) {
      if (this.user) {
        handleError(e, vm, `getter for watcher "${this.expression}"`)
      } else {
        throw e
      }
    } finally {
      // "touch" every property so they are all tracked as
      // dependencies for deep watching
      if (this.deep) {
        traverse(value)
      }
      popTarget()
      this.cleanupDeps()
    }
    return value
  }

}


## render
最后执行   vm._update(vm._render(), hydrating)
首先看看 render
Vue.prototype._render = function (): VNode { 
  const { render, _parentVnode } = vm.$options
  // a lot of code
   try {
    currentRenderingInstance = vm
    vnode = render.call(vm._renderProxy, vm.$createElement)
    } catch(e) {

    }

  return vnode
}

function createElement(
  context: Component,
  tag: any,
  data: any,
  children: any,
  normalizationType: any,
  alwaysNormalize: boolean
){
   return _createElement(context, tag, data, children, normalizationType)
}

_createElement 真正的创建 vnode

## update
vm._update(vm._render(), hydrating)
把 Vnode 生成真是的 DOM

Vue.prototype._update = function (vnode: VNode, hydrating ? : boolean) {
  if (!prevVnode) {
      // initial render
      vm.$el = vm.__patch__(vm.$el, vnode, hydrating, false /* removeOnly */ )
  } else {
      // updates
      vm.$el = vm.__patch__(prevVnode, vnode)
  }

}

Vue.prototype.__patch__ = inBrowser ? patch : noop

export const patch: Function = createPatchFunction({ nodeOps, modules })
__patch__ 最后 返回 patch 函数

functin createPatchFunction(){
  return functin patch(oldVnode, vnode, hydrating, removeOnly){
    if () {

    } else {
    // create an empty node and replace it
    // 初次渲染 新建一个 vnode ， elm 是我们的真实的 #app, 转化成 Vnode
      oldVnode = emptyNodeAt(oldVnode)

    }

    // replacing existing element 替换现有元素
    const oldElm = oldVnode.elm // 首次渲染 这个就是 id 为 app 的 dom
    const parentElm = nodeOps.parentNode(oldElm) // 父节点就是body
       // create new node
    createElm(
      vnode,
      insertedVnodeQueue,
      // extremely rare edge case: do not insert if old element is in a
      // leaving transition. Only happens when combining transition +
      // keep-alive + HOCs. (#4590)
      oldElm._leaveCb ? null : parentElm,
      nodeOps.nextSibling(oldElm)
    )

     // destroy old node 删除掉占位的 id 为 app 的 DOM 
    if (isDef(parentElm)) {
      removeVnodes([oldVnode], 0, 0)
    } else if (isDef(oldVnode.tag)) {
      invokeDestroyHook(oldVnode)
    }

  }
}

// createElm 的作⽤是通过虚拟节点创建真实的 DOM 并插⼊到它的⽗节点中
  function createElm (
    vnode,
    insertedVnodeQueue,
    parentElm,
    refElm,
    nested,
    ownerArray,
    index
  ){
    vnode.elm = vnode.ns
        ? nodeOps.createElementNS(vnode.ns, tag)
        : nodeOps.createElement(tag, vnode)
      setScope(vnode)

    if () {

    } else {
      // createChildren 遍历⼦虚拟节点，递归调⽤ createElm
      // 在遍历过程中会把 vnode.elm 作为⽗容器的 DOM 节 点占位符传⼊
      createChildren(vnode, children, insertedVnodeQueue)
      if (isDef(data)) {
        invokeCreateHooks(vnode, insertedVnodeQueue)
      }
      // 真实的 插入操作， 初始化的时候就是 插入 到 parentElm ：body 中
      // 此时 会有 2 个 id 为 app 的 div ！！！
      insert(parentElm, vnode.elm, refElm)
    }

  }

  //实际上整个过 程就是递归创建了⼀个完整的 DOM 树并插⼊到 Body 上。