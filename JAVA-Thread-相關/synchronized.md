---
aliases:
  - 同步
Created-Date: 2024-04-29T10:49:00
Last-Modified-Date: 2024-04-29T10:49:00
---
# [[Thread 安全問題及解決|Thread 的安全問題]]
# JAVA 對於 [[Thread 安全問題及解決|Thread 的安全問題]]的解決(synchronized)
## 方式1: 同步代碼塊
### 語法:
```java
synchronized(同步監視器){
	// 需要被同步的 code
}
```
#### 說明:
- 需要被同步的 code 指的就是操作共享數據的 code。
- 共享數據: 即多個 [[Thread]] 共用同一份的資料。ex: 範例2的 ticket。
- 同步監視器(**Object Monitor**): 又稱鎖，可以使用任何一個 class 來當同步監視器，但規定所有的 [[Thread]] 都必須共用同一個同步監視器，而哪個 [[Thread]] 獲得了鎖，就能執行被同步的 code。
### 範例:修改 [[Thread]] 安全問題的範例2
```java
public class SalesTicket implements Runnable {  
Integer ticket = 100;  
Object obj = new Object();  
  
@Override  
public void run() {  
while (true) {  
synchronized (obj) { // 要確保所有 thread 共享同一個 obj 物件  
/*  
通常會看到 synchronized(this) 這種寫法  
而以此 code 來說，this 就是 WindowTest 宣告的 salesTicket也是所有 Thread 共享同一個 salesTicket 物件  
*/  
if (ticket > 0) {  
try {  
Thread.sleep(10);  
} catch (InterruptedException e) {  
throw new RuntimeException(e);  
}  
System.out.println(Thread.currentThread().getName() + "售票, 票號為:" + ticket);  
ticket--;  
}else {  
break;  
}  
}  
}  
}  
}  
  
class WindowTest {  
public static void main(String[] args) {  
SalesTicket salesTicket = new SalesTicket();  
Thread thread1 = new Thread(salesTicket);  
Thread thread2 = new Thread(salesTicket);  
Thread thread3 = new Thread(salesTicket);  
thread1.start();  
thread2.start();  
thread3.start();  
}  
}
```
### synchronized 原理:
重缺，目前只會用，原理似乎要看 jvm 等知識，但可以參考[這篇文](https://www.cnblogs.com/aspirant/p/11470858.html)。
目前初步的想法是，java 底層會創建一個 monitor，並且由哪些 [[Thread]] 共享這個 monitor 就是由誰共享同一個 obj 所決定，一旦這個 monitor 被某個 [[Thread]] 佔有後，其他的 [[Thread]] 就進入阻塞狀態，更詳細的直接看[文章](https://www.cnblogs.com/aspirant/p/11470858.html)。


## 方式2: 同步方法
### 說明:
如果操作共享數據的 code 剛好是一整個完整的方法，則可以把 synchronized 宣告在方法上。
### 範例:
> [!NOTE] 同步方法默認使用的 monitor
> 不管是同步或非同步方法，就是固定使用this、當前類.class 當作 monitor，不可更改。
> non-static method:默認使用 this 當 monitor，因此使用時需考慮是否所有[[Thread|線程]]實例的 this 都是指同一個物件。
> static method:默認使用同步方法的 當前類.class 當作 monitor。
```
```java
public synchronized void methodName(){
	// 操作共享數據的 code block
}
```


## synchronized 的缺點
多[[Thread|線程]]原本是[[concurrency|併發]]([[concurrency]])或[[parallel|平行]]([[parallel]])執行的，但在 synchronized 這一塊為了保證[[Thread|線程]]安全，因此會讓[[Thread|線程]]順序執行，所以性能會相比原本降低。

>[!NOTE] synchronized 與 [[concurrency]] 的疑問
>影片教學是說 synchronized 會使得[[Thread|線程]]順序執行，因此原本 [[concurrency]] 或 [[parallel]] 的[[Thread|線程]]效能會降低，但 [[concurrency]] 原本就是在單個 cpu 交替執行多個[[Thread|線程]]，使得宏觀上看似 [[parallel]] 的概念，因此底層依舊是順序執行才對，目前不太明白synchronized 怎麼使得 [[concurrency]] 的效能降低的。


# synchronized 所引發的[[死鎖]]問題([[死鎖|DeadLock]])