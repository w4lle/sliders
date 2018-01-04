# 协程

author：王令龙

## 协程概念

协程 Coroutine，字面意思线程协作，完成异步任务。

可以理解为使用线程挂起替代阻塞，无阻塞的异步编写方式。

## 同步 or 异步

var a = a +b 

var result = loadImg()

## 处理异步的方式

### 回调/监听 

setOnclickListner(new OnclickListner() {
    //do something
})

### 通知 

registe(this)
onEvent(event) {
    //do something
}

post(event)

### Promise 对象 / Rxjava

### 回调陷阱：

```java
readFile(fileA, function (err, data) {
  loadImg(img, function (err, data) {
    // ...
  });
});
```

### Promise:

```
readFile(fileA)
.then(function(data){
  console.log(data.toString());
})
.then(function(){
  return readFile(fileB);
})
```

### Rxjava:

```java
Obsevable<Response> ob = readFile();
.map(new Func () {
    // do something
})
ob.onNext(new Func () {
    //do something
})
.then(xxx)
.onComplete();
```

## 进程 线程 协程

进程：切换耗资源，效率低，通信复杂
线程：切换线程需要资源一般，效率一般，控制线程数
协程：切换协程资源消耗小，效率高，写起来像同步，但是并不是适合所有场景

## 举个🌰

第一步，协程A开始执行。

第二步，协程A执行到一半，进入暂停，执行权转移到协程B。

第三步，（一段时间后）协程B交还执行权。

第四步，协程A恢复执行。


### js 的 async 实现协程：

```java
var asyncReadFile = async function (){
  var f1 = await readFile('/etc/fstab');
  var f2 = await readFile('/etc/shells');
  console.log(f1.toString());
  console.log(f2.toString());
};
```

### 生产者消费者问题：

```java
import time
def consumer():
    r = ''
    while True:
        n = yield r
        if not n:
            return
        print('[CONSUMER] Consuming %s...' % n)
        time.sleep(1)
        r = '200 OK'

def produce(c):
    c.next()
    n = 0
    while n < 5:
        n = n + 1
        print('[PRODUCER] Producing %s...' % n)
        r = c.send(n)
        print('[PRODUCER] Consumer return: %s' % r)
    c.close()

if __name__=='__main__':
    c = consumer()
    produce(c)
```

### 线程切换对比

### 使用协程

```java
println("Coroutines: start")
val jobs = List(100_000) {
    // 创建新的coroutine
    launch(CommonPool) {
        // 挂起当前上下文而非阻塞1000ms
        delay(1000L)
        println("." + Thread.currentThread().name)
    }
}
jobs.forEach { it.join() }
println("Coroutines: end")
println("No Coroutines: start")
```

### 使用阻塞

```java
val noCoroutinesJobs = List(100_000) {
    // 创建新的线程
    thread {
        // 阻塞
        Thread.sleep(1000L)
        println("." + Thread.currentThread().name)
    }
}
noCoroutinesJobs.forEach { it.join() }
println("No Coroutines: end")
```

在Nexus6P上：使用协程的大约在8s左右完成所有输出；而不使用协程的大约2min才完成所有输出

