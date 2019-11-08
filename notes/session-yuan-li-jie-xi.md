# Session 原理解析

Session存在的意义，估计每个做web开发的人都是了解的，就为了解决HTTP是无状态协议所带来的问题，不多说了。这里主要想说的是服务端与客户端是如何利用session进行交互的。

## 工作流程

先看下面这幅流程图：

![](https://raw.githubusercontent.com/haxianhe/pic/master/image/20191108194847.png)

当用户第一次访问站点时，PHP会用session\_start\(\)函数为用户创建一个session ID，这就是针对这个用户的唯一标识，每一个访问的用户都会得到一个自己独有的session ID，这个session ID会存放在响应头里的cookie中，之后发送给客户端。这样客户端就会拥有一个该站点给他的session ID。

当用户第二次访问该站点时，浏览器会带着本地存放的cookie（里面存有上次得到的session ID）随着请求一起发送到服务器，服务端接到请求后会检测是否有session ID，如果有就会找到响应的session文件，把其中的信息读取出来；如果没有就跟第一次一样再创建个新的。

通常站点的退出功能，实际上就是调用一下session\_destroy\(\)函数\(也有可能更复杂些\)，把该用户的session文件删除，再把用户的cookie清除。这样客户端和服务端就算没有联系了。

图中的红框部分就是一次完整的HTTP请求，因为HTTP是无状态的，所以一次请求完成后客户端和服务端就不再有任何关系了，谁也不认识谁。但由于一些需要（如保持登录状态等），必须让服务端和客户端保持联系，session ID就成了这种联系的媒介了。

## 客户端的工作

通过上面的分析我们可以知道session实际上是依赖与cookie的，当用户访问某一站点时，浏览器会根据用户访问的站点自动搜索可用的cookie，如果有可用的就随着请求一起发送到了服务端。每次接收到服务端的响应时又会更新本地的cookie信息。

当然也可以用GET方式来传递session ID，但不推荐用GET，这样不安全。

实际工程中都是通过将多个字段（一般包含用户ID，sessionId等）编码生成Cookie，每次访问接口的时候通过解码Cookie拿到sessionId的方式工作的。

## 服务端的工作

由上面的流程图可以看到，服务端实际上是把产生的一些数据存放在了session文件中，该文件的名字就是”sess“加上session ID，这些文件的存放位置就是phpinfo\(\)查到的session.savepath值。

![](https://raw.githubusercontent.com/haxianhe/pic/master/image/20191108195320.png)

由上图我们可以很清楚的看到，服务端和客户端保存着同样的session ID信息，这就是两者保持联系的钥匙。

## 反面影响

有好处必然也有坏处，session带来的最主要问题就是对性能的影响，可以想象一下，对于一个千万用户级的web站点，如果每个用户都保存session文件，那每次用户访问光寻找相应的session文件就要耗掉不少系统资源的。所以这时就要对session的存储做一些自定义的设定了，如分目录或哈希等等。除了保存到session文件，也可以抛弃PHP自带的session功能，自己实现session，将session信息存放到数据库当中，这样做最好对数据库进行一下缓存的设置了，不然对上千万的数据进行太频繁的检索，也是蛮耗资源的。

实际工作中都是通过缓存来进行session校验的。

## 定时清除

客户端和服务端的这种联系必然是需要有时间的规定的，所以需要定期清除session。这个问题就需要在两方面考虑了，一个是清除服务端session文件，一个是清除客户端的cookie信息，因为两者都各保存着一半的信息。

PHP GC进程可以扫描session存放目录清除session文件，但这个进程是特别耗资源的，所以PHP默认是1%的几率在一个session启动时去清理一次过期的session，所以并不是说一个用户session过期了，他对应的session文件就马上被清除，99%的几率是没被清除的。这就需要我们程序员自己动手了。可以在session信息中存放一个过期时间，值为用户最后一次访问的时间。当用户一访问，就用当前时间减去上次访问时间看是否超时，如果超时了就删除相应session文件，并设置cookie的Expires属性为负值，使其客户端的cookie信息也过期，这样浏览器就自动把它删掉了。

## PHP关于Session的常用函数

* **session\_start\(\)**： 启动session，这个没什么说的了。根据session ID打开session文件，如果没有session ID就创建一个ID和对应的session文件
* **$SESSION\[\]**：存放用户信息的全局数组，session文件中除了存放$SESSION中的数据实际也会存放其他的信息，如id等
* **sessionunset\(\)**： 清空$SESSION数组，它是把数组里的值清空了，而$SESSION这个变量还是存在的，和unset\($SESSION\)是完全不同的概念
* **sessioncommit\(\)**：提交session数据并结束session，把$SESSION数据写到文件里并结束session。实际上当一个页面执行结束后，php会自动执行与这个函数相同的操作。所以这个函数也很少能用上
* **session\_destroy\(\)**：注销session，这个就是关闭session，并删除掉相应的session文件了。切断了客户端和服务端的联系。

