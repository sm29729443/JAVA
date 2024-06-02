---
aliases:
  - singleton
  - 單例模式
  - 懶漢式
  - 餓漢式
  - Lazy Initialization
  - Greed Singleton
  - Eager Initialization
  - Singleton
Created-Date: 2024-04-29T11:11:00
Last-Modified-Date: 2024-04-29T00:45:00
---
# 定義
單例模式指的是使得整個系統只有一個實例物件的設計模式，依照物件被實例的時間點又分為 Lazy Initialization、Eager Initialzation。

# Lazy Initialization(懶漢式)
## 定義:
懶漢式指的是當第一次要使用到此單例物件時，才進行實例化的動作來創建物件。
## 使用場景:
目前理解的是，該單例實例可能在整個系統都運行完了，也沒有被調用到，那如果在一開始就直接創建此實例，並且此實例的初始化過程較為繁瑣，結果整個系統運行完了都沒使用到，就會造成資源浪費，因此可使用懶漢式，使得該單例實例在第一次要被使用到時，才進行創建的動作，這樣若沒使用到也不會創建該單例實例，即可減少資源浪費。
## 範例1:
```java
// Singleton Lazy Initialization  
// 會有線程安全問題
public class Bank {  
private static Bank bank = null;  
  
private Bank() {  
	  //初始化過程
}  
  
public static Bank getInstance() {  
if (bank == null) {  
bank = new Bank();  
}  
return bank;  
}  
  
}
```

## 範例2: 涉及 [[Thread]] 時
### 懶漢式在[[Thread|多線程]]的場景下出現的問題
由以下 code 可以看到，若是在多[[Thread|線程]]的場景下，會出現當 t1 [[Thread|線程]]調用 Bank.getInstance() 後進入阻塞，而 t2 隨後調用 Bank.getInstance() 也順利進入 if block 而導致出現了兩個 Bank 實例，此時就產生了[[Thread|線程]]問題。
> [!NOTE] Class BankTest
> ```java
> public class BankTest {  
static Bank b1;  
static Bank b2;  
public static void main(String[] args) {  
Thread t1 = new Thread() {  
@Override  
public void run() {  
b1 = Bank.getInstance();  
}  
};  
Thread t2 = new Thread() {  
@Override  
public void run() {  
b2 = Bank.getInstance();  
}  
};  
t1.start();  
t2.start();  
try {  
// 如果希望 t1、t2 線程執行完後 main 線程才接者執行，則可以使用 Thread.join()t1.join();  
t2.join();  
} catch (InterruptedException e) {  
throw new RuntimeException(e);  
}  
System.out.println("b1:" + b1);  
System.out.println("b2:" + b2);  
System.out.println(b1 == b2);  
}  
}
/*  
result:  
b1:test.Bank@682a0b20  
b2:test.Bank@3d075dc0  
false  
*/

> [!NOTE] Class Bank
> ```java
> // Singleton Lazy Initialization  
public class Bank {  
private static Bank bank = null;  
private Bank() {  
// 初始化過程  
}  
public static Bank getInstance() {  
try {  
// 使線程阻塞，為了看出因線程所產生的不安全問題  
Thread.sleep(5000);  
} catch (InterruptedException e) {  
throw new RuntimeException(e);  
}  
if (bank == null) {  
bank = new Bank();  
}  
return bank;  
}  
}

### 解決方法([[synchronized]]):
#### 方式1:
>[!NOTE] [[synchronized|同步]]方法
>```java
>public synchronized static Bank getInstance() {  
if (bank == null) {  
try {  
// 使線程阻塞，為了看出因線程所產生的不安全問題  
Thread.sleep(1000);  
} catch (InterruptedException e) {  
throw new RuntimeException(e);  
}  
bank = new Bank();  
}  
return bank;  
}
#### 方式2:
>[!NOTE] 方法內使用 synchronized
>```java
>public static Bank getInstance() {   
synchronized (Bank.class) {  
if (bank == null) {  
try {  
// 使線程阻塞，為了看出因線程所產生的不安全問題  
Thread.sleep(1000);  
} catch (InterruptedException e) {  
throw new RuntimeException(e);  
}  
bank = new Bank();  
}  
return bank;  
}  
}
>```
#### 方式3:
>[!NOTE] 優化方式1、2
>[[synchronized]] 裡的程式區塊功能，就只是為了在第一次調用Bank.getInstance() 時，不要因為[[Thread|線程]]關係而產生多個 Bank 實例，而在 Bank 實例被正確的創建出來後，接下來的每次調用已經保證了每個[[Thread|線程]]都一定會正確的拿到這個單例實例，但卻依然會因 [[synchronized]] 的關係，使得本該[[parallel|平行]]、[[concurrency|併發]]處理的[[Thread|線程]]因 [[synchronized]]的關係變成順序執行，因此在 [[synchronized]] 外加入一個 if 判斷，讓 Bank 實例正確創建出來後，接下來的[[Thread|線程]]不會再因 [[synchronized]] 而降低效能。
>補充:教學說此方式還有點問題，涉及到什麼指令重排的，大致問題是說， new Bank();這一行 code 在 JVM 
>底層會被拆解成好多步驟，其中有一步是創建，有一步是初始化，而可能在 t1 創建完後但尚未執行初始化，但 bank 已經不是 null 了，而 t2 一執行 getInstance 就直接 return bank 拿到尚未初始化完的 bank 實例了。
>```java
>public static Bank getInstance() {  
if(bank == null) {  
synchronized (Bank.class) {  
if (bank == null) {  
try {  
// 使線程阻塞，為了看出因線程所產生的不安全問題  
Thread.sleep(1000);  
} catch (InterruptedException e) {  
throw new RuntimeException(e);  
}  
bank = new Bank();  
}  
}  
}  
return bank;  
}
>```

# Eager Initialixation(餓漢式)
待補