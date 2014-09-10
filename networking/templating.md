# 模板（Templating）

当使用HTML项目时，通常需要使用模板驱动开发。服务器使用模板机制生成代码在服务器端对一个HTML根进行扩展。例如一个照片列表的列表头将使用HTML编码，动态图片链表将会使用模板机制动态生成。通常这也可以使用QML解决，但是仍然有一些问题。

首先，HTML开发者这样做的原因是为了克服HTML后端的限制。在HTML中没有组件模型，动态机制方面不得不使用这些机制或者在客户端边使用javascript编程。很多的JS框架产生（jQuery，dojo，backbone，angular，...）可以用来解决这个问题，把更多的逻辑问题放在使用网络服务连接的客户端浏览器。客户端使用一个web服务的接口（例如JSON服务，或者XML数据服务）与服务器通信。这也适用于QML。

第二个问题是来自QML的组件缓冲。当QML访问一个组件时，缓冲渲染树（render-tree），并且只加载缓冲版本来渲染。磁盘上的修改版本或者远程的修改在没有重新启动客户端时不会被检测到。为了克服这个问题，我们需要跟踪。我们使用URL后缀来加载链接（例如[http://localhost:8080/main.qml#1234](http://localhost:8080/main.qml#1234)），“#1234”就是后缀标识。HTTP服务器总是为相同的文档服务，但是QML将使用完整的链接来保存这个文档，包括链接标识。每次我们访问的这个链接的标识获得改变，QML缓冲无法获得这个信息。这个后缀标识可以是当前时间的毫秒或者一个随机数。

```
Loader {
    source: 'http://localhost:8080/main.qml#' + new Date().getTime()
}
```

总之，模板可以实现，但是不推荐，无法完整发挥QML的长处。一个更好的方法是使用web服务提供JSON或者XML数据服务。