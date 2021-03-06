## 由来
在文件上传时，有时候我们需要判断文件类型。但是又不能简单的通过扩展名来判断（防止恶意脚本等通过上传到服务器上），于是我们需要在服务端通过读取文件的首部几个二进制位来判断常用的文件类型。

## 使用
这个工具类使用非常简单，通过调用`FileTypeUtil.getType`即可判断，这个方法同时提供众多的重载方法，用于读取不同的文件和流。

```java
File file = FileUtil.file("d:/test.jpg");
String type = FileTypeUtil.getType(file);
//输出 jpg则说明确实为jpg文件
Console.log(type);
```

## 原理和局限性
这个类是通过读取文件流中前N个byte值来判断文件类型，在类中我们通过Map形式将常用的文件类型做了映射，这些映射都是网络上搜集而来。也就是说，我们只能识别有限的几种文件类型。但是这些类型已经涵盖了常用的图片、音频、视频、Office文档类型，可以应对大部分的使用场景。

> 对于某些文本格式的文件我们并不能通过首部byte判断其类型，比如`JSON`，这类文件本质上是文本文件，我们应该读取其文本内容，通过其语法判断类型。

## 自定义类型
为了提高`FileTypeUtil`的扩展性，我们通过`putFileType`方法可以自定义文件类型。

```java
FileTypeUtil.putFileType("ffd8ffe000104a464946", "new_jpg");
```

第一个参数是文件流的前N个byte的16进制表示，我们可以读取自定义文件查看，选取一定长度即可(长度越长越精确)，第二个参数就是文件类型，然后使用`FileTypeUtil.getType`即可。

> 注意
> xlsx、docx本质上是各种XML打包为zip的结果，因此会被识别为zip格式。