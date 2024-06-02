---
aliases:
  - 多線程
  - 線程
  - 多執行緒
  - 多執行序
  - 執行序
Created-Date: 2024-04-23T11:20:00
Last-Modified-Date: 2024-04-23T11:21:00
tags: 
學習資源: 尚硅谷 2024 JAVA 影片
---
# 1. [[Program]] 
---
# 2. [[Process]] 
---
# 3.Thread
- Thread 就是把 [[Process]] 再進一步細分，是[[Program|程式]]內部的一條執行路徑，一個 [[Process]] 至少包含一個 Thread。
- Thread 可以同時執行也可以順序執行，一個 [[Process]] 若是同時執行多個 Thread，就是支持 multithreading(多線程) 的。![[Pasted image 20240423135442.png]]
- Thread 是 CPU 調度和執行的最小單位，也就是說，實際上在運行的不是 [[Process]] 而是 Thread。
- 一個 [[Process]] 中若有多個 Thread 的話，有些資源是一個 Thread 一份，而有些資源是一個 [[Process]] 中的所有 Thread 共享一份。  如下圖所示:![[Pasted image 20240423135853.png]]
## 4. [[線程調度]]
## 5. [[parallel]] And [[concurrency]]
在單核 CPU 中，因為同一時刻就只能執行一個指令，所以只能透過[[concurrency|並發]]的方式來達到宏觀上的同時執行，但實際上還是順序執行的，而在多核 CPU 中，則可以把原本[[concurrency|併發]]處理的[[Program|程式]]，變成 [[parallel|平行]]處理，也就是同一時刻在多個 CPU 上執行多個[[Program|程式]]，以達到真正的同時執行。

---
## 6. [[JAVA Thread 的創建與使用|JAVA Thread]] 的創建與使用
## 7. JAVA [[Thread 安全問題及解決]]
# 8. [[線程間的通訊機制]]
# 9. callable 待補
# 10. 線程池 待補
