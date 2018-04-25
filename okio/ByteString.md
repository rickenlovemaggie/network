
## 简介

string 这个词本意是“串”，只不过在编程语言的世界中，我们基本都用它来指代“字符串”，其实字符串应该叫 CharString，因此 ByteString 的意义也就很好理解了，“字节串”，我们也可以看到 **ByteString 在字节处理中的作用就是相当于字符处理中的 String**

ByteString 代表一个 immutable 字节序列。对于字符数据来说，String 是非常基础的，但在二进制数据的处理中，则没有与之对应的存在，ByteString 应运而生。它为我们提供了对串操作所需要的各种 API，例如子串、判等、查找等，也能把二进制数据编解码为十六进制（hex），base64 和 UTF-8 格式。

它向我们提供了和 String 非常类似的 API：

- 获取字节：指定位置，或者整个数组；
- 编解码：hex，base64，UTF-8；
- 判等，查找，子串等操作；

ByteString 整个类不到 500 行，完全可以通读，但我们还是从实际的使用例子出发。

我们来看一下官方文档中 PNG 解码的例子：

```
private static final ByteString PNG_HEADER = ByteString.decodeHex("89504e470d0a1a0a");

public void decodePng(InputStream in) throws IOException {
  BufferedSource pngSource = Okio.buffer(Okio.source(in));

  ByteString header = pngSource.readByteString(PNG_HEADER.size());
  if (!header.equals(PNG_HEADER)) {
    throw new IOException("Not a PNG.");
  }
  // ...
  pngSource.close();
}

```
这里我们可以看到，我们可以直接从十六进制字符串得到它所表示的字节串，我们看看它的内部实现：

```
public static ByteString decodeHex(String hex) {
  // ...

  byte[] result = new byte[hex.length() / 2];
  for (int i = 0; i < result.length; i++) {
    int d1 = decodeHexDigit(hex.charAt(i * 2)) << 4;
    int d2 = decodeHexDigit(hex.charAt(i * 2 + 1));
    result[i] = (byte) (d1 + d2);
  }
  return of(result);
}

private static int decodeHexDigit(char c) {
  if (c >= '0' && c <= '9') return c - '0';
  if (c >= 'a' && c <= 'f') return c - 'a' + 10;
  if (c >= 'A' && c <= 'F') return c - 'A' + 10;
  throw new IllegalArgumentException("Unexpected hex digit: " + c);
}
```


