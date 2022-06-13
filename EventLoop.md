# Event Loop

Event Loop即事件循环， 是指浏览器或Node的一种解决JavaScript单线程运行是不会阻塞的一种机制，也就是我们经常使用的一步的原理。

---

- 任务队列

  所有的任何任务都可以分为同步任务和异步任务，同步任务就是立即执行的任务，同步任务一般会直接进入到主线程中执行；异步任务就是异步执行，比如ajax网络请求，setTimeout等都属于异步任务，异步任务会通过任务队列（Event Queue）的机制来进行协调。

在JavaScript中，任务被分为两种，一种是宏任务（MacroTask）也叫Task，一种叫微任务（MicroTask）也叫Jobs。

- 宏任务

  常见的宏任务有：script（整体代码）、setTimeout、setInterval、setImmediate（Node环境）、I/O。

- 微任务

  常见的微任务有：Promise、async/await、Generator函数、MutationObserver、Process.nextTick（Node环境）。

---

Javsctipt单线程任务被分为同步任务和异步任务，在代码执行过程中，同步任务和异步任务会分别进入不同的环境，同步进入主线程，即主行栈；异步则进入任务队里（Event Queue）。待主线程内的执行完毕后，回去任务队列中读取相应的任务，推入主线程执行。上述过程不断重复就是Event Loop（事件循环）。

---

- 经典面试题

  `console.log('1');
  async function async1() {
      console.log('2');
      await async2();
      console.log('3');
  }
  async function async2() {
      console.log('4');
  }

  process.nextTick(function() {
      console.log('5');
  })

  setTimeout(function() {
      console.log('6');
      process.nextTick(function() {
          console.log('7');
      })
      new Promise(function(resolve) {
          console.log('8');
          resolve();
      }).then(function() {
          console.log('9')
      })
  })

  async1();

  new Promise(function(resolve) {
      console.log('10');
      resolve();
  }).then(function() {
      console.log('11');
  });
  console.log('12');
  //-------------------------------答案-----------------------------
  // 1
  // 2
  // 4
  // 10
  // 12
  // 5
  // 3
  // 11
  // 6
  // 8
  // 7
  // 9

  `