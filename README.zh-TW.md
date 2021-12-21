# os_filesystem - 一個虛擬文件系統（C++）

## 簡介

這是一個仿linux的虛擬文件系統，系統由一個虛擬磁盤文件承載，以文件讀寫模擬磁盤讀寫，不涉及底層驅動。

寫一個簡單的仿linux文件系統，首先需要設計好包含inode、block、superblock、虛擬磁盤佈局，空間分配等信息的基本框架。文件系統的開頭是一個superblock，包含系統的重要信息，包括inode和block的數量和大小。對於inode，一般來說需要佔磁盤空間的百分之一，不過這是個小系統，總大小才5M多一點，所以分配給inode區的空間很少，剩下的空間大部分是block區。

該文件系統的總體規劃如下：  
![](./screenshots/00.png)

由於寫程序的時候時間比較緊張，只寫了4天就去驗收，所以代碼沒來得及優化，有的地方會顯得冗餘，大家不要見怪。

雖然時間有限，不過也額外實現了一個vi編輯器的功能，寫的比較簡陋，代碼也很亂，有時間改進一下。

總的來說，代碼還有待優化，歡迎多提意見，多挑毛病。

## 如何使用

### step 1：下載項目

`git clone https://github.com/windcode/os_filesystem.git`

### step 2：用VC++6.0打开项目

双击目录中的 **明操作系統**文件，或者將該文件拖到VC++6.0界面中。

### step 3：編譯，鏈接，運行

![](./screenshots/0.png)

### 或者

### step 1：直接運行**/調試**文件夾下**明操作系統**文件

## 特性

-   初次運行，創建虛擬磁盤文件

![](./screenshots/1.png)

-   登錄系統

默認用戶為root，密碼為root

![](./screenshots/2.gif)

-   幫助命令（help）

![](./screenshots/3.gif)

-   用戶添加、刪除、登錄、註銷（useradd、userdel、logout）

![](./screenshots/5.gif)

-   修改文件或目錄權限（chmod）

![](./screenshots/6.gif)

-   寫入、讀取受權限限制

![](./screenshots/7.gif)

-   文件/文件夾添加、刪除（touch、rm、mkdir、rmdir）

![](./screenshots/8.gif)

-   查看系統信息（super、inode、block）

![](./screenshots/9.gif)

-   仿寫一個vi文本編輯器（vi）

![](./screenshots/4.gif)

-   索引節點inode管理文件和目錄信息

-   使用**成組鏈接法**管理空閒block的分配
        * **block分配过程：**
    當需要分配一個block的時候，空閒塊堆棧頂拿出一個空閒塊地址作為新分配的block。
    當棧空的時候，將棧底地址代表的空閒塊中堆棧作為新的空閒塊堆棧。
        * **block回收过程：**
    當回收一個block的時候，檢查堆棧是否已滿，如果不滿，當前堆棧指針上移，將要回收的block地址放在新的棧頂。
    如果堆棧已滿，則將要回收的block作為新的空閒塊堆棧，將這個空閒塊堆棧棧底元素地址置為剛才的空閒塊堆棧。
        * 分配和回收的同时需要更新block位图，以及超级块。

-   inode的分配/回收  
    -   inode的分配和回收較為簡單，採用順序分配和回收。
    -   需要分配時，從inode位圖中順序查找一個空閒的inode，查找成功返回inode的編號。
    -   回收的時候，更新inode位圖即可。
    -   分配和回收都需要更新inode位圖。

## 注意

-   運行環境為VC++6.0
