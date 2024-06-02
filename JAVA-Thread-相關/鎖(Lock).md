---
aliases:
  - lock
  - Lock
Created-Date: 2024-04-29T15:05:00
Last-Modified-Date: 2024-04-29T15:05:00
---
JDK 5.0 新增的特性，是除了 [[synchronized]] 外的另一種解決[[Thread|線程]]安全的方式，Lock 是一個 interface，底下提供了很多種實現類。
補充:影片只做了很簡單的展示。
# 範例
簡單示範如何使用 lock 解決[[Thread|線程]]安全問題。
```java
import java.util.concurrent.locks.ReentrantLock;  
  
public class LockTest {  
public static void main(String[] args) {  
SalesTicket salesTicket = new SalesTicket();  
Thread t1 = new Thread(salesTicket);  
Thread t2 = new Thread(salesTicket);  
Thread t3 = new Thread(salesTicket);  
t1.start();  
t2.start();  
t3.start();  
}  
}  
  
class SalesTicket implements Runnable {  
int ticket = 100;  
// 要確保所有線程是使用同一把鎖
private static final ReentrantLock reentrantLock = new ReentrantLock();  
@Override  
public void run() {  
while (true) {  
try {  
reentrantLock.lock();  
if (ticket > 0) {  
try {  
Thread.sleep(10);  
} catch (InterruptedException e) {  
throw new RuntimeException(e);  
}  
System.out.println(Thread.currentThread().getName() + "售票, 票號為:" + ticket);  
ticket--;  
} else {  
break;  
}  
} finally {  
reentrantLock.unlock();  
}  
}  
}  
}
```
# synchronized 與 lock 對比
略，學 juc 再補充。