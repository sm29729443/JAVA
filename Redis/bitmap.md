---
aliases:
  - Bitmap
Created-Date: 2024-05-30T16:13:00
Last-Modified-Date: 2024-05-30T16:13:00
---
bitmap 底層是使用 string 類型，而 redis string 的容量為 512 MB，所以 bitmap 的最大範圍就是 512 * 2^10 * 2^10 * 8 = 4,294,967,296 bit。
bitmap 是直接操作 string 中的具體 bit 位，所以 set bit k1 1 1 指的是把第 1 位的bit 改成 數值 1。