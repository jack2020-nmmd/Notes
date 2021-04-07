## async默认返回一个promise对象
    在async后面可以使用then()方法添加回调函数。当函数执行的时候，一旦遇到await就会先返回，等到异步操作完成，再接着执行函数体内后面的语句。
## 语法
        async函数内部return语句返回的值，会成为then方法回调函数的参数.async函数内部抛出错误，会导致返回的 Promise 对象变为reject状态。抛出的错误对象会被catch方法回调函数接收到。
        async函数返回的 Promise 对象，必须等到内部所有await命令后面的 Promise 对象执行完，才会发生状态改变，除非遇到return语句或者抛出错误。也就是说，只有async函数内部的异步操作执行完，才会执行then方法指定的回调函数。
## await命令
        await命令后面是一个 Promise 对象，返回该对象的结果。如果不是 Promise 对象，就直接返回对应的值。
        如果await后面的是一个thenable对象(即定义了then方法),那么await会将其当作promise对象.
        如果await后面的对象状态变为reject状态,它会被catch方法的回调函数接收到.
        如果多个await一起执行,但一个变为reject的话,后面的await是不会执行的.如果希望异步操作失败，也不要中断后面的异步操作。这时可以将第一个await放在try...catch结构里面，这样不管这个异步操作是否成功，第二个await都会执行
```css
        async function f() {
            try {
                await Promise.reject('出错了');
            } catch(e) {
            }
            return await Promise.resolve('hello world');
        }

        f()
        .then(v => console.log(v))
        /* 另一种方法是await后面的 Promise 对象再跟一个catch方法，处理前面可能出现的错误 */
```
注意:多个await命令后面的异步操作，如果不存在继发关系，最好让它们同时触发
```css
     // 写法一
    let [foo, bar] = await Promise.all([getFoo(), getBar()]);

    // 写法二
    let fooPromise = getFoo();
    let barPromise = getBar();
    letfoo = await fooPromise;
    let bar = await barPromise;
```
## async 函数可以保留运行堆栈。相当于闭包