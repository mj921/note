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
## 数组方法
### forEach
``` javascript
function forEach (arr, fn) {
  for (let i = 0; i < arr.length; i++) {
    fn(arr[i], i, arr);    
  }
}
```
### map
``` javascript
function map (arr, fn) {
  const newArr = [];
  for (let i = 0; i < arr.length; i++) {
    newArr.push(fn(arr[i], i, arr));    
  }
}
```
### filter
``` javascript
function filter (arr, fn) {
  const newArr = [];
  for (let i = 0; i < arr.length; i++) {
    if (fn(arr[i], i, arr)) {
      newArr.push(arr[i]);  
    }  
  }
}
```
### some
``` javascript
function some (arr, fn) {
  for (let i = 0; i < arr.length; i++) {
    if (fn(arr[i], i, arr)) {
      return true;
    }  
  }
  return false;
}
```
### every
``` javascript
function every (arr, fn) {
  for (let i = 0; i < arr.length; i++) {
    if (!fn(arr[i], i, arr)) {
      return false;
    }  
  }
  return true;
}
```
### reduce
``` javascript
function reduce (arr, fn, initialValue) {
  for (let i = 0; i < arr.length; i++) {
    initialValue = fn(initialValue, arr[i], i, arr)
  }
  return initialValue;
}
```
### reduceRight
``` javascript
function reduceRight (arr, fn, initialValue) {
  for (let i = arr.length - 1; i >= 0; i--) {
    initialValue = fn(initialValue, arr[i], i, arr)
  }
  return initialValue;
}
```
### fill
``` javascript
function fill (arr, value, start = 0, end) {
  const length = arr.length;
  if (typeof start !== 'number') start = 0;
  if (typeof end !== 'number') end = length;
  if (start < 0) start += length;
  if (end < 0) end += length;
  const len = Math.min(length, end);
  for (let i = start; i < len; i++) {
    arr[i] =  value;
  }
}
```
### find
``` javascript
function find (arr, fn) {
  for (let i = 0; i < arr.length; i++) {
    if (fn(arr[i], i, arr)) {
      return arr[i];
    }  
  }
}
```
### findIndex
``` javascript
function findIndex (arr, fn) {
  for (let i = 0; i < arr.length; i++) {
    if (fn(arr[i], i, arr)) {
      return i;
    }  
  }
  return -1;
}
```
### flat
``` javascript
function flat (arr, depth = 1) {
  let newArr = [];
  for (let i = 0; i < arr.length; i++) {
    const item = arr[i];
    if (item instanceof Array) {
      if (depth > 1) {
        newArr = newArr.concat(flat(item, depth - 1));
      } else {
        newArr = newArr.concat(item);
      }
    } else {
      newArr.push(item);
    }
  }
  return newArr;
}
```
### flatMap
``` javascript
function flatMap (arr, fn) {
  let newArr = [];
  for (let i = 0; i < arr.length; i++) {
    const item = fn(arr[i], i, arr);
    if (item instanceof Array) {
      newArr = newArr.concat(item);
    } else {
      newArr.push(item);
    }
  }
  return newArr;
}
```
### includes
``` javascript
function includes (arr, valueToFind, fromIndex = 0) {
  const start = Math.max(fromIndex < 0 ? fromIndex + arr.length : fromIndex, 0);
  for (let i = start; i < arr.length; i++) {
    if (arr[i] === valueToFind) return true;
  }
  return false;
}
```
### indexOf
``` javascript
function indexOf (arr, valueToFind, fromIndex = 0) {
  const start = Math.max(fromIndex < 0 ? fromIndex + arr.length : fromIndex, 0);
  for (let i = start; i < arr.length; i++) {
    if (arr[i] === valueToFind) return i;
  }
  return -1;
}
```
### join
``` javascript
function join (arr, separator = ",") {
  if (arr.length <= 0) return ""
  let str = arr[0];
  for (let i = 1; i < arr.length; i++) {
    str += separator + arr[i];
  }
  return str;
}
```
### lastIndexOf
``` javascript
function lastIndexOf (arr, valueToFind, fromIndex = 0) {
  const start = Math.min(fromIndex < 0 ? fromIndex + arr.length : fromIndex, arr.length - 1);
  for (let i = start; i >= 0; i--) {
    if (arr[i] === valueToFind) return i;
  }
  return -1;
}
```
### slice
``` javascript
function slice (arr, start, end) {
  start = Math.max(start < 0 ? start + arr.length : start, 0);
  end = Math.min(end < 0 ? end + arr.length : end, arr.length - 1);
  const newArr = [];
  for (let i = start; i < end; i++) {
    newArr.push(arr[i]);
  }
  return newArr;
}
```
### splice
``` javascript
function splice (arr, start, deleteCount, ...args) {
  start = Math.max(start < 0 ? start + arr.length : start, 0);
  deleteCount = typeof deleteCount === "number" ? deleteCount : arr.length - start;
  const delArr = arr.slice(start, start + deleteCount);
  const otherArr = arr.slice(start + deleteCount);
  arr.length = start;
  for (let i = 0; i < args.length; i++) {
    arr.push(args[i]);
  }
  for (let i = 0; i < otherArr.length; i++) {
    arr.push(otherArr[i]);
  }
  return delArr;
}
```
### concat
``` javascript
function concat (arr, ...values) {
  const newArr = [];
  for (let i = 0; i < arr.length; i++) {
    newArr.push(arr[i]);
  }
  for (let i = 0; i < values.length; i++) {
    if (values[i] instanceof Array) {
      for (let j = 0; j < values[i].length; j++) {
        newArr.push(values[i][j]);
      }
    } else {
      newArr.push(values[i]);
    }
  }
  return newArr;
}
```
## apply、call、bind
### apply
``` javascript
function apply (fn, context, args) {
  const self = context ? Object(context) : window;
  args = args || [];
  const key = Symbol('key');
  self[key] = fn;
  const res = self[key](...args);
  delete self[key];
  return res;
}
```
### call
``` javascript
function call (fn, context, ...args) {
  const self = context ? Object(context) : window;
  const key = Symbol('key');
  self[key] = fn;
  const res = self[key](...args);
  delete self[key];
  return res;
}
```
### bind
``` javascript
function bind (fn, context, ...args) {
  const self = context ? Object(context) : window;
  if (args.length === 1 && args[0] instanceof Array) {
    args = args[0];
  }
  const key = Symbol('key');
  self[key] = fn;
  return function(...args1){
    self[key](...args, ...args1);
  };
}
```
## Promise
[链接](./Promise.md)

## 数字转中文
```javascript
function numToChinese(num) {
  const numList = [
    "零",
    "一",
    "二",
    "三",
    "四",
    "五",
    "六",
    "七",
    "八",
    "九",
  ];
  if (num < 10) {
    return numList[num];
  } else if (num < 20) {
    return `十${num % 10 > 0 ? numList[num % 10] : ""}`;
  } else if (num < 100) {
    return `${numList[Math.floor(num / 10)]}十${
      num % 10 > 0 ? numList[num % 10] : ""
    }`;
  } else if (num < 1000) {
    return `${numList[Math.floor(num / 100)]}百${
      num % 100 > 10
        ? numToChinese(num % 100)
        : num % 100 === 10
        ? "一十"
        : ("零" + numList[num % 100]).replace(/零{2,}/, '')
    }`;
  } else if (num < 10000) {
    return `${numList[Math.floor(num / 1000)]}千${
      num % 1000 > 100
        ? numToChinese(num % 1000)
        : ("零" + numToChinese(num % 1000)).replace(/零{2,}/, '')
    }`;
  } else if (num < 100000000) {
    return `${numToChinese(Math.floor(num / 10000))}万${
      num % 10000 > 1000
        ? numToChinese(num % 10000)
        : ("零" + numToChinese(num % 10000)).replace(/零{2,}/, '')
    }`;
  } else {
    return `${numToChinese(Math.floor(num / 100000000))}亿${
      num % 100000000 > 10000000
        ? numToChinese(num % 100000000)
        : ("零" + numToChinese(num % 100000000)).replace(/零{2,}/, '')
    }`;
  }
}
```