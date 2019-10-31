# 9 NIO（暂时未完成）

*   Java NIO (New IO，Non-Blocking IO) 是从Java 1.4版本开始引入的一套新的IO API，可以替代标准的Java IO API。NIO与原来的IO有同样的作用和目的，但是使用的方式完全不同，NIO支持**面向缓冲区**的(IO是面向流的)、**基于通道**的IO操作。NIO将以更加高效的方式进行文件的读写操作。
*   Java API中提供了两套NIO，一套是针对**标准输入输出**NIO，另一套就是**网络编程**NIO。
*   `java.nio.channels.Channel`
    *   `FileChannel `：处理本地文件 
    *   `SocketChannel `：TCP网络编程的客户端的Channel 
    *   `ServerSocketChannel `：TCP网络编程的服务器端的Channel 
    *   `DatagramChannel `：UDP网络编程中发送端和接收端的Channel 
*   随着 JDK 7 的发布，Java对NIO进行了极大的扩展，增强了对文件处理和文件系统特性的支持，以至于我们称他们为 NIO.2。 因为 NIO 提供的一些功能，NIO已经成为文件处理中越来越重要的部分。

-   早期的Java只提供了一个`File`类来访问文件系统，但`File`类的功能比较有限，所提供的方法性能也不高。而且，大多数方法在出错时仅返回失败，并不会提供异常信息（如`renameTo`只返回 boolean，文件存在时重命名失败）。

    NIO. 2为了弥补这种不足，引入了`Path`**接口**，代表一个平台无关的平台**路径**，描述了**目录结构中文件的位置**。`Path`可以看成是`File`类的升级版本，实际引用的资源也可以不存在。`Paths`则包含了两个返回`Path`的静态工厂方法。

    -   在以前IO操作都是这样写的

        ```java
        import java.io.File;
        File file = new File("index.html");
        ```

    *   在Java7 中，我们可以这样写

        ```java
        import java.nio.file.Path;
        import java.nio.file.Paths;
        Path path = Paths.get("index.html");		
        ```

        

-   `Path`：看成是`File`类的升级版本，实际引用的资源也可以不存在

    -   `String toString()` : 返回调用 Path 对象的字符串表示形式
    -   `boolean startsWith(String path)` : 判断是否以 path 路径开始
    -   `boolean endsWith(String path)` : 判断是否以 path 路径结束
    -   `boolean isAbsolute()` : 判断是否是绝对路径
    -   `Path getParent()` :返回Path对象包含整个路径，不包含 Path 对象指定的文件路径 
    -   `Path getRoot()`:返回调用 Path 对象的根路径
    -   `Path getFileName()` : 返回与调用 Path 对象关联的文件名
    -   `int getNameCount()` : 返回Path 根目录后面元素的数量
    -   `Path getName(int idx)` : 返回指定索引位置 idx 的路径名称
    -   `Path toAbsolutePath()` : 作为绝对路径返回调用 Path 对象
    -   `Path resolve(Path p)` :合并两个路径，返回合并后的路径对应的Path对象
    -   `File toFile()`: 将Path转化为File类的对象 

-   `Paths`：包含了两个返回`Path`的静态工厂方法

    -   `static Path get(String first, String ... more)`：用于将多个字符串串连成路径 
    -   `static Path get(URI uri)`：返回指定uri对应的Path路径 

-   `Files`：包含了大量静态的工具方法来操作文件或目录

    -   常用方法
        -   `Path copy(Path src, Path dest, CopyOption ... how)` : 文件的复制
        -   `Path createDirectory(Path path, FileAttribute<?> ... attr)` : 创建一个目录
        -   `Path createFile(Path path, FileAttribute<?> ... arr)` : 创建一个文件
        -   `void delete(Path path)` : 删除一个文件/目录，如果不存在，执行报错
        -   `void deleteIfExists(Path path)` : Path对应的文件/目录如果存在，执行删除
        -   `Path move(Path src, Path dest, CopyOption...how)` : 将 src 移动到 dest 位置 
        -   `long size(Path path)` : 返回 path 指定文件的大小 

    *   判断
        *   `boolean exists(Path path, LinkOption ... opts)` : 判断文件是否存在
        *   `boolean isDirectory(Path path, LinkOption ... opts)` : 判断是否是目录
        *   `boolean isRegularFile(Path path, LinkOption ... opts)` : 判断是否是文件 
        *   `boolean isHidden(Path path)` : 判断是否是隐藏文件
        *   `boolean isReadable(Path path)`: 判断文件是否可读
        *   `boolean isWritable(Path path) `: 判断文件是否可写
        *   `boolean notExists(Path path, LinkOption ... opts)` : 判断文件是否不存在 
    *   操作内容
        *   `SeekableByteChannel newByteChannel(Path path, OpenOption...how)` : 获取与指定文件的连 接，how 指定打开方式。 
        *   `DirectoryStream<Path> newDirectoryStream(Path path)` : 打开 path 指定的目录 
        *   `InputStream newInputStream(Path path, OpenOption...how)`:获取 InputStream 对象 
        *   `OutputStream newOutputStream(Path path, OpenOption...how)` : 获取 OutputStream 对象 

    

-   Java7中新加的类

    -   **Path接口**表示路径

    -   **Paths类**中`Paths.get()`接收一个或多个字符串，表示路径，**可以不存在**

    -   Files类中有静态方法

        -   `public static long copy(Path source,OutputStream out)throws IOException`   **复制文件**

            ```java
            Files.copy(Paths.get("数据库.txt"), new BufferedOutputStream(new                          FileOutputStream("copy数据库.txt")));
            ```

            

