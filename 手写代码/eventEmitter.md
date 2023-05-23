### EventEmitter
class EventEmitter {


  once(type, listener) {
    let wrapper = (...rest) => {
      listener.apply(this, rest);
      this.removeListener(this, wrapper)
    }
    this.on(type, wrapper);
  }

  removeListener(type, listener) {
    if (this._events[type]) {
      this._events[type] = this._events[type].filter(l => l != listener);
    }
  }

}