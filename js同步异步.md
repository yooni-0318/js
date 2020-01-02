# js同步异步
1. js是单线程，有任务队列
```
console.log(1);
    setTimeout(function () {
        console.log(2);
    }, 1000);
    console.log(3);
    console.log(4);
```
上面的代码中，我们很容易知道，打印的顺序是1，3，4，2。因为你会想到，要等一秒之后再打印2。<br/>
可如果我把延时的时间从1000改成0：
```
console.log(1);
    setTimeout(function () {
        console.log(2);
    }, 0);
    console.log(3);
    console.log(4);
```
上方代码中，打印的顺序仍然是1，3，4，2。

2. 执行顺序：
* 先执行同步任务
* 遇到异步任务挂起
* 执行同步任务
* 全部的同步任务执行完毕之后，再执行异步任务。
```
    console.log('A');

    setTimeout(function () {
        console.log('B');
    })

    while (1) {

    }
```
上方代码的打印结果是A。<br/>
因为while是同步任务，setTimeout是异步任务，所以还是那句话：**如果同步任务没有执行完，队列里的异步任务是不会执行的**。

3. 任务队列：
  只要主线程空了，就会去读取任务队列。
  异步任务不进入主线程，但是进入任务队列。
  
4. event lop： 事件循环
  主线程从任务队列中读取事件，这个过程是循环不断的，所以整个的运行机制又称为事件循环。

5. 前端使用异步的场景
**什么时候需要等待，就什么时候用异步**
* 定时任务：setTimeout（定时炸弹）、setInterval（循环执行）
* 网络请求：ajax请求、动态<img>加载
* 事件绑定（比如说，按钮绑定点击事件之后，用户爱点不点。我们不可能卡在按钮那里，什么都不做。所以，应该用异步）
* ES6中的Promise<br/>
<br/>
代码举例：

```
    console.log('start');
    var img = document.createElement('img');
    img.onload = function () {
        console.log('loaded');
    }
    img.src = '/xxx.png';
    console.log('end');
```
结果：
先打印start，<br/>
然后执行img.src = '/xxx.png'，<br/>
然后打印end，<br/>
最后打印loaded。<br/>

### 容易答错的题目
```
for (var i = 0; i < 3; i++) {
        setTimeout(function () {
            console.log(i);
        }, 1000);
    }
```
正确的答案是：3,3,3,3。<br/>
分析：for 循环是同步任务，setTimeout是异步任务。for循环每次遍历的时候，遇到settimeout，就先暂留着，等同步任务全部执行完毕（此时，i已经等于3了），再执行异步任务。

参考：[js运行机制：异步和单线程](https://github.com/qianguyihao/Web/blob/master/14-%E5%89%8D%E7%AB%AF%E9%9D%A2%E8%AF%95/09-02.js%E8%BF%90%E8%A1%8C%E6%9C%BA%E5%88%B6%EF%BC%9A%E5%BC%82%E6%AD%A5%E5%92%8C%E5%8D%95%E7%BA%BF%E7%A8%8B.md#%E5%89%8D%E7%AB%AF%E4%BD%BF%E7%94%A8%E5%BC%82%E6%AD%A5%E7%9A%84%E5%9C%BA%E6%99%AF)
