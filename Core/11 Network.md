# 8 Network

* **软件结构**
    * **C/S结构** ：全称为Client/Server结构，是指客户端和服务器结构。常见程序有ＱＱ、迅雷等软件。
    * **B/S结构** ：全称为Browser/Server结构，是指浏览器和服务器结构。常见浏览器有谷歌、火狐等。

## 8.1 网络编程概述

- **计算机网络**

    是指将地理位置不同的具有独立功能的多台计算机及其外部设备，通过通信线路连接起来，在网络操作系统，网络管理软件及网络通信协议的管理和协调下，实现资源共享和信息传递的计算机系统。

- **网络编程**

    就是用来实现网络互连的不同计算机上运行的程序间可以进行数据交换。

- **网络模型**

    - OSI（Open System Interconnection开放系统互连）参考模型
    - TCP/IP参考模型

    ![](images\网络参考模型.png)

    - 链路层：链路层是用于定义物理传输通道，通常是对某些**网络连接设备的驱动协议**，例如针对光纤、网线提供的驱动。
    - 网络层（互联网层）：网络层是整个TCP/IP协议的**核心**，它主要用于将传输的**数据进行分组**，将分组数据**发送到目标**计算机或者网络。
    - 传输层：主要使网络程序进行通信，在进行**网络通信**时，可以采用**TCP**协议，也可以采用**UDP**协议。
    - 应用层：主要负责**应用程序的协议**，例如HTTP协议、FTP协议等。

## 8.2 网络编程三要素

### 8.2.1 IP地址

- 指**互联网协议地址**（Internet Protocol Address）（网络中**计算机唯一标识**，**点分十进制**）

- 分类

    - IPv4：是一个**32位**的二进制数，通常被分为4个字节，表示成`a.b.c.d` 的形式，例如`192.168.65.100` 。其中a、b、c、d都是0~255之间的十进制整数，那么最多可以表示42亿个。

    - IPv6：由于互联网的蓬勃发展，IP地址的需求量愈来愈大，但是网络地址资源有限，使得IP的分配越发紧张。为了扩大地址空间，拟通过IPv6重新定义地址空间，采用**128位**地址长度，**每16个字节一组，分成8组十六进制数**，表示成`ABCD:EF01:2345:6789:ABCD:EF01:2345:6789`，号称可以为全世界的每一粒沙子编上一个网址，这样就解决了网络地址资源数量不够的问题。

    - **组成：网络号段+主机号段**

        A类：第一号段为网络号段+后三段的主机号段
        	一个网络号：256 * 256 * 256 = 16777216台主机
        B类：前二号段为网络号段+后二段的主机号段
        	一个网络号：256 * 256 = 65536台主机
        C类：前三号段为网络号段+后一段的主机号段
        	一个网络号：256台主机

    - **分类：**

        A类	`1.0.0.1---127.255.255.254`(1)10.X.X.X是私有地址(在互联网上不使用，在局域网络中的地址)

           		  					   (2)127.X.X.X是保留地址，用做循环测试用的。

        B类	`128.0.0.1---191.255.255.254`  172.16.0.0-172.31.255.255私有地址。169.254.X.X保留地址。
        C类	`192.0.0.1---223.255.255.254`	192.168.X.X是私有地址
        D类	`224.0.0.1---239.255.255.254`
        E类	`240.0.0.1---247.255.255.254`

    - **两个DOS命令：**
        `ipconfig` 查看本机IP地址等信息
        `ping` 后面跟IP地址。测试本机与指定的IP地址间的通信是否有问题

    - **特殊的IP地址：**
        `127.0.0.1`、`localhost` 回环地址(表示本机)
        `x.x.x.255` 广播地址
        `x.x.x.0` 网络地址

    - `InetAddress类`**的概述和使用**（方便对IP地址的获取和操作）

        - **如果一个类没有构造方法：**

            A:成员全部是静态的(Math,Arrays,Collections)

            B:单例设计模式(Runtime)

            C:类中有静态方法返回该类的对象(InetAddress)

    - **InetAddress/getByName/getHostName/getHostAddress使用**

        `public static InetAddress getByName(String host)`:根据**主机名**或**IP地址**字符串得到IP地址**对象**

        `public String getHostName()`：返回主机名

        `public String getHostAddress()`:返回IP地址字符串

        ```java
        InetAddress address = InetAddress.getByName("Conanan");//或"192.168.1.6"
        		
        String name = address.getHostName();
        String addr = address.getHostAddress();
        System.out.println(name+"---"+addr);
        ```



### 8.2.2 端口号（逻辑端口）

* **正在运行的程序的标识**

- **有效端口**：两个字节组成，0~ 65535，其中0~1024系统使用或保留端口。
    1. 每个网络程序都会至少有一个逻辑端口
    2. 用于标识进程的逻辑地址，不同进程的标识
    3. 有效端口：0~ 65535，其中0~ 1024系统使用或保留端口。


> **物理端口**：网卡口

* 常用端口号：
    * HTTP协议端口号：`80`
    * 数据库：MySQL`3306`、Oracle`1521`
    * Tomcat：`8080`



### 8.2.3 传输协议

**数据报**(Datagram):网络传输的基本单位 

* 通信的规则

- **UDP**：**用户数据报协议**(User Datagram Protocol)。UDP是**无连接通信协议**，即在数据传输时，数据的发送端和接收端不建立逻辑连接。由于使用UDP协议**消耗资源小，通信效率高**，所以通常都会用于音频、视频和普通数据的传输。但在使用UDP协议传送数据时，由于UDP的**面向无连接性，不能保证数据的完整性，不可靠**，因此在传输重要数据时不建议使用UDP协议。特点:**数据打包被限制在64kb以内**，超出这个范围就不能发送了。（聊天留言，在线视频，视频会议。发短信，邮局包裹）

- **TCP**：**传输控制协议** (Transmission Control Protocol)。TCP协议是**面向连接的通信协议**，即传输数据之前，在发送端和接收端建立逻辑连接，然后再传输数据，它提供了两台计算机之间**可靠无差错**的数据传输。（下载，浏览网页。打电话，QQ聊天(你在线吗,在线,就回应下,就开始聊天了)

    在TCP连接中必须要明确客户端与服务器端，由客户端向服务端发出连接请求，每次连接的创建都需要经过“三次握手”。

    - **三次握手**：TCP协议中，在发送数据的准备阶段，客户端与服务器之间的三次交互，以保证连接的可靠。
        - 第一次握手，客户端向服务器端发出连接请求，等待服务器确认。
        - 第二次握手，服务器端向客户端回送一个响应，通知客户端收到了连接请求。
        - 第三次握手，客户端再次向服务器端发送确认信息，确认连接。
    - 完成三次握手，连接建立后，客户端和服务器就可以开始进行数据传输了

### 8.2.4 Socket

* 网络上具有唯一标识的**IP地址**和**端口号**组合在一起才能构成唯一能识别的标识符**套接字**）

- **Socket原理机制**
    - 通信的两端都有Socket
    - 网络通信其实就是Socket间的通信
    - 数据在两个Socket间通过IO传输

## 8.3 发送接收数据

### 8.3.1 UDP(DatagramSocket、Datagrampacket)

* **UDP协议接收数据**

    1. 创建接收端Socket对象（`DatagramSocket`类）
    2. 创建一个数据包(接收容器)（`DatagramPacket`类）
    3. 调用Socket对象的接收方法接收数据（`ds.receive(dp)`）
    4. 解析数据包，并显示在控制台
    5. 释放资源(接收端应该一直开着)

    ```java
    DatagramSocket ds = new DatagramSocket(10086);
    
    byte[] bys = new byte[1024];
    DatagramPacket dp = new DatagramPacket(bys, bys.length);
    
    ds.receive(dp);
    
    String ip = dp.getAddress().getHostAddress();
    String msg = new String(dp.getData(), 0, dp.getLength());
    System.out.println(ip+":"+msg);
    
    //ds.close();//接收端应该一直开着
    ```

* **UDP协议发送数据**

    1. 创建发送端Socket对象（`DatagramSocket`类）
    2. 创建数据，并把数据打包（`DatagramPacket`类）
    3. 调用Socket对象的发送方法发送数据包（`ds.send(dp)`）
    4. 释放资源

    ```java
    DatagramSocket ds = new DatagramSocket();
    
    byte[] bys = "helloworld".getBytes();
    InetAddress address = InetAddress.getByName("Conanan");
    int port = 10086;
    DatagramPacket dp = new DatagramPacket(bys, bys.length, address, port);
    
    ds.send(dp);
    ds.close();
    ```

* **多线程实现聊天室**

    ```java
    //主线程
    public class MainDemo {
    	public static void main(String[] args) throws IOException {
    		DatagramSocket receiveSocket = new DatagramSocket(10086);
    		DatagramSocket sendSocket = new DatagramSocket(10000);
    
    		ReceiveThread rt = new ReceiveThread(receiveSocket);
    		SendThread st = new SendThread(sendSocket);
    		Thread t1 = new Thread(rt);
    		Thread t2 = new Thread(st);
    		t1.start();
    		t2.start();
    		......
    }
    //接收端
    public class ReceiveThread implements Runnable {
    	private DatagramSocket ds;
    	public ReceiveThread(DatagramSocket ds) {
    		this.ds = ds;
    	}
    	@Override
    	public void run() {
    		while (true) {
    			byte[] bys = new byte[1024];
    			DatagramPacket dp = new DatagramPacket(bys, bys.length);
    			try {
    				ds.receive(dp);
    			} catch (IOException e) {
    				e.printStackTrace();
    			}
    			String msg = new String(dp.getData(), 0, dp.getLength());
    			System.out.println("ip:"+dp.getAddress()+",port:"+dp.getPort()+",msg:"+msg);
    
    //发送端
    public class SendThread implements Runnable {
    	private DatagramSocket ds;
    	public SendThread(DatagramSocket ds) {
    		this.ds = ds;
    	}
    	@Override
    	public void run() {
    		Scanner sc = new Scanner(System.in);
    		String str = sc.nextLine();
    		while (!str.equals(null)) {
    			if (str.equals("886")) {
    				ds.close();
    				break;
    			} else {
    				byte[] bys = str.getBytes();
    				InetAddress address = null;
    				try {
    					address = InetAddress.getByName("Conanan");
    					DatagramPacket dp=new DatagramPacket(bys, bys.length, address, 10086);
    					ds.send(dp);
    					str = sc.nextLine();
    				} catch (IOException e) {
    					e.printStackTrace();
    
    ```

### 8.3.2 TCP(Socket、ServerSocket)

* **java.net.ServerSocket**：这个类**实现了服务器套接字**，该对象等待通过网络的请求。

    * `ServerSocket(int port)` 使用该构造方法在**创建ServerSocket对象**时，就可以将其绑定到一个指定的端口号上，参数port就是端口号。
    * `Socket accept()` ：**侦听并接受连接**，返回一个新的Socket对象，用于和客户端实现通信。==**该方法会一直阻塞直到建立连接**==。 
    * `void close()` ：**关闭此套接字**。详细介绍如Socket中`close`方法

* **java.net.Socket**：该类**实现客户端套接字**，套接字指的是两台设备之间通讯的端点。

    - `Socket(String host, int port)`：**创建套接字对象**并将其连接到**指定主机**上的**指定端口号**
    - `InputStream getInputStream()` ： 返回此**套接字的输入流**。
        - 如果此Scoket具有相关联的通道，则生成的InputStream 的所有操作也关联该通道。
        - 关闭生成的InputStream也将关闭相关的Socket。
    - `OutputStream getOutputStream()` ： 返回此**套接字的输出流**。
        - 同上两条
    - `void close()` ：**关闭此套接字**。
        - 一旦一个socket被关闭，它不可再使用。
        - 关闭此socket也将关闭相关的InputStream和OutputStream 。 
    - `void shutdownOutput()` ： **禁用此套接字的输出流**。   
        - **任何先前写出的数据将被发送**，并且后跟 TCP 的正常连接**终止序列**。 

    > ==**服务器使用客户端的流和客户端交互**==
    >
    > ==IO流中各**read**方法在**没有输入可用时将阻塞**，并且**在通道中输入流是读不到结束标记**的，所以需要**shutdownOutput**方法来发送数据并**带上终止序列**。==

* **服务器端接收数据(ServerSocket)**

    1. 创建TCP服务器端的Socket对象
    2. 监听客户端连接
    3. 获取**套接字**的输入流，读取数据
    4. 释放资源

    ```java
    ServerSocket ss = new ServerSocket(60001);
    		
    Socket s = ss.accept();
    		
    InputStream is = s.getInputStream();
    byte[] bys = new byte[1024];
    int len = is.read(bys);
    System.out.println(new String(bys,0,len));
    		
    s.close();
    ```

* **客户端发送数据(Socket)**

    1. 创建TCP客户端的Socket对象
    2. 获取**套接字**的输出流，写数据
    3. 释放资源

    ```java
    Socket s = new Socket("192.168.81.1", 60001);
    		
    OutputStream os = s.getOutputStream();
    os.write("hello java world".getBytes());
    		
    s.close();
    ```

* **文件上传优化**

    * **文件名称写死的问题**：建议使用系统时间优化，保证文件名称唯一

        ```java
        FileOutputStream fis = new FileOutputStream(System.currentTimeMillis()+".jpg")
        ```

    * **循环接收的问题**：服务端不应保存一个文件就关闭，需要不断的接收不同用户的文件，使用**循环**改进

        ```java
        // 让服务器一致处于监听状态。每次接收新的连接,创建一个Socket
        while（true）{
            Socket accept = serverSocket.accept();
            ......
        }
        ```

    * **效率问题**：服务端，在接收大文件时耗费时间长，此时不能接收其他用户上传，使用**多线程**技术优化

        ```java
        while（true）{
            Socket accept = serverSocket.accept();
            // accept 交给子线程处理.
            new Thread(() -> {
                ......
                    InputStream bis = accept.getInputStream();
                ......
            }).start();
        }
        //并且由于接口中没有抛异常，所以实现类中也不能抛异常，只能try...catch
        ```

#### 上传文件带反馈（多线程）

```java
//Server
ServerSocket socket = new ServerSocket(8888);
while (true) { //服务端循环接收不同客户端请求
    Socket s = socket.accept(); //监听到客户端请求并建立连接
    new Thread(() -> {  //利用lambda实现多线程，让每个用户的线程不受其他线程影响。并由于接口没有抛异常，所以以下用try捕获
        BufferedOutputStream bos = null;
        try {
            BufferedInputStream bis = new BufferedInputStream(s.getInputStream());
            File file = new File("F:\\upload"); //检查上传文件的目录是否存在
            if (!file.exists())
                file.mkdirs();
            String name = System.currentTimeMillis() + ".jpg"; //解决文件名写死问题
            bos = new BufferedOutputStream(new FileOutputStream(file+"\\"+name));
            //将从网络通道中的文件读取并写入磁盘
            byte[] bytes = new byte[1024];
            int len = 0;
            while ((len = bis.read(bytes)) != -1) {
                bos.write(bytes, 0, len);
            }
		   //回应客户端上传完毕
            BufferedWriter bw = new BufferedWriter(new OutputStreamWriter(s.getOutputStream()));
            bw.write("上传完毕");
            bw.newLine();
            bw.flush();

        } catch (IOException e) {
            e.printStackTrace();
        } finally {
            try {
                bos.close();
                s.close(); //close本身也抛异常。由于是多线程，每个线程使用的不同socket，所以socket可以关闭
            } catch (IOException e) {
                e.printStackTrace();
            }
        }
    }).start();
}

//Client
Socket s = new Socket("127.0.0.1",8888);

BufferedInputStream bis = new BufferedInputStream(new FileInputStream("day22-code\\src\\666.jpg"));
BufferedOutputStream bos = new BufferedOutputStream(s.getOutputStream());
//将本地磁盘文件读取并写入网络通道中
byte[] bytes = new byte[1024];
int len = 0;
while ((len=bis.read(bytes))!=-1){
    bos.write(bytes,0,len);
}
s.shutdownOutput();//发送数据并带上终止序列，以便服务器读取时可以读到终止序列
//读取网络通道中服务器回应的数据并打印
BufferedReader br = new BufferedReader(new InputStreamReader(s.getInputStream()));
String line = br.readLine();
System.out.println(line);

bis.close();
s.close();
```



