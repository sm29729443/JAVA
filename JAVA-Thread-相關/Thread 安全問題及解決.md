---
aliases:
  - ThreadSafe
  - threadsafe
  - 線程安全問題
  - Thread的安全問題
  - Thread 的安全問題
Created-Date: 2024-04-24T17:15:00
Last-Modified-Date: 2024-04-24T17:16:00
---
# [[Thread]] 的安全問題
若有多個[[Thread|線程]]同時對同一個共享資源進行會對值進行改變的操作的話，就會產生[[Thread|線程]]的安全問題。
## 範例 1:
若假設一開始由我取錢，並且通過了 getMoney < AccountMoney 的檢查，準備從 Account 裡取錢，而此時媳婦也正好發起了取錢的操作，並且此時我還沒完成取出錢且 Account 扣款的系列操作，而這時媳婦端判定的 getMoney < AccountMoney 依然是以 2000 < 3000 去執行，那最後就會變成我取出 2000，媳婦也取出 2000，戶頭變成 -1000，這就是由[[Thread|線程]]所引起的安全問題。
![[Pasted image 20240424172210.png]]
## 範例 2:
案例:火車站要賣票，我們模擬火車站的賣票過程。 因為疫情期間，本次列車的座位共100個（即，只能出售100張火車票）。 我們來模擬車站的售票窗口，實現多個窗口同時售票的過程。 注意：不能出現錯票、重票。
```java
public class SalesTicket implements Runnable {  
Integer ticket = 100;  
  
@Override  
public void run() {  
while (true) {  
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
/*  
result:  
Thread-2售票, 票號為:100  
Thread-1售票, 票號為:100  
Thread-0售票, 票號為:100  
Thread-0售票, 票號為:97  
Thread-1售票, 票號為:97  
Thread-2售票, 票號為:97  
Thread-0售票, 票號為:94  
Thread-1售票, 票號為:93  
Thread-2售票, 票號為:93  
...  
Thread-2售票, 票號為:3  
Thread-1售票, 票號為:3  
Thread-2售票, 票號為:0  
Thread-0售票, 票號為:-1  
*/
```
result 可以發現出現了重票，甚至票號賣到出現 -1 的問題，而發生原因是由於當 thread1 剛執行完 if 判斷進入 if 內時，Thread.sleep()使 thread1 進入了阻塞狀態，還尚未執行 ticket --，而 thread2 此時啟動也是拿者 ticket = 100 的值去執行 if 判斷，同理 thread3 也是，因此就會產生重票的情況，而票號出現 -1 也是相同道理，因為前面執行的[[Thread|線程]]阻塞了，而後面[[Thread|線程]]拿到的共享數據就是不正確的，因此出現最後結果是錯誤的。
個人認為這情況與計組裡的 MIPS 5-PIPELINE CPU 的 RAW DATA HAZARD 所描述的情況是一模一樣的。
# JAVA 對於 [[Thread]] 安全問題的解決([[synchronized]])
# JAVA 對於 [[Thread]] 安全問題的解決([[鎖(Lock)|lock]])