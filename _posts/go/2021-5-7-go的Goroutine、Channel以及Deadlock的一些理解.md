---
layout: post
title: 'go的Goroutine、Channel以及Deadlock的一些理解'
date: 2021-05-07
author: hebicheng
header-img: 'img/in-post/go_goroutine_channel_deadlock/20210507155212.png'
tags: [go]
---



# go 协程(Goroutine)

Go 协程是与其他函数或方法一起并发运行的函数或方法。Go 协程可以看作是轻量级线程。与线程相比，创建一个 Go 协程的成本很小。因此在 Go 应用中，常常会看到有数以千计的 Go 协程并发地运行。

在go语言中启动一个协程很简单：

```go
package main

import (  
    "fmt"
    "time"
)

func hello() {  
    fmt.Println("Hello world goroutine")
}
func main() {  
    go hello()
    time.Sleep(1 * time.Second)
    fmt.Println("main function")
}
```

# go 信道(Channel)
信道可以想像成 Go 协程之间通信的管道。如同管道中的水会从一端流到另一端，通过使用信道，数据也可以从一端发送，在另一端接收。
```go
package main

import (  
    "fmt"
)

func hello(done chan bool) {  
    fmt.Println("Hello world goroutine")
    done <- true
}
func main() {  
    done := make(chan bool)
    go hello(done)
    <-done
    fmt.Println("main function")
}
```
在上述程序里，我们在第 12 行创建了一个 bool 类型的信道 done，并把 done 作为参数传递给了 hello 协程。在第 14 行，我们通过信道 done 接收数据。这一行代码发生了阻塞，除非有协程向 done 写入数据，否则程序不会跳到下一行代码。

注意到在14行，通过信道接收数据时，发生了阻塞。实际上在go中信道在发送与接收默认是阻塞的。当把数据发送到信道时，程序控制会在发送数据的语句处发生阻塞，直到有其它 Go 协程从信道读取到数据，才会解除阻塞。与此类似，当读取信道的数据时，如果没有其它的协程把数据写入到这个信道，那么读取过程就会一直阻塞着(缓冲信道在缓冲已满的状态下才会阻塞)。

# 死锁(Deadlock)
什么是死锁呢？先看下面代码：
```go
package main

func main() {  
    done := make(chan bool)
    done <- false
}
```
这段程序并不能正常运行，上面我们说过，信道在发送和接收数据时会阻塞，所以在第五行，当我们尝试将`false`写入信道时，没有别的协程来将信道中的数据读走，所以程序会一直阻塞在第五行，造成死锁。控制台报错如下：
![图片](/img/in-post/go_goroutine_channel_deadlock/20210507161942.png)

再看下面代码：
```go
package main

import (
    "fmt"
    "time"
)

func hello(done chan bool) {
    fmt.Println("Hello world goroutine")
    done <- true
}

func main() {
    done := make(chan bool)
    go hello(done)
    for{
        time.Sleep(1*time.Second)
    }
}
```
(for循环只是体现程序不是因为主程序退出而使整个程序退出)  
以上代码会不会发生死锁呢？第10行代码也尝试向信道写入数据，并且没有其他协程来读取。 

不会。为什么呢？难道是因为阻塞发生在非main的协程里就不会出现死锁？继续看下面代码：
```go
package main

import (
    "fmt"
    "time"
)

func hello(done chan bool) {
    fmt.Println("Hello world goroutine")
    for{
        time.Sleep(1*time.Second)
    }
}

func main() {
    done := make(chan bool)
    go hello(done)
    <-done
}
```
实际上，这样写程序也不会死锁，即使现在阻塞是在main协程中发生的。

稍作修改，将`hello`函数改为以下代码：

```go
func hello(done chan bool) {
    fmt.Println("Hello world goroutine")
    <-done
    for{
        time.Sleep(1*time.Second)
    }
}
```

现在再运行程序就会触发panic，提示程序程序发生死锁而报错。

综上总结一下，并不是说在main协程或非main协程中出现阻塞就会出现死锁，阻塞是死锁发生的必要条件而不是充分条件。只有当程序中的所有协程都出现阻塞时才会死锁。实际上这个结论在我们第一次看到死锁报错的时候就能得出：
![图片](/img/in-post/go_goroutine_channel_deadlock/20210507161942.png)

`fatal error: all goroutines are asleep - deadlock!` 错误信息以及明确的说明了：所有的goroutines都阻塞了，所以程序死锁。


# 参考
* [https://studygolang.com/articles/12342](https://studygolang.com/articles/12342)
* [https://studygolang.com/articles/12402](https://studygolang.com/articles/12402)


> 原创作品，转载请注明来源 [https://hebicheng.github.io](https://hebicheng.github.io)  
