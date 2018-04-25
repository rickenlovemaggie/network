## 简介

我们看一下 Buffer 的类图：

![](https://blog.piasy.com/img/201608/buffer_class_diagram.png)

这里我们就可以看到，它集 BufferedSource 和 BufferedSink 的功能于一身，为我们提供了访问数据缓冲区所需要的一切 API。

Buffer 是一个可变的字节序列，就像 ArrayList 一样。我们使用时只管从它的头部读取数据，往它的尾部写入数据就行了，而无需考虑容量、大小、位置等其他因素。

和 ByteString 一样，Buffer 的实现也使用了很多高性能的技巧。它内部使用一个双向 Segment 链表来保存数据，Segment 是对一小段字节数组的封装，保存了这个字节数组的一些访问信息，数据的移动通过 Segment 的转让完成，避免了数据拷贝的开销。而且 Okio 还实现了一个 Segment 对象池，以提高我们分配和释放字节数组的效率。