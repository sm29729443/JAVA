---
aliases:
  - io stream
  - io Stream
  - IO Stream
  - io 流
  - IO 流
  - I/O 流
  - i/o 流
  - I/O Stream
  - i/o stream
  - i/o Stream
  - JAVA I/O Stream
Created-Date: 2024-05-02T09:59:00
Last-Modified-Date: 2024-05-02T09:59:00
---
# *JAVA IO Stream 的原理及功能*
>[!NOTE] JAVA I/O Stream 位於 java.io package 底下 
## 功能
在 JAVA 程式中，若想對 JAVA 程式外的資料(文件、資料夾、或透過網路傳輸等)進行讀寫操作，那就得透過 JAVA I/O Stream 來對這些 JAVA 程式外的進行操作。
## 原理
- 在 JAVA 中，把數據的 Input/Output 看成是一種數據的流動(Stream)。
- 而所謂的 Input/Output，指的是 JAVA 程式(即Memory)的角度下去看待。
	- 輸入(Input):將資料從外部(硬碟、光碟、網路等)輸入至 JAVA 程式(Memory)中。
	- 輸出(Output):將資料從 JAVA 程式(Memory) 輸出至外部(硬碟、光碟、網路等)。
	- ![[Pasted image 20240502105425.png]]W
# *JAVA I/O 的分類*
在 java.io package 底下提供了很多關於 stream 的 Class 及 Interface，而按照傳輸長度、使用方式等等又可區分為以下。
## 按照 Stream 傳遞資料的方向分類:
### 輸入流(Input Stream):
- 把數據從其他地方讀取到 JAVA程式(Memory) 中的 Stream。
- 在 JAVA 中，此功能的 Stream 都會以 InputStream、Reader 結尾。
### 輸出流(Output Stream):
- 把數據從 JAVA程式(Memory) 寫出到其他地方的 Stream。
- 在 JAVA 中，此功能的 Stream 都會以 OutputStream、Writer 結尾。
## 按照 Stream 傳遞資料的單位分類:
### 位元組流(Byte Stream):
- 以 1Byte(8 Bit) 為單位，讀寫數據的 Stream。
- 以 InputStream、OutputStream 結尾。
### Character Stream:
- 以 2 Byte(16 Bit) 為單位，讀寫數據的 Stream。
- 以 Reader、Writer 結尾。
## 按照 Stream 的角色不同分類:
**==這段分類沒很明白是什麼意思。==**
### 節點流:
指的是直接從數據源或目的地讀寫資料的 Stream。![[Pasted image 20240502111526.png]]
### 處理流:
指的不直接連接到數據源或目的地，而是連接到已存在的 Stream(可以是節點流也能是處理流) 上的 Stream，透過對數據的處理為程序提供更強大的讀寫功能。![[Pasted image 20240502111713.png]]
![[Pasted image 20240502111928.png]]
## JAVA 中關於 I/O Stream 的衍生類
以下都是由 InputStream、OutputStream，Reader、Writer 所衍生。
![[Pasted image 20240502112351.png]]
# *FileReader/FileWriter 的使用*
- 雖然 Stream 的衍生類很多，但使用規則上是很規範的，也就是只要熟悉一組，基本上大部分的 Stream Class 使用都沒問題。
>[!NOTE] 若是要讀取 img 圖片的話，要使用 FileInputStream，FileReader 讀取數據的方式是以 char 的方式，不能讀取圖片。
>這是因為 `Reader.read()` 的底層程式碼就是將讀取到的 16 Bit 數據轉換成 JAVA 的 char 資料型態，然後再 return 這個 char 的 Unicode 編碼值，譬如，當讀取到 0x0031 時，會將 0x0031 轉換成 char，也就是 數字1 ，然後 return 編碼值的十進位制 49 ，也就是 0x0031(HEX) -> 49(Decimal)，而若假設讀取到了一個無法轉換成 char 的 16 Bit 數據，則會 return 65533(HEX FFFD) 代表此 數據 無法解碼成 char，而FileWriter再寫入數據時，默認是使用 UTF-8 的編碼格是在寫入，因此 FFFD 透過 UTF-8 編碼後會變成EF BF BD，因此，若假設今天圖片的源數據可以完美映射成 char ，不產生任何 65533 的話，那透過 `Reader.read()` 應該扔會完美的複製圖片的源數據才對。
> 至於 Unicode -> UTF-8 的轉換邏輯這裡先不深究。
## *FileReader/FileWriter example*
>[!NOTE] FileReader/FileWriter 的使用範例
>- 對於 FileReader 來說，要讀取的 File 必須存在，否則會報 FileNotFoundException。
>- 對於 FileWriter 來說，要寫入的 File 可以不存在，會自動建立一個新的並寫入。
>```java
>public static void main(String[] args) {  
// 需求: 讀取 hello.txt 檔案  
FileReader fileReader = null;  
try {  
// 創建 File Obj 映射欲讀取的文件  
File file = new File("src/fileiotest/hello.txt");  
// 將欲讀取的 File Obj 傳遞到 FileReader 裡  
fileReader = new FileReader(file);  
int data;  
// 透過 JAVA I/O Stream 提供的方法讀取文件內容  
while ((data = fileReader.read()) != -1) {  
System.out.println((char) data);  
}  
} catch (IOException e) {  
throw new RuntimeException(e);  
} finally {  
try {  
if (fileReader != null) {  
// 關閉 Stream 的資源  
fileReader.close();  
}  
} catch (IOException e) {  
throw new RuntimeException(e);  
}  
}  
// 需求: 寫出數據到指定的文件中  
File file = new File("info.txt");  
FileWriter fileWriter = null;  
try {  
/*  
觀看 FileWriter Contructor 會看到還有個欄位是 boolean append若是文件本身已存在內容，則當 append 設定為 false 時，會直接把寫入的內容  
覆蓋掉原本文件的內容，若設定為 true，則會在原本的內容後拼接要寫入的內容  
*/  
fileWriter = new FileWriter(file);  
fileWriter.write("I V E");  
fileWriter.write("R G B");  
} catch (IOException e) {  
throw new RuntimeException(e);  
} finally {  
try {  
if(fileWriter != null) {  
fileWriter.close();  
}  
} catch (IOException e) {  
throw new RuntimeException(e);  
}  
}  
}


# *FileInputStream/FileOutputStream 的使用*
看教學的 md 檔看起來與 FileReader/FileWriter 使用上是一樣的，差別在 FileInputStream/File OutputStream 因為傳輸的資料長度為 byte，因此在使用 read() 裝載讀出來的資料時，從 char[] 變成了 byte[] 而已，因此不做詳細筆記。
## *FileInputStream example*
>[!NOTE] FileInputStream 範例(此為尚硅谷課件的範例)
>```java
>public class FISRead {  
//注意：应该使用try-catch-finally处理异常。这里出于方便阅读代码，使用了throws的方式  
@Test  
public void test() throws IOException {  
// 使用文件名称创建流对象  
FileInputStream fis = new FileInputStream("read.txt");  
// 读取数据，返回一个字节  
int read = fis.read();  
System.out.println((char) read);  
read = fis.read();  
System.out.println((char) read);  
read = fis.read();  
System.out.println((char) read);  
read = fis.read();  
System.out.println((char) read);  
read = fis.read();  
System.out.println((char) read);  
// 读取到末尾,返回-1  
read = fis.read();  
System.out.println(read);  
// 关闭资源  
fis.close();  
/*  
文件内容：abcde  
输出结果：  
a  
b  
c  
d  
e  
-1  
*/  
}  
@Test  
public void test02()throws IOException{  
// 使用文件名称创建流对象  
FileInputStream fis = new FileInputStream("read.txt");  
// 定义变量，保存数据  
int b;  
// 循环读取  
while ((b = fis.read())!=-1) {  
System.out.println((char)b);  
}  
// 关闭资源  
fis.close();  
}  
@Test  
public void test03()throws IOException{  
// 使用文件名称创建流对象.  
FileInputStream fis = new FileInputStream("read.txt"); // 文件中为abcde  
// 定义变量，作为有效个数  
int len;  
// 定义字节数组，作为装字节数据的容器  
byte[] b = new byte[2];  
// 循环读取  
while (( len= fis.read(b))!=-1) {  
// 每次读取后,把数组变成字符串打印  
System.out.println(new String(b));  
}  
// 关闭资源  
fis.close();  
/*  
输出结果：  
ab  
cd  
ed  
最后错误数据`d`，是由于最后一次读取时，只读取一个字节`e`，数组中，  
上次读取的数据没有被完全替换，所以要通过`len` ，获取有效的字节  
*/  
}  
@Test  
public void test04()throws IOException {  
// 使用文件名称创建流对象.  
FileInputStream fis = new FileInputStream("read.txt"); // 文件中为abcde  
// 定义变量，作为有效个数  
int len;  
// 定义字节数组，作为装字节数据的容器  
byte[] b = new byte[2];  
// 循环读取  
while (( len= fis.read(b))!=-1) {  
// 每次读取后,把数组的有效字节部分，变成字符串打印  
System.out.println(new String(b,0,len));// len 每次读取的有效字节个数  
}  
// 关闭资源  
fis.close();  
/*  
输出结果：  
ab  
cd  
e  
*/  
}  
}
## FileInputStream example
>[!NOTE] FileOutputStream 範例(此為尚硅谷課件的範例)
>```java
>public class FOSWrite {  
//注意：应该使用try-catch-finally处理异常。这里出于方便阅读代码，使用了throws的方式  
@Test  
public void test01() throws IOException {  
// 使用文件名称创建流对象  
FileOutputStream fos = new FileOutputStream("fos.txt");  
// 写出数据  
fos.write(97); // 写出第1个字节  
fos.write(98); // 写出第2个字节  
fos.write(99); // 写出第3个字节  
// 关闭资源  
fos.close();  
/* 输出结果：abc*/  
}  
@Test  
public void test02()throws IOException {  
// 使用文件名称创建流对象  
FileOutputStream fos = new FileOutputStream("fos.txt");  
// 字符串转换为字节数组  
byte[] b = "abcde".getBytes();  
// 写出从索引2开始，2个字节。索引2是c，两个字节，也就是cd。  
fos.write(b,2,2);  
// 关闭资源  
fos.close();  
}  
//这段程序如果多运行几次，每次都会在原来文件末尾追加abcde  
@Test  
public void test03()throws IOException {  
// 使用文件名称创建流对象  
FileOutputStream fos = new FileOutputStream("fos.txt",true);  
// 字符串转换为字节数组  
byte[] b = "abcde".getBytes();  
fos.write(b);  
// 关闭资源  
fos.close();  
}  
//使用FileInputStream\FileOutputStream，实现对文件的复制  
@Test  
public void test05() {  
FileInputStream fis = null;  
FileOutputStream fos = null;  
try {  
//1. 造文件-造流  
//复制图片：成功  
// fis = new FileInputStream(new File("pony.jpg"));  
// fos = new FileOutputStream(new File("pony_copy1.jpg"));  
//复制文本文件：成功  
fis = new FileInputStream(new File("hello.txt"));  
fos = new FileOutputStream(new File("hello1.txt"));  
//2. 复制操作（读、写）  
byte[] buffer = new byte[1024];  
int len;//每次读入到buffer中字节的个数  
while ((len = fis.read(buffer)) != -1) {  
fos.write(buffer, 0, len);  
// String str = new String(buffer,0,len);  
// System.out.print(str);  
}  
System.out.println("复制成功");  
} catch (IOException e) {  
throw new RuntimeException(e);  
} finally {  
//3. 关闭资源  
try {  
if (fos != null)  
fos.close();  
} catch (IOException e) {  
throw new RuntimeException(e);  
}  
try {  
if (fis != null)  
fis.close();  
} catch (IOException e) {  
throw new RuntimeException(e);  
}  
}  
}  
}
# *處理流*
## 緩衝流
### 功能及分類
#### 功能
緩衝流的目的是為了減少 I/O 的次數，若假設節點流要輸出 10kb 的資料到某一個指定文件，但一次只能輸出 1kb 那與硬碟交互的次數就為 10 次，而 I/O 是非常耗時間的，因此，緩衝流的目的就是為了減少 I/O 的次數，緩衝流就是會設立一個緩衝區，假設緩衝區的大小為 5kb ，那節點流會先將數據儲存到緩衝區，等到緩衝區滿了後，再一次向硬碟寫入 5kb 的資料，以減少 I/O 的次數。
**若查看 BufferedInputStream 的源碼，會發現就是創建了一個默認大小為 8kb 的 byte 陣列。**
![[Pasted image 20240502160309.png]]

#### 分類
##### 字節(Byte)緩衝流:用來處理非文本文件的
BufferedInputStream、BufferedOutputStream。
##### 字符(Character)緩衝流:用來處理文本文件的
BufferedReader、BufferedWriter。
### BufferedInputStream/BufferedOutputStream Example
>[!NOTE] Example
>BufferedStream 還有多提供一些 Method，但我覺得沒很複雜，這裡就不紀錄相關範例了。
>```java
>@Test  
public void test5(){  
FileInputStream fileInputStream = null;  
FileOutputStream fileOutputStream = null;  
BufferedInputStream bufferedInputStream = null;  
BufferedOutputStream bufferedOutputStream = null;  
try {  
// 建立要讀寫的文件  
File file1 = new File("anime.png");  
File file2 = new File("anime_copy.png");  
// 建立節點流  
fileInputStream = new FileInputStream(file1);  
fileOutputStream = new FileOutputStream(file2);  
// 建立緩衝流，並且把要緩衝的節點流放入至緩衝流  
bufferedInputStream = new BufferedInputStream(fileInputStream);  
bufferedOutputStream = new BufferedOutputStream(fileOutputStream);  
int len;  
// 原本是對節點流操作，改成對緩衝流操作而已  
while ((len = bufferedInputStream.read()) != -1) {  
bufferedOutputStream.write(len);  
}  
} catch (IOException e) {  
throw new RuntimeException(e);  
} finally {  
// 關閉資源，當關閉外層緩衝流時，內層節點流也會一起被關閉  
try {  
bufferedInputStream.close();  
bufferedOutputStream.close();  
} catch (IOException e) {  
throw new RuntimeException(e);  
}  
}  
}
## 轉換流
在介紹轉換流的功能時，需要先了解什麼是 ***[[Character Set 與 Character Encoding]]***。
### 轉換流的功能
轉換流的功能就是將 Byte -> Char 或 Char -> Byte。
![[Pasted image 20240503120650.png]]![[Pasted image 20240503120730.png]]
### Example 1:
題目:使用轉換流讀取一個以 utf-8 編碼的文件
```java
@Test  
public void test6() throws IOException {  
// 創建文件  
File file = new File("abc_utf8.txt");  
// 以 byte stream 讀取文件  
FileInputStream fileInputStream = new FileInputStream(file);  
// 用轉換流把 byte stream 轉換成 char stream，並指定 解碼格式  
// 若是使用非 utf-8 的解碼格式去解碼以 utf-8 編碼的文件，則會看到解碼出來的都是亂碼  
InputStreamReader inputStreamReader = new InputStreamReader(fileInputStream, "utf-8");  
char[] len = new char[3];  
while (inputStreamReader.read(len) != -1) {  
System.out.println( len );  
}  
inputStreamReader.close();  
}
```

### Example 2:
題目: 將 utf-8 文檔複製一份但編碼方式以 big5 編碼。
補充: java 如何將 utf-8 -> big5 的過程目前還沒詳細研究。
```java
@Test  
public void test7() throws IOException {  
// 讀取檔案的宣告  
File file1 = new File("abc_utf8.txt");  
FileInputStream fileInputStream = new FileInputStream(file1);  
InputStreamReader inputStreamReader = new InputStreamReader(fileInputStream, "utf-8");  
// 寫入檔案的宣告  
File file2 = new File("abc_ansi.txt");  
FileOutputStream fileOutputStream = new FileOutputStream(file2);  
OutputStreamWriter outputStreamWriter = new OutputStreamWriter(fileOutputStream, "big5");  
char [] chars = new char[3];  
while (inputStreamReader.read(chars) != -1) {  
outputStreamWriter.write(chars);  
}  
outputStreamWriter.close();  
inputStreamReader.close();  
}
```
## 數據流及對象流
### 數據流
#### 功能
- 可以將 JAVA 程式中的基本資料類型的變數寫入/寫出到流中。
- 但因為只能操作基本資料類型，所以基本上沒在用，都是用對象流。 
#### 具體類
DataInputStream、DataOutputStream。

### 對象流
#### 功能
可以讀寫基本資料類型/引用資料類型的變數。
#### 具體類
ObjectInputStream、ObjectOutputStream。


#### 物件的序列化機制
- 物件的序列化機制使得我們能將 JAVA 程式中的 JAVA 物件 轉換成一個與平台無關的***二進制流***，從而使得我們能永久地將此二進制流保存在硬碟上，也可通過網路將此***二進制流***傳輸出去，當其他系統程式獲得此二進制流時，再透過反序列化即可得到一個 JAVA 物件。


#### ObjectInputStream、ObjectOutputStream
#### ObjectOutputStream
物件的序列化由 ObjectOutputStream 實現，可以將 JAVA 物件儲存在一個文件中或透過網路傳輸出去。
#### ObjectInputStream
物件的反序列化由 ObjectInputStream 實現，可以將一個 Object 二進制流還原成一個 JAVA 物件。
#### Example
```java
@Test  
public void test8() throws IOException {  
File file = new File("object.txt");  
ObjectOutputStream objectOutputStream = new ObjectOutputStream(new FileOutputStream(file));  
// 序列化過程，即寫出數據  
// 若是要寫出物件，就是用 writeObject 方法  
objectOutputStream.writeUTF("大瓜瓜小瓜瓜都是瓜瓜");  
objectOutputStream.close();  
}  
@Test  
public void test9() throws IOException {  
File file = new File("object.txt");  
ObjectInputStream objectInputStream = new ObjectInputStream(new FileInputStream(file));  
// 反序列化，即讀取數據  
String s = objectInputStream.readUTF();  
System.out.println(s);  
objectInputStream.close();  
}
```
#### 自定義 Class 若想序列化的必要條件
- 需要 `implements Serializable` ，但不需要 override 任何方法，實現這個接口就像是一種標識，告訴 JVM 這是一個可序列化的物件。
- 需要聲明一個 `static final long serialVersionUID` 變數，具體值不可與其他 Class 相同即可。而 `serialVersionUID` 的作用就是在標識這個物件的所屬類，當沒有顯示的宣告 `serialVersionUID` 時，JAVA 底層會隱式的幫我們創建一個，但這個由底層自動創建的 `serialVersionUID`，可能會隨者 Class 的內容修改而改變，譬如，假設我透過網路把 Person Class 傳遞給了 B，之後修改了 Person Class 且在傳遞給 B，此時可能因為 `serialVersionUID` 的改變，導致 B 端程式認為這兩個 Person 是不同的 Class，因此沒辦法透過 (Person) objectInputStream 去拿到 Person 物件。
- 自定義 Class 的屬性也可是可序列化的，也就是說，對於引用數據類型的屬性，也得實現 Serializable。

#### 序列化的注意點
- 當對自定義 Class 的屬性加上 transient 時，代表該屬性並不會被序列化。
- 如果是 static 的屬性，也不會被序列化，因為 static 修飾的屬性是跟物件無關的，是跟者 Class 走的。