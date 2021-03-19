# Linux
## 1. 安装
1. 安装
    > 1. VM安装
    > 
    >    windows10家庭版，运行vm需要使用管理员权限。建议升级到专业版。
    >    
    > 2. 虚拟机安装时，网络连接类型：面试可能会问到
    > 
    >     * 桥接网络：虚拟机会和主机公用一个网段。虚拟机会自动生成一个IP，网段和主机相同。不使用。
    >     
    >        这样不好，因为：1、IP冲突。因为一个网段中的IP只能有1~255个，同时IP末尾为1和255的都用不了，实际只能用253个。因此可能会有IP不够用的情况。例如虚拟机申请的IP和局域网中其他电脑的IP冲突。2、不安全。虚拟机和主机公用一个网段，则该网段中其他的电脑都可以访问虚拟机。
    >     
    >         * 网段：IP地址前三位，例如192.168.1.132中的192.168.1就是网段。
    >     
    >     * 网络地址转换器：虚拟机和主机不会公用一个网段。虚拟机会转换主机网段，然后生成一个IP。使用该方式最好。
    >     
    >        解决了桥接网络的两大问题。
    >        
    >        虚拟机和主机不公用一个网段，按理说不可以通信。但是，因为虚拟机安装后，会生成两个虚拟网卡vmWare1和vmWare8在主机上，虚拟网卡中vmWare8和虚拟机的网段相同。
    >        
    >        因此，主机想要和虚拟机通讯，只需要通讯和vmWare8虚拟网卡即可。
    >     
    >     * 仅主机模式网络：仅保证虚拟机和主机在同一网段中。使用该功能虚拟机无法联网。
    > 3. Linux安装时手动分区：设置挂载点（文件夹）
    > 
    >     * 设置```/boot```：Linux启动的配置文件。一般设置2048mb，设置文件系统为ext4
    >     
    >     * 设置```swap```：交换分区，Linux内存不够用时，将此分区当做临时内存。一般设置2048mb，设置文件系统为swap
    >     
    >     * 设置```/```：根目录，Linux所有文件的根。不手动设置大小，设置文件系统为ext4
    > 3. xftp和xshell安装
    > 
    >    xftp：文件传输
    >    
    >    xshell：远程操作Linux 
    >    
## 2. Linux文件系统
2. Linux文件系统
    > 1. Linux文件结构
    >
    >     1. 介绍
    >     
    >        在Linux中，一切皆文件
    >        
    >     2. Linux文件结构
    >     
    >         * ```/``` ：根目录
    >         
    >             * **```/root```：需要牢记。系统管理员主目录，也叫超级权限者的用户主目录。**
    >             
    >             * **```/bin```：需要牢记。bin是Binary缩写，存放着最经常使用的命令，任何人都可以使用。在不同的文件夹中，也会有bin，```/usr/bin```，```/usr/local/bin```。这两个文件夹也要牢记。```/bin```其实就是```/usr/bin```的软链接**
    >             
    >             * **```/sbin```：牢记。s是Super User意思，存放系统管理员使用的系统管理程序。在不同的文件夹中，也会有sbin，例如：```/usr/sbin```，```/usr/local/sbin```。```/sbin```其实就是```/usr/sbin```的软链接**
    >               
    >                 * **牢记：**	
    >                 
    >                    **```/bin```，```/usr/bin```，```/usr/local/bin```，```/sbin```，```/usr/sbin```，```/usr/local/sbin```这六个文件夹的指令在Linux任何位置下都可以访问。因为环境变量中默认配置了其中五个文件夹路径。因为```/bin```是```/usr/bin```的软链接，因此```/bin```在环境变量中没有配置，不需要配置**
    >                
    >             * **```/home```：牢记。存放着普通用户的主目录，在Linux中，每个用户都有一个自己的目录，一般该目录名是用用户名来命名的** 
    >             
    >             * **```/etc```：牢记。所有的系统管理所需要的的配置文件和子目录**
    >             
    >             * **```/usr```：牢记。非常重要的目录，用户的很多应用程序和文件放在该目录下，类似于Windows中的Program File目录**
    >             
    >             * **```/usr/local```：牢记。存放额外安装软件的文件夹，一般存放的是通过编译源码方式安装的程序。**
    >             
    >             * **```/opt```：牢记。以后使用最多的目录。存放额外安装软件的文件夹，比如安装一个Oracle数据库就可以放在该目录下。默认为空。**
    >             
    >             * **```/var```：牢记。该目录存放不断扩充的东西。我们习惯将经常修改的文件放在该目录下，包括各种日志文件。**
    >             
    >             * **```/dev```：牢记。dev即Development缩写，类似于Windows的设备管理器，把所有的硬件用文件的形式存储起来。Linux一切皆文件最直观的体现**
    >             
    >             * **```/media```：牢记。Linux系统会自动识别一些设备，例如U盘、光驱等，识别后，会将设备挂载到该目录。只在centos6是这样，centos7设备全放在了```/run/media/```中**
    >             
    >             * **```/run```：牢记。进程产生的临时文件。虚拟机加载的光盘映像就在```/run/media/root/```下**
    >             
    >             * **```/boot```：牢记。存放启动Linux时需要的核心文件，包括镜像文件和连接文件，自己安装的不要放在此处。**
    >             
    >             * ```/prov```：了解。虚拟目录，它是系统内存的映射，可以通过它直接获取系统信息。
    >             
    >             * ```/srv```：了解。srv是Service缩写，存放服务启动之后要提取的数据。
    >             
    >             * ```/sys```：了解。linux2.6内核的新变化。该目录下安装了linux2.6出现的新文件系统sysfs。
    >             
    >             * ```/lib```：了解。Linux开机所需的动态连接共享库，类似于Windows中的DLL文件。几乎所有Linux应用程序都要用到这些共享库。
    >             
    >             * ```/tmp```：了解。存放临时文件
    >             
    >             * ```/lost+found```：了解。系统非法关机后，存放的文件
    >             
    >             * ```/mnt```：了解。让用户临时挂载别的文件系统，可以将外部存储挂载到```/mnt/```上，进入该目录就可以看到里面的内容
    >             * ...
    >         
    >     3. xshell中的复制快捷键为Ctrl+Insert，粘贴快捷键为Shift+Insert 
    >     
## 3. VI和VIM编辑器
3. VIM和VIM编辑器
    > 1. 介绍
    >
    >    类似于Windows下的记事本等，Linux下的文本编辑器，是用来对Linux下的文本进行编辑的。
    > 2. 命令：进入文件	
    >
    >    vi xxx文件 或 vim xxx文件
    >    
    >    因为VIM是VI的增强，因此一般都使用VIM
    >    
    > 3. VI和VIM的三种模式
    >
    >     1. 一般模式：默认进入的就是一般模式 。主要负责查看和一些简单的修剪
    >     
    >         * 常用命令：
    >         
    >             1. dd：删除光标所在行
    >             
    >             2. dnd：删除n行
    >             
    >             3. u：撤销上一步。可以一直撤销到最初形态
    >             
    >             4. x：删除一个字母，Delete从前往后删除一个字母  /  X：删除一个字母，BackSpace从后往前删除一个字母
    >             
    >             5. yy：复制光标所在行
    >             
    >             6. p：粘贴
    >             
    >             7. dw：删除一个词。从当前光标到下一个分隔符的位置。
    >             
    >             8. yw：复制一个词。从当前光标到下一个分隔符的位置。
    >             
    >             9. shift+g：光标移动到页尾
    >             
    >             10. 先按数字，再按shift+g：光标移动到数字指定的行数
    >             
    >             11. shift+6：光标移动到行头。因为正则表达式中，^代表行头，因此shift+6代表光标移动到行头
    >             
    >             12. shift+4：光标移动到行尾。因为正则表达式中，$代表行尾，因此shift+4代表光标移动到行尾
    >         
    >     2. 编辑模式：可以编写文字的模式。只有在一般模式中，按下i、a、o等字母才可以进入，进入后左下角有[insert]或[replace]字样；按下ESC退出编辑模式，进入一般模式
    >     
    >         * i、a、o按下去的效果
    >         
    >             1. i：光标前进行添加
    >         
    >             2. a：光标后进行添加
    >         
    >             3. o：光标下再创建一个新行，然后添加
    >             
    >         
    >     3. 命令模式：可以进行存盘、退出、显示行号、搜索、批量操作的模式。在一般模式下，输入:或者/进入命令模式，进入命令模式后光标进入最后一行；双击ESC退出命令模式
    >     
    >         * 常用命令
    >         
    >             1. :w -- 保存
    >             
    >             2. :q -- 退出
    >             
    >             3. :wq -- 保存并退出
    >             
    >             4. :q! -- 强制退出
    >             
    >             4. :set nu/:set nonu -- 显示行号/关闭行号
    >             
    >             5. :%s/旧字符串/新字符串/g -- 查找，并用新字符串替换旧字符串
    >             
    >             6. /要查找的词 -- 查找对应的词。n查找下一个，N查找上一个。
    >             
    >             7. :noh -- 取消高亮
    >             
## 4. 常用基本指令
4. 常用基本指令
   
    > 1. 常用基本指令。指令格式都参照帮助手册，[]代表可选。
### 1. 帮助手册
1. 帮助手册
    > 1. 帮助手册
    >    
    >     1. 具体指令 --help：进入该指令的帮助手册的文本形式。建议使用。
    >     
    >     2. man 具体指令：进入该指令的帮助手册。帮助手册中按空格进入下一页，按q直接退出。
    > 
### 2. 日期相关
2. 日期相关
    > 2. 日期有关
    >    
    >     1. ```date [选项1] [选项2] ... [+格式]```：以[格式]输出当前时间。选项、格式参见帮助手册。
    >        
    >        例子：```date +%Y-%m-%d' '%H:%M:%S```
    >        
    >         * **注意：Linux命令行中，空格用来做多个指令选项的分隔。如果想用普通空格，必须用' '表示**
    >        
    >     2. ```cal [选项] [[[日] 月] 年]```：cal即Calendar缩写。输出日历。
    > 
### 3. 显示当前目录（重要）
3. 显示当前目录
    > 3. **显示当前目录：```pwd```**
    >    
### 4. 切换目录（重要）
4. 切换目录
    > 4. **切换目录：**
    >    
    >     **1. ```cd ..```：切换到上一级目录**
    >         
    >     **2. ```cd /```：切换到根目录**
    >         
    >     **3. ```cd```或```cd ~```：返回家目录。在Linux命令行中，且只有在命令行中，~代表家目录**
    >         
    >     **4. ```cd /xx/xx/xx```：通过绝对路径访问目录**
    >         
    >     **5. ```cd ./../xx```：通过相对路径访问目录**
    >         
    >     
    >     * **注意：文件夹名称中有空格需要通过转义字符\\转义**
    >
### 5. 文件目录管理  （重要）
5. 文件目录管理    
    > 5. **文件目录管理**
    >    
    >     1. ```ls [选项]```：列出当前文件夹内的文件
    >        
    >         * 选项：
    >           
    >            * -a：列出全部文件，包括隐藏文件。Linux一切皆文件，文件夹也是文件。
    >              
    >            * -l： 列出文件详细信息列表。注意，```ls -l```可以简写为```ll```。
    >         
    >     2. ```| grep 关键词```：配合显示内容的命令，根据关键词查询指定内容。类似于模糊查询。任意显示内容的命令都可以加```| grep```做模糊查询。```| grep -i 关键字```，做不区分大小写的关键字模糊查询。
    >        * 实例：```ls -a | grep conf```
    >        
### 6. 文件和文件夹操作（重要）
6. 文件和文件夹操作
    > 6. **文件和文件夹操作**
    >    
    >     1. ```mkdir [选项] 文件夹的路径```：创建文件夹 
    >        
    >         * 选项：
    >           
    >             * -p：允许递归创建多级目录
    >         
    >     2. ```touch 文件路径```：创建一个新文件
    >        
    >         * 创建文件的另外两种方式：
    >         
    >             1. ```vi 文件路径```：打开一个文件，如果不存在，则创建一个新文件。注意，vim方式创建的文件必须有内容，否则无法保存
    >                
    >             2. ```vim 文件路径```：同上。一般用vim。
    >         
    >     3. ```rmdir 目录所在的路径```：删除一个**空目录**。
    >        
    >     4. ```rm [选项] 目录所在路径或文件所在路径```：删除文件或者目录。如果要删除多个，只需要在后面写多个路径，以空格隔开即可；路径写法，也支持\*.txt这种，代表将路径下的全部以txt结尾的都删除
    >        * 选项：
    >        
    >             1. -rvf：r代表Recursion即递归，v代表展示View，f代表强制force。递归删除**所有的目录内容**，给出删除内容的提示
    >               
    >             2. -rf：r代表Recursion即递归。递归删除**所有的目录内容**，不给出删除内容的提示
    >        
    >     5. ```cp [选项] 要复制的文件或文件夹路径 复制到的位置路径```：cp即copy。复制粘贴文件
    >        
    >         * 选项：
    >           
    >             1. -r：r代表Recursion即递归。递归复制整个文件夹所有内容
    >                
    >             2. -v：显示复制过程中文件的列表。
    >         
    >     6. ```mv 文件路径1 文件路径2```：重命名文件和剪切粘贴文件。
    >        
    >     7. ```cat 文件名```：查看轻量级文本文件，即查看内容少的文本文件。
    >        
    >         * cat命令的另外两个用法：
    >           
    >             * ```cat 文件1 文件2```：连接显示两个轻量级文件
    >               
    >             * ```cat 文件1 文件2>文件3```：创建新文件，内容为两个文件的合并
    >         
    >     8. ```more 文件名```：查看较长的文件。
    >        
    >         * 用more查看文件时，和用man查看参考手册一样。空格(翻页)，Enter(翻一行)，q(立即离开)，Ctrl+f(向下滚动一屏)，Ctrl+b(返回上一屏)
    >         
    >     9. ```less 文件名```：查看较长文件。功能比more多
    >        
    >         * 用less查看文件时，PageUp(向上翻页)，PageDown(向下翻页)，/xx(向下搜寻xx字符串)，?xx(向上搜寻xx字符串)，n(重复搜寻，搜寻方向与使用/xx还是?xx有关)，N(反向重复搜寻，方向与使用/xx还是?xx有关)
    >         
    >     10. ```tail [选项] 文件名```：从尾部开始，查看文件。非常适合日志的查看。
    >     
    >         * 选项：
    >           
    >             1. -f：跟随查看，即不退出查看
    >                
    >             2. -n行数：显示行数，n可以省略
    > 
### 7. 查看历史命令
7. 查看历史命令
    > 7. 查看历史命令：```history```
    >    
### 8. 输出环境变量
8. 输出环境变量
    > 8. 输出环境变量：```echo $PATH```
    > 
### 9. 查找命令
9. 查找命令
    > 9. 查找命令：```find 搜索路径 参数 "搜索关键字"```。提供了丰富的模糊搜索和条件搜索。
    >    
    >    find命令查询文件会先遍历目录下所有文件，找到符合条件的文件然后输出。因此效率低。
    >        
    >    * 实例：```find /home/esop -name "*.txt"```：搜索/home/esop文件夹下，文件名为，以txt后缀的文件
    >    
### 10. 查找命令
10. 查找命令
    > 10. 查找命令：```locate 文件路径```。比find命令高效的查找文件
    >     
    >
    >    locate命令是从内存中查找，因此更快。
    >     
    >    但是因为是从内存中查找，因此每次运行前要先updatedb来更新文件索引，否则它只会查找上次更新文件索引时加载进内存的文件。
    >     
    >    * 实例：```locate /home/esop/*.txt```：搜索/home/esop下的全部以txt后缀的文件
    >
### 11. 软链接
11. 软链接
    > 11. 软链接：也被称为符号链接，类似于Windows中的快捷方式，有自己的数据块，主要存放了链接到其他文件的路径。
    >     
    >     1. 软链接：```ln -s 原文件或目录路径 软链接路径```
    >        
    >     2. 查询：通过之前的```ll```命令即可查看到，软链接的列表属性第一位是l，同时尾部会有位置指向
    > 
### 12. 压缩文件与解压（重要）
12. 压缩文件与解压
    > 12. **压缩文件和解压缩文件**
    >     
    >     1. tar.gz后缀名的压缩文件：常用
    >        
    >         1. 压缩：```tar -zcvf 压缩文件地址(以tar.gz为后缀) 原文件1路径 原文件2路径 原文件3路径 ```或```tar -zcvf 压缩文件地址(以tar.gz为后缀) 原目录路径```
    >            
    >         2. 解压：```tar -zxvf 压缩文件地址(以tar.gz为后缀)```
    >         
    >     2. zip后缀名的压缩文件：不常用
    >        
    >         1. 压缩：
    >            
    >            ```zip 压缩文件地址(以zip为后缀) 原文件1路径 原文件2路径 原文件3路径 ```：压缩某几个文件
    >                
    >            ```zip -r 压缩文件地址(以zip为后缀) 某个路径/*```：r代表Recursion递归。批量压缩，压缩文件夹也用这种
    >            
    >         2. 解压：```unzip 压缩文件路径(以zip)```
    >            
    >         
    >     
    >     **3. 注意：压缩时，不要写原文件或目录的绝对路径，否则会将绝对路径下的全部目录都压缩，例如/root/桌面/E/F，压缩时会将root/桌面/E/F全部压缩**
    >     
### 13. 重启和关机（重要）
13. 重启和关机
    > 1. 重启：```reboot```
    > 
    > 2. 关机：```poweroff```
    > 
    > 3. 强制关机：```shutdown -h now```，仅root可用
    > 
    > 4. 强制重启：```shutdown -r now```，仅root可用
    >    
### 14. 磁盘操作
14. 磁盘操作
    > 14. 磁盘操作
    >     
    >     1. 磁盘分区
    >     
    >         1. 一个磁盘分为主分区和扩展分区。用户可以根据逻辑，将扩展分区分为多个逻辑分区。
    >         
    >         2. 常用的磁盘分区
    >         
    >             1. mbr：操作系统装在主分区，分为4个主分区，扩展分区占一个主分区。Windows7 64位以下使用该分区类型。
    >             
    >             2. gbt:无限主分区，支持超大硬盘3T以上。Windows7 64位以上使用该分区类型。
    >         
    >     2. 查看所有设备挂载情况：```lsblk```或者```lsblk -f```
    >        
    >         * 通过查看设备挂载命令可以看到，sda代表磁盘，sda1代表boot分区，sda2代表其他分区swap和根。sr0代表挂载的镜像文件光盘。
    >         
    >     3. 如何增加一块硬盘：
    >        
    >         1. 插硬盘，重启。虚拟机则可以直接右键虚拟机然后添加。
    >            
    >         2. 对硬盘进行分区。Linux中万物皆文件，因此硬盘也是一个文件，Linux硬件存储在/dev下。
    >            
    >             * 分区命令：```fdisk /dev/待分区磁盘```，输入分区命令后，开始进行分区，然后输入具体写入命令。例子```fdisk /dev/sdb```
    >               
    >             * 输入分区命令；输入n，新增分区，此时e代表扩展分区，p代表主分区；按下p选择主分区，分区号和起始扇区都默认即可直接回车；最后按下w写入并退出分区
    >               
    >                 * 写入命令：
    >                   
    >                     1. m：显示分区列表
    >                        
    >                     2. p：显示磁盘
    >                        
    >                     3. n：新增分区
    >                        
    >                     4. d：删除分区
    >                        
    >                     5. w：写入并退出分区
    >             
    >         3. 格式化分区。**注意，格式化的是新磁盘中的分区，给分区一个文件系统**
    >            
    >             * 格式化磁盘分区：```mkfs -t ext4 /dev/待格式化硬盘的分区```，ext4是文件系统类型。
    >               
    >             * 例子：```makf -t ext4 /dev/sdb1```
    >             
    >         4. 挂载。将一个硬盘分区和一个挂载点(目录)联系起来。要想使用该硬盘分区，必须挂载。类似于Windows下，U盘对应的目录和U盘空间的联系。
    >            
    >             1. 挂载：用命令行挂载，在重启后会失效
    >                
    >                 1. 挂载命令：```mount 设备名称 挂载目录```，如果挂载目录不存在，先创建一个挂载目录。类似于Windows下的插入U盘
    >                    
    >                 2. 取消挂载：```umount 设备名称或者挂载目录```，将挂载的硬件和挂载点切断联系，类似于Windows下的拔出U盘。取消挂载必须要在挂载目录外才可以进行。
    >                 
    >             2. 永久挂载
    >                
    >                 1. 修改/etc/fstab实现挂载。使用vim打开/etc/fstab，添加挂载信息：```待挂载硬盘分区的绝对路径 挂载点绝对路径 分区的文件系统 defaults 0 0```，例如```/dev/sdb1 /dev/newDisk1 ext4 defaults 0 0```
    >                 
    >                  2. 修改完成后，在挂载目录外，执行```mount -a```立即生效
    >         
    >     4. 通过上述添加硬盘后可知，Linux的磁盘分区是mbr类型。
    >     
    >     5. 查询磁盘整体使用情况：```df -h``` 
    >     
    >     6. 查询子目录的磁盘使用情况：```du -h /目录```
    >     
### 15. 网络配置
15. 网络配置
    > 1. 查看网络配置：```ifconfig```
    >    
    > 2. 如何修改IP
    >
    >     1. 图形化界面操作
    >     
    >        图形化界面设置IPv4，设置IP，子网掩码，网关，DNS
    >        
    >        **注意：一般设置网关会设置成网段.1，但是由于一个虚拟网卡已经占用了，因此要设置成网段.2。**
    >        
    >        **注意：DNS必须设置，且必须和网关保持一致**
    >        
    >     2. 使用vim修改网络配置文件：```vim /etc/sysconfig/network-scripts/ifcfg-ens33```
    >     
    > 3. 刷新网络设置：```service network restart```
    > 
### 16. 进程（重要）
16. **进程。进程英文process，简称ps**
    > 1. 显示当前终端的所有进程CPU占用率、内存占用率等信息：```ps -aux```  。后边可以加```| grep```模糊查询进程
    > 
    >     * PID：进程号；TIY：终端机号；TIME：此进程所需要的CPU时间；COMMAND：当前进程由哪个指令开启
    >     
    > 2. 显示当前终端的所有进程的进程号、父进程号等信息：```ps -ef```。后边可以加```| grep```模糊查询进程
    > 
    > 3. 根据进程的PID杀死进程：```kill 进程的PID```
    > 
    >     * 强制杀死某个进程：```kill -9 进程的PID```
    >     
### 17. 服务
17. 服务
    > 1. 服务介绍
    >
    >    服务就是注册在系统中的标准化程序
    >    
    > 2. centOS6中，服务使用service命令
    >
    >     1. 对于服务的管理方法
    >
    >         ```
    >         service 服务名 start
    >         ```
    >     
    >         ```
    >         service 服务名 stop
    >         ```
    >     
    >         ```
    >         service 服务名 restart
    >         ```
    >     
    >         ```
    >         service 服务名 reload
    >         ```
    >     
    >         ```
    >         service 服务名 status
    >         ```
    >     
    > 2. centOS7中，服务使用systemctl命令。但是某些指令还是沿用了service命令
    >
    >     1. 对于服务的管理
    >
    >         ```
    >         systemctl start 服务名
    >         ```
    >     
    >         ```
    >         systemctl stop 服务名
    >         ```
    >     
    >         ```
    >         systemctl restart 服务名
    >         ```
    >     
    >         ```
    >         systemctl reload 服务名
    >         ```
    >     
    >         ```
    >         systemctl status 服务名
    >         ```
    >         
    >     2. 查看服务的命令：```systemctl list-unit-files```。后边可以使用```| grep```进行服务的模糊查询。
    >     
    >     3. 设置服务的自启动和不自启动。
    >     
    >         1. 自启动服务：```systemctl enable 服务名``` 
    >         
    >         2. 不自启动服务：```systemctl disable 服务名```
    >     
    >         * **注意：一定要去关闭防火墙服务自启动，否则主机无法访问Linux上的Mysql或Redis等服务。通过```systemctl list-unit-files | grep firewalld```查找防火墙，然后使用关闭自启动的命令关闭防火墙服务，并且关闭防火墙服务。**
    >         
    >     4. centOS7中，将原本centOS6的七个运行级别简化为了两个。这两个也是最常用的
    >     
    >         1. multi-user.target：等价于centOS6中的等级3，无图形化界面
    >         
    >         2. graphical.target：等价于centOS6中的等级5，图形化界面
    >     
    >     5. 在centOS7中，使用```vim /etc/inittab```查看默认运行级别，同时可以修改运行级别。
    >     
### 18. 网络情况
18. 网络情况
    > 1. 查看网络情况：```netstat -anp```。可以使用```| grep```进行模糊查询
    > 
    >     * 如果要看哪个程序占用了哪个端口，使用```| grep```进行查询。例如查询8080端口的占用情况：```netstat -anp | grep 8080```
    >     
## 5. Linux用户与权限管理（重要）
### 1. 用户和用户组
1. 用户和用户组
    > 1. 用户
    > 
    >     1. Linux用户介绍
    >     
    >        Linux是个多用户多任务的操作系统。任何一个要使用系统资源的用户，都必须向管理员申请一个账号，然后以该账号的身份进入系统
    >        
    >     2. 新增用户：```useradd 新用户名```。root超级管理员才能完成
    >     
    >     3. 设置密码：```passwd 新增用户名```，然后输入密码即可。root超级管理员才能完成
    >     
    >     4. 用户信息：```id 用户名```，打印用户基本信息，也可以用来判断用户是否存在
    >     
    >         * 查看用户信息我们可以知道，root超级管理员的uid=0，其他用户的uid从1000包括1000，开始依次递增。
    >         
    >     5. 查看当前用户和查看最先登录的用户：```whoami```和```who am i```
    >     
    >     6. 切换用户：```su - 用户名```
    >     
    >     7. 删除用户：需要下列五个步骤。root超级管理员才能完成
    >     
    >         1. 执行删除命令：```userdel 用户名```，不完全删除
    >         
    >         2. 删除/home下的文件
    >         
    >         3. 删除/etc/passwd文件中的用户。经过验证，```userdel 用户名```可以自动删除/etc/passwd中的用户，不用执行这一步
    >         
    >         4. 删除/etc/group文件中的组。经过验证，```userdel 用户名```可以自动删除/etc/group中的用户，不用执行这一步
    >         
    >         5. 删除/var/spool/mail下的文件
    >         
    >     8. 完全删除用户：```userdel -rf 用户名```，r代表递归Recursion，f代表强制force
    >     
    > 2. 用户组
    > 
    >     1. 用户组介绍
    >     
    >        类似于角色，Linux可以对有共性的多个用户放在一个组中。
    >        
    >     2. 新增组：```groupadd 组名```。只有root超级管理员才能执行
    >     
    >     3. 删除组：```groupdel 组名```。只有root超级管理员才能执行
    >     
    >     4. 修改用户对应的用户组：```usermod -g 用户组 用户名```。只有root超级管理员才能执行
    >     
    >     5. 添加用户时，添加用户对应的用户组：```useradd -g 用户组 用户名 ```
    >     
    >         * 添加用户时不添加用户组，默认该用户单独一组，用户组名为用户名。同时，用户UID和组GID一样。
    >     
    > 3. Linux中，用户和用户组相关文件
    > 
    >     1. 用户的配置文件：/etc/passwd。**注意：里面没有明显的密码，记住了**
    >     
    >        用户配置文件中每行的组成：```用户名:口令:用户UID:组GID:注释性描述:主目录:登录Shell```。例如：```zhangshuyu:x:1000:1000::/home/zhangshuyu:/bin/bash```
    >        
    >     2. 口令的配置文件：/etc/shadow。**口令就是密码信息**。该配置文件需要root权限才允许查看，同时该配置文件只读。
    >     
    >        口令配置文件中每行的组成：```登录名:加密口令:最后一次修改时间:最小时间间隔:最大时间间隔:警告时间:不活动时间:失效时间:标志```。例如：```zhangshuyu:!!:18648:0:99999:7:::```
    >        
    >     3. 用户组的配置文件：/etc/group。
    >     
    >        用户组配置文件组成：```用户组名:口令:组GID:组内用户列表```。例如：```zhangshuyu:x:1000:```
    >        
### 2. 权限管理
2. 权限管理
    > 1. 介绍
    >
    >    Linux中一切皆文件，因此就存在着对文件的操作权限。
    >    
    > 2. 使用```ll```命令，可以得到文件信息，信息组成是这样的：```类型和权限. 该文件下文件数量（文件为1，文件夹会显示包括该层共几层文件夹） 创建的用户 用户所在用户组 创建时间 文件名或文件夹名```
    >
    > 3. 类型和权限组成
    >
    >    类型和权限组成共10位，从0开始到9.
    >    
    >    第0位为文件类型
    >    
    >    第1到第3位为文件创建用户权限
    >    
    >    第4到第6位为文件创建用户所在用户组权限
    >    
    >    第7到第9位为其他用户权限
    >    
    >    例如```drwx------.```
    >    
    > 4. 文件类型
    >
    >    常见的有四种，```-```代表普通file；```d```代表文件夹(directory)；```l```代表软链接；```b```代表block块设备，例如硬盘
    >    
    > 5. 权限
    >
    >    共三种，r(读)、w(写)、x(执行)。有哪一种权限就写上对应的字母，没有对应权限就写```-```
    >     * 对于文件：
    >    
    >        r代表可以读
    >        
    >        w代表可以修改文件。不代表可以删除文件，要**想删除文件前提是对文件所在目录有写权限**
    >        
    >        x代表可以被系统执行
    >    
    >     * 对于文件夹：
    >    
    >        r代表可以读取，即使用``ls```查看内容
    >    
    >        w代表可以目录内创建、删除目录、重命名目录
    >    
    >        x代表可以进入目录
    >    
    > 6. **权限设置：使用```chmod```命令。只有root超级管理员可以设置权限。ch即change简写**
    >
    >     1. u(用户)、g(用户组)、o(其他人)、a(u、g、a一起)
    >     
    >     2. +(添加权限)、-(撤销权限)、=(设置权限)
    >     
    >     3. **权限设置的两种方式**
    >     
    >         1. 方式1：u、g、o、a和+、-、=来变更权限，例如
    >         
    >             ```chmod u=rwx,g=rx,o=rx 文件路径```
    >             
    >             ```chmod u+r,g-x 文件路径```
    >             
    >             ```chmod a=rwx 文件路径```
    >             
    >             ```chomod a+x 文件路径```
    >             
    >         2. 方式2：使用数字重新设置权限，必须全部设置
    >         
    >             1. r=4 w=2 x=1 rwx=4+2+1=7
    >             
    >             2. 通过数字变更权限例子：
    >             
    >                ```chmod 755 文件路径```
    >                
    >                相当于
    >                
    >                ```chmod u=rwx,g=rx,o=rx 文件路径```
    >     
    > 7. 改变文件所有者：```chown [选项] 新所有者 文件路径```。ch即change简写，own即owner简写
    > 
    >     * 改变文件所有者和所有组：```chown [选项] 新所有者:新所有组 文件路径```
    >     
    >     * 选项：
    >     
    >         1. -R：如果是目录，则递归更改目录和目录下所有的子文件或目录
    >     
    > 8. 改变文件所有组：```chogrp 新所有组 文件路径```，ch即change简写，grp即group简写
    > 
## 6. rpm和yum
6. rpm和yum
    > 1. rpm
    > 
    >     1. 介绍
    > 
    >        rpm，Redhat软件包管理工具，类似于Windows中的setup.exe。
    >       
    >        查询Linux下的已安装的rpm软件列表：```rpm -qa```，可以使用```| grep```模糊查询。
    >       
    >        rpm包的名称：例如```firefox-68.10.0-1.el7.centos.x86_64```。其中firefox是名称，68.10.0-1是版本号，el7.centos.x86_64是适配哪个Linux系统（表示centos7x的64位系统）
    >        
    >     2. 安装rpm软件：```rpm -ivh rpm包路径```
    >     
    >         * 例如centOS里自带的火狐，安装包就在镜像文件中的packages文件夹中。同时镜像文件中的packages文件夹下有着所有的安装包。
    >     
    >     3. 卸载rpm软件：```rpm -e RPM软件包```
    >     
    > 2. yum
    > 
    >     1. yum介绍
    >     
    >        yum类似于我们的Maven工具，yum会从镜像网站上下应用，然后直接安装
    >        
    >        查询yum软件：```yum list```，可以使用```| grep```进行模糊查询
    >        
    >     2. yum下载安装软件：```yum install yum包```
    >     
## 7. Linux开发环境搭建
7. Linux开发环境搭建
    > 1. 安装jdk
    >
    >     1. 解压缩到/opt下。以后自己安装软件，建议都安装到/opt下
    >     
    >     2. 重命名jdk所在文件夹，重命名为jdk1.8
    >     
    >     3. 配置环境变量
    >     
    >        环境变量配置文件为：/etc/profile。使用vim打开，配置如下内容：
    >        
    >         ```
    >        JAVA_HOME=/opt/jdk1.8
    >        PATH=/opt/jdk1.8/bin:$PATH
    >        export JAVA_HOME PATH
    >         ```
    >        
    >     4. 加载配置文件：```source /etc/profile```
    >     
    > 2. 安装Tomcat
    >
    >     1. 解压缩到/opt
    >     
    >     2. 启动Tomcat：进入Tomcat解压后的bin目录中，执行```./startup.sh```
    >     
    >         * 停止Tomcat：进入Tomcat解压后的bin目录中，执行```./startdown.sh```
    >     
    > 3. 安装Mysql
    >
    >     1. 检查默认Mysql：因为centOS中有默认Mysql，因此先卸载默认Mysql。
    >     
    >         * centOS6中：检查```rpm -qa | grep mysql```，卸载```rpm -e --	nodeps mysql-libs```
    >         
    >         * centOS7中：```rpm -qa | grep mariadb```，卸载```rpm -e --	nodeps mariadb-libs```
    >         
    >     2. 检查tmp文件夹权限，设置tmp文件夹权限：```chmod -R 777 /tmp```
    >     
    >     3. 安装Mysql客户端和服务端的rpm包
    >     
    >     4. 检查是否安装成功：通过```rpm -qa | grep -i mysql```，-i表示忽略大小写。
    >     5. Mysql服务的启动和停止：Mysql在centOS7中服务的启动和停止保留了centOS6习惯
    >     
    >         * 启动Mysql服务：```service mysql start```
    >         
    >         * 停止Mysql服务：```service mysql stop```
    >         
    >     6. 设置Mysql中，root用户密码：```mysqladmin -u root -h 127.0.0.1 -p  '具体密码'```
    >     
    >     7. 登录Mysql：```mysql -uroot -p具体密码```
    >     
    >         * 登录成功后就可以写SQL了
    >         
    >     8. 设置Mysql编码方式
    >     
    >         1. 登录mysql
    >         2. 查看Mysql中的字符集：```show variables like 'character%';```
    >         3. 退出登录Mysql，终端里查看Mysql安装位置：```ps -ef | grep mysql```，找到Mysql配置文件目录/usr/share/mysql
    >         4. 将/usr/share/mysql/中的my-huge.cnf拷贝到/etc/下，改名为my.cnf。
    >         
    >             * 因为mysql启动先会去读取/etc/my.cnf文件
    >             
    >         5. 在/etc/my.cnf中的[client]、[mysqld]、[mysql]下添加如下内容：
    >         
    >             ```
    >             [client]
    >             default-character-set=utf8
    >             [mysqld]
    >             character_set_server=utf8
    >             character_set_client=utf8
    >             collation-server=utf8_general_ci
    >             [mysql]
    >             default-character-set=utf8
    >             ```
    >             
    >         6. 重启Mysql：```service mysql restart```
    >     
    > 4. Mysql默认root用户只允许本机登录，不允许外部工具访问，例如SQLyog等都不行。想要外部访问，必须创建一个外部访问的用户。
    > 
    >     1. Linux下登录Mysql
    >     
    >     2. 查询mysql.user表中常用字段：```select host, user, password, select_priv from mysql.user```。可以看到没有可以远程访问的用户。
    >     
    >     3. 创建可以外部访问的root用户并授予所有权限：```grant all privileges on *.* to root@'%' identified by '外部root用户的密码';```
    >     
    >         * 创建完成后，可以通过前边的查询mysql.user表语句进行再次查询，我们可以看到一个名为的新host被创建，这就说明我们可以在该网段，通过root用户外部访问Linux下的Mysql了。
    >     
    >     4. 刷新权限：```flush privileges;```。**在Mysql的user表作出了任何的修改，都需要进行刷新权限的操作**
    >     
    >     5. 之后，只要Linux的Mysql服务开启，root用户就可以在外部访问Linux下的Mysql了。
    >     
    >        
    >
    > 
    >
    > 
