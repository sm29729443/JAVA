在 JAVA 中，Char 就是占用 2 byte，並且 JAVA 是使用 Unicode [[Character Set|字符集]]當作 Char 的編碼[[Character Set|字符集]]。
JAVA 中的 String 在 JDK 9 之前底層是以 Char Array 在儲存者數據，但因為 JAVA Char 就是占用 2 byte，這導致像數字、英文字母等原本在文件只占用 1 byte 的數據，加載到 JAVA 後依然會占用 2 byte 導致資源浪費，因此在 JDK 9 之後，String 底層改以 Byte Array 儲存數據，並且 JAVA 默認使用 UTF-16 編碼，這代表 String 底層存的這個 Byte Array，實際儲存的就是 UTF-16 編碼後的 binary code。
# JAVA 中的 UTF-16
對於 JAVA 中使用 UTF-16 還不是很明白到底用在哪，之後再細究。
[JAVA中其实用的是UTF-16编码_java的字符编码是utf16-CSDN博客](https://blog.csdn.net/weixin_44958119/article/details/115579329)

>[!NOTE] 但是，java 默認使用 utf-16編碼的話，就算是英文不也還是得占用 2 byte? 並沒有比使用 char 當底層資料結構更划算呀?
>Claude 3 Ans:
你提出了一個很好的觀點。讓我來詳細解釋一下。
在 JDK 9 中引入的 Compact Strings(緊湊字符串)設計,並不是簡單地將所有字符都使用 UTF-16 編碼存儲,而是根據字符的範圍選擇使用 Latin-1 編碼或 UTF-16 編碼。
對於字符串中只包含 Latin-1 (ISO-8859-1)[[Character Set|字符集]]中的字符(編碼範圍為 0-255)的情況,Compact Strings 設計會使用 1 個 byte 來存儲每個字符。這樣,對於常見的英文字母、數字等 ASCII 字符,就可以節省存儲空間,每個字符只占用 1 個 byte,而不是之前的 2 個 byte。
只有當字符串中包含 Latin-1 [[Character Set|字符集]]之外的其他 Unicode 字符時,才會使用 UTF-16 編碼,每個字符占用 2 個 byte 進行存儲。
這種設計的優勢在於:
1.對於常見的英文字符串,可以節省存儲空間,提高內存利用率。
>2. 對於包含其他 Unicode 字符的字符串,仍然可以正確地使用 UTF-16 編碼進行存儲,確保對 Unicode [[Character Set|字符集]]的全面支持。
>3. 字符串的編碼方式是自適應的,根據字符串的內容選擇合適的編碼方式,透明地進行編碼和解碼,開發者無需要顯式處理編碼和解碼的問題。
雖然 UTF-16 編碼確實佔用 2 個 byte,但通過 Compact Strings 設計,Java 可以在常見的 ASCII 字符範圍內使用 1 個 byte 進行存儲,節省空間。只有在需要表示 Unicode 字符時才使用 UTF-16 編碼。
這種設計權衡了存儲空間和編碼支持的需求,在提高內存利用率的同時,也保證了對 Unicode [[Character Set|字符集]]的全面支持。
總結一下,雖然 Java 默認使用 UTF-16 編碼,但 Compact Strings 設計在 JDK 9 中引入了一種自適應的存儲方式。對於只包含 Latin-1 字符的字符串,每個字符使用 1 個 byte 存儲,節省了空間;而對於包含其他 Unicode 字符的字符串,則使用 UTF-16 編碼進行存儲。這種設計在提高內存利用率的同時,也保證了對 Unicode [[Character Set|字符集]]的完整支持,是一種權衡的結果。