## 防抖
``` javascript
function debounce (fn, delay) {
  let timer = null;
  return function (...args) {
    clearTimeout(timer);
    timer = setTimeout(() => {
      fn.apply(this, args);
    }, delay);
  }
}
```
## 节流
``` javascript
// 固定时间触发
function throttle (fn, delay) {
  let flag = true;
  return function (...args) {
    if (flag) {
      flag = false;
      setTimeout(() => {
        flag = true;
      }, delay);
      fn.apply(this, args);
    }
  }
}

// fn返回promise
function throttle (fn) {
  let flag = true;
  return function (...args) {
    if (flag) {
      flag = false;
      fn.apply(this, args).finally(() => {
        flag = true;
      });
    }
  }
}
```