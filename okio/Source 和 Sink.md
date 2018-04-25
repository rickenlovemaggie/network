## 简介

Okio 吸收了 java.io 一个非常优雅的设计：流（stream），流可以一层一层套起来，不断扩充能力，最终完成像加密和压缩这样复杂的操作。再次感谢 Stay 一针见血地指出这正是“修饰模式”的实践。

> 修饰模式，是面向对象编程领域中，一种动态地往一个类中添加新的行为的设计模式。就功能而言，修饰模式相比生成子类更为灵活，这样可以给某个对象而不是整个类添加一些功能。


Okio 有自己的流类型，那就是 Source 和 Sink，它们和 InputStream 与 OutputStream 类似，前者为输入流，后者为输出流。

但它们还有一些新特性：

超时机制，所有的流都有超时机制；
- API 非常简洁，易于实现；
- Source 和 Sink 的 API 非常简洁，为了应对更复杂的需求，Okio 还提供了 BufferedSource 和 BufferedSink 接口，便于使用（按照任意类型进行读写，BufferedSource 还能进行查找和判等）；
- 不再区分字节流和字符流，它们都是数据，可以按照任意类型去读写；
- 便于测试，Buffer 同时实现了 BufferedSource 和 BufferedSink 接口，便于测试；
Source 和 InputStream 互相操作，我们可以把它们等同对待，同理 Sink 和 OutputStream 也可以等同对待。


Source 和 InputStream 互相操作，我们可以把它们等同对待，同理 Sink 和 OutputStream 也可以等同对待。


