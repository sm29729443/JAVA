---
aliases:
  - InetAddress
  - JAVA TCP/IP
Created-Date: 2024-05-06T10:00:00
Last-Modified-Date: 2024-05-06T10:00:00
---
# InetAddress
## 作用
IP 在 JAVA 中是以一個 Class 所存在的，也就是 InetAddress。
在 JAVA API 文檔能看到相關描述。
所以，一個 InetAddress 物件就代表一個具體的 IP Address。
![[Pasted image 20240506100132.png]]
## 實例化方式
若查看 API 文檔，會看到 InetAddress 沒有提供關於 Contructor 的描述，這意味者 InetAddress Class 不直接提供透過 Contructor 的構建方式。
主要透過 `InetAddress getByName(String)` 與 `InetAddress getLocalHost()` 來得到一個 InetAddress 物件。
# TCP 與 UDP 協議
差別: 看起來，TCP 須確定通訊雙方正確地建立了通訊後，才會開始進行通訊，而 UDP 則不管接收端是否接收的到，就是只發就對了。
## TCP(Transmission Control Protocol)
**TCP协议：** 

- TCP协议进行通信的两个应用进程：客户端、服务端。
    
- 使用TCP协议前，须先`建立TCP连接`，形成基于字节流的传输数据通道
    
- 传输前，采用“三次握手”方式，点对点通信，是`可靠的`
    
    - TCP协议使用`重发机制`，当一个通信实体发送一个消息给另一个通信实体后，需要收到另一个通信实体确认信息，如果没有收到另一个通信实体确认信息，则会再次重复刚才发送的消息。
        
- 在连接中可进行`大数据量的传输`
    
- 传输完毕，需`释放已建立的连接，效率低`

## UDP(User Datagram Protocol)
**UDP协议：**

- UDP协议进行通信的两个应用进程：发送端、接收端。
    
- 将数据、源、目的封装成数据包（传输的基本单位），`不需要建立连接`
    
- 发送不管对方是否准备好，接收方收到也不确认，不能保证数据的完整性，故是`不可靠的`
    
- 每个数据报的大小限制在`64K`内
    
- 发送数据结束时`无需释放资源，开销小，通信效率高`
    
- 适用场景：音频、视频和普通数据的传输。例如视频会议

# TCP 的通訊過程
之後再整理，原理與之前學 UART、I2C 的底層的硬體通訊協議差不多，都是透過設置某個 Singal 為 1 or 0 ，告訴對方通訊的開啟或結束。
## 三次握手
## 四次揮手
# JAVA Socket Class
Socket 就是一組 IP Address + Port 的組合。
網路通訊實際上就是 Socket 之間的通訊。
個人理解是可以把 Socket Class 理解成計算機的代理對象，當兩台主機要彼此通信時，實際上是使用 Socket Class 來代理這台主機，然後建立一個網路通訊，而進行網路通訊的雙方，就是這個 Socket 物件，而 JAVA Socket 已經幫我們實現了 TCP 通訊的底層細節。
![[Pasted image 20240506111656.png]]
## Socket Example
先運行 伺服器端，再運行客戶端。
下面 Code 在伺服器端接收 byte[] 並轉換成 String 時實際上是會有問題的，譬如如果 byte 的大小只有 byte[5]，那默認使用 UTF-8 時，中文字大小是 3byte，那 byte[5] 是不能完整解析 兩個中文字的，所以它會造成亂碼，此時要透過 ByteArrayOutputStream 解決，具體分析看 尚硅谷 影片，這裡不再做展示。
```java
// 客戶端  
@Test  
public void test2() throws IOException {  
// 創建 Socket Class// 這裡聲明的 IP、Port 是伺服器端的，也就是在設定我們要向誰發送數據  
InetAddress inetAddress = InetAddress.getByName("127.0.0.1");  
int port = 8989;  
Socket socket = new Socket(inetAddress, port);  
// 發送數據  
OutputStream outputStream = socket.getOutputStream();  
outputStream.write("你好挖, 我是客戶端".getBytes());  
// 關閉 SocketoutputStream.close();  
socket.close();  
}  
  
// 伺服器端  
@Test  
public void test3() throws IOException {  
// 創建 ServerSocket Classint port = 8989;  
ServerSocket serverSocket = new ServerSocket(port);  
// 調用 accept(), 接收客戶端的 Socket// accept() method 是一個阻塞式的方法，也就是說  
// accpet() 執行後會等待者誰去連他，若一直沒有，它就一直等者  
Socket socket = serverSocket.accept();  
  
// 接收數據  
InputStream inputStream = socket.getInputStream();  
byte[] buffer = new byte[1024];  
int len = 0;  
while ( (len = inputStream.read(buffer)) != -1) {  
// 可能會出現亂碼
String s = new String(buffer, 0, len);  
System.out.println(s);  
}  
  
// 關閉 SocketinputStream.close();  
socket.close();  
serverSocket.close();  
}
```
# URL Class
看起來沒啥特殊的，有問題直接看 影片文檔 即可。