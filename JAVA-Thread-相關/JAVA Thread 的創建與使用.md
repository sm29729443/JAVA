---
aliases:
  - JAVA Thread
Created-Date: 2024-04-23T17:26:00
Last-Modified-Date: 2024-04-23T17:26:00
---

# 創建
又分為兩種方式
## 1.繼承 [[Thread]] Class
- ![[Pasted image 20240423145321.png]]關於 start() method 的作用:從 JAVA API 文檔可看到，start() 會去啟動此 [[Thread]]，然後 JVM 會去調用這個 [[Thread]] 的 run() method。
---
- ### 範例: 使用 [[Thread]] print 出 0-100 
從 result 可以看到 main 跟 t1 的 [[Thread|執行序]] 是交換執行的。
```java
public class ThreadTestBench {  
public static void main(String[] args) {  
ThreadTest t1 = new ThreadTest();  
t1.start();  
for (int i = 0; i <= 1000; i++) {  
System.out.println("main:" + i);  
}  
}  
}  
  
class ThreadTest extends Thread {  
@Override  
public void run() {  
for (int i = 0; i <= 1000; i++) {  
System.out.println(Thread.currentThread().getName() + ":" + i);  
}  
}  
}
```
#### Result:
![[Pasted image 20240423153750.png]]
**補充1:若是直接調用 t1.run()，而不是透過 start() 調用，則不會產生一個新的[[Thread|線程]]去執行 run method，而是直接在 main 這個[[Thread|線程]]去執行t1.run而已。**
**補充2:已經 start() 的[[Thread|線程]]不能再 starts() 一次，否則會報異常。**

## 2. 實現 Runnable Interface
- ![[Pasted image 20240423155011.png]]

## 兩種方式的使用選擇
- ![[Pasted image 20240423160541.png]]
- 第二種方式是把一個繼承 Runnable 的物件丟進 [[Thread]] 開啟[[Thread|線程]]，因此，若是此物件裡有屬性，則開啟的所有[[Thread|線程]]都會共享這個屬性的數據，所以若是有者讓[[Thread|線程]]共享數據的需求，使用第二種是優先的。
# [[Thread|線程]]的常用結構
## 1. [[Thread]] Contructor
![[Pasted image 20240423161804.png]]
## 2. [[Thread]] 常用方法
![[Pasted image 20240423163146.png]]
![[Pasted image 20240423163253.png]]


## 3. [[Thread]] 的優先級
若沒有指定 [[Thread]] 的優先級，[[Thread]] 也有
一個默認的優先級。
![[Pasted image 20240423163749.png]]
## 4. [[Thread]] 生命週期
還不了解實際上的運行規則。
![[Pasted image 20240423172643.png]]
