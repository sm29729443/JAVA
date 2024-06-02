---
aliases:
  - File Object
  - File 類
  - File Class
---
# JAVA 中的 File Class
>[!NOTE]  JAVA File Class 的功能
>- 看完本篇後，會發現 File 並不涉及任何對文件「內容」的讀寫操作，僅涉及到新增、刪除、獲取文件訊息等操作，若想對文件內容進行操作，需要使用 [[Java IO Stream|I/O Stream]]。
>- File Object 通常是作為 [[Java IO Stream|I/O Stream]] 操作文件的起點出現。
>	也就是說，若想透過 JAVA [[Java IO Stream|I/O Stream]] 操作一個文件，可以先創建一個 File Object 來映射外部文件，然後再透過 JAVA 的 [[Java IO Stream|I/O Stream]] 來操作這個文件的內容。 
>	另外，若觀察 I/O Stream 的底層程式碼，可以看到只要是要對文件進行操作，那就一定得先透過 File Class 加載文件到 JAVA，像 `FileReader(String name)` 的底層也是會隱式的幫我們 `new File(name)`。
## 1. 位於 java.[[IO流|io]] packeage
## 2. Contructor
>[!NOTE] 3 個 Contructor 的創建方式
>相對路徑: 以 IDEA 來說，若是使用 junit test，則是以當前 module 來說，若是使用 main，則是當前 project。
>無論路徑下是否真的存在檔案或資料夾，都不會影響 File 物件的創建。
>```java
>// 1. pathName 可以是絕對路徑或相對路徑
>public File(String pathName)
>// 2. 以 parent 為父路徑，child 為子路徑創建
>public File(String parent, String child)
>// 3. 以 parent 為父文件，child為子路徑創建
>public File(File parent, String child)
## 常用方法
### 獲取 File 的基本訊息
- `public String getName()` ：取得名稱 
- `public String getPath()` ：取得路徑 
- `public String getAbsolutePath()`：取得絕對路徑 
- `public File getAbsoluteFile()`：取得絕對路徑表示的文件，就是用 `getAbsolutePath()` 當作 pathName new出一個 File 物件再 return 回來。
- `public String getParent()`：取得上層檔案目錄路徑。 若無，返回null 
- `public long length()` ：取得檔案長度（即：位元組數）。 不能取得目錄的長度。 
- `public long lastModified()` ：取得最後一次的修改時間，毫秒值
### 列出 File 的下一級
- `public String[] list()` ：傳回String數組，表示該File目錄中的所有子檔案或目錄。
- `public File[] listFiles()` ：傳回一個File數組，表示該File目錄中的所有的子檔案或目錄。
### File 類的重創建
- `public boolean renameTo(File dest)`:把文件重創建到指定的文件路徑。
### 判斷功能
- `public boolean exists()` ：此File表示的檔案或目錄是否實際存在。
- `public boolean isDirectory()` ：此File表示的是否為目錄。
- `public boolean isFile()` ：此File表示的是否為檔案。
- `public boolean canRead()` ：判斷是否可讀
- `public boolean canWrite()` ：判斷是否可寫
- `public boolean isHidden()` ：判斷是否隱藏
### 創建、刪除功能
- `public boolean createNewFile()` ：建立檔案。 若檔案存在，則不創建，回傳false。
- `public boolean mkdir()` ：建立檔案目錄。 如果此檔案目錄存在，就不建立了。 如果此檔案目錄的上層目錄不存在，也不建立。
- `public boolean mkdirs()` ：建立檔案目錄。 如果上層檔案目錄不存在，一併建立。
- `public boolean delete()` ：刪除檔案或資料夾
  > 刪除注意事項：
   >- ① Java中的刪除不走回收站。 
   >- ② 若要刪除一個檔案目錄，請注意該檔案目錄內不能包含檔案或檔案目錄。