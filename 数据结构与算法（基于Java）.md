# 数据结构与算法（基于Java）
## 1.  稀疏数组（SparseArray）
1. 稀疏数组（SparseArray）
    > 1. 实例
    >
    >    如果我们使用二维数组存储一场关于五子棋的对局详情，假设一方棋子表示为1，另一方棋子表示为2，没有棋子表示为0，则直接将整个棋盘信息存储就会造成二维数组的空间的浪费。因此我们使用稀疏数组来做。
    >    
    > 2. 概念
    >
    >     * 利用五子棋对局详情的存储理解稀疏数组：
    >     
    >        我们定义一个二维数组，只储存对局关键信息。数组有n行3列，第一个列代表行数，第二个列代表列数，第三个列代表值，在```arr[0][0]```存储棋盘总行数，```arr[0][1]```存储棋盘总列数，```arr[0][3]```存储双方总共有多少棋子；将双方的每个棋子的行列信息和值依次存储，这样棋盘所有信息都已经存储在一个数组中，而且不会造成存储空间浪费。
    >     
    > 3. 使用稀疏数组存储五子棋信息
    >
    >     ```
    >     public class xishuShuZu {
    >         public static void main(String[] args) throws IOException {
    >             /**
    >              * 定义一个6 * 6的棋盘，人为制造一场对局，并打印
    >              */
    >             int[][] arr = new int[6][6];
    >             arr[2][4] = 1;
    >             arr[3][5] = 2;
    >             arr[2][5] = 1;
    >             arr[3][4] = 2;
    >             for (int i = 0; i < 6; i++) {
    >                 for (int j = 0; j < 6; j++) {
    >                     System.out.printf("%d\t", arr[i][j]);
    >                 }
    >                 System.out.printf("\n");
    >             }
    >             /**
    >              * 将该棋盘存入一个稀疏数组，然后将稀疏数组存储到文件
    >              */
    >             int sum = 0; //获取棋盘中棋子总数
    >             for (int i = 0; i < 6; i++) {
    >                 for (int j = 0; j < 6; j++) {
    >                     if (arr[i][j] != 0) {
    >                         sum++;
    >                     }
    >                 }
    >             }
    >             int[][] sprseArr = new int[sum + 1][3];
    >             sprseArr[0][0] = 6;
    >             sprseArr[0][1] = 6;
    >             sprseArr[0][2] = sum;
    >             int count = 0;
    >             for (int i = 0; i < 6; i++) {
    >                 for (int j = 0; j < 6; j++) {
    >                     if (arr[i][j] != 0) {
    >                         count++;
    >                         sprseArr[count][0] = i;
    >                         sprseArr[count][1] = j;
    >                         sprseArr[count][2] = arr[i][j];
    >                     }
    >                 }
    >             }
    >             System.out.println("稀疏数组");
    >             for (int[] i :
    >                     sprseArr) {
    >                 for (int j :
    >                         i) {
    >                     System.out.printf("%d\t", j);
    >                 }
    >                 System.out.println();
    >             }
    >             File out = new File("XiShuShuZu/data.txt");
    >             FileWriter fw = new FileWriter(out);
    >             for (int[] a :
    >                     sprseArr) {
    >                 for (int data :
    >                         a) {
    >                     fw.write(data + "\r\n");
    >                 }
    >             }
    >             fw.flush();
    >             fw.close();
    >         }
    >     }
    >     ```
    > 
## 2. 队列（Queue）
2. 队列（Queue）
    > 1. 概念
    >
    >    遵循先进先出原则的一个有序列表，可用数组或者链表实现。
    >    
    > 2. 数组模拟队列
    >
    >     1. 思路分析
    >     
    >         1. 需要一个变量```maxSize```存储数组的最大容量
    >           
    >         2. 需要两个变量，一个```font```来记录队列的前端，一个```rear```来记录队列的后端
    >           
    >         3. ```font```为队列前端，指向队列头前一个位置，即```queue[font + 1]```为队列头值。初始值为-1，每弹出一个元素索引加1；```rear```为队列后端，初始值为-1，每添加一个元素索引加1
    >           
    >         4. 队列为空判断：```rear == font```时为空
    >           
    >         5. 队列为满判断：```rear = maxSize - 1```
    >        
    >     2. 代码实现
    >     
    >         ```
    >         class ArrQueue {
    >         
    >             private int maxSize;
    >             private int font;
    >             private int rear;
    >             private int[] queue;
    >         
    >             public ArrQueue(int size) {
    >                 //初始化队列
    >                 maxSize = size;
    >                 font = -1;
    >                 rear = -1;
    >                 queue = new int[maxSize];
    >             }
    >         
    >             /**
    >              * 队列非空判断
    >              * @return
    >              */
    >             public boolean isEmpty() {
    >                 return font == rear;
    >             }
    >         
    >             /**
    >              * 队列为满判断
    >              * @return
    >              */
    >             public boolean isFull() {
    >                 return rear == maxSize - 1;
    >             }
    >         
    >             /**
    >              * 给队列添加值
    >              * @param value
    >              */
    >             public void addQueue(int value) {
    >                 //首先判断队列是否为满
    >                 if (isFull()) {
    >                     System.out.println("队列已满，无法添加~~");
    >                     return;
    >                 }
    >                 // 先让队尾指针后移
    >                 rear++;
    >                 // 添加元素进入队列
    >                 queue[rear] = value;
    >             }
    >         
    >             /**
    >              * 队列取值
    >              * @return
    >              */
    >             public int getQueue() {
    >                 // 首先判断队列是否为空
    >                 if (isEmpty()) {
    >                     throw new RuntimeException("队列空，不能取值");
    >                 }
    >                 // 先让队首指针后移一位
    >                 font++;
    >                 // 获取值
    >                 return queue[font];
    >             }
    >         
    >             /**
    >              * 显示队列的所有数据
    >              */
    >             public void showQueue() {
    >                 // 首先判断队列是否为空
    >                 if (isEmpty()) {
    >                     System.out.println("队列为空，不能显示值~~");
    >                     return;
    >                 }
    >                 // 从队列头一个元素开始遍历，遍历到队列最后一个元素
    >                 for (int i = font + 1; i <= rear; i++) {
    >                     System.out.printf("queue[%d]=%d\n", i, queue[i]);
    >                 }
    >             }
    >         
    >             /**
    >              * 显示队列的头部数据
    >              * @return
    >              */
    >             public int headQueue() {
    >                 // 判断是否为空
    >                 if (isEmpty()) {
    >                     throw new RuntimeException("队列为空，无法展示~~");
    >                 }
    >                 //展示数据
    >                 return queue[font + 1];
    >             }
    >         }
    >         ```
    >     
    > 3. 数组模拟环形队列
    >
    >     1. 思路分析
    >     
    >         1. 需要对两个记录队首和队尾的变量做些调整
    >         
    >         2. ```font```变量代表队首，指向队首的第一个元素，即```queue[font]```就是队列的第一个元素，初始值为0
    >         
    >         3. ```rear```变量，指向队尾最后一个元素的前一个元素。因为希望可以空出一个空间作为约定，初始值为0
    >         
    >             * 为何需要空出一个空间来作为约定？
    >             
    >                因为如果没有这个空间的约定，则队列满的判断条件```font == rear```和队列为空的判断条件```rear % maxSize == font```会冲突。该空间是动态变化的
    >                
    >             * ```%```符号的运用
    >             
    >                因为环形xxx的结构其实就是不停地周期性变化，因此我们需要通过```%```让我们只考虑一个周期的情况
    >             
    >         4. 队列为空的逻辑判断：```font == rear```
    >         
    >         5. 队列满的逻辑判断：```(rear + 1) % maxSize == font```
    >         
    >         6. 队列中所有有效数据的个数：```(rear + maxSize - font) % maxSize```
    >             * 用此方法实现的环形队列可存储的元素永远比数组大小少一，因为我们空出了一个空间来作为约定
    >         
    >             * 因为font和rear都是从第一个元素开始计算，初始值为0，因此需要在计算时加上一个周期的值，即maxSize
    >         
    >     2. 代码实现
    >     
    >         ```java
    >         class CircleQueue {
    >         
    >             private int maxSize;
    >             private int font;
    >             private int rear;
    >             private int[] queue;
    >         
    >             public CircleQueue(int size) {
    >                 //初始化队列
    >                 maxSize = size;
    >                 font = 0;
    >                 rear = 0;
    >                 queue = new int[maxSize];
    >             }
    >         
    >             /**
    >              * 队列非空判断
    >              * @return
    >              */
    >             public boolean isEmpty() {
    >                 return font == rear;
    >             }
    >         
    >             /**
    >              * 队列为满判断
    >              * @return
    >              */
    >             public boolean isFull() {
    >                 return (rear + 1) % maxSize == font;
    >             }
    >         
    >             /**
    >              * 给队列添加值
    >              * @param value
    >              */
    >             public void addQueue(int value) {
    >                 //首先判断队列是否为满
    >                 if (isFull()) {
    >                     System.out.println("队列已满，无法添加~~");
    >                     return;
    >                 }
    >                 //因为rear从第一个元素开始，初始值为0，因此先给队列赋值
    >                 queue[rear] = value;
    >                 //然后增加，注意，因为是环形数组，因此不能普通的递增
    >                 rear = (rear + 1) % maxSize;
    >             }
    >         
    >             /**
    >              * 队列取值
    >              * @return
    >              */
    >             public int getQueue() {
    >                 // 首先判断队列是否为空
    >                 if (isEmpty()) {
    >                     throw new RuntimeException("队列空，不能取值");
    >                 }
    >                 //因为rear从第一个元素开始，初始值为0，先将队列的值赋给一个临时变量
    >                 int temp = queue[font];
    >                 //递增,注意，因为是环形数组，因此不能普通的递增
    >                 font = (font + 1) % maxSize;
    >                 //返回队列的值
    >                 return temp;
    >             }
    >         
    >             /**
    >              * 显示队列的所有数据
    >              */
    >             public void showQueue() {
    >                 // 首先判断队列是否为空
    >                 if (isEmpty()) {
    >                     System.out.println("队列为空，不能显示值~~");
    >                     return;
    >                 }
    >                 // 从队列头一个元素开始遍历，遍历到队列最后一个元素
    >                 for (int i = font ; i < font + size(); i++) {
    >                     // 要小心i可能超出maxSize的情况，因此在环形xxx中，数据最好都使用i % maxSize来处理
    >                     System.out.printf("queue[%d]=%d\n", i % maxSize, queue[i % maxSize]);
    >                 }
    >             }
    >         
    >             /**
    >              * 显示队列有效元素个数
    >              * @return
    >              */
    >             public int size() {
    >                 return (rear + maxSize - font) % maxSize;
    >             }
    >         
    >             /**
    >              * 显示队列的头部数据
    >              * @return
    >              */
    >             public int headQueue() {
    >                 // 判断是否为空
    >                 if (isEmpty()) {
    >                     throw new RuntimeException("队列为空，无法展示~~");
    >                 }
    >                 //展示数据
    >                 return queue[font];
    >             }
    >         }
    >         ```
    >     
## 3. 链表(LinkedList)
3. 链表(LinkedList)
    > 1. 单链表
    >     1. 概念
    >     
    >        链表是有序的节点列表，但是它的节点们在内存中空间不一定是连续的。
    >        
    >        链表包含了head节点和其他节点。
    >        
    >        head节点不存储数据，相当于单链表的头，创建了head节点相当于初始化了一个空的单链表。
    >        
    >        其他节点存储数据，最后一个节点指向null
    >        
    >     2. 单链表的代码实现及相关操作
    >     
    >         * 链表节点
    >         
    >             ```java
    >         /**
    >              * 记录水浒传英雄的链表节点
    >              */
    >             class HeroLinkedListNode {
    >                 public int no;
    >                 public String name;
    >                 public String nickName;
    >                 public HeroLinkedListNode next;
    >             
    >                 /**
    >                  * 构造器
    >                  * @param no
    >                  * @param name
    >                  * @param nickName
    >                  */
    >                 public HeroLinkedListNode(int no, String name, String nickName) {
    >                     this.no = no;
    >                     this.name = name;
    >                     this.nickName = nickName;
    >                 }
    >             
    >                 @Override
    >                 public String toString() {
    >                     return "HeroLinkedListNode{" +
    >                             "no=" + no +
    >                             ", name='" + name + '\'' +
    >                             ", nickName='" + nickName + '\'' +
    >                             '}';
    >                 }
    >             }
    >             ```
    >         * 管理和创建单链表的类
    >         
    >             ```java
    >             class SingleLinkedList {
    >                 /**
    >                  * 初始化一个head节点，该节点无任何数据
    >                  */
    >                 private HeroLinkedListNode head = new HeroLinkedListNode(0, "", "");
    >             
    >                 /**
    >                  * 添加一个节点到单链表中。不考虑no的顺序
    >                  * @param hero
    >                  */
    >                 public void addNoOrder(HeroLinkedListNode hero) {
    >                     // 因为head节点无法动，定义一个辅助变量用来遍历链表
    >                     HeroLinkedListNode temp = head;
    >                     // 遍历链表至找到最后一个节点
    >                     while (true) {
    >                         // 找到最后一个链表节点
    >                         if (temp.next == null) {
    >                             break;
    >                         }
    >                         // 没到最后继续后移
    >                         temp = temp.next;
    >                     }
    >                     //在链表最后添加新节点
    >                     temp.next = hero;
    >                 }
    >             
    >                 /**
    >                  * 添加元素进链表。保证数据按照no的从大到小排列
    >                  * @param hero
    >                  */
    >                 public void addByOrder(HeroLinkedListNode hero) {
    >                     // 因为head节点不能动，定义一个temp变量来遍历链表
    >                     HeroLinkedListNode temp = head;
    >                     // 定义一个标识，标志我们链表中是否已经有这个编号了
    >                     boolean flag = false;
    >                     // 遍历链表至找到最后一个节点
    >                     while (true) {
    >                         // 说明已经temp就是最后一个节点，退出
    >                         if (temp.next == null) {
    >                             break;
    >                         }
    >                         // 找到了插入位置的前一个节点，退出
    >                         if (temp.next.no > hero.no) {
    >                             break;
    >                         }
    >                         // 如果已经存在，让flag置为true，退出
    >                         if (temp.next.no == hero.no) {
    >                             flag = true;
    >                             break;
    >                         }
    >                         temp = temp.next;
    >                     }
    >                     if (flag) {
    >                         System.out.printf("编号%d已经存在~~", hero.no);
    >                     } else {
    >                         hero.next = temp.next;
    >                         temp.next = hero;
    >                     }
    >             
    >                 }
    >             
    >                 /**
    >                  * 遍历链表并展示每个节点
    >                  */
    >                 public void showList() {
    >                     // 判断链表是否为空
    >                     if (head.next == null) {
    >                         System.out.println("链表为空~~");
    >                         return;
    >                     }
    >                     // 因为head节点不能动，定义一个辅助变量来遍历链表
    >                         /* 注意：在这里我们要让temp指向链表第一个节点，因为
    >                         *  在遍历时我们必须要先判断temp == null,再输出数据然后再后移，
    >                         *  如果指向head，会造成如果已经到链表末尾则后移后的temp == null，
    >                         *  再输出数据会出错。因此我们必须要让temp指向链表第一个节点而不是head
    >                          */
    >                     HeroLinkedListNode temp = head.next;
    >                     // 遍历链表并展示所有节点的数据
    >                     while (true) {
    >                         // 判断当前是否遍历完成
    >                         if (temp == null) {
    >                             break;
    >                         }
    >                         // 展示节点的数据
    >                         System.out.println(temp.toString());
    >                         temp = temp.next;
    >                     }
    >                 }
    >             
    >                 /**
    >                  * 根据编号修改节点
    >                  * @param heroUpdate
    >                  */
    >                 public void update(HeroLinkedListNode heroUpdate) {
    >                     // 判断链表是否为空
    >                     if (head.next == null) {
    >                         System.out.println("链表为空！！");
    >                         return;
    >                     }
    >                     // 因为head节点不能动，因此使用一个临时变量temp来遍历链表
    >                     HeroLinkedListNode temp = head.next;
    >                     // 定义一个标志，标志是否找到了节点
    >                     boolean flag = false;
    >                     // 遍历链表
    >                     while (true) {
    >                         // 判断链表是否遍历完成
    >                         if (temp == null) {
    >                             break;
    >                         }
    >                         // 判断是否找到了节点
    >                         if (temp.no == heroUpdate.no) {
    >                             flag = true;
    >                             break;
    >                         }
    >                         temp = temp.next;
    >                     }
    >                     // 修改链表节点数据
    >                     if (flag) {
    >                         temp.name = heroUpdate.name;
    >                         temp.nickName = heroUpdate.nickName;
    >                     } else {
    >                         System.out.printf("没有找到编号为%d的节点", heroUpdate.no);
    >                     }
    >                 }
    >             
    >                 /**
    >                  * 根据编号删除节点
    >                  * @param no
    >                  */
    >                 public void delete(int no) {
    >                     // 判断是否为空
    >                     if (head.next == null) {
    >                         System.out.println("链表为空~~");
    >                         return;
    >                     }
    >                     // 由于head节点无法移动，因此定义temp变量来遍历链表
    >                     HeroLinkedListNode temp = head;
    >                     // 标记是否有对应编号的节点
    >                     boolean flag = false;
    >                     // 遍历获取
    >                     while (true) {
    >                         // 判定是否遍历完毕
    >                         if (temp.next == null) {
    >                             break;
    >                         }
    >                         // 判定是否为编号对应节点的前一个节点
    >                         if (temp.next.no == no) {
    >                             flag = true;
    >                             break;
    >                         }
    >                         temp = temp.next;
    >                     }
    >                     if (flag) {
    >                         temp.next = temp.next.next;
    >                     } else {
    >                         System.out.printf("无编号%d的节点", no);
    >                     }
    >             
    >                 }
    >             }
    >             ```
    >             
    >             * 链表的遍历中，如果需要用到temp.next，则链表结束的判断为temp.next == null，如果用到temp，则判断为temp == null
    >         * 单链表的反转：
    >         
    >             ```java
    >             /**
    >                  * 反转单链表
    >                  * 方法1：通过递归实现
    >                  * @param head
    >                  * @return
    >                  */
    >                 public HeroLinkedListNode reverseSingleLinkedListTypeOne(HeroLinkedListNode head) {
    >                     if (head.next == null) {
    >                         return null;
    >                     }
    >                     HeroLinkedListNode reverseHeroHead = new HeroLinkedListNode(0, "", "");
    >                     HeroLinkedListNode reverse = reverseTypeOne(head, reverseHeroHead);
    >                     return reverse;
    >                 }
    >             
    >                 private HeroLinkedListNode reverseTypeOne(HeroLinkedListNode head, HeroLinkedListNode heroReverseHead) {
    >                     if (head.next == null) {
    >                         return heroReverseHead;
    >                     }
    >                     head = head.next;
    >                     HeroLinkedListNode temp1 = head;
    >                     HeroLinkedListNode temp2 = reverseTypeOne(head, heroReverseHead);
    >                     HeroLinkedListNode temp3 = temp2;
    >                     while (true) {
    >                         if (temp3.next == null) {
    >                             temp1.next = null;
    >                             temp3.next = temp1;
    >                             break;
    >                         }
    >                         temp3 = temp3.next;
    >                     }
    >                     return temp2;
    >                 }
    >             
    >                 /**
    >                  * 反转单链表
    >                  * 方法2：通过遍历和获取
    >                  * @param hero
    >                  * @return
    >                  */
    >                 public HeroLinkedListNode reverseSingledLinkedListTypeTwo(HeroLinkedListNode hero) {
    >                     // 判断单链表是否为空或者只有一个节点
    >                     if (hero.next == null || hero.next.next == null) {
    >                         return head;
    >                     }
    >                     // 创建一个新链表
    >                     HeroLinkedListNode heroNew = new HeroLinkedListNode(0, "", "");
    >                     // 遍历旧链表
    >                     HeroLinkedListNode temp = head.next;
    >                     HeroLinkedListNode temp1 = null;
    >                     while (temp != null) {
    >                         temp1 = temp.next;
    >                         temp.next = heroNew.next;
    >                         heroNew.next = temp;
    >                         temp = temp1;
    >                     }
    >                     return heroNew;
    >                 }
    >             ```
    >         * 单链表的反转打印
    >             ```java
    >             /**
    >                  * 逆序打印单链表
    >                  * 方法1：递归打印
    >                  * @param head
    >                  */
    >                 public void reversePrintTypeOne(HeroLinkedListNode head) {
    >                     if (head != null) {
    >                         reversePrintTypeOne(head.next);
    >                         System.out.println(head.next);
    >                     }
    >                 }
    >             
    >                 /**
    >                  * 逆序打印
    >                  * 方法2：栈
    >                  * @param head
    >                  */
    >                 public void reversePrintTypeTwo(HeroLinkedListNode head) {
    >                     if (head.next == null || head.next.next == null) {
    >                         return;
    >                     }
    >                     Stack<HeroLinkedListNode> stack = new Stack<>();
    >                     // 遍历链表，压栈
    >                     HeroLinkedListNode temp = head.next;
    >                     while (temp != null) {
    >                         stack.add(temp);
    >                         temp = temp.next;
    >                     }
    >                     // 出栈
    >                     while (stack.size() > 0) {
    >                         System.out.println(stack.pop());
    >                     }
    >                 }
    >             ```
    >     
    > 2. 双向链表
    >
    >     1. 单链表和双向链表的区别
    >     
    >        1、单向链表查找的方向只能为一个；双向链表可以从左或从右查找
    >        
    >        2、单向链表不能自我删除，需要依靠辅助节点；双向链表可以自我删除
    >        
    >     2. 双向链表的代码实现：
    >     
    >         * 双向链表节点的实现
    >         
    >             ```
    >         /**
    >              * 水浒英雄双向链表节点
    >              */
    >             class HeroDoubleLinkedListNode {
    >                 public HeroDoubleLinkedListNode pre;
    >                 public HeroDoubleLinkedListNode next;
    >                 public int no;
    >                 public String name;
    >                 public String nickName;
    >             
    >                 @Override
    >                 public String toString() {
    >                     return "HeroDoubleLinkedListNode{" +
    >                             "no=" + no +
    >                             ", name='" + name + '\'' +
    >                             ", nickName='" + nickName + '\'' +
    >                             '}';
    >                 }
    >             
    >                 public HeroDoubleLinkedListNode(int no, String name, String nickName) {
    >                     this.no = no;
    >                     this.name = name;
    >                     this.nickName = nickName;
    >                 }
    >             }
    >             ```
    >             
    >         * 双向链表的一系列操作
    >         
    >             ```
    >             /**
    >              * 水浒英雄双向链表管理类
    >              */
    >             class DoubleLinkedList {
    >                 /**
    >                  * 双向链表表头
    >                  */
    >                 private HeroDoubleLinkedListNode head = new HeroDoubleLinkedListNode(0, "", "");
    >             
    >                 /**
    >                  * 获取双向链表头
    >                  * @return
    >                  */
    >                 public HeroDoubleLinkedListNode getHead() {
    >                     return head;
    >                 }
    >             
    >                 /**
    >                  * 双向链表的添加
    >                  * @param value
    >                  */
    >                 public void add(HeroDoubleLinkedListNode value) {
    >                     // head不能动，因此引入辅助变量temp
    >                     HeroDoubleLinkedListNode temp = head;
    >                     while (true) {
    >                         if (temp.next == null) {
    >                             break;
    >                         }
    >                         temp = temp.next;
    >                     }
    >                     temp.next = value;
    >                     value.pre = temp;
    >                 }
    >             
    >                 /**
    >                  * 顺序添加元素到双向链表
    >                  * @param value
    >                  */
    >                 public void addByOrder(HeroDoubleLinkedListNode value) {
    >                     HeroDoubleLinkedListNode temp = head.next;
    >                     boolean flag = false;
    >                     while (true) {
    >                         if (temp == null) {
    >                             break;
    >                         }
    >                         if (temp.no > value.no) {
    >                             flag = true;
    >                             break;
    >                         }
    >                         temp = temp.next;
    >                     }
    >                     if (flag) {
    >                         temp.pre.next = value;
    >                         value.next = temp;
    >                         temp.pre = value;
    >                         value.pre = temp.pre;
    >                     } else {
    >                         this.add(value);
    >                     }
    >                 }
    >             
    >                 /**
    >                  * 遍历双向链表
    >                  */
    >                 public void show() {
    >                     if (head.next == null) {
    >                         System.out.println("双向链表为空~~");
    >                         return;
    >                     }
    >                     // 因为头结点不能动，因此使用辅助变量temp
    >                     HeroDoubleLinkedListNode temp = head.next;
    >                     while (true) {
    >                         if (temp == null) {
    >                             break;
    >                         }
    >                         System.out.println(temp.toString());
    >                         temp = temp.next;
    >                     }
    >                 }
    >             
    >                 /**
    >                  * 修改双向链表的节点内容
    >                  * @param value
    >                  */
    >                 public void update(HeroDoubleLinkedListNode value) {
    >                     if (head.next == null) {
    >                         System.out.println("双向链表为空~~");
    >                         return;
    >                     }
    >                     // 因为head不能动，引入temp变量
    >                     HeroDoubleLinkedListNode temp = head.next;
    >                     boolean flag = false;
    >                     while (true) {
    >                         if (temp == null) {
    >                             break;
    >                         }
    >                         if (temp.no == value.no) {
    >                             flag = true;
    >                             break;
    >                         }
    >                         temp = temp.next;
    >                     }
    >                     if (flag) {
    >                         temp.name = value.name;
    >                         temp.nickName = value.nickName;
    >                     } else {
    >                         System.out.println("未找到该元素~~");
    >                     }
    >                 }
    >             
    >                 /**
    >                  * 删除双向链表的某个节点
    >                  * @param value
    >                  */
    >                 public void del(HeroDoubleLinkedListNode value) {
    >                     if (head.next == null) {
    >                         System.out.println("双向链表为空~~");
    >                         return;
    >                     }
    >                     // 使用temp变量遍历链表
    >                     HeroDoubleLinkedListNode temp = head.next;
    >                     boolean falg = false;
    >                     while (true) {
    >                         if (temp == null) {
    >                             break;
    >                         }
    >                         if (temp.no == value.no) {
    >                             falg = true;
    >                             break;
    >                         }
    >                         temp = temp.next;
    >                     }
    >                     if (falg) {
    >                         if (temp.next == null) {
    >                             temp.pre.next = null;
    >                         } else {
    >                             temp.pre.next = temp.next;
    >                             temp.next.pre = temp.pre;
    >                         }
    >                     } else {
    >                         System.out.println("无法找到该元素~~");
    >                     }
    >                 }
    >             }
    >             ```
    >             
    >             * 注意：双向链表做增删操作时要判断表中元素和表尾元素，他们用不同逻辑，否则会有空指针异常
    >     
    > 3. 单向环形链表（circleLinkedList）
    >
    >     1. 单向环形链表应用场景：解决约瑟夫环问题
    >     
    >        Josephu 问题为：设编号为1，2，… n的n个人围坐一圈，约定编号为k（1<=k<=n）的人从1开始报数，数到m 的那个人出列，它的下一位又从1开始报数，数到m的那个人又出列，依次类推，直到所有人出列为止，由此产生一个出队编号的序列。
    >        
    >     2. 单向环形链表解决约瑟夫环问题代码实现：
    >     
    >         * 单向环形链表节点
    >         
    >             ```
    >              /**
    >              * 约瑟夫环单向环形链表节点
    >              */
    >             class JosephuCircleLinkedListNode {
    >                 public int no;
    >                 public String name;
    >                 public JosephuCircleLinkedListNode next;
    >             
    >                 public JosephuCircleLinkedListNode(int no, String name) {
    >                     this.no = no;
    >                     this.name = name;
    >                 }
    >             
    >                 @Override
    >                 public String toString() {
    >                     return "JosephuCircleLinkedListNode{" +
    >                             "no=" + no +
    >                             ", name='" + name + '\'' +
    >                             '}';
    >                 }
    >             }
    >             ```
    >             
    >         * 单向环形链表管理类
    >         
    >             ```
    >             class CircleLinkedList {
    >             
    >                 /**
    >                  * 创建环形链表头结点
    >                  */
    >                 private JosephuCircleLinkedListNode head = new JosephuCircleLinkedListNode(0, "");
    >             
    >                 /**
    >                  * 获取环形链表头结点
    >                  * @return
    >                  */
    >                 public JosephuCircleLinkedListNode getHead() {
    >                     return head;
    >                 }
    >             
    >                 /**
    >                  * 添加元素到环形链表中
    >                  * @param value
    >                  */
    >                 public void add(JosephuCircleLinkedListNode value) {
    >                     if (value.no < 1) {
    >                         System.out.println("元素错误，无法添加~~");
    >                         return;
    >                     }
    >                     // 定义temp变量完成遍历
    >                     JosephuCircleLinkedListNode temp = head.next;
    >                     while (true) {
    >                         if (head.next == null) {
    >                             temp = head;
    >                             break;
    >                         }
    >                         if (temp.next == head.next) {
    >                             break;
    >                         }
    >                         temp = temp.next;
    >                     }
    >                     temp.next = value;
    >                     value.next = head.next;
    >                 }
    >             
    >                 /**
    >                  * 约瑟夫问题
    >                  * @param k
    >                  * @param startNum
    >                  * @param num
    >                  */
    >                 public void josephu(int k, int startNum, int num) {
    >                     if (head.next == null) {
    >                         System.out.println("链表为空~~");
    >                         return;
    >                     }
    >                     if (k < 1 || startNum < 1 || startNum > num ) {
    >                         System.out.println("参数有误");
    >                     }
    >             
    >                     JosephuCircleLinkedListNode helper = head.next;
    >                     while (true) {
    >                         if (helper.next == head.next) {
    >                             break;
    >                         }
    >                         helper = helper.next;
    >                     }
    >             
    >                     JosephuCircleLinkedListNode first = head.next;
    >                     for (int i = 0; i < k - 1; i++) {
    >                         helper = helper.next;
    >                         first = first.next;
    >                     }
    >             
    >                     while (true) {
    >                         // 环形链表只有一个元素
    >                         if (helper == first) {
    >                             break;
    >                         }
    >                         // 移动，出圈
    >                         for (int j = 0; j < startNum - 1; j++) {
    >                             helper = helper.next;
    >                             first = first.next;
    >                         }
    >                         System.out.printf("%d出圈\n", first.no);
    >                         first = first.next;
    >                         helper.next = first;
    >                     }
    >             
    >                     System.out.printf("最后%d出圈\n", helper.no);
    >                 }
    >                 
    >                 /**
    >                  * 约瑟夫问题解决方法2
    >                  * @param k
    >                  * @param startNum
    >                  * @param num
    >                  */
    >                 public void josephuTow(int k, int startNum, int num) {
    >                     if (head.next == null) {
    >                         System.out.println("链表为空~~");
    >                         return;
    >                     }
    >             
    >                     if (k < 1 || startNum < 1 || startNum > num ) {
    >                         System.out.println("参数有误");
    >                     }
    >             
    >                     JosephuCircleLinkedListNode temp = head.next;
    >                     for (int i = 0; i < k - 2 + num; i++) {
    >                         temp = temp.next;
    >                     }
    >             
    >                     while (true) {
    >                         // 环形链表只有一个元素
    >                         if (temp.next == temp) {
    >                             break;
    >                         }
    >                         // 移动，出圈
    >                         for (int j = 0; j < startNum - 1; j++) {
    >                             temp = temp.next;
    >                         }
    >                         System.out.printf("%d出圈\n", temp.next.no);
    >                         temp.next = temp.next.next;
    >                     }
    >             
    >                     System.out.printf("最后%d出圈\n", temp.no);
    >                 }
    >             
    >                 /**
    >                  * 环形链表的遍历
    >                  */
    >                 public void show() {
    >                     if (head.next == null) {
    >                         System.out.println("链表为空~~");
    >                         return;
    >                     }
    >                     // 定义temp变量完成遍历
    >                     JosephuCircleLinkedListNode temp = head.next;
    >                     while (true) {
    >                         System.out.println(temp.toString());
    >                         temp = temp.next;
    >                         if (temp == head.next) {
    >                             break;
    >                         }
    >                     }
    >                 }
    >             
    >             }
    >             ```
    >         
    >             * 单向环形链表解决约瑟夫问题时，需要两个变量的做法
    >         
    >                 ```
    >                 /**
    >                      * 约瑟夫问题
    >                      * @param k
    >                      * @param startNum
    >                      * @param num
    >                      */
    >                     public void josephu(int k, int startNum, int num) {
    >                         if (head.next == null) {
    >                             System.out.println("链表为空~~");
    >                             return;
    >                         }
    >                         if (k < 1 || startNum < 1 || startNum > num ) {
    >                             System.out.println("参数有误");
    >                         }
    >                 
    >                         JosephuCircleLinkedListNode helper = head.next;
    >                         while (true) {
    >                             if (helper.next == head.next) {
    >                                 break;
    >                             }
    >                             helper = helper.next;
    >                         }
    >                 
    >                         JosephuCircleLinkedListNode first = head.next;
    >                         for (int i = 0; i < k - 1; i++) {
    >                             helper = helper.next;
    >                             first = first.next;
    >                         }
    >                 
    >                         while (true) {
    >                             // 环形链表只有一个元素
    >                             if (helper == first) {
    >                                 break;
    >                             }
    >                             // 移动，出圈
    >                             for (int j = 0; j < startNum - 1; j++) {
    >                                 helper = helper.next;
    >                                 first = first.next;
    >                             }
    >                             System.out.printf("%d出圈\n", first.no);
    >                             first = first.next;
    >                             helper.next = first;
    >                         }
    >                 
    >                         System.out.printf("最后%d出圈\n", helper.no);
    >                     }
    >                 ```
    >                 
    >             * 单向链表解决约瑟夫问题时，只需要一个变量的做法
    >         
    >                 ```
    >                 /**
    >                      * 约瑟夫问题解决方法2
    >                      * @param k
    >                      * @param startNum
    >                      * @param num
    >                      */
    >                     public void josephuTow(int k, int startNum, int num) {
    >                         if (head.next == null) {
    >                             System.out.println("链表为空~~");
    >                             return;
    >                         }
    >                 
    >                         if (k < 1 || startNum < 1 || startNum > num ) {
    >                             System.out.println("参数有误");
    >                         }
    >                 
    >                         JosephuCircleLinkedListNode temp = head.next;
    >                         for (int i = 0; i < k - 2 + num; i++) {
    >                             temp = temp.next;
    >                         }
    >                 
    >                         while (true) {
    >                             // 环形链表只有一个元素
    >                             if (temp.next == temp) {
    >                                 break;
    >                             }
    >                             // 移动，出圈
    >                             for (int j = 0; j < startNum - 1; j++) {
    >                                 temp = temp.next;
    >                             }
    >                             System.out.printf("%d出圈\n", temp.next.no);
    >                             temp.next = temp.next.next;
    >                         }
    >                 
    >                         System.out.printf("最后%d出圈\n", temp.no);
    >                     }
    >                 ```
    >         
    >         * 单向环形链表解决约瑟夫问题
    >         
    >             ```
    >             public class CircleLinkedLIstUser {
    >                 public static void main(String[] args) {
    >                     CircleLinkedList cl = new CircleLinkedList();
    >                     cl.add(new JosephuCircleLinkedListNode(1, "一"));
    >                     cl.add(new JosephuCircleLinkedListNode(2, "二"));
    >                     cl.add(new JosephuCircleLinkedListNode(3, "三"));
    >                     cl.add(new JosephuCircleLinkedListNode(4, "四"));
    >                     cl.add(new JosephuCircleLinkedListNode(5, "五"));
    >             //        cl.show();
    >             		// 使用两个变量解决
    >                     cl.josephu(1, 3, 5);
    >                     // 使用一个变量解决
    >                     cl.josephuTwo(1, 3, 5);
    >                 }
    >             }
    >             ```
    >     
    >

## 4. 栈(Stack)
4. 栈(Stack)
    > 1. 概述
    >
    >    栈(stack)是一个先入后出的有序列表。允许插入和删除的一端，即变化的一端，称为栈顶；另一端为固定的一端，称为栈底。
    >    
    > 2. 利用栈实现简单计算器
    >
    >     * 中缀表达式字符串的简单运算器
    >     
    >         * 数字/运算符栈
    >     
    >             ```
    >         /**
    >              * 数字/符号栈
    >              * 因为char底层是int数组，因此可以和数字使用一个栈
    >              */
    >             class ArrayStack {
    >             
    >                 private int maxSize;
    >                 private int[] stack;
    >                 private int top = -1; // 栈顶
    >             
    >                 public ArrayStack(int maxSize) {
    >                     this.maxSize = maxSize;
    >                     this.stack = new int[this.maxSize];
    >                 }
    >             
    >                 /**
    >                  * 判断栈满
    >                  * @return
    >                     */
    >                   public boolean isFull() {
    >                     return top == maxSize - 1;
    >                   }
    >             
    >                 /**
    >                  * 判断栈空
    >                  * @return
    >                     */
    >                   public boolean isEmpty() {
    >                     return top == -1;
    >                   }
    >             
    >                 /**
    >                  * 入栈
    >                  * @param value
    >                     */
    >                   public void push(int value) {
    >                     // 判断是否栈满
    >                     if (this.isFull()) {
    >                         System.out.println("栈满");
    >                         return;
    >                     }
    >                     this.top = this.top + 1;
    >                     this.stack[this.top] = value;
    >                   }
    >             
    >                 /**
    >                  * 出栈
    >                  * @return
    >                     */
    >                   public int pop() {
    >                     if (this.isEmpty()) {
    >                         throw new RuntimeException("栈空");
    >                     }
    >                     int value = stack[top];
    >                     top--;
    >                     return value;
    >                   }
    >             
    >                 /**
    >                  * 遍历栈
    >                     */
    >                   public void show() {
    >                     if (this.isEmpty()) {
    >                         return;
    >                     }
    >                     for (int i = top; i >= 0; i--) {
    >                         System.out.printf("stack[%d]=%d\n", i, stack[i]);
    >                     }
    >                   }
    >             
    >                 /**
    >                  * 返回运算符优先级，优先级越高值越大
    >                  * @param oper
    >                  * @return
    >                     */
    >                   public int priority(int oper) {
    >                     if (oper == '*' || oper == '/') {
    >                         return 3;
    >                     } else {
    >                         if ( oper == '-') {
    >                             return 2;
    >                         } else {
    >                             if (oper == '+') {
    >                                 return 1;
    >                             } else {
    >                                 return -1; // 假定此运算器只有+、-、*、/运算符
    >                             }
    >                         }
    >                     }
    >                   }
    >             
    >                 /**
    >                  * 判断该字符是否为运算符
    >                  * @param val
    >                  * @return
    >                     */
    >                   public boolean isOper(char val) {
    >                     return val == '*' || val == '/' || val == '+' || val == '-';
    >                   }
    >             
    >                 /**
    >                  * 计算方法
    >                  * @param num1
    >                  * @param num2
    >                  * @param oper
    >                  * @return
    >                     */
    >                   public int cal(int num1, int num2, int oper) {
    >                     int res = 0;
    >                     switch (oper) {
    >                         case '+':
    >                             res = num1 + num2;
    >                             break;
    >                         case '-':
    >                             res = num2 - num1;
    >                             break;
    >                         case '*':
    >                             res = num1 * num2;
    >                             break;
    >                         case '/':
    >                             res = num2 / num1;
    >                             break;
    >                     }
    >                     return res;
    >                   }
    >             
    >                 /**
    >                  * 查看栈顶元素
    >                  * @return
    >                     */
    >                   public int peek() {
    >                     return stack[top];
    >                   }
    >             }
    >             ```
    >             
    >         * 简单计算器：无多位运算，无括号运算
    >         
    >             ```
    >         class SimpleCalculator {
    >                 /**
    >                  * 对一个运算表达式字符串进行计算
    >                  * 简单运算，但是无法进行多位数运算，无法进行括号运算
    >                  *
    >                  * @param e
    >                  * @return
    >                  */
    >                 public int calSimple(String e) {
    >                     // 创建两个栈，一个存储数，一个存储运算符
    >                     ArrayStack numStack = new ArrayStack(10);
    >                     ArrayStack operStack = new ArrayStack(10);
    >                     int index = 0;
    >                     int num1 = 0;
    >                     int num2 = 0;
    >                     int oper = 0;
    >                     char c = ' ';
    >                     int res = 0;
    >             
    >                     // 遍历字符串
    >                     while (true) {
    >                         c = e.charAt(index);
    >                         // 判断是什么字符
    >                         // 判断是否为运算符
    >                         if (operStack.isOper(c)) {
    >                             // 是运算符
    >                             // 判断当前运算符栈是否为空
    >                             if (!operStack.isEmpty()) {
    >                                 // 运算符栈不为空
    >                                 // 判断当前运算符和栈顶运算符的优先级
    >                                 if (operStack.priority(c) <= operStack.peek()) {
    >                                     // 当前运算符优先级低于栈顶运算符
    >                                     num1 = numStack.pop();
    >                                     num2 = numStack.pop();
    >                                     oper = operStack.pop();
    >                                     res = operStack.cal(num1, num2, oper);
    >                                     numStack.push(res);
    >                                 }
    >                             }
    >                             operStack.push(c);
    >                         } else {
    >                             // 不是运算符，将char类型的数字转为int类型存入
    >                             numStack.push(c - 48);
    >                         }
    >                         index++;
    >                         if (index >= e.length()) {
    >                             break;
    >                         }
    >                     }
    >                 
    >                     // 运算数栈和符号栈的元素
    >                     while (true) {
    >                         if (operStack.isEmpty()) {
    >                             break;
    >                         }
    >                         num1 = numStack.pop();
    >                         num2 = numStack.pop();
    >                         oper = operStack.pop();
    >                         res = operStack.cal(num1, num2, oper);
    >                         numStack.push(res);
    >                     }
    >                 
    >                     // 将最后的结果从栈中弹出
    >                     return numStack.pop();
    >                 }
    >             }
    >             ```
    >             
    >         * 简单计算器：利用逆波兰表达式（后缀表达式）完成
    >         
    >             * 逆波兰表达式（后缀表达式）
    >             
    >                 1. 概念
    >                 
    >                    数值在前，操作符在后的表达式。中缀表达式转后缀表达式，例如：
    >                    
    >                     ```
    >                     a+b ==> ab+ 
    >                     a+(b-c) ==> abc-+ ;
    >                     a+(b-c)*d ==> abc-d*+ 
    >                     a+d*(b-c) ==> adbc-*+  
    >                     a=1+3 ==> a13+=
    >                     ```
    >                    
    >                 2. 后缀表达式的计算机求值
    >                 
    >                    从左到右扫描后缀表达式，遇到数值，压入栈，遇到运算符，弹出栈中两个数值，用运算符进行数值计算，并将结果压栈。重复上述过程直到扫描完表达式，最后结果便为表达式结果。
    >                    
    >                 3. 逆波兰表达式计算器代码实现：
    >                 
    >                     ```
    >                     /**
    >                          * 扫描逆波兰表达式并计算出结果
    >                          * @param e
    >                          * @return
    >                          */
    >                         public int cal(String e) {
    >                     
    >                             Stack<Integer> stack = new Stack<>();
    >                             int index = 0; // 索引
    >                             char c = ' '; // 存放字符
    >                             int num1 = 0;
    >                             int num2 = 0;
    >                             while(true) {
    >                                 if (index >= e.length()) {
    >                                     break;
    >                                 }
    >                                 c = e.charAt(index);
    >                                 // 如果是运算数
    >                                 if (c >= 48 && c <= 57) {
    >                                     stack.push(Integer.parseInt(c + ""));
    >                                 } else {
    >                                     // 如果是运算符
    >                                     num1 = stack.pop();
    >                                     num2 = stack.pop();
    >                                     switch (c) {
    >                                         case '+':
    >                                             stack.push(num2 + num1);
    >                                             break;
    >                                         case '-':
    >                                             stack.push(num2 - num1);
    >                                             break;
    >                                         case '*':
    >                                             stack.push(num2 * num1);
    >                                             break;
    >                                         case '/':
    >                                             stack.push(num2 / num1);
    >                                             break;
    >                                     }
    >                                 }
    >                                 index++;
    >                             }
    >                             return stack.pop();
    >                         }
    >                     ```
    >                     
    >                 4. 中缀表达式转后缀表达式算法步骤：
    >                 
    >                     * 算法步骤
    >                 
    >                         ```
    >                         1. 定义两个栈，一个S1存储运算符，一个S2做中间结果
    >                         2. 从左到右扫描扫描中缀表达式
    >                         3. 遇到操作数，压入S1
    >                         4. 遇到操作符，比较其与S2栈顶操作符的优先级关系
    >                         	1> 如果S2栈为空，或者栈顶的运算符为(，则直接将该运算符压入栈S2
    >                         	2> 否则，如果优先级大于栈顶运算符优先级，则将运算符压入栈S2
    >                         	3> 否则，弹出S2栈顶的运算符，压入S1中，继续从4.1>开始
    >                         5. 遇到括号时
    >                         	1> 遇到(，直接压入S2
    >                         	2> 遇到)，依次弹出S2栈顶的运算符，并压入S1，直到遇到左括号为止，此时这一对括号废弃
    >                         6. 重复步骤2到5，直到表达式最右边
    >                         7. 将S1剩余的运算符依次弹出并压入S2
    >                         8. 依次弹出S2中的元素并输出，结果的逆序便是中缀表达式的后缀表达式形式
    >                         ```
    >                         
    >                     * 假设有一个中缀表达式```1+((2+3)*4)-5```，转为后缀表达式
    >                     
    >                         ```
    >                         扫描的元素 S1（栈底->栈顶） S2（栈底->栈顶）
    >                         1		 空				1
    >                         +		 +				 1
    >                         (	     +(				 1
    >                         (		 +((			 1
    >                         2	     +((             12
    >                         +        +((+            12
    >                         3        +((+            123
    >                         )        +(              123+
    >                         *        +(*             123+
    >                         4        +(*             123+4
    >                         )        +               123+4*
    >                         -        -               123+4*+
    >                         5        -               123+4*+5
    >                         空       空               123+4*+5-
    >                         ```
    >                         
    >                     * 中缀表达式和逆波兰表达式的转换代码
    >                     
    >                         ```
    >                         /**
    >                              * 中缀表达式转后缀表达式
    >                              * 不支持表达式中有空格等其他特殊符号
    >                              * @param e
    >                              * @return
    >                              */
    >                             public String change(String e) {
    >                                 // 将中缀表达式中的运算符和运算数取出组成一个String集合以方便计算
    >                                 List<String> zhongjian = new ArrayList<>();
    >                                 char c = ' ';
    >                                 int index = 0;
    >                                 String temp = "";
    >                                 do {
    >                                     if ((c = e.charAt(index)) < 48 || (c = e.charAt(index)) > 57) {
    >                                         zhongjian.add(c + "");
    >                                         index++;
    >                                     } else {
    >                                         temp = "";
    >                                         while (index < e.length() && (c = e.charAt(index)) >= 48 && (c = e.charAt(index)) <= 57) {
    >                                             temp = temp + c;
    >                                             index++;
    >                                         }
    >                                         zhongjian.add(temp);
    >                                     }
    >                                 } while (index < e.length());
    >                                 
    >                                 // 转换
    >                                 Stack<String> stack = new Stack<>();
    >                                 int indexx = 0;
    >                                 String res = "";
    >                                 List<String> nibolan = new ArrayList<>();
    >                                 // 因为使用栈来存储转换后的后缀表达式的各个元素，需要先出栈后再反转，因此为了方便
    >                                 // 使用集合来存储，就可以不用反转
    >                                 for (String s :
    >                                         zhongjian) {
    >                                     if (s.charAt(0) >= 48 && s.charAt(0) <= 57) {
    >                                     	// 非数字直接加入中间体
    >                                         nibolan.add(s);
    >                                     } else if (s.charAt(0) == '(') {
    >                                     	// (，直接入栈
    >                                         stack.push(s);
    >                                     } else if (s.charAt(0) == ')') {
    >                                     	// )，一次从栈中弹出运算符并加入中间体直到栈顶为(，最后弹出栈顶的(废弃掉
    >                                         while (!stack.peek().equals("(")) {
    >                                             nibolan.add(stack.pop());
    >                                         }
    >                                         stack.pop();
    >                                     } else if (stack.size() == 0 || stack.peek().equals("(")) {
    >                                     	// 对于运算符，如果栈为空或者栈顶为(，直接入栈
    >                                         stack.push(s);
    >                                     } else if (youXianJi(s) > youXianJi(stack.peek())) {
    >                                     	// 否则，对于运算符，如果该运算符优先级大于栈顶运算符优先级，直接入栈
    >                                         stack.push(s);
    >                                     } else {
    >                                     	// 否则，对于运算符，如果该运算符优先级小于栈顶运算符优先级，先弹出栈顶运算符，加入中间体
    >                                         nibolan.add(stack.pop());
    >                                         while (true) {
    >                                         // 持续比较栈顶的运算符和该运算符，直到栈为空或者或者栈顶为(，该运算符优先级大于栈顶运算符优先级
    >                                             if (stack.size() == 0 || stack.peek().equals("(") || youXianJi(s) > youXianJi(stack.peek())) {
    >                                             // 如果栈为空或者栈顶为(，该运算符优先级大于栈顶运算符优先级，入栈，退出
    >                                                 stack.push(s);
    >                                                 break;
    >                                             } else {
    >                                             // 该运算符优先级小于栈顶运算符优先级，先弹出栈顶运算符，加入中间体
    >                                                 nibolan.add(stack.pop());
    >                                             }
    >                                         }
    >                                     }
    >                                 }
    >                                 while (stack.size() != 0) {
    >                                     nibolan.add(stack.pop());
    >                                 }
    >                                 for (String s :
    >                                         nibolan) {
    >                                     res = res + s;
    >                                 }
    >                                 return res;
    >                             }
    >                         
    >                             /**
    >                              * 计算一个运算符的优先级
    >                              * @param e
    >                              * @return
    >                              */
    >                             public int youXianJi(String e) {
    >                                 if (e.charAt(0) == '+' || e.charAt(0) == '-') {
    >                                     return 1;
    >                                 } else {
    >                                     return 2;
    >                                 }
    >                             }
    >                         ```
    >                         
## 5. 递归（Recursion）
5. 递归（Recursion）
   
    > 1. 递归的时间复杂度Master公式：
    > 
    >    T(N) = a * T(N / b) + O(n^d)。
    >    if log(b)a < d,  O(n^d)
    >    if log(b)a > d,  O(n^log(b)a)
    >    if log(b)a = d,  O(n^d*logn)
    >
    > 2. 迷宫回溯问题的代码实现
    >
    >     ```
    >     /**
    >          * 显示迷宫
    >          * @param map
    >          */
    >         public void show(int[][] map) {
    >             for (int[] i :
    >                     map) {
    >                 for (int j:
    >                      i) {
    >                     System.out.print(j + "\t");
    >                 }
    >                 System.out.println();
    >             }
    >         }
    >     
    >         /**
    >          * 创建一个8*7的迷宫
    >          */
    >         public int[][] create() {
    >             // 创建地图
    >             int[][] map = new int[8][7];
    >             // 设墙壁为1
    >             for (int i = 0; i < 8; i++) {
    >                 for (int j = 0; j < 7; j++) {
    >                     if (i == 0 || i == 7) {
    >                         map[i][j] = 1;
    >                     } else {
    >                         map[i][0] = 1;
    >                         map[i][6] = 1;
    >                     }
    >                     if (i == 4) {
    >                         map[i][1] = 1;
    >                         map[i][2] = 1;
    >                     }
    >                 }
    >             }
    >             return map;
    >         }
    >     
    >         /**
    >          * 使用递归回溯帮助小球从左上走到右下
    >          * 小球探路策略为：向下->向右->向上->向左
    >          * @param map 地图
    >          * @param i 小球y坐标
    >          * @param j 小球x坐标
    >          * @return 找到通路为真，否则为假
    >          */
    >         public boolean setWay(int[][] map, int i, int j) {
    >             if (map[6][5] == 2) {
    >                 // 迷宫重点被激活，回退寻找路线
    >                 count[c]++;
    >                 return true;
    >             } else {
    >                 // 判断此坐标小球是否走过
    >                 if (map[i][j] == 0) {
    >                     // 小球未走过，先假设此路线坐标正确，继续寻路
    >                     map[i][j] = 2;
    >                     // 小球继续按照策略走
    >                     // 小球先向下走
    >                     if (setWay(map, i + 1, j)) {
    >                         // 向下走可以，该路线坐标正确
    >                         count[c]++;
    >                         return true;
    >                     } else if (setWay(map, i, j + 1)) {
    >                         // 向下走不可以，判断是否可以向右走，可以向右走，该路就是通的
    >                         count[c]++;
    >                         return true;
    >                     } else if (setWay(map, i - 1, j)) {
    >                         // 向下走和向上走都不可以，判断是否可以向右走，可以则该路为通
    >                         count[c]++;
    >                         return true;
    >                     } else if (setWay(map, i, j - 1)) {
    >                         count[c]++;
    >                         return true;
    >                     } else {
    >                         // 此路不通，标记此路线坐标为死路，并说明错误
    >                         map[i][j] = 3;
    >                         return false;
    >                     }
    >                 } else {
    >                     // 小球走过，是墙，死路，都说明该路线坐标错误
    >                     return false;
    >                 }
    >             }
    >         }
    >     ```
    >
    > 3. 八皇后问题
    >
    >     1. 问题概况
    >
    >        在一个8*8的的国际象棋棋盘中，摆列8个皇后，保证无法相互攻击。
    >        
    >     2. 解决代码 
    >       
    >         ```
    >         /**
    >          * 解决八皇后问题
    >          */
    >         class EightQueensSolve {
    >         
    >             public int[] solve = new int[8];
    >             private int temp = 0;
    >         
    >             /**
    >              * 判断第n个皇后和前面的皇后是否满足八皇后问题的条件
    >              * @param n
    >              * @return
    >              */
    >             public boolean judge (int n) {
    >                 for (int i = 0; i < n; i++) {
    >                     // 判断是否在同一列，或同一斜线
    >                     if (solve[i] == solve[n] || Math.abs(n - i) == Math.abs(solve[n] - solve[i])) {
    >                         return false;
    >                     }
    >                 }
    >                 return true;
    >             }
    >         
    >             /**
    >              * 放置皇后
    >              * 分解放置问题，就是在每一行都试一次，从第一个列开始，如果有一个情况和之前的皇后都满足八皇后问题，
    >              * 则继续向后放置，否则，验证下一个列，直到验证完全部的列。结束递归条件就是第八行的皇后也放置了
    >              * @param n
    >              */
    >             public void eightQueen(int n) {
    >                 if (n == 8) {
    >                     // 说明八个皇后已经放好了，这个时候要结束递归了
    >                     temp++;
    >                     for (int i = 0; i < 8; i++) {
    >                         System.out.printf("%d\t", solve[i]);
    >                     }
    >                     System.out.printf("第%d个解法", temp);
    >                     System.out.println();
    >                     return;
    >                 }
    >                 // 放入皇后，并判断是否冲突
    >                 for (int i = 0; i < 8; i++) {
    >                     // 放入皇后
    >                     solve[n] = i;
    >         
    >                     // 判断是否满足八皇后问题
    >                     if (judge(n)) {
    >                         // 满足的话继续向后放入第 n + 1 个皇后
    >                         eightQueen(n + 1);
    >                     }
    >                 }
    >                 // 不满足回溯到满足的那一步
    >             }
    >         }
    >         ```
    >         
## 6. 排序算法（SortingAlgorithm）
6. 排序算法（SortingAlgorithm）
    > 1. 排序分类
    > 
    >     * 内部排序
    >     
    >        将全部的数据加入内存进行排序
    >        
    >     * 外部排序
    >     
    >        数据量大，无法全部加入内存，需要借助外部存储排序
    >     
    > 2. 内部排序
    > 
    >     1. 插入排序
    >     
    >         1. 直接插入排序
    >         
    >         2. 希尔排序
    >         
    >     2. 选择排序
    >     
    >         1. 简单选择排序
    >         
    >         2. 堆排序
    >         
    >     3. 交换排序
    >     
    >     4. 归并排序
    >     
    >     5. 基数排序（桶排序扩展版）
    > 
### 1. 冒泡排序（BubbleSort）
1. 冒泡排序(BubbleSort)
    > 1. 介绍
    >
    >    基本思想是：通过对待排序序列的从前到后，一次比较相邻元素的值，若发现逆序则交换，使得值较大的元素逐渐从前移动到后。
    >    
    >     * 注意：因为排序的过程中，各个元素会不断去接近自己的位置，因此，如果一趟下来没有元素进行交换，则说明序列有序，不用继续下去了，因此需要一个flag来判断元素是否交换过，以减少不必要的比较
    >    
    > 2. 冒泡排序规则
    >
    >     1. 需要进行数组.length - 1次排序
    >     
    >     2. 每一次排序次数在减少
    >     
    > 3. 实例代码
    >
    >    ```
    >    /**
    >         * 冒泡排序
    >         */
    >        public static void sort() {
    >    
    >            int temp = 0;
    >            // 优化，加入flag
    >            boolean flag = false;
    >            for (int i = bubble.length - 1; i > 0; i--) {
    >                for (int j = 0; j < i; j++) {
    >                    if (bubble[j] < bubble[j + 1]) {
    >                        temp = bubble[j];
    >                        bubble[j] = bubble[j + 1];
    >                        bubble[j + 1] = temp;
    >                        flag = true;
    >                    }
    >                }
    >                if (!flag) {
    >                    break;
    >                } else {
    >                    // 重置
    >                    flag = true;
    >                }
    >           }
    >        }
    >    ```
    >    
### 2. 选择排序（SelectionSort）
2. 选择排序（SelectionSort）
   > 1. 介绍
   >
   >    第一次从arr[0]到arr[n - 1]中选取最小/最大的元素和arr[0]交换，第二次从arr[1]到arr[n - 1]中选取最小/最大的元素和arr[1]交换，依次到第arr[n - 2]到arr[n - 1]选取最小/最大的元素和arr[n - 2]交换，结束。最终得到的就是有序数组。
   >    
   > 2. 实例代码
   >
   >     ```
   >     /**
   >          * 选择排序
   >          */
   >         public static void selection() {
   >             int temp = 0;
   >             for (int i = 0; i < selection.length - 1; i++) {
   >                 for (int j = i + 1; j < selection.length; j++) {
   >                     if (selection[i] < selection[j]) {
   >                         temp = selection[i];
   >                         selection[i] = selection[j];
   >                         selection[j] = temp;
   >                     }
   >                 }
   >             }
   >         }
   >     ```
   >     
   > 3. 通过对比可以得到：选择排序要比冒泡排序速度快，因为选择排序比冒泡排序进行的内存数据交换操作少，因此快
   > 
### 3. 插入排序（InsertionSort）
3. 插入排序（InsertionSort）
    > 1. 介绍
    >
    >    是对于与排序的元素以插入的方式找寻元素的适当位置。基本思想：把n个待排序元素看成一个有序表和无序表开始时有序表有一个元素，无序表中有n - 1个元素，排序过程为每次从无序表中取出一个元素，把它的排序码依次与有序表中元素的排序码进行比较，将它插入到有序表中的适当位置，使之成为一个新的有序表。
    >    
    > 2. 实例代码
    >
    >     ```
    >     /**
    >          * 插入排序
    >          */
    >         public static void insertion() {
    >             int temp = 0;
    >             for (int i = 1; i < insertion.length; i++) {
    >                 temp = insertion[i];
    >                 for (int j = i - 1; j >= 0; j--) {
    >                     if (temp > insertion[j]) {
    >                         insertion[j + 1] = insertion[j];
    >                     } else {
    >                     	insertion[j] = temp;
    >                     }
    >                 }
    >             }
    >         }
    >     ```
    >     
### 4. 希尔排序（ShellSort）
4. 希尔排序（ShellSort）
    > 1. 插入排序存在的问题
    >
    >    假如有一个数组arr = {2, 3, 4, 5, 6, 1}，此时1要插入，使用插入排序做到从小到大排序。过程就是：
    >    
    >     ```
    >     arr = {2, 3, 4, 5, 6, 6} temp = 1
    >      arr = {2, 3, 4, 5, 5, 6} temp = 1
    >       arr = {2, 3, 4, 4, 5, 6} temp = 1
    >        arr = {2, 3, 3, 4, 5, 6} temp = 1
    >         arr = {2, 2, 3, 4, 5, 6} temp = 1
    >          arr = {2, 2, 3, 4, 5, 6} temp = 1
    >           arr = {1, 3, 4, 5, 6, 1} temp = 1，结束
    >     ```
    >    
    >     * 由此可见，当需要插入的数很小时，后移的次数明显变多，对效率造成影响
    >    
    > 2. 介绍
    >
    >    它是简单插入排序的一个更高效的版本，也被成为缩小增量排序
    >    
    >    基本思想：把记录按下标的一定增量分组，对每一组直接使用排序算法排序，随着增量的减少，每组包含的关键词越来越多，当增量减小至1时，整个文件被分为一组，算法终止。
    >    
    > 3. 图解希尔排序
    >
    >     ```
    >     
    >     假设有一个数组arr = {8, 9, 7, 1, 2, 3, 4, 5, 6, 3}
    >     
    >     使用希尔排序：
    >     
    >     	1> 定义一个最初的增量S，假设S=3
    >     	
    >     	2> S=3，根据下标，则从0索引开始每第S个索引的元素作为一组，则将arr分成S组。
    >     	   即
    >     	    arr={ #, 9, 7, #, 2, 3, #, 5, 6, #}, arr1={8, 1, 4, 3}；
    >     	     arr={8, #, 7, 1, #, 3, 4, #, 6, 3}, arr2={9, 2, 5}；
    >     	      arr={8, 9, #, 1, 2, #, 4, 5, #, 3}, arr3={7, 3, 6}，
    >     	   将arr1, arr2, arr3组进行插入排序，并影射回arr中，arr变成了arr={1, 2, 3, 3, 5, 6, 4, 9, 7, 8}。减少增量S，S = S / 2 = 1
    >     	
    >     	3> S=1，根据下标，则从0索引开始每第1个索引的元素作为一组，即就是整个数组arr。对arr进行插入排序，排序完成，结束。
    >     ```
    > 4. 希尔排序，代码如下
    >
    >     ```
    >     	/**
    >          * 希尔排序
    >          * @param s 增量
    >          */
    >         public static void shell(int s) {
    >             // 对s个数组进行插入排序
    >             int temp = 0;
    >             
    >             // 每个数组
    >             for (int i = 0; i < s; i++) {
    >                 // 插入排序
    >                 for (int k = i; k < shell.length; k = k + s) {
    >                     temp = shell[k];
    >                     for (int l = k - s; l >= 0; l = l - s) {
    >                         if (temp > shell[l]) {
    >                             shell[l + s] = shell[l];
    >                             shell[l] = temp;
    >                         }
    >                     }
    >                 }
    >             }
    >             
    >             // 当s==1，算法结束，开始返回
    >             if (s == 1) {
    >                 return;
    >             } else {
    >                 // s变小，继续排序
    >                 s = s / 2;
    >                 shell(s);
    >             }
    >         }
    >         
    >         /**
    >          * 希尔排序，递归改为for循环
    >          * @param s 增量
    >          */
    >         public static void shell(int s) {
    >             // 对s个数组进行插入排序
    >             int temp = 0;
    >     
    >             // 当s==1，算法结束
    >             for (int m = s; m > 0; m = m / 2) {
    >                 // 每个数组
    >                 for (int i = 0; i < m; i++) {
    >                     // 插入排序
    >                     for (int k = i + m; k < shell.length; k = k + m) {
    >                         temp = shell[k];
    >                         for (int l = k - m; l >= 0; l = l - m) {
    >                             if (temp > shell[l]) {
    >                                 shell[l + m] = shell[l];
    >                                 shell[l] = temp;
    >                             }
    >                         }
    >                     }
    >                 }
    >             }
    >         }
    >     ```
    >     
    > 5. 通过对比可知，希尔排序比简单插入排序快
    >
### 5. 快速排序（QuickSort）
5. 快速排序（QuickSort）
    > 1. 介绍
    >
    >    是对冒泡排序的优化
    >    
    >    基本思想：通过一趟排序将原数据n分为两部分，保证其中一部分的所有数据比另一部分的所有数据要小，然后继续使用此方法排序这两部分的数据，整个过程可以递归进行，以达到整个数据有序。
    >    
    > 2. 算法图解
    >
    >     ```
    >     假设有个数组arr={2, 10, 8, 7, 2, 12, 6, 5, 3}
    >     	第一趟
    >     		1> 定义两个索引，一个指向数组头，一个指向数组尾以arr数组中间的数的索引为基准，即基数S = (int) (arr.lenth / 2 + 1) = 4
    >     		2> 最开始以arr[S]即12为基准数
    >     		
    >     		3> 
    >     ```
    >     
    > 3. 快速排序代码
    >
    >     ```
    >     /**
    >          * 快速排序
    >          * @param arr
    >          * @param left
    >          * @param right
    >          */
    >         public static void quick(int[] arr, int left, int right) {
    >             int l = left;
    >             int r = right;
    >             int temp = 0;
    >             int pivot = arr[(left + right) / 2];
    >             while (l < r) {
    >                 while (arr[l] < pivot) {
    >                     l++;
    >                 }
    >                 while (arr[r] > pivot) {
    >                     r--;
    >                 }
    >                 if (l >= r) {
    >                     break;
    >                 } else {
    >                     temp = arr[l];
    >                     arr[l] = arr[r];
    >                     arr[r] = temp;
    >                 }
    >     			
    >     			// 下面两个判断是防止左右两边都有arr[l] == pivot, arr[r] == pivot而导致的死循环
    >                 if (arr[l] == pivot) {
    >                     r--;
    >                 }
    >                 if (arr[r] == pivot) {
    >                     l++;
    >                 }
    >             }
    >             if (l == r) {
    >                 r--;
    >                 l++;
    >             }
    >             if (left < r) {
    >                 quick(arr, left, r);
    >             }
    >             if (right > l) {
    >                 quick(arr, l, right);
    >             }
    >         }
    >     ```
    >     
### 4. 归并排序（MergeSort）
4. 归并排序（MergeSort）
    > 1. 介绍
    >
    >     利用归并的思想实现排序的方法，该算法采用经典的分治策略然后递归求解。
    >     
    > 2. 代码实现
    >
    >     ```
    >     /**
    >          * 归并排序
    >          * @param arr
    >          * @param left
    >          * @param right
    >          * @param temp
    >          */
    >         public static void merge(int[] arr, int left, int right, int[] temp) {
    >     
    >             //分
    >             if (left >= right) {
    >                 return;
    >             } else {
    >                 merge(arr, left, (left + right) / 2, temp);
    >                 merge(arr, (left + right) / 2 + 1, right, temp);
    >     
    >                 //治
    >                 int mid = (left + right) / 2;
    >                 int i = left;
    >                 int j = mid + 1;
    >                 int k = 0;
    >                 // 将有序的两个序列的元素按顺序压入临时数组temp
    >                 while (i <= mid && j <= right) {
    >                     if (arr[i] <= arr[j]) {
    >                         temp[k] = arr[i];
    >                         k++;
    >                         i++;
    >                     } else {
    >                         temp[k] = arr[j];
    >                         k++;
    >                         j++;
    >                     }
    >                 }
    >                 // 将剩余数据全部压入temp
    >                 while (i <= mid) {
    >                     temp[k] = arr[i];
    >                     i++;
    >                     k++;
    >                 }
    >                 while (j <= right) {
    >                     temp[k] = arr[j];
    >                     j++;
    >                     k++;
    >                 }
    >                 // 将temp拷贝回arr
    >                 int l = 0;
    >                 int t = left;
    >                 while(t <= right) {
    >                     arr[t] = temp[l];
    >                     l++;
    >                     t++;
    >                 }
    >             }
    >         }
    >     ```
    >     
    > 3. 以上基于待排序元素的大小做排序的算法，下界为O(nlogn)
    >    
### 5. 基数排序（RadixSort）
5. 基数排序（RadixSort）
    > 1. 介绍
    >
    >    基数排序属于分配式排序，又称桶子法。它是通过键值的各个位（个、十、百、千...）的值，将要排序的元素分配到不同的桶中，达到排序的目的。
    >    
    >    基数排序法属于稳定型的排序，是效率高的稳定型排序法
    >    
    >    基数排序是桶排序的扩展。
    >    
    >     * **稳定型和不稳定型算法：算法保证待排序列中，有arr[i] == arr[j]且arr[i]在arr[j]前，排序后arr[i]仍在arr[j]前，则称该算法稳定；否则不稳定**
    >    
    > 2. 算法图解
    >
    >     ```
    >     arr={32, 12, 17, 6, 556, 47, 352, 2, 1, 845, 65}
    >     定义10个桶用来存储数据，我们使用二维数组来做，int[][] temp = new int[10][arr.length]
    >         1> 第一次，判断个位
    >            temp[0] = {}
    >            temp[1] = {1} 
    >            temp[2] = {32, 12, 352, 2}
    >            temp[3] = {}
    >            temp[4] = {}
    >            temp[5] = {845, 65}
    >            temp[6] = {6, 556}
    >            temp[7] = {17, 47}
    >            temp[8] = {}
    >            temp[9] = {}
    >            这样，就按照个位大小将原数组arr排序好了，之后，将temp数组中的数据，重新按顺序重新放入arr，
    >            得到arr={1, 32, 12, 352, 2, 845, 65, 6, 556, 17, 47}
    >         2> 第二次，判断十位，将1>得到的新数组进行十位的判断，方法同上，得到新数组arr
    >         3> 持续判断，直到arr中最大的数的最大的位判断完为止
    >         4> 最后得到的就是排序好的数组
    >     ```
    >     
    > 3. 代码
    >
    >     ```
    >     /**
    >          * 基数排序
    >          * 不支持负数
    >          * @param arr
    >          */
    >         public static void radix(int[] arr) {
    >     
    >             // 找出最大的数
    >             int max = arr[0];
    >             for (int i = 1; i < arr.length; i++) {
    >                 if (arr[i] > max) {
    >                     max = arr[i];
    >                 }
    >             }
    >             // 得到最大数的最大位数
    >             int maxLen = (max + "").length();
    >             // 针对每个数不同的位排序
    >             int k = 1;
    >             // 定义一个二维数组，用来存储arr中的元素
    >             // 为了防止数组溢出造成数组越界，我们定义每个一维数组（桶）的长度为arr.length
    >             int[][] temp = new int[10][arr.length];
    >             // 为了记录每个桶的实际数据，定义一个一维数组专门用来记录每个桶的有效数据个数，每个桶的初始实际数据都为0
    >             int[] bucketElementCounts = new int[10];
    >              
    >             for (int m = 0; m < maxLen; m++) {
    >                 // 取出个位的数
    >                 for (int i = 0; i < arr.length; i++) {
    >                     // 取出arr每个数据的个位数
    >     
    >                     int digitOfElement = arr[i] / k % 10;
    >                     // 存放arr数据在桶中
    >                     temp[digitOfElement][bucketElementCounts[digitOfElement]] = arr[i];
    >                     // 让桶的实际数据+1
    >                     bucketElementCounts[digitOfElement]++;
    >                 }
    >                 // 将temp中的每个数据依次回写给arr
    >                 int index = 0; // arr的下标
    >                 // 遍历每一个桶，桶中如果有数据回写给arr
    >                 for (int i = 0; i < temp.length; i++) {
    >                     if (temp[i].length != 0) {
    >                         for (int j = 0; j < temp[i].length; j++) {
    >                             arr[index] = temp[i][j];
    >                             index++; // 每回写一条数据，arr索引向后移动
    >                         }
    >                         // 桶中数据回写完毕，清空桶，即桶中实际元素个数置为0
    >                         bucketElementCounts[i] = 0;
    >                     }
    >                 }
    >                 // 每次循环，k = k * 10以便于依次获取每个数的不同位
    >                 k = k * 10;
    >             }
    >         }
    >     ```
    >     
    > 4. 可见，基数排序是典型的空间换时间的排序。基数排序需要10个桶，每个桶的大小都是待排数组的大小，因此该算法是需要消耗大量空间的。
    >
    > 5. 对于有负数的待排序列尽量不要使用基数排序，如果一定要使用，需要对之前的代码进行优化，这里不再写出。


## 7. 算法的复杂度
### 1. 算法的时间复杂度    
1. 算法的时间复杂度
    > 1. 度量一个程序的时间复杂度方法
    >
    >     1、事后统计
    >     
    >     2、事前估计
    >     
    > 2. 时间频度
    >
    >    一个算法花费的时间和语句的执行次数成正比例，因此，一个算法的语句执行次数成为算法的语句频度或者时间频度，记为T(n)
    >
    > 3. 时间频度分析
    >
    >    忽略常数项，忽略低次项，忽略系数
    >    
    > 4. 时间复杂度
    >
    >     1. 一般情况下，算法的基本语句的执行次数是问题规模n的某个函数，用T(n)表示。设有个辅助函数f(n)，使得当n趋近于无穷大，T(n)/f(n)为不等于零的常数，则称T(n)和f(n)是同一个数量级的函数，记做T(n)=O(f(n))，称O(f(n))为算法的渐进时间复杂度，即时间复杂度
    >     
    >     2. T(n)不同，但是时间复杂度可能相同，如：T(n)=2n^2+7n+6和T(n)=3n^2+6n+4，他们的时间复杂度都为O(n^2)
    >     
    >     3. 计算时间复杂度的方法
    >     
    >         * 用常数1代表程序运行中的一切加法常数，如：T(n)=2n^2+7n+6 => T(n)=2n^2+7n+1
    >         
    >         * 在修改后的运行次函数中，只保留最高阶，如：T(n)=2n^2+7n+1 => T(n)=2n^2
    >         
    >         * 去掉最高阶的系数，如：T(n)=2n^2 => T(n)=n^2 => T(n) =O(n^2)
    >         
    >     4. 常见的算法的时间复杂度
    >     
    >         * 常数阶O(1)
    >         
    >         * 对数阶O(lgn)
    >         
    >         * 线性阶O(n)
    >         
    >         * 线性对数阶O(nlog_2n)
    >         
    >         * 平方阶O(n^2)
    >         
    >         * 立方阶O(n^3)
    >         
    >         * k次方阶O(n^k)
    >         
    >         * 指数阶O(2^n)
    >         
    >         * **注意：**
    >         
    >            常见的算法的时间复杂度从小到大依次为：O(1) < O(lgn) < O(n) < O(nlgn) < O(n^2) < O(n^3) < O(n^k) < O(2^n)，n的规模越大，算法的效率越低
    >            
    >            因此，我们应该尽量避免使用指数阶算法
    >         
    >    5. 平均时间复杂度和最坏时间复杂度
    >    
    >        1. 平均时间复杂度
    >        
    >           所有的输入实例以等概情况出现时，该算法运行时间
    >           
    >        2. 最坏时间复杂度
    >        
    >           最坏情况下的时间复杂度，一般讨论的时间复杂度就是最坏时间复杂度，因为这样就保证了该算法运行时间不会比最坏情况更差
    >           
    >        3. 常见的排序算法的平均时间复杂度和最坏时间复杂度
    >        
    >           参见D:\JavaZiLiao\DataStructurePhoto\DataStructure1.png图片
    >        
### 2. 算法的空间复杂度
2. 算法的空间复杂度
    > 1. 介绍
    > 
    >    类似于算法的时间复杂度分析，定义为算法占用的存储空间，也是一个问题规模n的函数
    >    
    > 2. 在做算法分析时，主要讨论的是时间复杂度。从用户体验看，更看重程序执行时间，一些缓存产品如redis等和算法如基数排序本质就是空间换时间
    > 
## 8. 查找算法（SelectionAlgorithm）
### 1. 线性（顺序查找）查找（LinearSearch）
### 2. 二分查找（BinarySearch）
2. 二分查找（BinarySearch）
    > 1. 介绍
    >
    >    又名折半查找，是一种对**有序列表**查找的算法
    >    
    > 2. 代码实现
    >
    >     ```
    >     	/**
    >          * 二分查找
    >          * @param arr
    >          * @param left
    >          * @param right
    >          * @param findValue
    >          * @return
    >          */
    >         public static int binary(int[] arr, int left, int right, int findValue) {
    >             // 如果数组中没有要查找的元素，则结束递归，返回-1
    >             if (left > right) {
    >                 return -1;
    >             }
    >     
    >             int midIndex = (left + right) / 2; // 获取中间值得坐标
    >             int mid = arr[midIndex]; // 获取中间值
    >     
    >             if (findValue > mid) {
    >                 return binary(arr, midIndex + 1, right, findValue);
    >             } else if (findValue < mid) {
    >                 return binary(arr, left, midIndex - 1, findValue);
    >             } else {
    >                 return midIndex;
    >             }
    >         }
    >         
    >         
    >         /**
    >          * 二分查找
    >          * 如果查找的值在数组中有多个，则把所有值的下标都拿到
    >          * @param arr
    >          * @param left
    >          * @param right
    >          * @param findValue
    >          * @return
    >          */
    >         public static List binary(int[] arr, int left, int right, int findValue) {
    >             // 如果数组中没有要查找的元素，则结束递归，返回-1
    >             if (left > right) {
    >                 return new ArrayList<Integer>();
    >             }
    >     
    >             int midIndex = (left + right) / 2; // 获取中间值得坐标
    >             int mid = arr[midIndex]; // 获取中间值
    >     
    >             if (findValue > mid) {
    >                 return binary(arr, midIndex + 1, right, findValue);
    >             } else if (findValue < mid) {
    >                 return binary(arr, left, midIndex - 1, findValue);
    >             } else {
    >                 // 找到了值，就创建一个集合存放值得坐标;
    >                 List<Integer> value = new ArrayList<>();
    >                 value.add(midIndex);
    >                 // 查找左边有无相同的值
    >                 int l = midIndex - 1;
    >                 while (true) {
    >                     // 左边没有相同的值，或者左边没有值了退出
    >                     if (l < 0 || arr[l] != findValue) {
    >                         break;
    >                     }
    >                     value.add(l);
    >                     l = l - 1;
    >                 }
    >                 // 查找右边有无相同的值
    >                 int r = midIndex + 1;
    >                 while (true) {
    >                     // 右边没有相同的值，或者右边没有值了退出
    >                     if (r > arr.length - 1 || arr[r] != findValue) {
    >                         break;
    >                     }
    >                     value.add(r);
    >                     r = r + 1;
    >                 }
    >                 return value;
    >             }
    >     ```
    >     
### 3. 插值查找（InsertionSearch）
3. 插值查找（InsertionSearch）
    > 1. 介绍
    >
    >    实际是对二分查找的优化，也是对有序列表的查找算法。
    >
    >    基本思想：我们定义low为最左边的索引，high为最右边索引，key为我们要找的值。二分查找的mid是固定的，mid基于(high - low)的比例系数1/2：mid=low+**1/2**\*(high-low)；而插值查找的mid是自适应的，插值查找的mid基于(high - low)的比例系数**((key-arr[low])/(arr[high]-arr[low]))**：mid=low+**((key-arr[low])/(arr[high]-arr[low]))**\*(high-low)
    >
    > 2. 代码实现
    >
    >     ```
    >     /**
    >          * 插值查找
    >          * @param arr
    >          * @param left
    >          * @param right
    >          * @param findValue
    >          */
    >         public static List insert(int[] arr, int left, int right, int findValue) {
    >     
    >             //没有找到值，或者要查找的值不存在，返回空集合
    >             // 为了防止mid越界，必须要加findValue < arr[0] || findValue > arr[arr.length - 1]的判断
    >             if (left > right || findValue < arr[0] || findValue > arr[arr.length - 1]) {
    >                 return new ArrayList<Integer>();
    >             }
    >     
    >             int insert = left + ((findValue - arr[left]) / (arr[right] - arr[left])) * (right - left);
    >             int insertValue = arr[insert];
    >     
    >             if (findValue > insertValue) {
    >                 return insert(arr, insert + 1, right, findValue);
    >             } else if (findValue < insertValue) {
    >                 return insert(arr, left, insert - 1, findValue);
    >             } else {
    >                 // 找到了值，就创建一个集合存放值得坐标;
    >                 List<Integer> val = new ArrayList<>();
    >                 val.add(insert);
    >                 // 查找左边有无相同的值
    >                 int l = insert - 1;
    >                 while (true) {
    >                     // 左边没有相同的值，或者左边没有值了退出
    >                     if (l < 0 || arr[l] != findValue) {
    >                         break;
    >                     }
    >                     val.add(l);
    >                     l = l - 1;
    >                 }
    >                 // 查找右边有无相同的值
    >                 int r = insert + 1;
    >                 while (true) {
    >                     // 右边没有相同的值，或者右边没有值了退出
    >                     if (r > arr.length - 1 || arr[r] != findValue) {
    >                         break;
    >                     }
    >                     val.add(r);
    >                     r = r + 1;
    >                 }
    >                 return val;
    >             }
    >         }
    >     ```
    >     
    > 3. 插值查找对于分布均匀、数据量庞大的有序列表查找速度快于二分查找；但是对于分布不均匀的有序列表来说不一定比二分查找快
    > 4. 插值查找一定要注意：为了防止mid越界，在退出递归条件上，除了基础的left > right，必须要加findValue < arr[0] || findValue > arr[arr.lenght - 1]的判断
    > 
    >     
### 4. 斐波那契查找（又名黄金分割法查找）（FibonacciSearch）
4. 斐波那契查找（FibonacciSearch）
    > 1. 介绍
    >
    >    斐波那契查找和二分查找和插值查找相似，它不过是改变了mid的值，mid=low+F(k-1)-1，F为斐波那契数列。也是对有序列表的查找
    >    
    >    基本原理：已知斐波那契数列特性为前两个数之和等于这个数，即F(k)=F(k-1)+F(k-2)，可以得到F(k)-1=F(k-1)-1+F(k-2)-1+1，说明，只要数组索引长度为F(k)-1，则可以将数组分为F(k-1)-1，F(k-2)-1两部分，中间夹着一个mid，因此，mid=low+F(k-1)-1
    >    
    >    但是顺序表长度n不一定等于F(k)-1，因此需要适当的对数组进行扩容，直到数组长度n=F(k)-1，之后再去用斐波那契查找。  
    >    
    > 2. 代码实现
    >
    >     ```
    >     /**
    >          * 非递归方法完成斐波那契查找
    >          * @param arr
    >          * @param findValue
    >          * @return
    >          */
    >         public static int fiboSearch(int[] arr, int findValue) {
    >             int left = 0;
    >             int right = arr.length - 1;
    >             int k = 0; // 斐波那契数列的下标，从第一个开始
    >             int mid = 0;
    >             int[] fib = fibonacci();
    >     
    >             // 找到最接近数组长度的斐波那契数列的值
    >             while (right > fib[k] - 1) {
    >                 k++;
    >             }
    >             // 扩充数组使之长度等于k对应的斐波那契数列的值
    >             // 克隆原来的数组，长度等于斐波那契数列的值，不够的用0填充
    >             int[] temp = Arrays.copyOf(arr, fib[k]);
    >             // 用arr数组的最后元素填充新数组的扩容的元素
    >             for (int i = right + 1; i < temp.length; i++) {
    >                 temp[i] = arr[right];
    >             }
    >     
    >             // 对新数组使用斐波那契查找
    >             while (left <= right) {
    >                 mid = left + fib[k - 1] - 1;
    >                 // 如果查找的数小于mid对应的数
    >                 if (findValue < temp[mid]) {
    >                     // 向前查找
    >                     right = mid - 1;
    >                     // 将前一个部分当做新斐波那契数列的值
    >                     k = k - 1;
    >                 } else if (findValue > temp[mid]){
    >                     left = mid + 1;
    >                     // 将后一个部分当做新斐波那契数列的值
    >                     k = k - 2;
    >                 } else {
    >                     // 因为该数组是经过填充的，因此需要判断找到的值的下标
    >                     if (mid < right) {
    >                         return mid;
    >                     } else {
    >                         return right;
    >                     }
    >                 }
    >             }
    >             return -1;
    >         }
    >     
    >         /**
    >          * 获取斐波那契数列
    >          * @return
    >          */
    >         public static int[] fibonacci() {
    >             // 获取长度为20的斐波那契数列
    >             int k = 0;
    >             int[] fib = new int[20];
    >             fib[0] = 1;
    >             fib[1] = 1;
    >             for (int i = 2; i < 20; i++) {
    >                 fib[i] = fib[i - 1] + fib[i - 2];
    >             }
    >             return fib;
    >         }
    >     ```
    >     
## 9. 哈希表（HashTable）
9. 哈希表（HashTable）
   
    > 1. 介绍
    >
    >    哈希表，又叫散列表。是根据关键码值，直接进行访问的数据结构，意思就是，它通过关键码值和表中某个位置的一一映射，来达到加快查找速度的目的。映射函数称为散列函数，存放数据的数组称为散列表。
    >    
    >    哈希表有两种实现：数组+链表、数组+二叉树
    >    
    > 2. 利用哈希表存储雇员信息（哈希表：数组+链表）
    >
    >     1. 思路分析
    >     
    >         ```
    >         1> 定义一个哈希表的类，该类存放了一个链表数组、增删改查方法、映射函数
    >         2> 定义一个链表节点的类，该类存放员工的id、姓名等信息
    >         3> 定义一个链表类，该类存放链表头结点、链表的增删改查方法
    >         ```
    >        
    >     2. 代码实现
    >     
    >         ```
    >         /**
    >          * 哈希表
    >          */
    >         class HashTable {
    >         
    >             private Employee[] employeeArray;
    >         
    >             private int size;
    >         
    >             public HashTable(int size) {
    >                 // 初始化员工链表数组
    >                 this.size = size;
    >                 employeeArray = new Employee[size];
    >                 // 初始化数组中的每个链表
    >                 for (int i = 0; i < size; i++) {
    >                     employeeArray[i] = new Employee();
    >                 }
    >             }
    >         
    >             /**
    >              * 添加元素进哈希表
    >              * @param emp
    >              */
    >             public void add(EmployeeNode emp) {
    >                 // 获取要添加的员工的id
    >                 int empId = emp.id;
    >                 // 根据映射函数寻找要添加到哈希表中的位置
    >                 int index = hashFund(empId);
    >                 // 添加
    >                 employeeArray[index].add(emp);
    >             }
    >         
    >             /**
    >              * 遍历哈希表
    >              */
    >             public void show() {
    >                 for (int i = 0; i < size; i++) {
    >                     employeeArray[i].show(i);
    >                 }
    >             }
    >         
    >             /**
    >              * 根据id查找
    >              * @param id
    >              */
    >             public void select(int id) {
    >                 int index = hashFund(id);
    >                 System.out.printf("第%d链表：", index);
    >                 employeeArray[index].select(id);
    >             }
    >         
    >             /**
    >              * 根据id删除元素
    >              * @param id
    >              */
    >             public void delete(int id) {
    >                 int index = hashFund(id);
    >                 System.out.printf("第%d链表：", index);
    >                 employeeArray[index].delete(id);
    >             }
    >         
    >             /**
    >              * 散列函数
    >              * 通过简单的模运算进行映射
    >              * @param id
    >              * @return
    >              */
    >             public int hashFund(int id) {
    >         
    >                 return id % size;
    >         
    >             }
    >         }
    >         
    >         /**
    >          * 链表
    >          */
    >         class Employee {
    >         
    >             private EmployeeNode head = null;
    >         
    >             public EmployeeNode getHead() {
    >                 return head;
    >             }
    >         
    >             /**
    >              * 添加雇员
    >              * @param value
    >              */
    >             public void add(EmployeeNode value) {
    >                 // 如果是添加第一个雇员
    >                 if (head == null) {
    >                     head = value;
    >                 } else {
    >                     EmployeeNode temp = head;
    >         
    >                     while (true) {
    >                         if (temp.next == null) {
    >                             break;
    >                         }
    >                         temp = temp.next;
    >                     }
    >                     temp.next = value;
    >                 }
    >             }
    >         
    >             /**
    >              * 遍历链表
    >              */
    >             public void show(int no) {
    >                 // 如果head为空
    >                 if (head == null) {
    >                     System.out.printf("第%d链表为空~~\n", no);
    >                     return;
    >                 }
    >                 System.out.printf("第%d链表的信息为：", no);
    >                 EmployeeNode temp = head;
    >                 while (true) {
    >                     if (temp == null) {
    >                         break;
    >                     }
    >                     System.out.printf("id=%d\n", temp.id);
    >                     temp = temp.next;
    >                 }
    >             }
    >         
    >             /**
    >              * 根据id查找元素
    >              * @param id
    >              */
    >             public void select(int id) {
    >                 if (head == null || id < head.id) {
    >                     System.out.printf("第%d元素不存在", id);
    >                     return;
    >                 }
    >                 EmployeeNode temp = head;
    >                 while (true) {
    >                     if (temp == null) {
    >                         System.out.printf("第%d元素不存在", id);
    >                         return;
    >                     }
    >                     if (temp.id == id) {
    >                         break;
    >                     }
    >                     temp = temp.next;
    >                 }
    >                 System.out.printf("第%d元素的姓名为%s", id, temp.name);
    >             }
    >         
    >             /**
    >              * 根据id删除节点
    >              * @param id
    >              */
    >             public void delete(int id) {
    >                 if (head == null || id < head.id) {
    >                     System.out.printf("要删除的元素%d不存在", id);
    >                     return;
    >                 }
    >                 if (head.id == id) {
    >                     head = head.next;
    >                     System.out.printf("第%d元素已删除", id);
    >                 } else {
    >                     EmployeeNode temp = head;
    >                     while (true) {
    >                         if (temp.next == null) {
    >                             System.out.printf("要删除的元素%d不存在", id);
    >                             return;
    >                         }
    >                         if (temp.next.id == id) {
    >                             break;
    >                         }
    >                         temp = temp.next;
    >                     }
    >                     temp.next = temp.next.next;
    >                     System.out.printf("第%d元素已删除", id);
    >                 }
    >             }
    >         }
    >         
    >         /**
    >          * 员工链表节点类
    >          */
    >         class EmployeeNode {
    >             int id;
    >             String name;
    >         
    >             public EmployeeNode(int id, String name) {
    >                 super();
    >                 this.id = id;
    >                 this.name = name;
    >             }
    >         
    >             EmployeeNode next;
    >         }
    >         ```
    >     
    >     3. 注意：哈希表做初始化时，初始化链表数组过后，必须要初始化链表数组中的每一条链表
    >     
## 10. 树结构基础
10. 树结构基础
    > 1. 数组结构和链表结构各自的优缺点
    >
    >     1. 数组结构
    >     
    >         * 优点：通过下标查找元素，速度快。对于有序数组，可以使用二分查找提高检索效率。
    >         
    >         * 缺点：如果要检索具体的值，或者按照某个顺序插入一个值会整体移动，效率较低
    >             * ```<<```、```>>```和```>>>```
    >                上述三种都是java中的位运算符，```y << x```表示左移位，将y用二进制表示，然后向左移动x位，低位用0补齐；```y >> x```表示右移位，将y用二进制表示，然后向右移位x个，移出的部分抛弃；```y >>> x```表示无符号右移位，将y用二进制表示，然后向右移位x个，移出的部分抛弃，左边的部分全部用0补齐。没有无符号左移位```<<<```
    >         
    >     2. 链表结构
    >     
    >         * 优点：一定程度上跟数组相比存储数据做了优化，比如：插入数直接点只用将节点链接到链表，删除的效率也很高
    >         
    >         * 缺点：检索具体的值时，效率仍然较低（临时节点需要从后往前移动）
    >     
### 1. 二叉树（BinaryTree）
1. 二叉树（BinaryTree）
    > 1. 介绍
    >    
    >    每个节点最多有两个子节点的树称为二叉树。二叉树子节点分为左节点和右节点
    >    
    >    如果所有的叶子节点都在最后一层，并且节点总数=2^n-1，n为层数，则我们称之为满二叉树
    >    如果所有的叶子节点都在最后一层或者倒数第二层，而且最后一层的叶子节点在左连续，倒数第二层叶子节点在右连续，我们称之为完全二叉树   
    >    
    >    完全二叉树的一切子树都是完全二叉树
    >    
    >    满二叉树=完全二叉树是充分不必要的
    >    
    > 2. 二叉树的前序遍历、中序遍历、后序遍历 
    >    
    >    根据父节点的输出顺序来说，父节点、左树、右树的顺序就是前序；左树、父节点、右树就是中序；左树、右树、父节点就是后序
    >    
    > 3. 使用二叉树，做前序遍历、中序遍历、后序遍历
    >    
    >    * 二叉树及二叉树节点：二叉树和二叉树节点的关系相当于哈希表和哈希表中的链表关系一样，可以看成二叉树节点提供具体的底层实现，二叉树提供相应的接口用来调用
    >      
    >        ```
    >            class BinaryTree {
    >            
    >                private HeroBinaryTreeNode root;
    >            
    >                /**
    >                 * 添加根节点
    >                 * @param root
    >                 */
    >                public void setRoot(HeroBinaryTreeNode root) {
    >                    this.root = root;
    >                }
    >            
    >                /**
    >                 * 前序遍历
    >                 */
    >                public void preOrder() {
    >                    if (root != null) {
    >                        root.preOrder();
    >                    } else {
    >                        System.out.println("当前二叉树为空无法遍历~~");
    >                    }
    >                }
    >            
    >                /**
    >                 * 中序遍历
    >                 */
    >                public void infixOrder() {
    >                    if (root != null) {
    >                        root.infixOrder();
    >                    } else {
    >                        System.out.println("当前二叉树为空无法遍历~~");
    >                    }
    >                }
    >            
    >                /**
    >                 * 中序遍历
    >                 */
    >                public void postOrder() {
    >                    if (root != null) {
    >                        root.postOrder();
    >                    } else {
    >                        System.out.println("当前二叉树为空无法遍历~~");
    >                    }
    >                }
    >                
    >            }
    >            
    >            /**
    >             * 水浒英雄的二叉树节点
    >             */
    >            class HeroBinaryTreeNode {
    >                public int no;
    >                public String name;
    >                public HeroBinaryTreeNode left;
    >                public HeroBinaryTreeNode right;
    >            
    >                public HeroBinaryTreeNode(int no, String name) {
    >                    this.no = no;
    >                    this.name = name;
    >                }
    >            
    >                @Override
    >                public String toString() {
    >                    return "HeroBinaryTreeNode{" +
    >                            "no=" + no +
    >                            ", name='" + name + '\'' +
    >                            '}';
    >                }
    >            
    >                /**
    >                 * 前序遍历方法
    >                 */
    >                public void preOrder() {
    >                    // 输出父节点
    >                    System.out.printf("%d英雄是%s\n", this.no, this.name);
    >                    // 输出左边的树
    >                    if (this.left != null) {
    >                        this.left.preOrder();
    >                    }
    >                    // 输出右边的树
    >                    if (this.right != null) {
    >                        this.right.preOrder();
    >                    }
    >                }
    >            
    >                /**
    >                 * 中序遍历方法
    >                 */
    >                public void infixOrder() {
    >                    // 输出左边的树
    >                    if (this.left != null) {
    >                        this.left.infixOrder();
    >                    }
    >                    // 输出父节点
    >                    System.out.printf("%d英雄是%s\n", this.no, this.name);
    >                    // 输出右边的树
    >                    if (this.right != null) {
    >                        this.right.infixOrder();
    >                    }
    >                }
    >            
    >                /**
    >                 * 后序遍历方法
    >                 */
    >                public void postOrder() {
    >                    // 输出左边的树
    >                    if (this.left != null) {
    >                        this.left.postOrder();
    >                    }
    >                    // 输出右边的树
    >                    if (this.right != null) {
    >                        this.right.postOrder();
    >                    }
    >                    // 输出父节点
    >                    System.out.printf("%d英雄是%s\n", this.no, this.name);
    >                }
    >            }
    >        ```
    >        
    >    * 前序遍历
    >      
    >        ```
    >            /**
    >                 * 前序遍历
    >                 */
    >                public void preOrder() {
    >                    if (root != null) {
    >                        root.preOrder();
    >                    } else {
    >                        System.out.println("当前二叉树为空无法遍历~~");
    >                    }
    >                }
    >        ```
    >        
    >        ```
    >            /**
    >                 * 前序遍历方法
    >                 */
    >                public void preOrder() {
    >                    // 输出父节点
    >                    System.out.printf("%d英雄是%s\n", this.no, this.name);
    >                    // 输出左边的树
    >                    if (this.left != null) {
    >                        this.left.preOrder();
    >                    }
    >                    // 输出右边的树
    >                    if (this.right != null) {
    >                        this.right.preOrder();
    >                    }
    >                }
    >        ```
    >        
    >    * 中序遍历
    >      
    >        ```
    >            /**
    >                 * 中序遍历
    >                 */
    >                public void infixOrder() {
    >                    if (root != null) {
    >                        root.infixOrder();
    >                    } else {
    >                        System.out.println("当前二叉树为空无法遍历~~");
    >                    }
    >                }
    >        ```
    >        
    >        ```
    >            /**
    >                 * 中序遍历方法
    >                 */
    >                public void infixOrder() {
    >                    // 输出左边的树
    >                    if (this.left != null) {
    >                        this.left.infixOrder();
    >                    }
    >                    // 输出父节点
    >                    System.out.printf("%d英雄是%s\n", this.no, this.name);
    >                    // 输出右边的树
    >                    if (this.right != null) {
    >                        this.right.infixOrder();
    >                    }
    >                }
    >        ```
    >        
    >    * 后序遍历
    >      
    >        ```
    >            /**
    >             * 后序遍历
    >                 */
    >                public void postOrder() {
    >                    if (root != null) {
    >                        root.postOrder();
    >                    } else {
    >                        System.out.println("当前二叉树为空无法遍历~~");
    >                    }
    >                }
    >        ```
    >        
    >        ```
    >            /**
    >                 * 后序遍历方法
    >                 */
    >                public void postOrder() {
    >                    // 输出左边的树
    >                    if (this.left != null) {
    >                        this.left.postOrder();
    >                    }
    >                    // 输出右边的树
    >                    if (this.right != null) {
    >                        this.right.postOrder();
    >                    }
    >                    // 输出父节点
    >                    System.out.printf("%d英雄是%s\n", this.no, this.name);
    >                }
    >        ```
    >    
    > 4. 使用二叉树做前序查找、中序查找、后序查找
    >    
    >    * 前序查找
    >      
    >        ```
    >            /**
    >             * 前序查找
    >                 * @param id
    >                 * @return
    >                 */
    >                public HeroBinaryTreeNode preSearch(int id) {
    >                    if (root != null) {
    >                        return root.preSearch(id);
    >                    } else {
    >                        return null;
    >                    }
    >                }
    >        ```
    >        
    >        ```
    >            /**
    >                 * 前序查找方法
    >                 * @param id
    >                 * @return
    >                 */
    >                public HeroBinaryTreeNode preSearch(int id) {
    >                    // 先判断父节点是否为要查找的节点
    >                    if (this.no == id) {
    >                        return this;
    >                    }
    >                    HeroBinaryTreeNode res = null; // 最终的结果
    >                    // 判断左节点是否为空，不为空继续向下查找
    >                    if (this.left != null) {
    >                        res = this.left.preSearch(id);
    >                    }
    >                    // 找到了就返回res
    >                    if (res != null) {
    >                        return res;
    >                    }
    >                    // 判断右节点是否为空，不为空继续向下查找
    >                    if (this.right != null) {
    >                        res = this.right.preSearch(id);
    >                    }
    >                    // 找到了就返回res
    >                    // res为null，证明左右和自己都不是，直接返回res
    >                    return res;
    >                }
    >        ```
    >        
    >    * 中序查找
    >      
    >        ```
    >            /**
    >             * 中序查找
    >                 * @param id
    >                 * @return
    >                 */
    >                public HeroBinaryTreeNode infixSearch(int id) {
    >                    if (root == null) {
    >                        return null;
    >                    } else {
    >                        return root.infixSearch(id);
    >                    }
    >                }
    >        ```
    >        
    >        ```
    >            /**
    >                 * 中序查找方法
    >                 * @param id
    >                 * @return
    >                 */
    >                public HeroBinaryTreeNode infixSearch(int id) {
    >                    HeroBinaryTreeNode res = null;
    >                    //先判断左节点是否为空，不为空继续向下查找
    >                    if (this.left != null) {
    >                        res = this.left.infixSearch(id);
    >                    }
    >                    // 左边找到了，直接返回
    >                    if (res != null) {
    >                        return res;
    >                    }
    >                    // 判断父节点是否为要查找的节点
    >                    if (this.no == id) {
    >                        return this;
    >                    }
    >                    // 判断右节点是否为空，不为空继续向下查找
    >                    if (this.right != null) {
    >                        res = this.right.infixSearch(id);
    >                    }
    >                    // res为null，则该节点自己和左右的树都没有要找的，返回null即res
    >                    return res;
    >                }
    >        ```
    >        
    >    * 后序查找
    >      
    >        ```
    >            /**
    >             * 后序查找
    >                 * @param id
    >                 * @return
    >                 */
    >                public HeroBinaryTreeNode postSearch(int id) {
    >                    if (root == null) {
    >                        return null;
    >                    } else {
    >                        return root.postSearch(id);
    >                    }
    >                }
    >        ```
    >        
    >        ```
    >            /**
    >                 * 后序查找方法
    >                 * @param id
    >                 * @return
    >                 */
    >                public HeroBinaryTreeNode postSearch(int id) {
    >                    HeroBinaryTreeNode res = null;
    >                    //先判断左节点是否为空，不为空继续向下查找
    >                    if (this.left != null) {
    >                        res = this.left.infixSearch(id);
    >                    }
    >                    // 左边找到了，直接返回
    >                    if (res != null) {
    >                        return res;
    >                    }
    >                    // 判断右节点是否为空，不为空继续向下查找
    >                    if (this.right != null) {
    >                        res = this.right.infixSearch(id);
    >                    }
    >                    // 右边找到，直接返回
    >                    if (res != null) {
    >                        return res;
    >                    }
    >                    // 判断父节点是否为要查找的节点
    >                    if (this.no == id) {
    >                        return this;
    >                    }
    >                    // res为null，则该节点自己和左右的树都没有要找的，返回null即res
    >                    return null;
    >                }
    >        ```
    >        
    >     5. 二叉树删除节点
    >    
    >        1. 如果待删除节点是叶子节点，直接删除；如果待删除节点是非叶子节点，则删除这个子树
    >            * 代码实现
    >            
    >                ```
    >                /**
    >                     * 删除节点
    >                 * 如果为叶子节点删除叶子节点，为非叶子节点直接删除子树
    >                     * @param id
    >                     */
    >                    public void delNodeSimple(int id) {
    >                        if (root == null) {
    >                            System.out.println("二叉树为空~~");
    >                            return;
    >                        }
    >                        if (root.no == id) {
    >                            root = null;
    >                            System.out.printf("编号为%d的已经删除", id);
    >                            return;
    >                        }
    >                        root.delNodeSimple(id);
    >                    }
    >                ```
    >                
    >                ```
    >                	/**
    >                     * 删除节点
    >                     * 如果为叶子节点删除叶子节点，为非叶子节点直接删除子树
    >                     * @param id
    >                     */
    >                    public void delNodeSimple(int id) {
    >                        if (this.left != null && this.left.no == id) {
    >                            this.left = null;
    >                            System.out.printf("编号为%d的已删除", id);
    >                            return;
    >                
    >                        }
    >                        if (this.right != null && this.right.no == id) {
    >                            this.right = null;
    >                            System.out.printf("编号为%d的已删除", id);
    >                            return;
    >                        }
    >                
    >                        if (this.left != null) {
    >                            this.left.delNodeSimple(id);
    >                        }
    >                        if (this.right != null) {
    >                            this.right.delNodeSimple(id);
    >                        }
    >                    }
    >                ```
    >            
    >        2. 如果待删除节点是叶子节点，直接删除；如果待删除节点是非叶子节点，则删除这个节点，拼接剩下的
    >        
    >           1. 思路
    >           
    >              假设我们删除一个非叶子节点S时，如果S只有一个left或者right为null，直接提不为null的节点来取代他；
    >              
    >              如果left和right都不为null，则提该非叶子节点的左子节点S1来替代它的位置，设非叶子节点的右子节点为S2，思路如下：将左子节点S1的右子树S12暂时拆下，遍历左子节点的左子树S11，找到有一个节点的left或者right为null，拼接暂时拆下的右子树S12。将左子节点S1的right指向非叶子节点的右子节点S2。将原来指向删除非叶子结点S的指针指向左子节点S1。结束
#### 1. 顺序存储二叉树：一般只考虑完全二叉树
1. 顺序存储二叉树：一般只考虑完全二叉树
    >    
    >       1. 基本说明
    >       
    >          顺序存储二叉树指的是用顺序表存储二叉树。顺序表存储二叉树通常只考虑完全二叉树，从数据存储的结构来看，数组结构和树结构可以相互转换，即数组可以转换为树，树也可以转换为数组。
    >          
    >          顺序存储二叉树，从根节点开始，将根节点存入arr[0]，从上而下，从左到右，依次存入数组
    >          
    >       2. 顺序表存储二叉树特点
    >       
    >          1、顺序表存储二叉树通常只考虑完全二叉树，树的节点元素和数组的索引对应关系如下
    >          
    >          2、第arr[n]节点的左子节点为arr[2*n+1]
    >          
    >          3、第arr[n]节点的右子节点为arr[2*n+2]
    >          
    >          4、第arr[n]节点的父节点为arr[(n-1)/2]
    >          
    >           * n：表示二叉树的第n个节点。从根节点开始，根节点记为第0个，从上而下，从左往右。
    >          
    >       3. 存储了二叉树的顺序表的前序、中序、后序遍历
    >       
    >           1. 思路
    >           
    >              根据顺序存储二叉树的数组索引和二叉树的节点对应关系来编写逻辑
    >              
    >           2. 代码实例
    >           
    >               ```
    >               /**
    >                    * 前序遍历
    >                    * 对存储了二叉树的顺序表使用前序遍历
    >                    * @param index
    >                    */
    >                   public static void preShow(int index) {
    >                       if (arr == null || arr.length == 0) {
    >                           System.out.println("数组为空，无法遍历~");
    >                           return;
    >                       }
    >                       // 输出当前元素
    >                       System.out.println(arr[index]);
    >                       // 向左递归
    >                       if ((index * 2 + 1) < arr.length) {
    >                           preShow(2 * index + 1);
    >                       }
    >                       // 向右递归
    >                       if ((index * 2 + 2) < arr.length) {
    >                           preShow(2 * index + 2);
    >                       }
    >                   }
    >               ```
    >               
    >               ```
    >               /**
    >                    * 中序遍历
    >                    * 对存储了二叉树的顺序表使用前序遍历
    >                    * @param index
    >                    */
    >                   public static void infixShow(int index) {
    >                       if (arr == null || arr.length == 0) {
    >                           System.out.println("数组为空，无法遍历~");
    >                           return;
    >                       }
    >                       // 向左递归
    >                       if ((index * 2 + 1) < arr.length) {
    >                           infixShow(2 * index + 1);
    >                       }
    >                       // 输出当前元素
    >                       System.out.println(arr[index]);
    >                       // 向右递归
    >                       if ((index * 2 + 2) < arr.length) {
    >                           infixShow(2 * index + 2);
    >                       }
    >                   }
    >               ```
    >               
    >               ```
    >               /**
    >                    * 后序遍历
    >                    * 对存储了二叉树的顺序表使用前序遍历
    >                    * @param index
    >                    */
    >                   public static void postShow(int index) {
    >                       if (arr == null || arr.length == 0) {
    >                           System.out.println("数组为空，无法遍历~");
    >                           return;
    >                       }
    >                       // 向左递归
    >                       if ((index * 2 + 1) < arr.length) {
    >                           postShow(2 * index + 1);
    >                       }
    >                       // 向右递归
    >                       if ((index * 2 + 2) < arr.length) {
    >                           postShow(2 * index + 2);
    >                       }
    >                       // 输出当前元素
    >                       System.out.println(arr[index]);
    >                   }
    >               ```
    >    
#### 2. 线索化二叉树（ThreadedBinaryTree）
2. 线索化二叉树（ThreadedBinaryTree）
    > 1. 引言
    >
    >    通过对普通的完全二叉树的分析可知，最后一层的叶子节点和倒数第二层的叶子节点和某些节点，它们都存在一个或者两个指针没有使用的情况，资源被白白浪费。
    >    
    >    为解决资源浪费，引出了线索化二叉树。
    >    
    > 2. 介绍  
    >
    >    对于有n个节点的二叉树，它含有的n+1个空指针域存放指向前驱和后继节点的指针【这些指针被称为线索】。这种二叉树被称为加上了线索的二叉树即线索化二叉树（ThreadedBinaryTree）
    >    
    >    【空指针域计算公式：2n-(n-1)。每个节点拥有2个空指针域，除了根节点，其他节点的地址都要被储存，因此用掉n-1个，还剩2n-(n-1)个】
    >    
    >    根据线索性质不同分为前序线索二叉树、中序线索二叉树、后序线索二叉树
    >    
    >     * 一个节点的前一个节点，称为**前驱**
    >    
    >     * 一个节点的后一个节点，称为**后继**
    >    
    > 3. 线索化二叉树的特性
    >
    >    当二叉树变成线索化二叉树后，节点的left和right的指向会有所不同
    >    
    >    left指向可能是左子树，也可能是前驱节点
    >    
    >    rigth指向可能是右子树，也可能是后驱节点
    >    
    >    因此，需要添加两个新属性，leftType和rightType用来区分left和right指向的是什么。
    >    
    > 4. 前序、中序、后序线索化代码实现
    >
    >     ```
    >     /**
    >          * 中序线索化方法（一定没错）
    >          * @param node
    >          */
    >         public void infixThreadedNode(HeroThreadedBinaryTreeNode node) {
    >             if (node == null) {
    >                 return;
    >             }
    >             // 进行线索化
    >     
    >             // 先线索化左子树
    >             infixThreadedNode(node.left);
    >     
    >             // 线索化当前节点
    >             // 1、先处理前驱
    >             if (node.left == null) {
    >                 node.left = preNode;
    >                 node.leftType = 1;
    >             }
    >             // 2、处理后继
    >             // 当前节点的后继节点在本次关于当前节点的迭代中无法得知，因此我们需要等到后继节点的轮次
    >             // 那时当前节点就是后继节点的前置，对前置使用后继逻辑
    >             if (preNode != null && preNode.right == null) {
    >                 preNode.right = node;
    >                 preNode.rightType = 1;
    >             }
    >             // 每个节点的前置和后继处理完成后，当前节点成为下一个节点的前置
    >             preNode = node;
    >             // 线索化右子树
    >             infixThreadedNode(node.right);
    >         }
    >     ```
    >     
    >     ```
    >     /**
    >          * 前序线索化方法
    >          * @param node
    >          */
    >         public void preThreadedNode(HeroThreadedBinaryTreeNode node) {
    >             if (node == null) {
    >                 return;
    >             }
    >             // 进行线索化
    >     
    >             // 线索化当前节点
    >             HeroThreadedBinaryTreeNode left = node.left;
    >             HeroThreadedBinaryTreeNode right = node.right;
    >             // 1、先处理前驱
    >             if (node.left == null) {
    >                 node.left = preNode;
    >                 node.leftType = 1;
    >             }
    >             // 2、处理后继
    >             if (preNode != null && preNode.right == null) {
    >                 preNode.right = node;
    >                 preNode.rightType = 1;
    >             }
    >             // 每个节点的前置和后继处理完成后，当前节点成为下一个节点的前置
    >             preNode = node;
    >     
    >             // 线索化左子树
    >             preThreadedNode(left);
    >     
    >             // 线索化右子树
    >             preThreadedNode(right);
    >         }
    >     ```
    >     
    >     ```
    >     /**
    >          * 后序线索化节点
    >          * @param node
    >          */
    >         public void postThreadedNode(HeroThreadedBinaryTreeNode node) {
    >             if (node == null) {
    >                 return;
    >             }
    >             // 线索化
    >             HeroThreadedBinaryTreeNode left = node.left;
    >             HeroThreadedBinaryTreeNode right = node.right;
    >             // 先线索化左树
    >             postThreadedNode(left);
    >             // 线索化右树
    >             postThreadedNode(right);
    >             // 线索化当前节点
    >             if (node.left == null) {
    >                 node.left = preNode;
    >                 node.leftType = 1;
    >             }
    >             if (preNode != null && preNode.right == null) {
    >                 preNode.right = node;
    >                 preNode.rightType = 1;
    >             }
    >             preNode = node;
    >         }
    >     ```
    >     
    > 5. 线索化二叉树的遍历
    >
    >     1. 思路
    >     
    >        由于二叉树经过线索化后空指针域都被利用了，因此不能使用以前遍历普通二叉树的方法，否则会造成死循环而栈溢出。
    >        
    >        遍历线索化二叉树的次序应该和线索化的方式有关，注意使用画图法来理清思路
    >        
    >     2. 代码实现
    >     
    >     	  * 中序线索化二叉树图解参见D:\JavaZiLiao\DataStructurePhoto\DataStructure2.png
    >     	  
    >             ```
    >             	 /**
    >               * 中序线索化二叉树遍历（一定没错）
    >               * 
    >               */
    >              public void show(HeroThreadedBinaryTreeNode node) {
    >                  // 定义一个临时变量来做遍历
    >                  HeroThreadedBinaryTreeNode temp = node;
    >                  while (temp != null) {
    >                      // 找到线索化的节点
    >                      if (temp.leftType == 0) {
    >                          temp = temp.left;
    >                      }
    >                      // 打印该线索化的节点
    >                      System.out.printf("%d英雄是%s\n", temp.no, temp.name);
    >                      // 打印所有的后继节点
    >                      while (temp.rightType == 1) {
    >                          temp = temp.right;
    >                          System.out.printf("%d英雄是%s\n", temp.no, temp.name);
    >                      }
    >                      // 替换遍历的节点
    >                      temp = temp.right;
    >                  }
    >              }
    >             ```
    >             ```
    >             /**
    >                  * 前序线索化二叉树遍历（不一定正确）
    >                  *
    >                  */
    >                 public void showPre(HeroThreadedBinaryTreeNode node) {
    >                     // 定义一个临时变量来做遍历
    >                     HeroThreadedBinaryTreeNode temp = node;
    >                     while (temp.right != null) {
    >                         // 找到线索化的节点
    >                         if (temp.leftType == 0) {
    >                             System.out.printf("%d英雄是%s\n", temp.no, temp.name);
    >                             temp = temp.left;
    >                         }
    >                         // 打印该线索化的节点
    >                         System.out.printf("%d英雄是%s\n", temp.no, temp.name);
    >                         // 打印所有的后继节点
    >                         while (temp.rightType == 1) {
    >                             temp = temp.right;
    >                             System.out.printf("%d英雄是%s\n", temp.no, temp.name);
    >                         }
    >                         temp = temp.right;
    >                     }
    >                 }
    >             ```
    >             ```
    >             后序遍历线索化二叉树最为复杂，通用的二叉树数节点存储结构不能够满足后序线索化，因此我们扩展了节点的数据结构，增加了父节点的指针。后序的遍历顺序是：左右根，先找到最左子节点，沿着right后继指针处理，当right不是后继指针时，并且上一个处理节点是当前节点的右节点，则处理当前节点的右子树，遍历终止条件是：当前节点是root节点，并且上一个处理的节点是root的right节点。这里不做赘述
    >             ```
    >     
    >   6. 线索二叉树的意义
    >      
    >      通过使用线索二叉树，可以让我们将一个非线性的结构转化为一个线性的结构，可以让我们线性的遍历整个二叉树。
    >      
#### 3. 堆排序（HeapSort）
3. 堆排序（HeapSort）
    > 1. 介绍
    >
    >    堆排序是基于堆这种数据结构而设计的一种排序算法，本质上还是一种选择排序，它的最好、最坏时间复杂度都是O(nlogn)，它也是种不稳定排序。堆排序速度非常快
    >    
    > 2. 堆
    >
    >    1. 介绍
    >    
    >       堆是一种有以下性质的完全二叉树：每个节点的值都大于或者等于其左右孩子的值，称为大顶堆；每个节点的值都小于其左右孩子的值，称为小顶堆。
    >       
    >        * **注意：没有要求节点的左右孩子值的大小关系**
    >    
    > 3. 一般如果要升序排列使用大顶堆，要降序排列使用小顶堆
    >
    > 4. 堆排序基本思想
    >
    >    假设我们要升序排列：
    >    
    >    1>将一个待排序列整个调整成一个大顶堆；（利用的是树和数组的相互转化，数组本身就可以表示成一个树）
    >    
    >    2>此时整个序列最大的元素就是根节点的值，将根节点的值和队列末尾的值进行交换，末尾就成了最大的值；
    >    
    >    3>将剩下的n-1个元素继续调整成大顶堆，继续前面的步骤，直到形成一个有序序列。
    >    
    > 5. 将一个数组（完全二叉树）调整成一个堆步骤
    >
    >    以大顶堆为例：
    >    
    >    1> 叶子节点不用调整
    >    
    >    2>从最后一个非叶子节点x开始：假设最后一个非叶子节点是第x个元素，因为是完全二叉树，最后一个叶子节点即arr[arr.length-1]父节点一定是最后一个非叶子节点，通过公式可知x=arr.length/2-1
    >    
    >    3>从左往右，从上往下调整（类似于插入排序，将arr[i]原来的数据保存在一个临时变量，从左往右从上往下找它应该在的位置）
    >    
    >    4>找到倒数第二个非叶子节点x，调整，继续寻找倒数第n个
    >    
    >    5>直到找到根节点，调整
    >    
    >    6>根节点调整后，一定是最大的值
    >    
    > 5. 代码实现
    >
    >     * 堆排序
    >     
    >         ```
    >          public static void main(String[] args) {
    >                 int[] arr = new int[80000];
    >                 for (int i = 0; i < 80000; i++) {
    >                     arr[i] = (int)(Math.random() * 80000);
    >                 }
    >         
    >                 // 开始堆排序
    >                 int temp = 0;
    >                 // 从最后一个非叶子节点开始调整，将无序列表整个变成一个大顶堆
    >                 for (int i = arr.length / 2 - 1; i >= 0; i--) {
    >                     adjustHeap(arr, i, arr.length);
    >                 }
    >                 // 交换
    >                 for (int j = arr.length - 1; j > 0; j--) {
    >                     temp = arr[j];
    >                     arr[j] = arr[0];
    >                     arr[0] = temp;
    >                     // 因为堆顶变动了，因此需要调整这个大顶堆
    >                     adjustHeap(arr, 0, j);
    >                 }
    >                 
    >             }
    >         ```
    >         
    >     * 调整
    >     
    >         ```
    >         /**
    >              * 调整局部树为一个堆
    >              * @param arr
    >              * @param i
    >              * @param length
    >              */
    >             public static void adjustHeap(int[] arr, int i, int length) {
    >                 // 先将当前元素存起来
    >                 int temp = arr[i];
    >                 // 调整
    >                 // 遵循从左向右，k为要与arr[i]比较的节点的索引
    >                 for (int k = i * 2 + 1; k < length; k = k * 2 + 1) {
    >                     // 如果有右子节点，并且右子节点值大于左子节点，让k指向右子节点
    >                     if (k + 1 < length && arr[k + 1] > arr[k]) {
    >                         k = k + 1;
    >                     }
    >                     // 比较arr[k]和arr[i]的大小
    >                     if (arr[k] > arr[i]) {
    >                         // 将子节点的值赋给本节点
    >                         arr[i] = arr[k];
    >                         // 将k节点作为本节点继续循环
    >                         i = k;
    >                     } else {
    >                         // 因为是从下往上遍历的非叶子节点，因此下面的一定排好了序，因此可以直接退出循环
    >                         break;
    >                     }
    >                     // 将原来的arr[i]即temp放到现在调整后的位置
    >                     arr[i] = temp;
    >                 }
    >             }
    >         ```
    >
#### 4. 哈夫曼树（HaffmanTree）
4. 哈夫曼树（HaffmanTree）
    > 1. 介绍
    >
    >    我们对有n个权值的n个节点构造一颗二叉树，如果使得该树的带权路径长度(wpl)达到最小，则称这样的二叉树为最优二叉树，也称作哈夫曼树。
    >    
    >    哈夫曼树是带权路径长度最短的树，权值较大的离根节点越近
    >    
    >    哈夫曼树是自下而上创建的二叉树，它不一定是个完全二叉树。
    >    
    >     * 路径：在一颗树中，从一个节点往下，可以达到的孩子或者孙子节点的通路称为路径。
    >    
    >     * 路径长度：通路中分支的数目。我们如果规定根节点的层数为1，L节点的层数为L，易得根节点到L节点的路径长度为L-1（因为是对于二叉树举例的）
    >    
    >     * 节点的权：我们给节点附一个带某种含义的数值，则该数值称为节点的权
    >    
    >     * 带权路径长度（wpl）：从根节点到该节点路径长度与该节点的权的乘积。
    >    
    >     * 树的带权路径长度：树的所有叶子节点的带权路径长度之和
    >    
    > 2. 哈夫曼树创建分析
    >
    >     1. 假设有一个数列{2, 3, 4, 5, 2, 8, 3, 5}，将其转换为哈夫曼树
    >     
    >         * 思路分析：
    >         
    >            1.将数列按照从小到大排列，将数列每个元素看成一个二叉树
    >            
    >            2.取根节点权最小的两棵树，组成新的二叉树，新二叉树权值为前面两个二叉树权值的和
    >            
    >            3.将这个新的二叉树根节点和数列中剩余的进行再次排序
    >            
    >            4.重复1~3直到形成哈夫曼树
    >            
    >         * 代码实现
    >         
    >             ```
    >             /**
    >                  * 创建哈夫曼树的方法
    >                  * @param arr
    >                  */
    >                 public static void createHaffmanTree(int[] arr) {
    >                     // 1. 将哈夫曼树节点对象进行基于权重的排序
    >                     // 遍历arr，将每个值作为哈夫曼树节点对象的权重，将节点对象存在集合中以便于之后的排序等
    >                     List<HaffmanTreeNode> haffman = new ArrayList<>();
    >                     for (int i :
    >                             arr) {
    >                         haffman.add(new HaffmanTreeNode(i));
    >                     }
    >                     // 排序所有的节点对象
    >                     Collections.sort(haffman);
    >                     // 2. 创建哈夫曼树
    >                     while (true) {
    >                         // 持续取出权重最小的两个树，组成新树，将新树的根节点添加进集合进行排序
    >                         HaffmanTreeNode first = haffman.get(0);
    >                         HaffmanTreeNode second = haffman.get(1);
    >                         HaffmanTreeNode temp = new HaffmanTreeNode(first.value + second.value);
    >                         temp.left = first;
    >                         temp.right = second;
    >             
    >                         haffman.remove(first);
    >                         haffman.remove(second);
    >                         haffman.add(temp);
    >                         Collections.sort(haffman);
    >             
    >                         // 如果集合中只有一个根节点，意味着哈夫曼树创建完毕
    >                         if (haffman.size() <= 1) {
    >                             break;
    >                         }
    >                     }
    >             
    >                     // 前序遍历哈夫曼树
    >                     preOrder(haffman.get(0));
    >                 }
    >             ```
    >             
    >             ```
    >             /**
    >              * 哈夫曼树节点类
    >              * 为了能够让节点类可以进行排序，需要先让类实现Compareable接口
    >              */
    >             class HaffmanTreeNode implements Comparable<HaffmanTreeNode>{
    >             
    >                 int value; // 权重
    >                 HaffmanTreeNode left;
    >                 HaffmanTreeNode right;
    >             
    >                 public HaffmanTreeNode(int value) {
    >                     this.value = value;
    >                 }
    >             
    >                 @Override
    >                 public String toString() {
    >                     return "HaffmanTreeNode{" +
    >                             "value=" + value +
    >                             '}';
    >                 }
    >             
    >                 /**
    >                  * 节点之间比较的方法
    >                  * @param o
    >                  * @return
    >                  */
    >                 @Override
    >                 public int compareTo(HaffmanTreeNode o) {
    >                     // 从小往大排序
    >                     return this.value - o.value;
    >                 }
    >             }
    >             ```
    > 
##### 1. 哈夫曼编码  
1. 哈夫曼编码 
    > 1. 介绍
    >    
    >    基于哈夫曼树的一种编码方式，属于一种程序算法。
    >    
    >    哈夫曼码是可变字长编码（VLC）的一种。
    >    
    >     * 可变字长编码（VLC）：对于编码来说，我们第一要保证编码为前缀编码，但是如果使用定长编码，则会造成文件过大，难于传输的问题，这就引出了可变字长编码。可变字长编码是保证了前缀编码的同时，对于出现频率越高的字符，让其编码的码长越小，从而缩小了长度易于传输。
    >    
    >     * 前缀编码：字符编码要求必须为前缀编码，前缀编码意思为任意字符的编码不可以为其他字符的前缀
    >    
    > 2. 哈夫曼编码原理剖析
    >
    >     1. 假设有一段字符串"i like like like you you "，对该字符使用赫夫曼编码
    >     
    >         * 步骤
    >         
    >            1.统计该字符串中所有字符的出现频率，记为该字符的权重
    >            
    >            2.对所有字符按照权重生成赫夫曼树
    >            
    >            3.规定遍历时向左为0，向右为1，这样就可以定义字符串中所有字符的编码，且为前缀编码
    >            
    >            4.按照该字符串的哈夫曼编码来编码字符串
    >         
    >     2. 哈夫曼编码为前缀编码的原因
    >     
    >        因为字符串中所有字符都按照权重成为了哈夫曼树中的叶子节点，每个叶子节点不是任何其他节点的父节点，因此他们不可能为任何节点的前置，因此哈夫曼编码就是前缀编码
    >        
    >     3. 根据排序方法不同（如对于多个权值相同的节点如何进行排序），生成的哈夫曼树可能会不相同，对应的哈夫曼编码会不同，但是wpl都是一样的，是最小的，压缩的长度也是一样的。但是为了码子的方差较小，建议合并的值在排序时，放在相同的值之后
    >     
    >     4. 哈夫曼编码压缩和解压代码实例：以"i like like like java do you like a java"为例子
    >     
    >         1. 压缩步骤
    >         
    >             1. 统计将字符串的字符按照出现次数为权重，转换成哈夫曼树
    >             
    >                 ```
    >                 /**
    >                  * 哈夫曼树节点类
    >                  * 为了能够让节点类可以进行排序，需要先让类实现Compareable接口
    >                  */
    >                 class HuffmanTreeNode implements Comparable<HuffmanTreeNode>{
    >                 
    >                     byte data; // 存放字符
    >                     int value; // 权重
    >                     HuffmanTreeNode left;
    >                     HuffmanTreeNode right;
    >                 
    >                     public HuffmanTreeNode(byte data, int value) {
    >                         this.data = data;
    >                         this.value = value;
    >                     }
    >                 
    >                     @Override
    >                     public String toString() {
    >                         return "HuffmanTreeNode{" +
    >                                 "value=" + value +
    >                                 '}';
    >                     }
    >                 
    >                     /**
    >                      * 节点之间比较的方法
    >                      * @param o
    >                      * @return
    >                      */
    >                     @Override
    >                     public int compareTo(HuffmanTreeNode o) {
    >                         // 从小往大排序
    >                         return this.value - o.value;
    >                     }
    >                 }
    >                 ```
    >                 
    >                 ```
    >                 // 1.将字符串的字符根据出现次数，转换为哈夫曼树
    >                         byte[] targetBytes = target.getBytes();
    >                         // 获取哈夫曼树的叶子节点集合
    >                         List<HuffmanTreeNode> nodeList = getNodeList(targetBytes);
    >                         // 创建哈夫曼树
    >                         HuffmanTreeNode huffmanRoot = createHuffman(nodeList);
    >                 ```
    >                 
    >             2. 获得每个字符的编码集
    >             
    >                 ```
    >                 /**
    >                      * 为每个字符指定哈夫曼编码
    >                      * @param root
    >                      * @return
    >                      */
    >                 
    >                     private static StringBuilder stringBuilder = new StringBuilder();
    >                     private static Map<Byte, String> huffmanCode = new HashMap<>();
    >                 
    >                     public static void huffmanEncode(HuffmanTreeNode root) {
    >                         if (root == null) {
    >                             System.out.println("没有哈夫曼树~");
    >                             return;
    >                         }
    >                         // 根节点左树
    >                         StringBuilder stringBuilder1 = new StringBuilder();
    >                         huffmanEncode(root.left, "0", stringBuilder1);
    >                         // 根节点右树
    >                         StringBuilder stringBuilder2 = new StringBuilder();
    >                         huffmanEncode(root.right, "1", stringBuilder2);
    >                     }
    >                 
    >                     public static void huffmanEncode(HuffmanTreeNode node, String code, StringBuilder stringBuilder) {
    >                         // 定义一个字符串缓冲来存放编码
    >                         StringBuilder stringBuilder1 = new StringBuilder(stringBuilder);
    >                         // 将code加入字符串缓冲区
    >                         stringBuilder1.append(code);
    >                         if (node != null) {
    >                             if (node.left != null) {
    >                                 // 如果是非叶子节点
    >                                 // 先递归向左
    >                                 huffmanEncode(node.left, "0", stringBuilder1);
    >                                 // 递归向右
    >                                 huffmanEncode(node.right, "1", stringBuilder1);
    >                 
    >                             } else {
    >                                 // 找到某个叶子节点
    >                                 // 将叶子结点的字符和字符的编码放入map集合
    >                                 huffmanCode.put(node.data, stringBuilder1.toString());
    >                             }
    >                         }
    >                     }
    >                 ```
    >                 
    >                 ```
    >                 // 2. 获得每个字符的编码集
    >                         huffmanEncode(huffmanRoot);
    >                 ```
    >                 
    >             3. 对字符串进行编码压缩
    >             
    >                 ```
    >                 /**
    >                      * 为字符串的byte[]数组进行哈夫曼编码，生成压缩过的byte[]
    >                      * 将原始字符串按照编码规则生成编码后的字符串，然后将编码后的字符串每8位一个比特，转化为byte[]数组
    >                      * @param target
    >                      * @param huffmanCode
    >                      * @return
    >                      */
    >                     public static byte[] zip(byte[] target, Map<Byte, String> huffmanCode) {
    >                         // 遍历字符串的byte[]，生成编码后的字符串
    >                         StringBuilder strCode = new StringBuilder();
    >                         for (byte i :
    >                                 target) {
    >                             strCode.append(huffmanCode.get(i));
    >                         }
    >                         String huffmanStr = strCode.toString();
    >                 
    >                         // 将编码后的字符串转化为byte[]数组
    >                         // 统计编码后的字符串按照8位一个byte需要的byte[]数组长度
    >                         int len = 0;
    >                         if (huffmanStr.length() % 8 == 0) {
    >                             len = huffmanStr.length() / 8;
    >                         } else {
    >                             len = huffmanStr.length() / 8 + 1;
    >                         }
    >                         // 创建存储压缩后的byte[]数组
    >                         byte[] huffmanBytes = new byte[len];
    >                         int index = 0;
    >                         for (int i = 0; i < huffmanStr.length(); i = i + 8) {
    >                             String strByte;
    >                             if (i + 8 < huffmanStr.length()) {
    >                                 // 剩下的够8位，截取8位
    >                                 strByte = huffmanStr.substring(i, i + 8);
    >                             } else {
    >                                 // 剩下的不够8位，从i截取到结尾
    >                                 strByte = huffmanStr.substring(i);
    >                             }
    >                             // 将strByte转成一个byte放入huffmanBytes
    >                             huffmanBytes[index] = (byte) Integer.parseInt(strByte, 2);
    >                             index++;
    >                         }
    >                         return huffmanBytes;
    >                     }
    >                 ```
    >                 
    >                 ```
    >                 byte[] zip = zip(targetBytes, huffmanCode);
    >                 ```
    >             
    >         2. 解压步骤
    >         
    >             1. 将赫夫曼编码过的字节数组zip转换成对应的二进制字符串
    >             
    >                 ```
    >                 	x/**     * bye转换成对应的二进制字符串     * @param flag 判断该字节是否为压缩后的最后一个字节,true表示是，false表示不是     * @param b     * @return     */    public static String byteToBitString(boolean flag, byte b) {        // Integer有一个将int类型的数转换成binaryString的方法，因此，先将byte转成int，然后使用Integer的方法        // Integer.toBinaryString() -- 返回int值对应的二进制的补码        // 因为一个int对应4个字节，因此：如果传的b是负数，转换成int后对应的补码需要截取后面八位才是真正需要的；如果是正数，需要补位        int temp = b;        String s = null;        if (!flag) {            temp |= 256; // 让传进来的数和256按位或        }        s = Integer.toBinaryString(temp);        if (!flag) {            return s.substring(s.length() - 8);        } else {            return s;        }    }
    >                 ```
    >                 
    >                 ```
    >                 /**
    >                      * 压缩数据的解码
    >                      * 压缩过后的字节数组解码成原来字符串对应的字节数组
    >                      * @param huffmanCode
    >                      * @param zip
    >                      * @return
    >                      */
    >                     public static byte[] decode(Map<Byte, String> huffmanCode, byte[] zip) {
    >                         // 1.先得到zip的字节对应的二进制字符串
    >                         StringBuilder stringBuilder = new StringBuilder();
    >                         for (int i = 0; i < zip.length; i++) {
    >                             // 如果不是最后一个字节，需要补位到8位，是最后一个字节，只需要原样获得其对应的二进制字符串
    >                             if (i < zip.length - 1) {
    >                                 stringBuilder.append(byteToBitString(false, zip[i]));
    >                             } else {
    >                                 stringBuilder.append(byteToBitString(true, zip[i]));
    >                             }
    >                 
    >                         }
    >                 
    >                         // 2.开始解码
    >                         // 将赫夫曼编码表进行键值调换，形成一个解码表
    >                         Map<String, Byte> huffmanDecode = new HashMap<>();
    >                         Set<Map.Entry<Byte, String>> entries = huffmanCode.entrySet();
    >                         for (Map.Entry<Byte, String> entry :
    >                                 entries) {
    >                             huffmanDecode.put(entry.getValue(), entry.getKey());
    >                         }
    >                 
    >                         // 用于存放byte的集合
    >                         List<Byte> targetList = new ArrayList<>();
    >                         // 扫描stringBuilder
    >                         int index = 0;
    >                         while (index < stringBuilder.length()) {
    >                             int count = 1;
    >                             boolean flag = true;
    >                             Byte b = null;
    >                 
    >                             while (true) {
    >                                 String key = stringBuilder.substring(index, index + count);
    >                                 b = huffmanDecode.get(key);
    >                 
    >                                 if (b == null) {
    >                                     count++;
    >                                 } else {
    >                                     break;
    >                                 }
    >                             }
    >                             targetList.add(b);
    >                             index = index + count;
    >                         }
    >                         // 组装
    >                         byte[] targetBytes = new byte[targetList.size()];
    >                         for (int i = 0; i < targetList.size(); i++) {
    >                             targetBytes[i] = targetList.get(i);
    >                         }
    >                 
    >                         return targetBytes;
    >                     }
    >                 ```
    >                 
    >             2. 将二进制字符串对照赫夫曼编码进行解码
    >             
    >                 ```
    >                 // 4.解压缩
    >                         byte[] decode = decode(huffmanCode, zip);
    >                 ```
    >             
    >         5. 文件压缩：和上述字符串压缩大致相同，只是还会涉及到I/O操作，不在这里列粗，参见D:\DataStructure_Java\BinaryTree\src\HuffmanTreeUser.java
    >     
    > 3. 哈夫曼编码的注意事项
    > 
    >     1. 如果文件已经进行过哈夫曼编码压缩了，则二次压缩效果不会很明显了。
    >     
    >     2. 哈夫曼编码是按照字节来进行处理，因此可以处理任何的文件
    >     
    >     3. 如果一个文件中，重复数据量很少，则压缩效果也不会很明显 
    >     
    >     4. **将原始字符串按照编码规则生成编码后的字符串，然后将编码后的字符串每8位一个比特，转化为byte[]数组。这时需要对最后的byte的长度进行储存**，我的代码还没改，改了后请删除这一句话。
    >     
#### 5. 二叉排序树（BST，BinarySortingTree）
5. 二叉排序树（BST，BinarySortingTree）  
    > 1. 简介
    >
    >    一个树，保证其父节点的值大于左子节点，右子节点的值永远大于等于父节点，则称这个树为二叉排序树（BST）
    >    
    > 2. 二叉排序树的创建、遍历（只以前序为例子）、删除
    >
    > 	  1. 创建和遍历
    > 	  
    >         * 代码实现
    >         
    >             ```
    >         public BinarySortTreeNode root;
    >             ```
    >             
    >             ```
    >             /**
    >                  * 创建二叉排序树
    >                  * @param node
    >                  */
    >                 public void createBinarySortTree(BinarySortTreeNode node) {
    >                    if (root == null) {
    >                        root = node;
    >                        return;
    >                    }
    >                    root.createBinarySortTree(node);
    >                }
    >            ```
    >            
    >            ```
    >            /**
    >                 * 创建二叉排序树
    >                 * @param node
    >                 */
    >                public void createBinarySortTree(BinarySortTreeNode node) {
    >                    if (node.value < this.value) {
    >                        if (this.left != null) {
    >                            this.left.createBinarySortTree(node);
    >                        } else {
    >                            this.left = node;
    >                        }
    >                    } else {
    >                        if (this.right != null) {
    >                            this.right.createBinarySortTree(node);
    >                        } else {
    >                            this.right = node;
    >                        }
    >                    }
    >                }
    >            ```
    >            
    >            ```
    >            /**
    >                 * 前序遍历
    >                 */
    >                public void preOrder() {
    >                   if (root == null) {
    >                      System.out.println("二叉排序树为空，无法遍历~~");
    >                      return;
    >                  }
    >                  root.preOrder();
    >              }
    >            ```
    >         
    >           ```
    >          /**
    >               * 前序遍历
    >               */
    >              public void preOrder() {
    >                  System.out.println(this.value);
    >                  if (this.left != null) {
    >                      this.left.preOrder();
    >                  }
    >                  if (this.right != null) {
    >                      this.right.preOrder();
    >                  }
    >              }
    >           ```
    >         
    >     2. 删除
    >     
    >         1. 思路分析
    >         
    >            对于删除节点来说，有三种情况：根节点、叶子节点、不包括根节点的非叶子节点。叶子节点最好处理，根节点次之。其他非叶子节点有两种情况：只要一个子节点、有两个子节点
    >            
    >            对于只有一个子节点的来说很好处理；对于有两个子节点的来说，处理思路为：既然是二叉排序树，则待删除节点n的右子节点n+1一定要保证比替换删除节点的节点m值大，因此持续向左寻找n+1的左子节点直到找到一个叶子节点k，该叶子节点k值一定比n+1的值小，将该节点k从二叉排序树中剥离并且保存，然后替换节点n即可
    >            
    >         2. 代码实现
    >         
    >             ```
    >             /**
    >                  * 根据节点的值删除节点
    >                  * @param value
    >                  */
    >                 public void delete(int value) {
    >                     if (root == null) {
    >                         System.out.println("二叉树为空，无法删除~");
    >                         return;
    >                     }
    >                     if (value == root.value) {
    >                         root = null;
    >                         System.out.println("删除成功~");
    >                         return;
    >                     }
    >                     root.delete(value);
    >                 }
    >             ```
    >     
    >             ```
    >             /**
    >                  * 根据值删除二叉树的节点
    >                  * @param value
    >                  */
    >                 public void delete(int value) {
    >                     if (this.left != null) {
    >                         if (this.left.value == value) {
    >                             // 左子节点为待删除节点
    >                             System.out.println("删除成功~");
    >                             if (this.left.left == null && this.left.right == null) {
    >                                 // 待删除节点是叶子结点
    >                                 this.left = null;
    >                             } else if (this.left.right == null) {
    >                                 // 待删除节点有左子节点
    >                                 this.left = this.left.left;
    >                             } else if (this.left.left == null) {
    >                                 // 待删除节点有右子节点
    >                                 this.left  = this.left.right;
    >                             } else {
    >                                 // 待删除节点有左右子节点
    >                                 // 定义一个临时节点来遍历
    >                                 BinarySortTreeNode temp = this.left.right;
    >                                 // 定义一个临时节点，用来存储代换节点
    >                                 BinarySortTreeNode tempNode = null;
    >                                 if (temp.left == null) {
    >                                     // 如果待删除节点的右子节点是没有左子节点
    >                                     tempNode = temp;
    >                                     // 替换节点
    >                                     tempNode.left = this.left.left;
    >                                 } else {
    >                                     // 如果待删除节点的右子节点不是叶子节点
    >                                     while (true) {
    >                                         if (temp.left.left == null) {
    >                                             break;
    >                                         }
    >                                         temp = temp.left;
    >                                     }
    >                                     // 将代换节点从树上取下
    >                                     tempNode = temp.left;
    >                                     temp.left = null;
    >                                     // 替换节点
    >                                     tempNode.left = this.left.left;
    >                                     tempNode.right = this.left.right;
    >                                 }
    >                                 this.left = tempNode;
    >                             }
    >                         } else {
    >                             this.left.delete(value);
    >                         }
    >                     }
    >                     if (this.right != null) {
    >                         if (this.right.value == value) {
    >                             // 右子节点为待删除节点
    >                             System.out.println("删除成功~");
    >                             if (this.right.left == null && this.right.right == null) {
    >                                 // 待删除节点是叶子结点
    >                                 this.right = null;
    >                             } else if (this.right.right == null) {
    >                                 // 待删除节点有左子节点
    >                                 this.right = this.right.left;
    >                             } else if (this.right.left == null) {
    >                                 // 待删除节点有右子节点
    >                                 this.right  = this.right.right;
    >                             } else {
    >                                 // 待删除节点有左右子节点
    >                                 // 定义一个临时节点来遍历
    >                                 BinarySortTreeNode temp = this.right.right;
    >                                 // 定义一个临时节点，用来存储代换节点
    >                                 BinarySortTreeNode tempNode = null;
    >                                 if (temp.left == null) {
    >                                     // 如果待删除节点的右子节点是没有左子节点
    >                                     tempNode = temp;
    >                                     // 替换节点
    >                                     tempNode.left = this.left.left;
    >                                 } else {
    >                                     // 如果待删除节点的右子节点不是叶子节点
    >                                     while (true) {
    >                                         if (temp.left.left == null) {
    >                                             break;
    >                                         }
    >                                         temp = temp.left;
    >                                     }
    >                                     // 将代换节点从树上取下
    >                                     tempNode = temp.left;
    >                                     temp.left = null;
    >                                     // 替换节点
    >                                     tempNode.left = this.right.left;
    >                                     tempNode.right = this.right.right;
    >                                 }
    >                                 this.right = tempNode;
    >                             }
    >                         } else {
    >                             this.right.delete(value);
    >                         }
    >                     }
    >                 }
    >             ```
    >     
    > 3. 注意事项
    > 
    >     1. 我的上述删除代码不够优美，不够清楚明了，可以继续优化，这里就不指出了
    >     
    >     2. 在做删除中，删除非根节点的非叶子节点中，对于左右子节点都有的待删除节点n，需要注意待删除节点的右子节点n+1可能没有左子节点，需要进行判断，如果没有左子节点，则将n+1取代待删除节点n；否则，继续向左寻找取代的节点k
    >     
#### 6. 平衡二叉树（BalancedBinaryTree，又叫AVL树）
6. 平衡二叉树（BalancedBinaryTree，又叫AVL树）
    > 1. 出现原因
    >
    >    如果将一个数列{1, 2, 3, 4, 5, 6}转换成一个二叉排序树，则生成的二叉排序树结构上左子树全为空，形式上更像是一个单链表。插入速度无影响，但是查询速度很慢，甚至因为每次还要比较左子树，要低于普通的链表，无法发挥出二叉排序树的优势。
    >    
    >    平衡二叉树就是为了解决二叉排序树的不足而设计的。
    >    
    > 2. 基本介绍
    >
    > 	 平衡二叉树是在二叉排序树的基础上升级版本。
    > 	
    >    平衡二叉树又叫平衡二叉搜索树，又叫AVL树，可以保证查询效率较高。
    >    
    >    它的特点：平衡二叉树是一个空树或者左右子树的高度差绝对值不超过1，并且左右子树也为平衡二叉树。
    >    
    >    平衡二叉树的实现方法：红黑树、AVL、替罪羊树、Treap（Treap=Tree+Heap）、伸展树等
    >    
    > 3. 左旋转
    >
    >     1. 案例
    >     
    >        对于一个数列{4, 3, 6, 5, 7}，将它转换成一个二叉排序树，转成的二叉排序树就是一个平衡二叉树。
    >        
    >        假设需要对这个平衡二叉树再添加一个8，此时该二叉排序树rightTreeHeight()-leftTreeHeight()>1，,不是一个AVL树了，需要进行一次左旋转。图解在D:\JavaZiLiao\DataStructruePhoto\DataStructure3.png
    >        
    >        可见，左旋转的原因是右子树高度-左子树高度>1
    >        
    >     2. 思路
    >     
    >        左旋转实质为：左边新添加一层、右边减少一层
    >        
    >        左旋转步骤为：新建、设置、回收
    >        
    >        创建一个新节点k，该节点保存要左旋转的节点n的值，将n的左子树m地址交给k，将n的右子树n+1的左子树l的地址交给k，让n节点指向k；将n的右节点n+1的值交给n，n指向n+1的右节点r。因为Java中一个对象如果没有任何东西指向它，则它会被JVM的垃圾回收机制回收，因此，节点n+1会被直接回收。思路图解如D:\JavaZiLiao\DataStructurePhoto\DataStructure4.png
    >        
    >     3. 代码实现
    >     
    >         ```
    >         /**
    >              * 左旋转当前节点
    >              */
    >             public void leftRotate() {
    >         
    >                 BalancedBinaryTreeNode temp = new BalancedBinaryTreeNode(this.value);
    >                 temp.left = this.left;
    >                 temp.right = this.right.left;
    >                 this.left = temp;
    >         
    >                 this.value = this.right.value;
    >                 this.right = this.right.right;
    >             }
    >         ```
    >     4. 注意事项
    >     
    >        因为旋转的时间为右边高度-左边高度大于1，左边高度最小为0，右边最小为2，因此不用考虑空指针异常的问题。
    >     
    > 4. 计算左子树高度、右子树高度、以当前节点为根节点的树高度
    >
    >     1. 思路分析
    >     
    >        
    >        
    >     2. 代码实现
    >     
    >         ```
    >         /**
    >              * 返回以当前节点为根节点的树的高度
    >              * @return
    >              */
    >             public int getNodeHeight() {
    >                 return Math.max(this.left == null ? 1 : this.left.getNodeHeight(), this.right == null ? 1 : this.right.getNodeHeight()) + 1;
    >             }
    >         ```
    >         
    >         ```
    >         /**
    >              * 返回当前节点的左子树高度
    >              * @return
    >              */
    >             public int getLeftTreeHeight() {
    >                 if (this.left == null) {
    >                     return 0;
    >                 }
    >                 return this.left.getNodeHeight();
    >             }
    >         ```
    >         
    >         ```
    >         /**
    >              * 返回当前节点的右子树高度
    >              * @return
    >              */
    >             public int getRightTreeHeight() {
    >                 if (this.right == null) {
    >                     return 0;
    >                 }
    >                 return this.right.getNodeHeight();
    >             }
    >         ```
    >     
    > 5. 右旋转
    >
    >    1. 思路
    >    
    >       右旋转就是左旋转思想的反转。
    >       
    >       右旋转实质为右边减少一层、左边增加一层
    >       
    >    2. 代码实现
    >    
    >        ```
    >        /**
    >             * 右旋转当前节点
    >             */
    >            public void rightRotate() {
    >        
    >                BalancedBinaryTreeNode temp = new BalancedBinaryTreeNode(this.value);
    >                temp.right = this.right;
    >                temp.left = this.left.right;
    >                this.right = temp;
    >        
    >                this.value = this.left.value;
    >                this.left = this.left.left;
    >            }
    >        ```
    >        
    >    4. 注意事项
    >    
    >       因为旋转的时间为左边高度-右边高度大于1，右边高度最小为0，左边最小为2，因此不用考虑空指针异常的问题。
    >    
    > 6. 双旋转
    >
    >    1. 介绍
    >    
    >    	某些情况，单旋转无法完成转换平衡二叉树。
    >    	
    >       对于数列{10, 11, 7, 6, 8, 9}这个数列，它要转成平衡二叉树，用单旋转无法完成转换。因此，我们需要对单旋转进行优化。
    >       
    >    2. 以上述数列{10, 11, 7, 6, 8, 9}为例，具体情况参见视频
    >    
    >    3. 右旋转的优化
    >    
    >        1. 思路
    >        
    >           对于右旋转节点n的左节点l，l是null，n普通右旋转；l的高度为1，n普通右旋转；l的左子树高度大于等于右子树高度，n普通右旋转即可；l的左子树高度小于右子树高度，先对l进行左旋转，让其左右子树高度相等，然后再对n进行右旋转。    
    >        2. 代码实现
    >        
    >            ```
    >            /**
    >                 * 右旋转当前节点(考虑双旋转)
    >                 */
    >                public void rightRotate() {
    >            
    >                    BalancedBinaryTreeNode l = this.left;
    >                    if (l.getRightTreeHeight() > l.getLeftTreeHeight()) {
    >                        // 考虑双旋转
    >                        l.leftRotate();
    >                    }
    >            
    >                    BalancedBinaryTreeNode temp = new BalancedBinaryTreeNode(this.value);
    >                    temp.right = this.right;
    >                    temp.left = this.left.right;
    >                    this.right = temp;
    >            
    >                    this.value = this.left.value;
    >                    this.left = this.left.left;
    >                }
    >            ```
    >        
    >    4. 左旋转的优化
    >    
    >        1. 思路
    >        
    >           类似于右旋转的优化
    >           
    >        2. 代码实现
    >        
    >            ```
    >            /**
    >                 * 左旋转当前节点(考虑双旋转)
    >                 */
    >                public void leftRotate() {
    >            
    >                    BalancedBinaryTreeNode r = this.right;
    >                    if (r.getLeftTreeHeight() > r.getRightTreeHeight()) {
    >                        //考虑双旋转
    >                        // 先让l右旋转
    >                        r.rightRotate();
    >                    }
    >            
    >                    BalancedBinaryTreeNode temp = new BalancedBinaryTreeNode(this.value);
    >                    temp.left = this.left;
    >                    temp.right = this.right.left;
    >                    this.left = temp;
    >            
    >                    this.value = this.right.value;
    >                    this.right = this.right.right;
    >                }
    >            ```
    >            
    >        5. 注意事项
    >        
    >           左旋转优化时：因为当前节点要左旋转，因此当前节点一定有右子节点，不用考虑空指针异常。右旋转优化师：因为当前节点要右旋转，因此当前节点一定有左子节点，不用考虑空指针异常。
    >    
    > 7. **特别注意事项：我规定添加完毕，先判断是否进入左旋转，再判断是否进入右旋转。如果进入左旋转，旋转完毕必须直接结束，加return结束，不能继续向下右旋转；右旋转本来就是最后一个判断，不用加return**
    >        
### 2. 多路查找树：仅做了解，不做深入探究
2. 多路查找树
    > 1. 出现经过
    > 
    >    二叉树的操作效率很高，但是也存在问题：二叉树要加载到内存中，如果二叉树的节点较少，没有问题；如果节点较多，则存在如下两个问题，问题1：构建二叉树需要进行大量的I/O操作（数据存在于文件中或数据库中），会对创建速度造成影响；问题2：节点很多，造成二叉树的高度很高，操作速度会降低。
    >    
    > 2. 多叉树（MultiwayTree）
    > 
    >     1. 简介
    >     
    >        对于二叉树来说，每个节点只能有一个数据项，并且只能有两个子节点。如果允许每个节点有多个数据项和多个子节点，就是多叉树（MultiwayTree）
    >        
    >        2-3树、2-3-4树就是多叉树，多叉树通过重新组织节点，降低树的高度，可以对二叉树进行优化。
    >        
    >        **多个数据项两两之间代表的是范围**
    >        
##### 1. 2-3树
1. 2-3树
    > 1. 简介
    > 
    >    2-3树就是最简单的B树，它有以下的特点，也必须满足下面的特点：
    >    
    >    1>2-3树的所有叶子节点都在同一层（只要是B树都有这个特点）
    >    
    >    2>有两个子节点的节点叫做二节点，二节点要么没有子节点，要么有两个子节点
    >    
    >    3>有三个子节点的节点叫做三节点，三节点要么没有子节点，要么有三个子节点
    >    
    >    4>2-3树只有二节点或者三节点
    >    
    >    5>2-3树仍然要满足排序树的特点
    >    
    > 2. 插入一个数值到2-3树规则
    > 
    >     1.保证2-3树叶子节点在同一层
    >     
    >     2.保证只有二节点和三节点
    >     
    >     **3.按照规则插入数值到某个节点时时，不能满足上述规则时，要拆分。先向上拆，上层满拆本层，拆后必须保证仍满足1和2。（该规则最为重要）**
    >     
##### 2. 2-3-4树
2. 2-3-4树
    > 1. 简介
    > 
    >    2-3-4树是2-3树基础上的升级，它也是B树，有以下特点：
    >    
    >    1>叶子节点在同一层
    >    
    >    2>只有二节点、三节点、四节点
    >    
    >    3>满足排序树要求
    >    
##### 3. B树（B-Tree）
3. B树（B-Tree）
    > 1. 简介
    > 
    >    B-Tree即B树，B是Balance，平衡的意思。有的翻译把B-Tree翻译成B-树，其实B-树就是B树，即B-Tree。B树是多路查找树。B树图解见D:\JavaZiLiao\DataStructurePhoto\DataStructure5.png
    >    
    >    2-3树、2-3-4树就是B树
    >    
    > 2. 说明
    > 
    >    在学习MySql的时候，经常可以听到某种类型的索引是基于B树或者B+树的。这里对B树的一些专用术语进行说明：
    >    
    >     * B树的阶：一个节点的最多子节点数。2-3数的阶为3,2-3-4的阶为4
    >    
    >     * B树的搜索：从根节点开始，对节点中的关键字进行二分查找（因为B树满足顺序树，因此节点的关键字一定是有序的），如果命中则结束，如果没有命中，则进入关键字所属范围的儿子节点；重复查找过程，直到儿子指针为空，或者已经是叶子节点
    >    
    >     * 关键字集合分布在整个树中，即叶子节点和非叶子节点都存储了数据
    >    
    >     * 搜索过程可能在非叶子节点结束。因为非叶子节点也存储了关键字，找到了自然结束了
    >    
    >     * B树搜索性能等同于在关键字全集内做一次二分查找
    >    
##### 3. B+树
3. B+树
    > 1. 简介
    > 
    >    B+树是基于B树的升级版本，也是多路查找树。B+树图解见D:\JavaZiLiao\DataStructurePhoto\DataStructure6.png
    >    
    > 2. 说明
    > 
    >     * B+树搜索和B树基本相同，区别是：B+树的关键字存储在叶子节点，只有达到叶子节点才会命中，B树可以在非叶子节点命中。相同点是：B树和B+树搜索性能都等价于对关键字集合做一次二分查找。
    >     
    >     * B+树所有关键字只出现在叶子节点的链表中（该链表也叫稠密索引），且链表中的关键字（数据）都是有序的
    >     
    >     * B+树永远不可能在非叶子节点命中
    >     
    >     * **B+树的非叶子节点的关键字相当于叶子节点的索引（稀疏索引），叶子节点是存储数据的数据层**
    >     * B+树更适合做文件索引系统
    >     
    >     * **B树和B+树都有自己的工作场景，不是说B+树一定比B树好**
    >     
    > 3. B+树的应用场景：图书馆中，不同书架有不同的层，每一层都有一个编号，每一层的书都是按照编号依次向后排序的。图书馆的书籍管理就可以使用B+树，不同书架的不同层作为非叶子节点，每层的书籍存储在叶子节点。
    > 
##### 4. B*树
4. B*树  
    > 1. 简介
    > 
    >    B\*树是B+树的升级版本，也是多路查找树。B\*树是在B+树的基础上，在非根节点和非叶子节点上，增加了指向兄弟的指针（紫色的Q）。B\*树图解见D:\JavaZiLiao\DataStructurePhoto\DataStructure7.png
    >    
    > 2. B\*树说明
    > 
    >     * B*树定义了非叶子节关键字（可以理解为叶子节点索引）个数至少为(2/3)*M，M为树的阶，即块的最低使用率为2/3。与此相比B+树块的最低使用率为1/2
    >     
    >     * 基于以上特点，B\*树分配新节点概率比B+树小，空间利用率比B+树高
    >     
## 11. 图（Graph）
11. 图（Graph）
    > 1. 出现历程
    > 
    >    前面学习的线性表：局限于一个前驱、一个后继，即一对一关系；树：一个节点只能有一个父节点，局限于一个前驱、多个后继，即一对多的关系，但是这两种都无法实现多对多关系。
    >    图可以实现多对多关系
    >    
    > 2. 介绍
    > 
    >    图是一种数据结构，它要求：一个结点可以有零个或多个相邻元素。两个结点之间连接，称为边；结点在图中也被称为顶点。
    >    
    > 3. 图的常用概念
    > 
    >     * 边（edge）：两个结点相连称为边
    >     
    >     * 顶点（vertex）：结点
    >     
    >     * 路径
    >     
    >     * 无向图；如果图中顶点之间连接没有方向，称为无向图
    >     
    >     * 有向图：图中边有专门的指向，则称为有向图
    >     
    >     * 带权图：边带有权值，称之为带权图，也叫作网
    >     
    > 4. 图的表示方式
    > 
    >     * 邻接矩阵：通过二维数组方式来表示图，称之为邻接矩阵
    >     
    >         * 之前实现的迷宫回溯，就是用的邻接矩阵表示图
    >         
    >     * 邻接表：通过数组+链表方式来表示图，称之为邻接表
    >     
    > 5. 邻接矩阵
    > 
    >     1. 介绍
    >     
    >        假设一张图中有n个顶点，我们使用n*n的邻接矩阵（二维数组）表示这n个顶点之间的连通关系，0为连通，1为不连通。
    >        
    >     2. 举例说明
    >     
    >        假设有一张无向图，图形为一个正方形。我们假设从左上角开始，顺时针为每个角赋值，左上角为0，依次为0,1,2,3。用邻接矩阵存储该正方形则表示为：
    >        
    >        {0, 1, 0, 1} 
    >        {1, 0, 1, 0}
    >        {0, 1, 0, 1}
    >        {1, 0, 1, 0}
    >        建议画张图自己体会。
    >     
    > 6. 邻接表
    > 
    >    1. 介绍
    >    
    >       邻接矩阵需要为n个顶点分配n*n的二维空间，边存在才赋值为1，这回造成不存在的边浪费空间的情况。邻接表的实现只关心存在的边，不会浪费空间。
    >       
    >    2. 实现
    >    
    >       邻接表由数组+链表实现
    >       
    >    3. 举例
    >    
    >       继续以上述正方形为例，以邻接表形式实现：
    >       
    >       {(1, 3), (0, 2), (1, 3), (0, 2)}
    >    
    > 7. 表示一个图的类中，除了添加顶点、设置边等基础方法之外的常用方法
    > 
    >     * 返回边的数目
    >     
    >     * 返回顶点数目
    >     
    >     * 返回给定两个顶点的连通状态
    >     
    >     * 返回对应index的数据（针对邻接矩阵）
    >     
    >     * 显示图对应的邻接矩阵（针对邻接矩阵）
    >       
##### 1. 图的遍历（还未进行使用邻接表存储图，进行DFS和BFS的验证，写完了可以删除这一行）
1. 图的遍历
    > 1. 介绍
    >
    >    图的遍历，就是对图上所有结点的访问。一个图结点的访问有两种策略：深度优先遍历、广度优先遍历
    >    
    >    其实，深度优先和广度优先最初都是基于树结构进行访问的两种算法，都是从root节点开始访问整棵树，但是在图上也可以使用，只不过我们需要记录访问过的结点，防止重复访问。
    >    
    >     * 八皇后问题其实就是树的深度优先的一个变种
    >    
###### 1. 深度遍历搜索（DFS，DeepFirstSearch）  
1. 深度优先搜索（DFS，DeepFirstSearch）算法
    > 1. 介绍
    >    
    >    1>深度优先是：从初始访问结点出发，对于初始节点的多个邻接结点，先访问第一个邻接结点，然后再以第一个邻接结点作为初始节点，重复访问直到访问完成。**深度优先就是每次访问完当前节点后，访问当前节点的第一个邻接节点**
    >    
    >    2>优先纵向深入挖掘，而不是对所有邻接节点进行横向访问，这种就叫深度优先。
    >    
    >    3>可见，深度访问是一个递归过程
    >    
    >     * 第一次写的迷宫回溯的路径寻找就是用的深度优先算法。
    >    
    > 2. 深度遍历算法步骤：深度优先遍历基于深度优先搜索
    >    
    >     1. 访问初始节点v1，并标记节点v1已经被访问
    >        
    >     2. 查找初始节点v1的第一个邻接节点w
    >        
    >     3. **如果w不存在，返回第1步，从v1的下一个节点继续进行深度优先遍历，直到所有的节点都遍历完**；如果w存在，进行第4步
    >        
    >     4. 如果w没有被访问过，则让邻接节点作为初始节点v1进入递归继续进行深度优先遍历；如果w被访问过，退回第2步，从下一个邻接节点x开始访问
    >     
    > 3. 算法代码：基于邻接矩阵记录图中边的信息
    >    
    >     ```
    >     // 定义一个记录节点是否被访问过的标记数组
    >         boolean[] isVisited;
    >     ```
    >     
    >     ```
    >         // 在构造器中初始化节点的标记数组，n为结点数量
    >         this.isVisited = new boolean[n];
    >     ```
    >     
    >     ```
    >         /**
    >              * 获取第一个相邻节点的下标
    >              * @param v1 初始节点下标
    >              * @return
    >              */
    >             public int getFirstNeighbor(int v1) {
    >                 for (int i = 0; i < vertex.size(); i++) {
    >                     if (edges[v1][i] > 0) {
    >                         // i这个节点和v1连通，即i为v1相邻节点
    >                         return i;
    >                     }
    >                 }
    >                 // 没有相邻节点
    >                 return -1;
    >             }
    >     ```
    >     
    >     ```
    >         /**
    >              * 根据上一个相邻节点的下标，获取下一个相邻节点
    >              * @param v1 初始节点下标
    >              * @param v2 上一个相邻节点下标
    >              * @return
    >              */
    >             public int getNextNeighbor(int v1, int v2) {
    >                 for (int j = v2 + 1; j < vertex.size(); j++) {
    >                     if (edges[v1][j] > 0) {
    >                         // 初始节点和j节点连通，说明j为下一个相邻节点
    >                         return j;
    >                     }
    >                 }
    >                 // 没有下一个相邻节点
    >                 return -1;
    >             }
    >     ```
    >     
    >     ```
    >         /**
    >              * 某个节点的深度优先遍历
    >              * @param isVisited 结点是否被访问
    >              * @param v1 初始结点下标
    >              */
    >             private void deepFirstOrder(boolean[] isVisited, int v1) {
    >                 // 输出结点信息
    >                 System.out.print(getVertexByIndex(v1) + "->");
    >                 // 将节点设为被访问过
    >                 isVisited[v1] = true;
    >         
    >                 // 获取第一个相邻结点下标
    >                 int v2 = getFirstNeighbor(v1);
    >                 while (v2 > 0) {
    >                     // 如果相邻节点存在
    >                     if (!isVisited[v2]) {
    >                         // 如果相邻节点没有被访问过，将下一个结点作为初始节点
    >                         deepFirstOrder(isVisited, v2);
    >                     }
    >                     // 如果相邻节点被访问过，获取下一个相邻节点直到没有相邻节点
    >                     v2 = getNextNeighbor(v1, v2);
    >                 }
    >                 // 没有第一个相邻节点，结束，进入下一个结点的深度遍历
    >             }
    >             
    >             public void deepFirstOrder() {
    >                 // 深度遍历所有的节点
    >                 // 如果每个有某些节点不连通，该循环是为了让不连通图也遍历到
    >                 for (int i = 0; i < getNumOfVertex(); i++) {
    >                 	// 完成所有节点的深入优先遍历
    >                 	// 如果有些节点连通，则在递归时就遍历过了，标识已经改变了，筛选出不连通的节点继续遍历
    >                     if (!isVisited[i]) {
    >                         deepFirstOrder(isVisited, i);
    >                	    }
    >                 }
    >             }
    >     ```
    >     
    > 4. **注意事项：**
    >    
    >    因为图的某些节点可能是独立的的，因此我们在深入优先遍历时一定要加个循环将所有节点都进行深入优先遍历。例如D:\JavaZiLiao\DataStructurePhoto\DataStructure8.png所表现的图，F不和任何节点连通，无法通过其他节点的深度优先遍历的递归找到F，因此需要循环。
    > 
###### 2. 广度优先搜索（BFS，BreadthFirstSearch）
2. 广度优先搜索（BFS，BreadthFirstSearch）
    > 1. 介绍
    >
    >    广度优先搜索类似于一个分层搜索的过程，需要一个队列，用来保证访问过的节点的顺序，以便按照该顺序来访问这些节点的邻接节点。
    >    
    >    广度优先算法以树的形式来考虑会更好理解。
    >    
    > 2. 算法步骤
    >
    >    1>访问初始节点v1，标记节点v1为已经访问过
    >    
    >    2>节点v1入队列
    >    
    >    3>当队列不为空，继续执行，否则算法结束
    >    
    >    4>出队列，取得队列头结点u
    >    
    >    5>查找队列头结点u的第一个邻接节点w
    >    
    >    6>如果队列头结点u的第一个邻接节点w不存在，转回到步骤3；如果存在，继续循环执行下列步骤
    >    
    >    6.1>如果w没有被访问，则访问w节点，并标记为w已经被访问
    >    
    >    6.2>节点w入队列
    >    
    >    6.3>查找u在w之后的下一个邻接节点w，继续步骤6
    >    
    > 3. 算法代码
    >
    >     ```
    >     // 定义一个数组作为广度查询的队列，该队列存储结点的下标
    >         int [] vertexQueue;
    >     ```
    >     
    >     ```
    >     // 构造器中，初始化广度查询需要的队列，n为节点数量
    >         this.vertexQueue = new String[n];
    >     ```
    >     
    >     ```
    >     /**
    >          * 广度优先遍历
    >          */
    >         public void breadthFirstOrder() {
    >             for (int i = 0; i < getNumOfVertex(); i++) {
    >                 if (!isVisited[i]) {
    >                     breadthFirstOrder(isVisited, i);
    >                 }
    >             }
    >         }
    >         
    >         public void breadthFirstOrder(boolean[] isVisited, int v) {
    >             // 初始化队列尾指针和头指针
    >             int last = 0;
    >             int first = 0;
    >     
    >             // 打印初始节点信息
    >             System.out.print(getVertexByIndex(v) + "->");
    >             // 将初始节点置为被访问过
    >             isVisited[v] = true;
    >             // 初始节点入队列
    >             vertexQueue[last] = v;
    >             last++;
    >     
    >             // 如果队列不为空
    >             while (last != first) {
    >                 // 队列头元素出队列
    >                 int v1 = vertexQueue[first];
    >                 vertexQueue[first] = 0;
    >                 first++;
    >     
    >                 // 获取第一个邻接节点
    >                 int firstNeighbor = getFirstNeighbor(v);
    >                 while (firstNeighbor > 0) {
    >                     // 邻接节点存在
    >                     if (!isVisited[firstNeighbor]) {
    >                         // 邻接节点没有被访问过
    >                         System.out.print(getVertexByIndex(firstNeighbor) + "->");
    >                         isVisited[firstNeighbor] = true;
    >                         // 邻接节点入队列
    >                         vertexQueue[last] = firstNeighbor;
    >                         last++;
    >                     }
    >                     // 获取下一个邻接节点
    >                     firstNeighbor = getNextNeighbor(v, firstNeighbor);
    >                 }
    >             }
    >         }
    >     ```
    >     
##### 3. 深度优先遍历和广度优先遍历区别
3. 深度优先遍历和关顾优先遍历区别
    > 1. 实例
    >
    >     1. 假设有一张图，图为D:\JavaZiLiao\DataStructurePhoto\DataStructure9.png
    >     
    >     2. 对该图进行深度优先遍历和广度优先遍历，分别输出结果
    >     
    >         ```
    >         /**
    >         * 深度优先遍历结果为
    >         */
    >         A->B->D->G->E->C->F->
    >         ```
    >         
    >         ```
    >         /**
    >         * 广度优先遍历结果为
    >         */
    >         A->B->C->D->G->E->F->
    >         ```
    >         
## 12. 程序员常用的10种算法
### 1. 二分查找（BinarySearch）
1. 二分查找（BinarySearch）
    > 1. 递归方式
    >
    >    在前面的课程中，已经学习过递归方式实现的二分查找，如下：
    >    
    >     ```
    >    /**
    >         * 递归方式实现二分查找
    >         * @param left 左指针
    >         * @param right 右指针
    >         * @param arr 待查找数组
    >         * @param target 目标元素
    >         * @return 目标元素下标
    >         */
    >        public static int binarySearchByRecursion(int left, int right, int[] arr, int target) {
    >    
    >            if (left > right || target < arr[0] || target > arr[arr.length - 1]) {
    >                return -1;
    >            }
    >    
    >            int mid = (left + right) / 2;
    >            if (arr[mid] > target) {
    >                return binarySearchByRecursion(left, mid - 1, arr, target);
    >            }
    >            if (arr[mid] < target) {
    >                return binarySearchByRecursion(mid + 1, right, arr, target);
    >            }
    >            return mid;
    >        }
    >    
    >        public static int binarySearchByRecursion(int[] arr, int target) {
    >            return binarySearchByRecursion(0, arr.length - 1, arr, target);
    >        }
    >     ```
    >    
    > 2. 非递归方式
    >
    >     ```
    >     /**
    >          * 非递归实现二分查找
    >          * @param arr 待查询数组
    >          * @param target 查询的元素
    >          * @return 查询的元素在数组的下标
    >          */
    >         public static int binarySearchNotRecursion(int[] arr, int target) {
    >             int left = 0;// 左指针
    >             int right = arr.length - 1;// 右指针
    >             int mid = 0; // 存储中间元素的下标
    >     
    >             while (true) {
    >                 if (left > right || target < arr[0] || target > arr[arr.length - 1]) {
    >                     // 数组中没有该元素，返回-1
    >                     return -1;
    >                 }
    >                 mid = (left + right) / 2;
    >                 if (arr[mid] == target) {
    >                     // 找到元素对应的下标，退出迭代
    >                     break;
    >                 }
    >                 if (arr[mid] > target) {
    >                     right = mid - 1;
    >                 }
    >                 if (arr[mid] < target) {
    >                     left = mid + 1;
    >                 }
    >             }
    >             // 将元素对应下标返回
    >             return mid;
    >         }
    >     ```
    >     
### 2. 分治算法（DivideAndConquer）
2. 分治算法（DivideAndConquer）
    > 1. 介绍
    >
    >    将一个规模为N的问题分解为K个规模较小的子问题，这些**子问题相互独立**且与原问题性质相同。求出子问题的解，就可得到原问题的解
    >    
    >    分治算法是许多经典高效算法的基础：排序算法中的快速排序、归并排序；傅里叶变换（这里是快速傅里叶变化，FFT）
    >    
    >    分治算法可以解决许多经典问题：汉诺塔问题、快速排序、归并排序、二分搜索等
    >    
    > 2. 分值算法解决问题的逻辑
    >
    >     1. 分解：将n个问题分解成k个规模的小问题
    >     
    >     2. 解决：如果无法继续分解，则递归解决小问题
    >     
    >     3. 合并：将每个小问题的解合并变成原问题的解
    >     
    > 3. 分治算法解决汉诺塔问题
    >
    >     1. 汉诺塔问题
    >     
    >        假设一个汉诺塔有n层，一共有A、B、C三个柱子，假设一开始汉诺塔全部放在柱子A上，经过解决全部放在C上。每次只能移动一层
    >        
    >     2. 分析
    >     
    >        无论有多少层，我们都看成两层，即第n层和上面的n-1层。
    >        
    >        把上面的n-1层移动到中间的过渡柱子，将第n层直接移动到目标柱子，将上边的n-1层从过渡柱子移动到目标柱子，则完成了n层从所在柱子移动到目标柱子。
    >        
    >     3. 汉诺塔问题代码
    >     
    >         ```
    >         /**
    >              * 将a上的n层圆盘通过b移动到c
    >              * @param n 汉诺塔层数
    >              * @param a n层所在柱子
    >              * @param b 过渡用柱子
    >              * @param c n层的目标柱子
    >              */
    >             public static void hanoi(int n, String a, String b, String c) {
    >                 if (n == 1) {
    >                     // 如果a塔上只有一层，直接把圆盘从a移动到c，算法结束
    >                     System.out.println(a + "移动到" + c);
    >                 } else {
    >                     // 如果a塔上有多于一层的圆盘
    >                     // 先让上面的n-1层通过c，从a移动到b
    >                     hanoi(n - 1, a, c, b);
    >                     // 让第n层的圆盘直接从a移动到c
    >                     System.out.println(a + "移动到" + c);
    >                     // 让剩下的圆盘通过a，从b移动到c
    >                     hanoi(n-1, b, a, c);
    >                 }
    >             }
    >         ```
    >     
    > 4. 注意事项：分治算法使用场景最重要的一点就是**子问题相互独立**，和动态规划算法作区分就是这一点。
    >    

### 3. 动态规划算法（DynamicPrograming） （还未进行使用哈希表存储动态规划表，完成背包问题的代码书写，写完了删除这条提示信息）
3. 动态规划算法（DynamicPrograming）
    > 1. 简介
    >
    >    动态规划算法的核心思想和分治算法一样：将大问题划分成小问题进行解决，从而得到最优解。
    >    和分治算法不同的是：分治算法它分解的子问题相互独立；动态规划算法分解的子问题往往不是相互独立的，意思就是可能下一个阶段的子问题的求解要考虑上一个阶段的子问题的解。
    >    
    >    **动态规划求得最优解方法：通过填表的方式逐步推进，从而得到最优解**
    >    
    > 2. 动态转移方程和动态转移表
    >
    >     1. 动态转移方程
    >     
    >         * 动态转移方程的设计满足：最优化原理和无后效性
    >         
    >             最优化原理：一个最优化策略的子策略永远是最优的
    >           
    >             无后效性：每个状态都是过去历史的总结 
    >             
    >         * 动态转移方式设计步骤：
    >         
    >            一、确定问题的决策对象：背包问题中就是背包
    >            
    >            二、对决策对象划分阶段：背包容量动态化
    >            
    >            三、对各阶段确定状态变量：背包问题中就是各个物品
    >            
    >            四、根据状态变量确定费用函数和目标函数：即确定在j容量下背包的最大价值装法
    >            
    >            五、建立各阶段的状态变量的转移方程，写出状态转移方程
    >            
    >            六、编程实现 
    > 3. 背包问题
    >
    >     1.介绍
    >     
    >        该问题就是经典的用了动态规划算法的例子。
    >     
    >        假设有一个背包，指定它最大的装包质量为mkg；假设我们有n个物品，不同的物品有各自的重量和价格。如何选择物品，使得放入背包的物品总价值最大。这就是背包问题
    >     
    >        背包问题分为01背包问题和完全背包问题，01背包问题要求每个物品只有一份，完全背包问题对物品数量没有规定。
    >     
    >        **完全背包问题可以转化成01背包问题。因此弄清楚01背包问题，完全背包问题就很好解决了**
    >     
    >     2. 01背包问题
    >     
    >         1. 题目
    >         
    >            假设有个背包，背包容量为C。假设有n件物品，遍历每件物品，每次遍历到的物品记为i，w[i]记为该物品重量，v[i]记为该物品价格。
    >            
    >         2. 动态规划解决问题的思想
    >         
    >            虚拟一个容量可以增长的动态背包，容量从0涨到C。假设每次变化过后动态背包的容量为j，遍历每个物品往背包装物品。
    >            
    >            动态背包容量为0或者动态背包有容量没有装入商品时，该动态背包最大价值为0
    >            
    >            遍历每一个物品，装入每个不同容量的动态背包
    >         
    >            假设装到第i个物品，如果背包容量j不够装下i，则j容量的动态背包此时价值等于装i之前的价值；
    >            
    >            如果够装下i，则需要比较：当动态背包容量为j-w[i]时，装前i-1个商品获得最大价值```c[i-1][j-w[i]]```，如果此价值```c[i-1][j-w[i]]```加上第i个物品价值```v[i]```高于，在动态背包容量为j时，装前i-1个物品能获得的最大价值```c[i-1][j]```，说明此时将第i个商品装入容量为j的背包是价值最大的。
    >            
    >         3. 动态转移公式
    >         
    >             ```
    >            c[i][0]=c[0][j]=0 // 表示填入表的第一行和第一列为0
    >            
    >            w[i] > j，c[i][j] = c[i - 1][j]：// 当准备加入新增的商品的容量大于当前动态背包的，直接使用上一个单元格的装入策略
    >            
    >            w[i] <= j，max{c[i - 1][j], c[i - 1][j - w[i]] + v[i]} // 当准备加入新增的商品的容量小于等于当前动态背包的容量，比较上一个单元格的最大价值和j-w[i]容量时的动态背包装入i-1的单元格价值+v[i]的总价值，谁大填入谁
    >            ```
    >            
    >         4. 背包问题动态规划的动态规划表图解
    >         
    >            D:\DataStructurePhoto\DataStructure10.png
    >            
    >            D:\DataStructurePhoto\DataStructure10.png
    >            
    >         5. 代码解决背包问题
    >         
    >             ```java
    >              /**
    >                  * 背包问题
    >                  * 数组作为动态规划表
    >                  * @param size 背包大小
    >                  * @param things 物品名数组
    >                  * @param prize 物品价格数组
    >                  * @param weight 物品质量数组
    >                  */
    >                 public static void solveByArray(int size, String[] things, int[] prize, int[] weight) {
    >                     // 初始化动态规划表
    >                     int[][] table = new int[things.length + 1][size + 1];
    >                     // 因为要记录背包中加入的物品，初始化一个记录的表
    >                     boolean[][] thing = new boolean[things.length + 1][size + 1];
    >             
    >                     // 进行动态规划
    >                     for (int i = 1; i < things.length + 1; i++) {
    >                         for (int j = 1; j < size + 1; j++) {
    >                             if (weight[i - 1] > j) {
    >                                 // 因为程序的i是从1开始，因此物品质量和物品价格和物品的下标都是从i-1
    >                                 // 如果动态背包容量j小于物品质量
    >                                 table[i][j] = table[i - 1][j];
    >                             } else {
    >                                 // 如果动态背包容量j大于等于物品质量
    >                                 if (table[i - 1][j] < table[i - 1][j - weight[i - 1]] + prize[i - 1]) {
    >                                     // 如果加入该物品符合在j容量下的最优策略，将最优策略写入
    >                                     table[i][j] = table[i - 1][j - weight[i - 1]] + prize[i - 1];
    >                                     // 将该i-1对应的物品加入j容量的背包
    >                                     thing[i][j] = true;
    >                                 } else {
    >                                     // 如果加入该物品不符合最优策略
    >                                     table[i][j] = table[i - 1][j];
    >                                 }
    >                             }
    >                         }
    >                     }
    >             
    >                     //动态规划完毕，输出最终背包里的物品
    >                     int i = things.length; // 动态规划表最大行
    >                     int j = size; // 动态规划表最大列
    >                     while (i > 0 && j > 0) {
    >                         if (thing[i][j]) {
    >                             // 说明该物品加入了背包，输出该物品
    >                             System.out.printf("%s加入背包\n", things[i - 1]);
    >                             j = j - weight[i - 1];
    >                         }
    >                         i--;// 遍历所有物品
    >                     }
    >                 }
    >             ```
    >             
    >             * **注意事项：**
    >             
    >                1、物品的索引是i-1；
    >                
    >                2、因为要输出最终背包的物品，需要一个记录在j容量下是否将i-1的物品加入背包的二维数组thing；
    >                
    >                3、输出背包的物品思路为：获取从最终容量的背包中从后往前判断物品是否加入，加入了则减去物品的重量，向下遍历其他物品。这里需要好好思考
    >         
### 4. 暴力匹配算法（BruteForce）
4. 暴力匹配算法（BruteForce）
    > 1. 介绍
    >
    >    即通过遍历所有可能性找到所有的可能，匹配需要的结果。
    >    
    >    该算法使用递归，会产生大量的回溯，浪费大量的时间，因此如果非特别必要，不使用该算法。
    >    
    > 2. 实例：暴力匹配字符串
    >
    >     1. 代码实现
    >     
    >         ```
    >         /**
    >              * 暴力匹配字符串
    >              * @param str1 目标字符串
    >              * @param str2 匹配字符串
    >              * @return
    >              */
    >             public static int stringBruteForce(String str1, String str2) {
    >                 char[] strChar1 = str1.toCharArray();
    >                 char[] strChar2 = str2.toCharArray();
    >         
    >                 int i = 0; // 指向str1的指针
    >                 int j = 0;// 指向str2的指针
    >                 while (i < strChar1.length && j < strChar2.length) {
    >                     if (strChar1[i] == strChar2[j]) {
    >                         // 匹配成功
    >                         i++;
    >                         j++;
    >                     } else {
    >                         // 匹配失败，i向后移动一位，j重新置为0
    >                         i = i - j + 1;
    >                         j = 0;
    >                     }
    >                 }
    >                 
    >                 if (j == strChar2.length) {
    >                 // str1匹配到了str2
    >                 // 返回str1中匹配到str2首字母的下标
    >                     i = i - j;
    >                     return i;
    >                 } else {
    >                     return -1;
    >                 }
    >             }
    >         ```
    >         
### 5. KMP算法（字符串查找算法）(比较难理解，多看几遍)
5. KMP算法（字符串查找算法）
    > 1. 简介 
    >
    >    常用于在一个文本串S中，查找一个模式串P的位置
    >    
    >    是在暴力匹配算法基础上的优化
    >    
    > 2. 暴力匹配的不足
    >
    >    加入str2="ABCDAB"，使用这个字符串为格式串。假设str1="ABCDFHABCDAB"
    >    
    >    假设通过暴力匹配，str1在匹配str2的第二个"A"时失败，则会回溯，向后移动1位，开始继续匹配str2。但是这样其实是不明智的，因为对于str1来说"BCD"其实是已经比较过的。
    >    
    > 3. KMP算法思想
    >
    >    继续以str2="ABCDAB"，str1="ABCDFHABCDAB"为例子
    >    
    >    KMP算法：对str2计算出一个**部分匹配表**，str1在匹配失败后，回溯，按照KMP算法移动位数公式来移动，继续匹配
    >    
    >     * **KMP算法移动公式：移动位数=已经匹配的字符数-字符对应在部分匹配表的部分匹配值**
    > 4. 部分匹配表
    >
    >     1. 介绍
    >     
    >        部分匹配表也叫部分匹配池，是KMP算法的核心。
    >        
    >     2. 生成步骤
    >     
    >         1. 获取字符串的前缀和后缀
    >         
    >            要想创建部分匹配表，先要了解字符串的前缀和后缀是什么，以字符串"HELLO"为例子，介绍前缀后缀
    >            
    >             * 前缀：
    >            
    >                "H"、"HE"、"HEL"、"HELL"
    >                
    >             * 后缀
    >            
    >                "ELLO"、"LLO"、"LO"、"O"
    >            
    >         2. 通过字符串的前缀和后缀，生成字符串的部分匹配表
    >         
    >            部分匹配表的部分匹配值，就是前缀和后缀的最长共有元素的长度
    >            
    >             * 以"ABCDABD"为例子，生成部分匹配表：
    >            
    >                "A" -- 前缀和后缀为空，部分匹配值0
    >                
    >                "AB" -- ["A"]和["B"]最长共有元素没有，长度为0，部分匹配值0
    >                
    >                "ABC" -- ["A", "AB"]和["BC", "C"]最长共有元素没有，长度为0，部分匹配值0
    >                
    >                "ABCD" -- ["A", "AB", "ABC"]和["BCD", "CD", "D"]最长共有元素没有，长度为0，部分匹配值0
    >                
    >                "ABCDA" -- ["A", "AB", "ABC", "ABCD"]和["BCDA", "CDA", "DA", "A"]最长共有元素"A", 长度为1，部分匹配值1
    >                
    >                "ABCDAB" -- ["A", "AB", "ABC", "ABCD", "ABCDA"]和["BCDAB", "CDAB", "DAB", "AB", "B"]最长共有元素"AB, 长度为2，部分匹配值2
    >                
    >                "ABCDABD" -- ["A", "AB", "ABC", "ABCD", "ABCDA", "ABCDAB"]和["BCDABD", "CDABD", "DABD", "ABD", "BD", "D"]最长共有元素没有, 长度为0，部分匹配值0
    >                
    >                综上，字符串"ABCDABD"部分匹配值表为[0, 0, 0, 0, 1, 2, 0]
    >     
    > 5. 代码实例
    >
    >     * kmp主体部分
    >     
    >         ```
    >         /**
    >              * kmp匹配
    >              * @param str1 目标字符串
    >              * @param str2 匹配字符串
    >              * @return
    >              */
    >             public static int kmp(String str1, String str2) {
    >                 // 生成部分匹配表
    >                 int[] kmpTable = createTable(str2);
    >         
    >                 // 利用部分匹配表进行字符串匹配
    >                 char[] strChar1 = str1.toCharArray();
    >                 char[] strChar2 = str2.toCharArray();
    >         
    >                 int i = 0; // 指向str1的指针
    >                 int j = 0;// 指向str2的指针
    >                 while (i < strChar1.length && j < strChar2.length) {
    >                     if (strChar1[i] == strChar2[j]) {
    >                         // 匹配成功
    >                         i++;
    >                         j++;
    >                     } else {
    >                         // 匹配失败
    >                         if (j > 0) {
    >                             // 匹配的有字符
    >                             // i的移动位数=已经匹配的字符数-字符对应在部分匹配表的部分匹配值
    >                             i = i - j + (j - kmpTable[j - 1]);
    >                         } else {
    >                             // 没有匹配到字符
    >                             i = i + 1;
    >                         }
    >                         j = 0;
    >                     }
    >                 }
    >         
    >                 if (j == strChar2.length) {
    >                     // str1匹配到了str2
    >                     // 返回str1中匹配到str2首字母的下标
    >                     i = i - j;
    >                     return i;
    >                 } else {
    >                     return -1;
    >                 }
    >         
    >             }
    >         ```
    >         
    >     * 部分匹配表生成部分（该部分是最难理解的，多从网上看看资料学习一下）
    >     
    >         ```
    >         /**
    >              * 根据传入的字符串生成该字符串的部分匹配表
    >              * @param str
    >              * @return
    >              */
    >             public static int[] createTable(String str) {
    >                 // 将字符串转成char数组
    >                 char[] strChar = str.toCharArray();
    >                 // 初始化部分匹配表
    >                 int[] table = new int[str.length()];
    >                 table[0] = 0;// 首字符的部分匹配值一定是0
    >         
    >                 // 生成字符串的部分匹配表
    >                 int j = 0;
    >                 for (int i = 1; i < strChar.length; i++) {
    >                     // 为了阻止第一次j=0时就进入while循环，while判断先加入j>0判断
    >                     while (j > 0 && strChar[i] != strChar[j]) {
    >                         j = table[j - 1];
    >                     }
    >                     if (strChar[i] == strChar[j]) {
    >                         j++;
    >                     }
    >                     table[i] = j;
    >                 }
    >         
    >                 // 返回部分匹配表
    >                 return table;
    >             }
    >         ```
    >     
### 6. 贪心算法（GreedyAlgorithm）
6. 贪心算法（GreedyAlgorithm）
    > 1. 应用实例
    >
    >    假设存在如下几个广播台，每个台可以覆盖的地区不同，问：如何选择最少的广播台来覆盖到所有区域。
    >    
    >     ```
    >    k1={"北京", "上海", "天津"} 
    >    k2={"广州", "北京", "深圳"}
    >    k3={"成都", "上海", "杭州"} 
    >    k4={"上海", "天津"}
    >    k5={"杭州", "大连"}
    >     ```
    > 2. 思想
    >
    >    贪心算法指的是：在对问题求解时，每一步都采取最好或者最优的结果，希望其最终能导致结果最好
    >    
    >    **注意：贪心算法得到的结果不一定是最优的（有时候会是最优解），但是该解一定是近似于最优解的。**
    >    
    > 3. 利用贪心算法解决广播台问题
    >
    >     1. 思路
    >     
    >        假设我们利用穷举法解决广播台问题，假设有n个广播台，则解有2^n-1个，时间复杂度为O(2^n)，可见穷举法效率过低，不可行。
    >       
    >        假设我们使用贪心算法：
    >       
    >        1>遍历所有的广播台，找到一个覆盖了**最多未覆盖地区**的电台
    >       
    >        2>将该电台从电台池中取出，记录下来该电台，将该电台覆盖地区在下次比较时去掉
    >       
    >        3>重复上述步骤
    >       
    >        4>当全部地区都被覆盖到，结束算法
    >        
    >     2. 代码
    >     
    >         ```
    >         /**
    >              * 贪心算法解决广播站问题
    >              * @param stations
    >              * @param cities
    >              * @param finalStations
    >              */
    >             public static void greedy(HashMap<String, HashSet<String>> stations, HashSet<String> cities, HashSet<String> finalStations) {
    >                 // 定义临时集合
    >                 HashSet<String> temp = new HashSet<>();
    >                 String maxStation = null;
    >                 int preCount = 0;
    >                 
    >                 while (cities.size() != 0) {
    >                     // 每次都初始化覆盖最大范围的电台编号
    >                     maxStation = null;
    >         
    >                     // 遍历电台集合
    >                     for (String i :
    >                             stations.keySet()) {
    >                         // 每次都清空temp
    >                         temp.clear();
    >         
    >                         // 获取电台覆盖的城市
    >                         HashSet<String> areas = stations.get(i);
    >                         // 用临时变量临时存储这些电台的值
    >                         temp.addAll(areas);
    >                         // 获取电台覆盖的城市和所有城市的交集
    >                         temp.retainAll(cities);
    >                         if (temp.size() > 0) {
    >                             // 如果是第一次进入while循环且第一次进入foreach循环，maxStation==null，maxStation需要赋值，这个判断也要加上
    >                             if (maxStation == null || temp.size() > preCount) {
    >                                 maxStation = i;
    >                                 preCount = stations.get(i).size();
    >                             }
    >                         }
    >                     }
    >         
    >                     // 将最大范围电台添加入最终电台集合，并从cities删除对应的城市
    >                     if (maxStation != null) {
    >                         finalStations.add(maxStation);
    >                         // 移除城市
    >                         cities.removeAll(stations.get(maxStation));
    >                     }
    >                 }
    >             }
    >         ```
    >         
    >     3. 在解决广播站问题时遇到需要注意的事项
    >     
    >         1. 临时存储电台存储的城市的变量temp，我们需要使用addAll()方法，而不是add()方法。
    >         
    >         2. temp在每一次foreach逻辑结束都要清空
    >         
    >         3. 如果是第一次进入while循环，并且是第一次进入foreach循环，maxStation==null，需要赋值，因此在if判断时这个条件也要加上
    >         
### 7. 普里姆算法（PrimAlgorithm）
7. 普里姆算法（PrimAlgorithm）
    > 1. 应用场景
    >
    >    公交车站问题：某个城市有{A, B, C, D, E, F, G}个站点，需要修路修通7个站点，求如何花费最少
    >    
    >    公交车站问题图：D:\JavaZiLiao\DataStructurePhoto\DataStructure14.png
    >    
    > 2. 修路问题的解题思考
    >
    >    修路问题其实就是在n个节点的网（带权图）中，求一个边的子集构成的树中，连通了所有的节点，而且所有边的权值的和最小。该问题是经典的求最小生成树的问题，可以使用克鲁斯卡尔算法（kruskal算法）和普利姆算法（prim算法）。
    >    
    >    克鲁斯卡尔算法适合求边稀疏的无向网的最小生成树。
    >    
    >    普利姆算法适合求边稠密的无向网的最小生成树。
    >    
    >    * 最小生成树：Mininum Cost Spanning Tree，简称MST
    >    
    > 3. 普利姆算法
    >
    >     1. 简介
    >     
    >        普利姆算法求最小生成树，就是在n个节点的无向连通网中，找出只有(n-1)条边包含n个顶点的连通子图，该连通子图就是所谓的**极小连通子图**。
    >        
    >     2. 思想
    >     
    >        此算法可以称为“加点法”，每次迭代选择代价最小的边对应的点，加入到最小生成树中。算法从某一个顶点s开始，逐渐长大覆盖整个连通网的所有顶点。
    >        
    >        普里姆算法的运行效率只与连通网中包含的顶点数相关，而和网所含的边数无关。所以普里姆算法适合于解决边稠密的网
    >        
    >        图解D:\JavaZiLiao\DataStructurePhoto\DataStructure13.png
    >        
    >     3. 代码实现
    >     
    >         ```
    >         /**
    >          * 村庄的无向网
    >          */
    >         class Graph {
    >         
    >             // 存储顶点信息
    >             public char[] vertex;
    >             // 存储图的邻接矩阵
    >             public int[][] matrix;
    >             // 存储边的数量
    >             public int numOfEdges;
    >             // 顶点是否被访问过
    >             public boolean[] isVisited;
    >         
    >             public Graph(char[] vertex, int[][] matrix) {
    >         
    >                 int n = vertex.length;
    >         
    >                 // 初始化顶点
    >                 this.vertex = new char[n];
    >                 for (int i = 0; i < n; i++) {
    >                     this.vertex[i] = vertex[i];
    >                 }
    >         
    >                 // 初始化顶点访问状态数组
    >                 this.isVisited = new boolean[n];
    >         
    >                 // 初始化邻接矩阵
    >                 this.matrix = new int[n][n];
    >                 int num = 0; // 存储节点之间连通的数量
    >                 for (int i = 0; i < n; i++) {
    >                     for (int j = 0; j < n; j++) {
    >                         this.matrix[i][j] = matrix[i][j];
    >                         if (matrix[i][j] != 10000) {
    >                             num++;
    >                         }
    >                     }
    >                 }
    >                 
    >                 // 初始化边的数量
    >                 this.numOfEdges = num / 2; // 要特别注意，矩阵表示的是双向的连通，因此边的数量应该为节点之间连通数量的一半
    >             }
    >         }
    >         ```
    >         ```
    >             /**
    >              * 通过普利姆算法获取村庄的最小生成树
    >              * @param graph 村庄的无向网
    >              * @param v 开始节点
    >              */
    >             public void prim(Graph graph, int v) {
    >                 // 标记开始节点为已经访问过
    >                 graph.isVisited[v] = true;
    >         
    >                 int minWeight = 10000; // 存储最小边的权重，初始值一定要设置成一个很大的数
    >                 int v1 = 0; // 存储最小边的第一个顶点
    >                 int v2 = 0; // 存储最小边的第二个顶点
    >                 // 村庄的n个顶点经过prim算法计算后，一定会得到n-1条边
    >                 for (int i = 1; i < graph.vertex.size(); i++) {
    >         
    >                     for (int j = 0; j < graph.vertex.size(); j++) {
    >                         for (int k = 0; k < graph.vertex.size(); k++) {
    >                             if (graph.isVisited[j] && !graph.isVisited[k]) {
    >                                 // 如果j顶点被访问过，j顶点没被访问过，判断i-j边的权重
    >                                 if (graph.matrix[j][k] < minWeight) {
    >                                     // 如果i-j边权重小于上一个最小的边，则重新赋值minWeight，并且记录i,j
    >                                     minWeight = graph.edges[j][k];
    >                                     v1 = j;
    >                                     v2 = k;
    >                                 }
    >                             }
    >                         }
    >                     }
    >         
    >                     // 上述两个for循环计算出的v1,v2,minWeight一定是最小权重的边
    >                     // 打印权重最小的边和其权重
    >                     System.out.println(v1 + "-" + v2 + ":" + minWeight);
    >                     // 将v2标记为已经访问
    >                     graph.isVisited[v2] = true;
    >                     // 重新初始化minWeight = 10000好进入下次循环
    >                     minWeight = 10000;
    >                 }
    >             }
    >         ```
    >         
### 8. 克鲁斯卡尔算法(kruskal算法)
8. 克鲁斯卡尔算法（kruskal算法）
    > 1. 简介
    >
    >    克鲁斯卡尔算法也是求连通图的最小生成树的算法。是典型利用了贪心算法的思想。
    >    
    >    该算法的时间复杂度只和边的稠密程度有关，因此非常适合求点稠密但是边稀疏的连通网的最小生成树
    >    
    > 2. 应用场景
    >
    >    修路问题：假设有7个村庄（a, b, c, d, e, f, g），每个村庄的距离用边线表示，如何保证村庄都连通并且费用最小。村庄图片为D:\JavaZiLiao\DataStructurePhoto\DataStructure12.png
    >    
    > 3. 思想
    >
    >    此算法可以称为“加边法”，按照权值从小到大选择n-1条边，并且保证n-1条边不构成回路
    >    
    >    图解D:\JavaZiLiao\DataStructurePhoto\DataStructure13.png
    > 	
    > 4. 具体做法
    >
    >    1>按照权值大小将边排序
    >    
    >    2>将最小的边添加到最小生成树中，且保证每个边不连成回路。
    >    
    >     * 是否构成回路的判定（这个判定是整个算法的灵魂核心）
    >    
    >         * 节点的终点
    >         
    >            将所有节点按照从小到大的顺序排列好后，某个节点的终点就是：与该节点相连的最大节点
    >            
    >        * 是否构成回路的判定
    >        
    >           构成边的两个节点，他们如果终点为同一个节点，则两节点构成回路
    >    
    > 5. 步骤
    >
    >     1. 需要一个记录边信息的类
    >     
    >         ```
    >         /**
    >          * 存储边的信息
    >          */
    >         class EData {
    >             int start; // 存储边的一端
    >             int end; // 存储边的另一端
    >             int weight; // 存储边的权重
    >         
    >             public EData(int start, int end, int weight) {
    >                 this.start = start;
    >                 this.end = end;
    >                 this.weight = weight;
    >             }
    >         
    >             @Override
    >             public String toString() {
    >                 return "EData{" +
    >                         "start=" + start +
    >                         ", end=" + end +
    >                         ", weight=" + weight +
    >                         '}';
    >             }
    >         }
    >         ```
    >         
    >     2. 需要在原来的图的类中，加入两个属性，一个记录边的数量，一个记录每条边信息的数组。记录每条边信息的数组的长度由边的数量决定。
    >     
    >         ```
    >          // 存储边的数量，该属性可以在图初始化时，获取到
    >          public int numOfEdges;
    >         ```
    >         
    >         ```
    >         // 存储边信息的数组，在初始化时，根据获取到的边数量初始化数组长度，并且封装每条边的信息
    >         public EData[] edges;
    >         ```
    >         
    >         ```
    >         // 初始化存储边信息的数组
    >                 this.edges = new EData[numOfEdges];
    >                 int index = 0;
    >                 for (int i = 0; i < n; i++) {
    >                     for (int j = i + 1; j < n; j++) {
    >                         if (matrix[i][j] != 10000) {
    >                             EData eData = new EData(i, j, matrix[i][j]);
    >                             this.edges[index] = eData;
    >                             index++;
    >                         }
    >                     }
    >                 }
    >         ```
    >         
    >     3. 需要定义一个方法，用来对边根据权重进行排序
    >     
    >         ```
    >         /**
    >              * 根据传入的边的信息数组，进行排序
    >              * @param edges
    >              */
    >         public void sortEdges(EData[] edges) {
    >                 for (int i = 0; i < edges.length - 1; i++) {
    >                     for (int j = i + 1; j < edges.length; j++) {
    >                         if (edges[i].weight > edges[j].weight) {
    >                             EData temp = edges[i];
    >                             edges[i] = edges[j];
    >                             edges[j] = temp;
    >                         }
    >                     }
    >                 }
    >             }
    >         ```
    >         
    >     4. 需要定义一个int[]数组来存储每个节点对应的终点，该数组可以定义在图的类中，也可以需要的时候定义；需要一个方法，获取给定节点的终点的值 
    >     
    >         ```
    >         int[] ends = new int[graph.vertex.length];
    >         ```
    >         
    >         ```
    >         /**
    >              * 根据传入的节点坐标v，获取其对应的终点坐标
    >              * @param ends
    >              * @param v
    >              * @return
    >              */
    >             public int getEnds(int[] ends, int v) {
    >                 // 通过while循环找到v对应的终点，非常巧妙，可以通过画图等方法好好了解
    >                 while (ends[v] != 0) {
    >                     // 如果ends[v]进行过终点的赋值，将终点的值赋给v，返回
    >                     v = ends[v];
    >                 }
    >                 // 如果v节点没有被赋值，则其终点值就是其本身，返回v
    >                 return v;
    >             }
    >         ```
    >         
    >     5. 克鲁斯卡尔算法
    >     
    >         ```
    >         /**
    >              * 通过克鲁斯卡尔算法求村庄的最小生成树
    >              * @param graph
    >              */
    >             public void kruskal(Graph graph) {
    >                 // 最终边的数组
    >                 EData[] res = new EData[graph.vertex.length - 1];
    >                 int[] ends = new int[graph.vertex.length];
    >         
    >                 // 将边的集合按照权值从小到大排序
    >                 sortEdges(graph.edges);
    >                 EData[] edges = graph.edges;
    >         
    >                 // 遍历边的集合，判断边是否构成回路，如果没有则加入；否则不能加入
    >                 int index = 0; // 在最终入选的边的索引
    >                 for (int i = 0; i < edges.length; i++) {
    >                     // 获取i条边的第一个顶点
    >                     int p1 = edges[i].start;
    >                     // 获取i条边的第二个顶点
    >                     int p2 = edges[i].end;
    >                     // 获取p1的终点
    >                     int m = getEnds(ends, p1);
    >                     // 获取p2的终点
    >                     int n = getEnds(ends, p2);
    >                     // 判断p1和p2是否构成回路
    >                     if (m != n) {
    >                         // 不构成回路，设置m的终点是n
    >                         ends[m]= n;
    >                         // 将边加入集合
    >                         res[index] = edges[i];
    >                         index++;
    >                     }
    >                 }
    >         
    >                 for (EData i:
    >                      res) {
    >                     System.out.println(graph.vertex[i.start] + "->" + graph.vertex[i.end]);
    >                 }
    >             }
    >         ```
    > 6. 注意事项
    >
    >     1. **最关键的代码就是getEnds(int[] ends, int v)这个方法，该方法是克鲁斯卡尔算法的灵魂，用来获取给定顶点的终点** 。
    >     
    >     2. 要注意无向连通网的邻接矩阵是个对称矩阵，因此边的数量为节点的连通数量的一半。
    >     
### 9. 迪杰斯特拉算法(DijkstraAlgorithm)
9. 迪杰斯特拉算法(DijkstraAlgorithm)
    > 1. 邮差送信问题
    >
    >    见图D:\JavaZiLiao\DataStructurePhoto\DataStructure16.png
    >    
    > 2. 最短路径问题
    >
    >    迪杰斯特拉算法和弗洛伊德算法都是用来求解最短路径问题的。**注意：迪杰斯特拉算法和弗洛伊德算法都不可以应用于负权图，因为负权则找不到最短路径**
    >    
    > 3. 迪杰斯特拉算法
    >
    >     1. 简介
    >     
    >        经典的最短路径算法，用来计算一个节点到其他节点的最短路径。以起始点为中心向外扩散，直到找到终点为止。使用的是图的广度遍历思想+贪心算法的思想，广度优先遍历所有节点，贪心算法求最短路径
    >        
    >     2. 步骤
    >        
    >        1. **首先把\**起点置为已经访问\****
    >        
    >        2. **剩余的结点\**除起点外全部置为未访问\****
    >        
    >        3. **更新起点到其他节点的dist和所有节点的preNode**
    >        
    >        4. **找出起点和未访问的点中，\**最短路径\**的那一个点，假设是点X**
    >        
    >        5. **将x置为已经访问**
    >        
    >        6. **然后把结点X作为新的中间结点，与未访问的各个结点（用结点Y代替）进行计算，\**算出点Y和点X的距离\****
    >        
    >        7. **作比较：把\**起点->Y的路径与起点->X->Y作比较\**，若起点->X->Y 比起点直接到Y距离短，那么则更新dist和path，，把dist【y】的长度改成起点->X->Y（原先是起点直接到Y），把path【Y】改成x，证明Y的前驱结点是X**
    >        
    >        8. **然后再重复从U中找距离Y结点最短的点放入S中，重复这个1~6过程，直至U中没有结点为止**
    >        
    >        9. **最后想拿到最短路径的时候，可以通过dist找出距离的长度，可以通过path回溯找到路径**
    >        
    >     3. 代码
    >     
    >         ```
    >         /**
    >          * 访问顶点集合
    >          */
    >         class VisitedVertex {
    >         
    >             public int[] isVisited;// 算法过程中动态变化
    >             public int[] dis; // 出发节点到每个节点的最短距离
    >             public int[] preNode; // 每个节点的前驱节点下标集合。用来做最终回溯的
    >         
    >             /**
    >              * 构造器中传入节点数量，初始节点下标
    >              * @param length 节点数量
    >              * @param index 初始节点下标
    >              */
    >             public VisitedVertex(int length, int index) {
    >         
    >                 // 初始化每个节点的访问状态
    >                 this.isVisited = new int[length];
    >                 this.isVisited[index] = 1; // 将初始节点置为1，表示已经访问过
    >         
    >                 // 初始化每个节点到初始节点的距离
    >                 this.dis = new int[length];
    >                 for (int i = 0; i < length; i++) {
    >                     if (i == index) {
    >                         dis[i] = 0; // 初始节点和自己肯定是0
    >                     } else {
    >                         dis[i] = 65535; // 这时候算法未开始，初始节点和其他节点最小距离置为最大，这里用65535表示
    >                     }
    >                 }
    >         
    >                 // 初始化每个节点的前驱节点
    >                 this.preNode = new int[length];
    >             }
    >         
    >             // 需要的基础方法
    >             /**
    >              * 判断一个节点是否被访问过
    >              * @param index 待判断节点下标
    >              * @return
    >              */
    >             public boolean isVisited(int index) {
    >                 return isVisited[index] == 1;
    >             }
    >         
    >             /**
    >              * 更新下标为index的节点到出发节点的距离
    >              * @param index
    >              * @param len
    >              * @return
    >              */
    >             public void updateDis(int index, int len) {
    >                 dis[index] = len;
    >             }
    >         
    >             /**
    >              * 更新下标为index的节点的前驱节点为pre
    >              * @param index
    >              * @param pre
    >              */
    >             public void updatePre(int index, int pre) {
    >                 preNode[index] = pre;
    >             }
    >         
    >             /**
    >              * 返回index对应的节点到出发节点的距离
    >              * @param index
    >              * @return
    >              */
    >             public int getDis(int index) {
    >                 return dis[index];
    >             }
    >         
    >         
    >             
    >             /**
    >              * 在未访问节点中，选择下一个顶点作为出发节点，并将该节点置为已经访问
    >              * @return
    >              */
    >             public int updateNext() {
    >                 int min = 65535; // 用来跟没有被访问过的结点的路径做比较，存储最短路径的值，因此初始化为最大65535
    >                 int n = 0; // 用来记录最短路径的节点下标
    >                 for (int i = 0; i < isVisited.length; i++) {
    >                     if (isVisited[i] != 1) {
    >                         // 寻找没有被访问过的节点
    >                         if (dis[i] < min) {
    >                             // 寻找路径最短的节点
    >                             min = dis[i];
    >                             n = i;
    >                         }
    >                     }
    >                 }
    >                 // 经过上述循环，得到的n为未访问节点中，路径最短的节点的下标
    >                 isVisited[n] = 1;
    >                 return n;
    >             }
    >         
    >         }
    >         ```
    >         
    >         ```
    >         /**
    >              * 更新以index节点为出发节点，到周围节点的距离和周围节点的前驱结点
    >              * @param index
    >              * @param dij 图
    >              * @param vv 顶点集合
    >              */
    >             public static void update(int index, DijGraph dij, VisitedVertex vv) {
    >                 int len = 0; // 定义一个变量用来存储上一个出发节点到index的距离和index到周围节点的距离和
    >                 // 遍历index的周围节点
    >                 for (int i = 0; i < dij.matrix[index].length; i++) {
    >                     if (!vv.isVisited(i)) {
    >                         // 如果i节点没有被访问过
    >                         len = vv.getDis(index) + dij.matrix[index][i]; // 存储距离和
    >                         if (len < vv.getDis(i)) {
    >                             // 如果距离和更小
    >                             vv.updatePre(i, index); // 更新i的前驱
    >                             vv.updateDis(i, len); // 更新初始节点到i的最短路径
    >                         }
    >                     }
    >                 }
    >             }
    >         ```
    >         
    >         ```
    >         /**
    >              * 以index为下标的节点作为初始节点，使用迪杰斯特拉算法求最短路径
    >              * @param dij
    >              * @param index
    >              */
    >             public static void dijkstra(DijGraph dij, int index) {
    >                 VisitedVertex vv = new VisitedVertex(dij.vertex.length, index); // 以index作为出发节点创建节点集合
    >                 update(index, dij, vv); // 更新初始节点周围节点的距离和周围节点的前驱结点
    >                 for (int i = 1; i < vv.isVisited.length; i++) {
    >                     index = vv.updateNext(); // 更新出发节点
    >                     update(index, dij, vv); // 更新出发节点到周围节点的距离和周围节点的前驱结点
    >                 }
    >                 
    >                 // 验证算法
    >                 for (int i :
    >                         vv.dis) {
    >                     System.out.println(i);
    >                 }
    >             }
    >         ```
    >         
    >     4. 注意事项：
    >     
    >         1. 迪杰斯特拉算法的核心就是顶点集合类、顶点集合类中的寻找下一个出发节点方法以及更新方法
    >         2. **迪杰斯特拉算法中的preNode数组和前边的克鲁斯卡尔算法中的记录节点终点的ends数组以及KMP算法中的next数组作用原理相似，都是通过不停回溯来寻找要找的结果**
    >
### 10. 弗洛伊德算法(FloydAlgorithm)
10. 弗洛伊德算法(FloydAlgorithm)
    > 1. 简介
    >
    >    弗洛伊德算法也是一种用于求给定加权图中，顶点之间最短路径的算法。
    >    
    >    弗洛伊德算法计算图中每个顶点之间的最短路径；迪杰斯特拉算法计算图中某个顶点到其他顶点的最短路径。
    >    
    > 2. **迪杰斯特拉算法和弗洛伊德算法的区别**（面试常考）
    >
    >    迪杰斯特拉算法：通过一个**初始节点**，求出每次从**出发节点**到其他顶点的路径
    >    
    >    弗洛伊德算法：弗洛伊德偏重于多源最短路径的求解。该算法中，遍历每个节点，让每个节点都是**出发节点**（即中间节点），求出每一个顶点到其他顶点的最短路径。
    >    
    >    弗洛伊德能够求出任意两个节点的最短路径，当然迪杰斯特拉重复N次也能达到目标，两种方式的时间复杂度均为O(n^3)，但弗洛伊德形式上会更简易一些
    >    
    > 3. 思想
    >
    >    弗洛伊德算法维护着两张表，一张是图的邻接矩阵表示的节点距离表；一张是前驱关系表，该表显示了每个节点之间，需要连通则需要寻找的前驱。
    >    
    > 4. 步骤
    >
    >    1>从第一个节点开始遍历，假设每次遍历到的节点为vi，每次都让vi作为中继节点。
    >    
    >    2>寻找所有以vi为中继节点的边的两个节点vl和vk，则lk边的最短路径为：vl、vk最短路径和vl、vi最短路径+vi、vk最短路径作对比，取最小的，并重新设置前驱节点。
    >    
    >    3>遍历所有节点，结束后，邻接矩阵每个点的值就为两节点的最短路径。
    >
    > 6. 代码
    >
    >     ```
    >     // 在图的类中，需要加入一个属性
    >     public int[][] preNode; // 存放每个节点的前驱节点的下标
    >     // 在构造其中为其初始化
    >     // 初始化前驱结点的表
    >             this.preNode = new int[vertex.length][vertex.length];
    >             for (int i = 0; i < vertex.length; i++) {
    >                 for (int j = 0; j < vertex.length; j++) {
    >                     preNode[i][j] = i; // 初始时，每个边的前驱结点都是第一个节点
    >                 }
    >             }
    >     ```
    >     
    >     ```
    >     /**
    >          * 根据传入的图，用弗洛伊德算法求每个节点直接的最短路径
    >          * @param graph
    >          */
    >         public static void floyd(FloydGraph graph) {
    >             int len = 0; // 记录距离
    >             for (int i = 0; i < graph.vertex.length; i++) {
    >     
    >                 // 寻找以i为中继的两个节点
    >                 for (int j = 0; j < graph.vertex.length; j++) {
    >                     for (int k = 0; k < graph.vertex.length; k++) {
    >                         // 找到两节点j,k
    >                        len = graph.matrix[j][i] + graph.matrix[i][k]; // 求出从j出发，经过i，到达k的长度
    >                         if (len < graph.matrix[j][k]) {
    >                             // 重置matrix
    >                             graph.matrix[j][k] = len;
    >                             graph.matrix[k][j] = len;
    >                             // 重置前驱表
    >                             graph.preNode[j][k] = i;
    >                             graph.preNode[k][j] = i;
    >                         }
    >                     }
    >                 }
    >     
    >             }
    >     
    >             for (int i = 0; i < graph.vertex.length; i++) {
    >                 for (int j = i + 1; j < graph.vertex.length; j++) {
    >                     System.out.printf("%d\t", graph.matrix[i][j]);
    >                 }
    >                 System.out.println();
    >             }
    >         }
    >     ```
    >     
### 11. 骑士周游问题(又叫跳马问题)(RiderQuestion)
11. 骑士周游问题(跳马问题)(RiderQuestion)
    > 1. 题目介绍
    >
    >    在一个国际象棋(8*8)棋盘上，假设随机一个格子放入一个马进行移动，马走日字。要求：任意格子上的马在每个格子仅允许进入一次的情况下，走遍棋盘上所有的64个格子。
    >    
    > 2. 应用的算法
    >
    >    马踏棋盘问题，其实就是图的深度优先搜索(DFS)的应用。
    >    
    > 3. 单纯使用深度优先搜索（回溯的思想）解决
    >
    >     1. 代码
    >     
    >         ```
    >         **
    >          * 跳马问题棋盘图
    >          */
    >         class RiderGraph {
    >             int[][] matrix;
    >             // 记录格子是否被访问过，不用使用二维数组，将二维数组的思维转化为一维数组即可
    >             boolean[] isVisited;
    >         
    >             public RiderGraph(int col) {
    >                 this.matrix = new int[col][col];
    >                 this.isVisited = new boolean[col * col];
    >             }
    >         }
    >         ```
    >     
    >         ```
    >        代码过多，参见RiderQuestion类
    >        ```
    >       
    >    2. 注意事项 
    >    
    >        1. **在判断棋子下一次能走的位置方法getNext()中，每次add添加都必须new个point，point传参p，表示复制p对象。不能直接add添加p，否则同一个引用中的值会覆盖**
    >        
    >        2. Point是个Java自带的表示一个点的类
    >        
    >        3. 记录格子是否被访问过，不用使用二维数组，将二维数组的思维转化为一维数组即可
    >        
    >       4. **在二维图像中，y轴坐标表示矩阵的行，x轴坐标表示矩阵的列，这个要牢记**
    >       
    >       5. **不能只靠step来判断是否走完整个棋盘，```step<X*Y```有两种状态：1.没有走完棋盘 2.处于回溯过程中；因此必须还要靠一个finished属性来判断是否走完整个棋盘。没有走完整个棋盘且```step<x*y```才重新赋值 **
    >    
    > 4. 使用贪心算法优化
    >
    >     1. 介绍
    >     
    >        单纯使用DFS来解决该问题，使用的回溯次数过多，因此效率过低，不实用。使用贪心算法将DFS优化。
    >        
    >     2. 优化思路
    >     
    >        效率的低下主要是因为回溯次数的原因。如果我们能让棋子的行走策略动态化，使其每次选择的行走策略都是以后可能回溯时，回溯次数最少的策略，这就会大大优化我们的效率。使用贪心算法进行策略的动态化。
    >        
    >     3. 使用贪心算法的步骤
    >     
    >        1>如果一个格子的下次行走的可选位置很多，则说明该格子如果进行回溯的话，该格子回溯次数会很多。我们尽量在选择下次行走位置时，选择下下次行走位置最少的格子为下次行走位置。因此，需要对下次行走位置的集合，进行集合元素按照元素的下次行走位置数量的非递减排序。
    >        
    >        2>因此，我们在获取到可以走的下一个位置的集合points，对该集合的元素point，按照point的下次行走可选位置数量进行非递减排序。排完序后，再进行操作
    >        
    >         * **非递减排序和递增排序区别：递增排序必须保证元素从小到大；非递减排序从小到大排序过程中，允许相等元素**
    >        
    >    4. 添加的代码
    >    
    >        ```
    >        /**
    >             * 对下一个位置的集合，按照元素的下一个位置数量非递减排序
    >             * @param points
    >             */
    >            public static void sort(ArrayList<Point> points) {
    >                points.sort(new Comparator<Point>() {
    >                    @Override
    >                    public int compare(Point o1, Point o2) {
    >                        // 获取o1下一个位置集合大小
    >                        int num1 = getNext(o1).size();
    >                        // 获取o2下一个位置集合大小
    >                        int num2 = getNext(o2).size();
    >                        // 非递减排序
    >                        if (num1 < num2) {
    >                            return -1;
    >                        } else if (num1 == num2) {
    >                            return 0;
    >                        } else {
    >                            return 1;
    >                        }
    >                    }
    >                });
    >            }
    >        ```
    >        
    >        ```
    >        // 获取当前点可以走的下一个位置的集合
    >        ArrayList<Point> points = getNext(new Point(column, row));
    >        
    >        /*
    >        获取到当前点可走的下一个位置集合后，贪心算法优化
    >        */
    >        sort(points);
    >        
    >        // 遍历points
    >        while (!points.isEmpty()) {
    >        ```