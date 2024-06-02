---
aliases:
  - Redis String
Created-Date: 2024-05-30T14:19:00
Last-Modified-Date: 2024-05-30T14:19:00
---
# keepttl
當重新 set 一個 key 的 value 時，原先設定的 expire time 會不見，key 的 expire time 又變成永不過時。
![[Pasted image 20240530142241.png]]
而使用 keepttl 可以再重新 set key value 時，讓原先的 expire time 設定繼續維持下去。![[Pasted image 20240530142445.png]]