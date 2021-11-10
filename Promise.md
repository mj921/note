## Promise
``` javascript
function MyPromise (executor) {
  const resolve = function(result){
    this.promiseState = "fulfilled";
    this.result = result;
    this.handleList.forEach(handle => {
      handle(this.promiseState, result);
    })
  }
  const reject = function(error) {
    this.promiseState = "rejected";
    this.result = error;
    this.handleList.forEach(handle => {
      handle(this.promiseState, error);
    })
  }
  this.handleList = []; // 处理队列
  this.result;
  this.promiseState = "pending";
  executor(resolve.bind(this), reject.bind(this));
}
MyPromise.prototype.then = function(onResolved, onRejected) {
  if (this.promiseState === "pending") {
    return new MyPromise((resolve, reject) => {
      const handle = (promiseState, result) => {
        if (promiseState === "fulfilled") {
          try {
            if (onResolved) {
              resolve(onResolved(result));
            } else {
              resolve(result);
            }
          } catch(e) {
            reject(e);
          }
        } else {
          try {
            if (onRejected) {
              resolve(onRejected(result));
            } else {
              reject(result);
            }
          } catch(e) {
            reject(e);
          }
        }
      }
      this.handleList.push(handle)
    })
  } else if (this.promiseState === "fulfilled") {
      try {
        if (onResolved) {
          return MyPromise.resolve(onResolved(this.result));
        } else {
          MyPromise.resolve(this.result);
        }
      } catch(e) {
        return MyPromise.reject(e);
      }
      
  } else {
      try {
        if (onRejected) {
          return MyPromise.resolve(onRejected(this.result));
        } else {
          return MyPromise.reject(this.result);
        }
      } catch(e) {
        return MyPromise.reject(e);
      }
  }
}
MyPromise.prototype.catch = function(onRejected) {
  if (this.promiseState === "pending") {
    return new MyPromise((resolve, reject) => {
      const handle = (promiseState, result) => {
        if (promiseState === "rejected") {
          try {
            if (onRejected) {
              resolve(onRejected(result));
            } else {
              reject(result);
            }
          } catch(e) {
            reject(e);
          }
        } else {
          resolve();
        }
      }
      this.handleList.push(handle)
    })
  } else if (this.promiseState === "rejected") {
      try {
        if (onRejected) {
          return MyPromise.resolve(onRejected(this.result));
        } else {
          return MyPromise.reject(result);
        }
      } catch(e) {
        return MyPromise.reject(e);
      }
  } else {
      return MyPromise.resolve(this.result);
  }
}
MyPromise.prototype.finally = function(onFinally) {
  if (this.promiseState === "pending") {
    return new MyPromise((resolve, reject) => {
      const handle = (promiseState, result) => {
        try {
          onFinally()
          resolve(result);
        } catch(e) {
          reject(e);
        }
      }
      this.handleList.push(handle)
    })
  } else {
    onFinally();
    return MyPromise.resolve(this.result);
  }
}
MyPromise.resolve = function(result) {
  return new MyPromise(function(resolve) {
    resolve(result);
  })
}
MyPromise.reject = function(error) {
  return new MyPromise(function(resolve, reject) {
    reject(error);
  })
}
MyPromise.all = function(promises) {
  return new MyPromise((resolve, reject) => {
    const results = [];
    let count = 0;
    for (let i = 0; i < promises.length; i++) {
      promises[i].then(result => {
        results[i] = result;
        count++;
        if (count === promises.length) {
          resolve(results);
        }
      }, error => {
        reject(error);
      })
    }
  })
}
MyPromise.allSettled = function(promises) {
  return new MyPromise((resolve, reject) => {
    const results = [];
    let count = 0;
    for (let i = 0; i < promises.length; i++) {
      promises[i].then(result => {
        results[i] = { status: "fulfilled", value: result };
        count++;
        if (count === promises.length) {
          resolve(results);
        }
      }, error => {
        results[i] = { status: "rejected", reason: error };
        count++;
        if (count === promises.length) {
          resolve(results);
        }
      })
    }
  })
}
MyPromise.any = function(promises) {
  return new MyPromise((resolve, reject) => {
    let count = 0;
    for (let i = 0; i < promises.length; i++) {
      promises[i].then(result => {
        resolve(result);
      }, error => {
        count++;
        if (count === promises.length) {
          reject(new Error("All promises were rejected"));
        }
      })
    }
  })
}
MyPromise.race = function(promises) {
  return new MyPromise((resolve, reject) => {
    let count = 0;
    for (let i = 0; i < promises.length; i++) {
      promises[i].then(result => {
        resolve(result);
      }, error => {
        reject(error);
      })
    }
  })
}
```