# Operating System

# Leraning Operating System
 ![Harmony OS](/HarmonyOS.png)


## 一、写在前面：

​	我计划在这个暑假学习有关操作系统的知识。在查阅了有关的资料后我打算跟随书籍进行学习。我选择的书籍是国外的教材的译本，原书名Operating System-Three Easy Pieces(中名是：操作系统导论）。如果你的英语水平过关的话，那么可以到下面的连接下载它的电子原版。

> https://github.com/csukuangfj/Operating-Systems-Three-Easy-Pieces-all-in-one-pdf



## 二、为什么要根据这本书学？

-  视频   OR  书籍 

​	其实我在决定之前找了很多的问答和视频。但操作系统的学习本身并不是一个有趣的过程。所以在我看来找到一个能激发求知欲的视频或是一本书显然是至关重要的。在购买译本之前我勉强的看了一些原本的电子版。发现确实如推荐的人说的一般，行文较为有趣。这里引用知乎上的一篇回答：

> https://www.zhihu.com/question/31863104/answer/54334367

* 为什么不选择经典的黑皮书呢？

  我觉得黑皮书系列虽然硬核，但是并不太适合初学者。

## 三、有和打算？

* 顺带干点别的

​	操作系统导论一书除了语言较为轻松意外。还会根据要讲解的内容给出对应的c语言代码。由于我的c语言只停留在指针的指针、数组的指针、指针的数组这个大圈中。所以我并不打算抄书上的代码。但不跟着实现的话又觉得浪费了作者的苦心。所以我打算另寻它路。

* Go语言

Go语言是类c语言。而且和C一样，它也具有**指针**的操作。区别是Go语言的指针较C语言要简单很多。

```go
const (
   sLoop = 1000

   BLoop = 100000
)


type google struct {}

// 此处可以在worker中开启两个goroutine，但是那样做似乎就用不了context。
// 如果你想在worker中开启两个goroutine的话，那么你可以使用sync.WaitGroup包。
// 或者在main函数中创建一个channel，等待两个goroutine结束时发送信号。如果接收到两个信号则表明都完成了任务。
func (g google) worker(count *int, ctx context.Context, loop int) context.Context{
   ctx, cancel := context.WithCancel(ctx)
   go func() {
      for i := 0; i < loop; i++ {
         *count++
      }
      cancel()
   }()
   return  ctx
}

func main() {
   g1 := google{}
   g2 := google{}
   var result1 int
   ctx1 := g1.worker(&result1, context.Background(), sLoop)
   ctx2 := g2.worker(&result1, context.Background(), sLoop)
   <- ctx1.Done()
   <- ctx2.Done()
   fmt.Println("Small loop's result: " + strconv.Itoa(result1))

   var result int
   ctx1 = g1.worker(&result, context.Background(), BLoop)
   ctx2 = g2.worker(&result, context.Background(), BLoop)
   <- ctx1.Done()
   <- ctx2.Done()
   fmt.Println("Large loop's result: " + strconv.Itoa(result))
}
```

输出：

`Small loop's result: 2000`
`Large loop's result: 142830`

> 上面便是我仿照着书里的例子写的小Demo。目的是测试并发伴随的问题。（代码并不简洁，甚至可以算得上糟糕。）

## 四、坚持

​	行文的幽默并不能覆盖这是一个严肃的、具有逻辑性的专业知识。加之我加入了一些项目的工作。所以我必须保证在完成项目的同时不落下操作系统学习的进度。

