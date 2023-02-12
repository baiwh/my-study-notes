
相当于一个容器，储存着一个异步操作的结果。

三种状态：

* pending 进行中、fulfilled 已成功、rejected 已失败

特点：

* 状态不受外界影响。只有异步操作的结果可以决定状态。
* 从 pending 变为其他两种状态后，不会再次发生改变。（此时称为 resolved 已定型）
* 一但建立就会立即执行，中途无法取消。

缺点：

* 一但建立就会立即执行，中途无法取消。
* 如果不设置回调函数，promise 内部抛出的错误不会反映到外部。
* 处于 pending 状态时，无法得知目前进展。

基本用法：

* Promise 对象是一个构造函数，可以用new Promise来生成实例
* Promise 构造函数接收一个函数作为参数，该函数的两个参数分别是 resolve 和 reject，这两个参数也是函数。
* resolve 函数：异步操作成功时调用，并将异步操作的结果作为参数传递出去。此时状态变为成功 resolved
* reject 函数：异步操作失败时调用，并将异步操作的报错作为参数传递出去。此时状态变为失败 rejected
* 可以用 .then() 分别指定 resolved 和 rejected 的回调函数。.then()接受两个函数作为参数，第一个是resolved 时调用的，第二个可选是 rejected 时调用的。Promise 实例状态改变后会触发.then()里的回调参数。
* Promise 实例中，如果调动 resolve 函数和 reject 函数时，带有参数，参数会被传递给回调函数。
* 如果一个 Promise 实例中 resolve 函数的参数是另一个 Promise 实例，参数实例的状态决定了另一个实例的状态。
* Promise 实例中调动 resolve() 并不会终结 Promise 的参数函数的执行，但最好将后边的内容写到 .then() 里
* 在 resolve 语句后边再抛出错误，可能不会被捕获。因为状态一旦改变，就会冻结，等于没有抛出
* 1Promise 内部的错误不会影响到外部的代码。

Promise 实例方法：

* .then()
  * 用于给 Promise 实例添加状态改变时的回调函数。
  * 返回一个新的 Promise 实例，可采用链式写法
* .catch()
  * 是 .then(null,rejection) 或 .then(undefined,rejection) 的别名
  * 用于指定发生错误时的回调函数。
  * .catch() 也有可能会抛错，再加一个 .cahch() 可以捕获前一个抛错
* .then()指定的回调函数如果抛错，也会被 .catch() 捕获。
  * 错误具有“冒泡”性质，会一直向后传递，直到被捕获为止。
  * 建议使用 catch() ，而不使用 then() 的第二个参数。
  * 会返回一个新的 Promise 实例，因此可以接着链式调用 .then()
  * 链式调用时如果没有报错，会跳过 .catch()
* .finally()        es2018
  * 不管 Promise 实例最后状态如何，都会执行。
  * 不接受任何参数，里边的操作与状态无关，不依赖 Promise 的执行结果。
  * 本质上是 .then() 的特例，代表无论失败和成功都要执行的操作
  * 总会返回原来的值

方法：
1、.all()

* const p = Promise.all([p1,p2,p3])
* 用于将多个 Promise 实例包装成一个新的实例
* 接收一个Promise 实例的数组作为参数（如果不是 Promise 实例，会被转为 Promise 实例。如果不是数组，biubiu 具有 Iterator 接口，切返回的每个成员都是 Promise 实例）
* p 的状态由p1、p2、p3 决定：只有状态全部变成 fulfilled，p 才会变成 fulfilled，此时p1、p2、p3的返回值组成一个数组，传递给 p 的回调函数。只要有一个变为 rejected，p 就会变成 rejected，此时第一个变为 rejected 的实例的返回值会传递给 p 的回调函数（但如果参数自己定义了.catch() 就不会触发 Promise.all() 的 .catch() 了，此时 p 的状态会是 resolved）。

2、.race()

* const p = Promise.race([p1,p2,p3])
* 用于将多个 Promise 实例包装成一个新的实例
* 只要p1、p2、p3有一个实例状态改变，p 就会跟着改变，那个最先改变的实例返回值会传递给 p 的回调函数。

3、.allSettled()         es2020

* Promise.allSettled(promises)
* 接收一组 Promise 实例作为参数，包装成一个新的 Promise 实例。
* 只有参数实例都返回成功/失败，包装实例才会结束
* 返回的新 Promise 实例结束后状态总是 fulfilled，不会变成 rejected。
* 返回值是一个 由每个成员的结果组成的 数组。
* 适用于只关心异步操作是否结束，不关心结果的情况。

4、.any()        提案

* Promise.any(promises).then()
* 接收一组 Promise 实例作为参数，包装成一个新的 Promise 实例。
* 只要参数实例有一个变成 fulfilled，包装实例就会变成 fulfilled；如果所有参数实例变成 rejected，包装实例状态就会变成 rejected。

5、.resolve()

* 将现有对象转换为 Promise 对象

6、.reject()

* 返回一个新的 Promise 实例，该实例状态为 rejected

7、.try()       提案

* Promise.try(()=>{}).then().catch()
* 模拟 try-catch 代码块。
* 不知道或不想区分函数是同步的还是异步的，但是想用 Promise 来处理。

类promise实现。
异步promise。原理。实现。
promise，和callback比较。具体应用。
