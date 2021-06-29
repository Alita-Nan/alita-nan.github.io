# 关于Docker的踩坑


# Before

关于Docker的学习我是在官方给出的入门教程中进行的。我非常推荐新手跟着官方给出的教程学习。

一是质量有得保证。二是官方推出的入门教程较其他更为简单且有趣，不易产生劝退的念头。

具体的教程在Docker的官方网站上即可找到。(参照下面的链接)

> https://www.docker.com/101-tutorial

**需要注意的是我是用的Docker Desktop进行docker的一系列操作。而该教程也是面向Docker Desktop的。**

# 关于教程中的“坑”

**需要提前声明的是，此处的坑并不是指官方的教程有问题，而是指新手在跟随教程走时可能犯的错误。**

## 一、Docker Build 好慢

我遇到的第一个坑是在教程的第三部分。

{{< figure src="/Docker-Directory" title=docker教程目录" >}}

这个章节的主题是***Our Application***。教程在给出一个与既定程序匹配的***dockerfile***后会引导你用

`docker build -t getting-started .`

命令构建容器。进行到这里一切都非常的顺利，但是构建的过程却异常的缓慢。

![joke-dockerBuild](/joke-dockerBuild.JPG)

由于这是一个基于**Node.js**的App，所以在构建容器时还需要提供相应的环境依赖（这样才能保证程序能运行）。

问题就出在这里，当你敲下回车后会发现它在构建时是从外网上拉取的依赖。而后我在网上查阅资料试图修改dockerfile，让其从国内的镜像网上拉取。但都失败了（失败的原因目前还未可知）。于是我只能忍痛购买一个梯子（先前推荐的那个梯子我一年的免费试用已经到期。）

> 它家的梯子是我在对比了很多我找到的梯子中费用最低的。而且它家的客服非常给力，会远程帮你解决问题。一般来说这个梯子能满足绝大部分的“跨海”需求。

在搭好梯子之后，重新敲下上面的代码你会发现它的速度快了很多。原本需要半小时（可能更长）缩短为几分钟！

* 这里需要补充的一点是我一开始怀疑是教程给出的dockerfile有问题。毕竟其原文中写道***If you've created Dockerfiles before, you might see a few flaws in the Dockerfile below.*** 但是在解决了问题以及跟着教程前进之后我个人觉得所谓的缺陷可能是它没有把运行所需的依赖和程序本身分离开来。在教程的Multi-Contanier Apps中写道***In general, each container should do one thing and do it well.*** 即一个容器需要专注于一件事。

## 二、Bind Mounts需要注意的事

随着教程的深入我来到了***Using Bind Mounts***。我查阅了一下，这个的中文翻译似乎都是直译，即绑定挂载。这个章节是教你如何在不重新构建容器的情况下修改代码并让容器运行修改后的代码。

这个章节中教程给出了一段指令：

`docker run -dp 3000:3000 \    `

`-w /app -v "$(pwd):/app" \   `

` node:12-alpine \    ` 

`sh -c "yarn install && yarn run dev"`

* `-w /app`的意义在于指定命令针对的工作的目录。(原文是：***sets the "working directory" or the current directory that the command will run from***)

* `-v "$(pwd):/app"`的意思是将主机中的容器内的当前目录绑定到/app的工作目录下。（原文是：***bind mount the current directory from the host in the container into the/app directory***)

  

  如果你不在app路径下执行此命令的话，你会发现容器并不能正常运行。在Desktop中点击启动后一会就会自动推出。

  所以这里笔者建议你使用powerShell，然后在powershell中移动到app所在的位置再执行此命令。

  

  ## 三、命令的“误会”

  > 这是一个小乌龙，权当记录。

  解决了上述问题之后来到了***Muti-Contaniner Apps***一章中

  这一章中教程给出了一段代码供操作容器内运行的Mysql数据库

  `docker exec -it <mysql-container-id> mysql -p`

  这段命令是在容器中连接mysql数据库。但值得注意的是，代码中的**尖括号**应该是起隔离的作用。在实际操作中**不能**输入这对尖括号！！！！！！


