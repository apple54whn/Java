# 9 NIO（暂时未完成）

-   **Java7中新加的类**

    -   **Path接口**表示路径

    -   **Paths类**中`Paths.get()`接收一个或多个字符串，表示路径，**可以不存在**

    -   Files类中有静态方法

        -   `public static long copy(Path source,OutputStream out)throws IOException`   **复制文件**

            ```java
            Files.copy(Paths.get("数据库.txt"), new BufferedOutputStream(new                          FileOutputStream("copy数据库.txt")));
            ```

            

