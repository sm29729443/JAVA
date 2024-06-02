---
aliases:
  - process
  - 進程
Created-Date: 2024-04-23T11:31:00
Last-Modified-Date: 2024-04-23T11:31:00
學習資源: 尚硅谷 2024 JAVA 影片
---
# 定義
- Process 是 [[Program]] 的一次執行。譬如，當 Intelij 未運行時就只是一個儲存於硬碟的應用[[Program|程式]]，而當運行 Intelij 時則會將 Intelij 加載到 Memory 中開始執行，此時就成為了一個進程(Process)。
- 每個 Prcoess 都有一個獨立的 memory space，當系統運行一個 [[Program]] 時，即是一個 Process 從 創建->運行->消亡的生命週期過程。
-  Process 是動態的，[[Program]] 是靜態的。
- Process 是作業系統調度和分配資源的最小單位，也就是作業系統要運行 [[Program]] 時的基本單位，作業系統會為每個 Process 分配不同的記憶體空間。

