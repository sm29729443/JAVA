---
aliases: 
Created-Date: 2024-05-06T12:35:00
Last-Modified-Date: 2024-05-06T12:35:00
學習資源: 尚硅谷 20231025 JAVAWEB 影片
---
***主要筆記以影片所提供的課件 PDF 為主，這邊只記錄影片裡有提到，但筆記沒有的，且我認為是細節點容易忘記的部分，因此這裡不做知識的細分，就一個筆記頁做到底，且標題對應影片提供的課件 PDF。***

# 2.5 超鏈接標籤(a標籤)
## 相對路徑
a 標籤的 href 可以使用相對路徑，當使用相對路徑時，是以當前文件所在的位置為出發點去找其他資源。譬如當前文件位於資料夾 abc 底下，那就是以 abc 為出發點去找其他資源。![[Pasted image 20240506142714.png]]

# 2.8 表單標籤(form 標籤)
form 標籤當 method = get 時，此時 form 裡的 input 標籤的數值會以 url parameter 的方式提交，但 url parameter 的參數名要透過 input name設定
譬如:
此時提交的url = /test?username=value。
**備註: input 標籤的 value 屬性就是輸入框輸入的值。**
```html
<form action="/test" method=get>
 <input type="text" name="username"/>
</form>
```
![[Pasted image 20240506145325.png]]
# 2.10 佈局相關標籤(div、span)
![[Pasted image 20240506145950.png]]
# 