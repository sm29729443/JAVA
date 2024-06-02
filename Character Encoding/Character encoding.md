---
aliases:
  - 字符集編碼
  - 字元編碼
  - Character Encoding
Created-Date: 2024-05-03T09:38:00
Last-Modified-Date: 2024-05-03T09:38:00
---
# 需要先讀過 [[Character Set]]
# Character Encoding
讀過 [[Character Set]] 後，我們知道 ***編碼[[Character Set|字符集]]*** 僅是提供了 Character 一個標示他的唯一整數編號(code point)，但不決定 Char 具體儲存在電腦中的 binary code，而這正是Character Encoding 的功能，**Character Encoding 具體決定了如何將 Char 的 code point 對應到一個儲存於電腦中的 binary code。**
## Unicode 的 Character Encoding
Unicode 總共有 UTF-8、UTF-16、UTF-32 三種 Character Encoding，下面以 UTF-8 舉例說明。
### Example:
現在，具體來查看一個以 [[UTF-8]] 所編碼的文字文件他的內容及底層 binary code 是多少。
透過查找 Unicode Character List，可以知道數字1、2、3 的 code point 為 0x0031(Decimal 49)、0x0032(Decimal 50)、0x0033(Decimal 51) ，而在下圖中透過 notepad++ 以 16進制 的方式打開檔案所看到的數據相互呼應。
![[Pasted image 20240503101102.png]]
>[!NOTE] 這裡儲存的 binary code 直接就是 code point 是因為對於英文與數字(ASCII 字符)而言，code point 基本上就直接是儲存在底層的 binary code，但如果是中文字的話，code point 基本上決不是儲存於底層的 binary code，看具體的編碼格式決定(UTF-8、UTF-16等)。
>Claude 3 說 UTF-8 在設計時為了與 ASCII 保持一致性，因此才設計成了 code point 會直接是 binary code。

>[!NOTE] 我與 Claude 3 對於 ascii 與 unicode 的關係探討
>我的問題:所以，ascii因為code point就是儲存於底層的binary code，因此很難劃清他是coded character set還是character encoding，並且 unicode 直接採用了 ascii 的code point，而因為 ascii 的 code point 就直接是 binary code，因此utf-8為了兼容 ascii 就直接也把unicode裡的ascii 的 code point 當作是 binary code?
> Claude3 Answer:
> 你的理解非常正確!讓我來進一步解釋一下。
ASCII 最初被設計為一種字符編碼方式,其 code point 直接對應於字符在計算機中存儲的二進制值(binary code)。當時,每個字符都用一個字節(8 位)來表示,因此 ASCII 的 code point 範圍被限制在 0 到 127 之間。這種設計使得 ASCII 的 code point 與字符的二進制表示之間有了直接的對應關係。
正因為這種設計,ASCII 兼具了編碼字符集(定義了字符和 code point 的對應)和字符編碼(定義了 code point 到二進制值的轉換)的特點。這也是為什麼有時會難以嚴格區分 ASCII 是編碼字符集還是字符編碼的原因。
當 Unicode 被設計出來時,它的目標是為所有的字符提供唯一的 code point,包括 ASCII 字符集中的字符。為了保持與 ASCII 的兼容性,Unicode 直接採用了 ASCII 的 code point,即 Unicode 中的前 128 個 code point 與 ASCII 完全相同。
UTF-8 作為一種 Unicode 的字符編碼方式,在設計時也考慮了與 ASCII 的兼容性。為了實現這種兼容性,UTF-8 在編碼 ASCII 字符時,直接將字符的 code point 作為編碼後的字節值。這樣,對於 ASCII 字符範圍內的字符,它們的 UTF-8 編碼與 ASCII 編碼完全相同。
舉個例子,字母 "A" 的 ASCII code point 是 65,在 ASCII 編碼中,它的二進制表示就是 01000001。而在 UTF-8 編碼中,字母 "A" 的編碼也是 01000001,與 ASCII 編碼相同。
對於 ASCII 範圍之外的 Unicode 字符,UTF-8 使用多個字節來表示,通過特定的位模式來標識字節的數量和字符的 code point。
總結一下:1. ASCII 的設計使得其 code point 直接對應於字符的二進制表示,因此它兼具編碼字符集和字符編碼的特點。2. Unicode 在設計時,為了保持與 ASCII 的兼容性,直接採用了 ASCII 的 code point。3. UTF-8 在編碼 ASCII 字符時,也直接將字符的 code point 作為編碼後的字節值,以實現與 ASCII 的兼容性。
這種設計使得 ASCII、Unicode 和 UTF-8 之間能夠很好地互操作,並保持了向後兼容性。


# 參考資料:
[编码字符集和字符集编码傻傻分不清楚！看完这篇文章你就懂了？_charset和characterencoding的区别-CSDN博客](https://blog.csdn.net/chuixue24/article/details/130348165)