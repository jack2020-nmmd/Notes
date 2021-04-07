## Promise 提供统一的 API，各种异步操作都可以用同样的方法进行处理。
## 它有两个特点
    (1)对象状态不收外界影响,它有3个状态,peding是进行中,fulfilled是已经成功和reject是已拒绝.只有当异步操作完成之后才可以确定状态,状态一经确定就无法更改了.该函数的两个参数分别是resolve和reject。它们是两个函数，由 JavaScript 引擎提供，不用自己部署,promise接收的参数是同步执行的
    (2)当状态一经确定即不可再更改,只能从peding到fulfilled或者reject,这个时候状态确定了就不会改变了,状态确定就叫做resolve(已定型),状态只能改变一次  
        如果调用resolve函数和reject函数时带有参数，那么它们的参数会被传递给回调函数作为参数,比如then里面函数的参数
        reject函数的参数通常是Error对象的实例，表示抛出的错误；resolve函数的参数除了正常的值以外，还可能是另一个 Promise 实例，比如像下面这样。
```css
    const p1 = new Promise(function (resolve, reject) {
    // ...
    });

    const p2 = new Promise(function (resolve, reject) {
    // ...
    resolve(p1);
    })
```
## 基本用法
    Promise是一个构造函数,用来生产实例对象
```css
    const promise = new Promise(function(resolve, reject) {
    // ... some code

    if (/* 异步操作成功 */){
        resolve(value);
    } else {
        reject(error);
    }
    });
    它接受一个函数作为参数,该函数接受两个参数,他们也是函数,当执行resolve时,状态由未定义变为成功,当异步参数执行成功时并将结果作为参数传递出去,reject是将它变为失败,并将失败报出的错误作为参数传递出去
    当实例生成后,可以使用then分别指定resolved状态和reject状态的回调函数
    promise.then(function(value) {
        // success
        }, function(error) {
        // failure
    });then可以指定成功的或者两个都指定(两个都是可选的).
    一般执行resolve或者reject后,promise的使命就结束了,后续的操作应该放在then中,所以在resolve他们前面加return,这样就不会有意外发生.
```
## then方法
        由于实例可以使用then方法,所以then方法是定义在它的原型对象上的,then返回的是一个新的promise实例,可以用then对其实现链式调用,并且每一个的then调用都会等前一个then返回状态才执行.
        如果是reject,可以进行错误穿透(冒泡),直接传递到捕抓错误的函数
## catch方法
        它是用于执行错误的函数,一般then就写正确的函数,catch就写错误的函数,不要正确和错误都写在then中,try...catch(它们是同步的)在try里面执行,如果报错了会把它放到catch中,其实就是相当于reject函数.
        如果有错误没有使用catch捕抓,这个错误会被promise吃掉,错误不会报出控制台影响后面程序执行
        catch也会执行完也会返回一个promise对象,如果它完成程序执行并且没有报错或者指定reject(),那么它的状态是成功
## promise.finally(function())
    它是无论你的状态是什么,它最后都会执行这个回调函数,并且这个回调函数不会接收任何参数,并且它还会返原来的值(即传给resolve或者reject的值)
## Promise.all([p1,p2])
    只要一个参数不是resolve,那么它的状态就是reject,
##   Promise.race([p1,p2])
    只要p1,p2中有一个状态发生改变,那么race的状态就会变成它一样
## Promise.allSettled([p1,p2])
    它是等所有promise都确定状态才确定状态,并且返回状态是resolve,该数组的每个成员都是一个对象，对应传入Promise.allSettled()的两个 Promise 实例。每个对象都有status属性，该属性的值只可能是字符串fulfilled或字符串rejected。fulfilled时，对象有value属性，rejected时有reason属性，对应两种状态的返回值。
```css
    const promises = [ fetch('index.html'), fetch('https://does-not-exist/') ];
    const results = await Promise.allSettled(promises);

    // 过滤出成功的请求
    const successfulPromises = results.filter(p => p.status === 'fulfilled');

    // 过滤出失败的请求，并输出原因
    const errors = results
    .filter(p => p.status === 'rejected')
    .map(p => p.reason);
    这个方法可以用来判断是不是所有请求都结束了
```
## Promise.resolve()
    将现有对象转为 Promise 对象，Promise.resolve()方法就起到这个作用
```css
    const jsPromise = Promise.resolve($.ajax('/whatever.json'));
    上面代码将 jQuery 生成的deferred对象，转为一个新的 Promise 对象。
    Promise.resolve('foo')
    // 等价于
    new Promise(resolve => resolve('foo'))

    注意:立即resolve()的 Promise 对象，是在本轮“事件循环”（event loop）的结束时执行，而不是在下一轮“事件循环”的开始时。就是和promise的then一样,在setTimeout之前.

```
## Promise.reject()
    和上面的一样,但是它的状态是reject
## Promise.try()
    实际开发中，经常遇到一种情况：不知道或者不想区分，函数f是同步函数还是异步操作，但是想用 Promise 来处理它,如果是异步的,会执行then函数,如果是同步直接返回函数结果。
```css
    Promise.try(() => database.users.get({id: userId}))
    .then(...)
    .catch(...)
```