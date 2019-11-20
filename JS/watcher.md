// 实现观察者模式代码
```js
class Sub {
  constructor (state) {
     this.state = state;
     this.sub = [];
  }
  geto() {
      return this.state;
  }
  seto(val) {
      this.state = val;
      this.notify();
  }
  add(obj) {
      this.sub.push(obj);
  }
  notify() {
     this.sub.forEach(item => {
       item.update(this.state);
     })
  }
}

class Watcher {
  constructor (name,sub) {
    this.name = name;
    this.state = 0;
    sub.add(this);
  }
  update(val) {
    this.state = val;
    console.log('name: ', this.name, ' state: ', this.state)
  }
}
```
