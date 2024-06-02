---
aliases:
  - 字符集
  - 字元集
Created-Date: 2024-05-03T10:31:00
Last-Modified-Date: 2024-05-03T10:31:00
---
# Character 與 Character Set
Character 指的是文字與符號的總稱，譬如文字、圖形符號、數學符號等等，而一組***抽象***的 Character 的集合，就是 ***Character Set***。
而在上文中為什麼特別指出***抽象***，這是因為 ***Character Set*** 裡儲存的就是一個**不具任何具體形式，僅是一個*抽象概念*的 Char 的集合**，譬如，以「碗」這個字說明，當你在紙上、平板上看到「碗」這個字時，他是以象形的具體方式表現出來，而當你念出「碗」的讀音時，他則是以聲音的具體方式表現出來，因此，一個 Character 的具體表現方式可以有很多種，若要把一個 Character 的不同具體表現方式都納入到 Character Set 的話，那這個 Set 會過於龐大，因此，才特別說明 ***Character Set*** 指的是***抽象***的 ***Character*** 的集合。
而 ***Character Set*** 為每一個抽象的 ***Character*** 都分配了一個唯一的**整數編號**(***code point***)，因此，這個 ***Character Set*** 就有了順序，成為了一個***編碼字符集(Coded character set)***，而我們也能透過編號(***code point***)去找出對應的 ***Character*** 是誰。 
當然，Character 在不同的***編碼字符集(Coded character set)*** 所對應的  code point 肯定不同，譬如「你」這個 ***Character***，在 ***Unicode*** 跟其他***編碼字符集(Coded character set)*** 的 ***code point*** 肯定就**不同**。
>[!NOTE] code point 指的並不是 Char 儲存在電腦的 binary code，僅是一個代表這個 Char 的唯一編號而已，Char 具體儲存於電腦的 binary code 是由 [[Character Encoding]](ex: UTF-8、UTF-16 等) 所決定。

# Unicode
Unicode 就是一種典型的編碼字符集，並不是 [[Character Encoding]]。
# 參考資料:
[编码字符集和字符集编码傻傻分不清楚！看完这篇文章你就懂了？_charset和characterencoding的区别-CSDN博客](https://blog.csdn.net/chuixue24/article/details/130348165)