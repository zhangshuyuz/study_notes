# LeetCode算法刷题
## day01
### 1. 题目一
1. 已知一个搜索二叉树(也叫二叉排序树)后序遍历后的数组posArr，请根据posArr，重建出整棵树，返回新建树的头结点。

   解题思路：
   
    1. 解决方法1：
   
       ```
       /**
            * 第一个解决方法。没有优化
            * @return
            */
           public static TreeNode processToBST1(int[] posArr) {
               return process1(0, posArr.length - 1, posArr);
           }
       
           /**
            * 递归还原二叉排序树，最终返回二叉排序树的根节点
            * @param n 待重构的数组左指针
            * @param m 待重构的数组右指针
            * @param posArr
            * @return
            */
           public static TreeNode process1(int n, int m, int[] posArr) {
       
               /* 递归结束条件*/
               if (n > m) {
                   return null;
               }
       
               /* 数组的n-1号索引一定是重构的根节点，重构根节点 */
               TreeNode head = new TreeNode();
               head.node = posArr[m];
       
               // 如果左右指针相等，直接返回head。同时还可以解决数组长度为一的问题
               if (n == m) {
                   return head;
               }
       
       
               /* 循环重构左树 */
               // 找到左树的范围
       
               // 最为重要的一点，这个是为了防止没有左子树的情况，即左边没有小于posArr[m]的值。
               // 一旦没有左子树，则temp=n-1进入section1(n, temp, posArr);，直接返回null
               int temp = n - 1;
       
               for (int i = n; i < m; i++) {
                   if (posArr[i] < posArr[m]) {
                       temp = i;
                   }
               }
               // 构建左树
               // 拼接
               head.left = process1(n, temp, posArr);
       
               /* 递归重构右树 */
               // 根据找到的左树范围，推出右树范围。构建右树
               head.right = process1(temp + 1, m - 1, posArr);
       
               return head;
           }
       ```
       
        * 时间复杂度：O(n^2)。非最优解
   
    2. 解决方法2：

        * 解题思路：
        
           每次寻找左子树的根节点，其实不必使用遍历的方式，我们可以使用二分查找的方式。
           
           因为使用二分查找后，每次查找结果无非两种，一种小于posArr[R]，一种大于等于posArr[R]。
           
           根据搜索二叉树+后序遍历的特性，小于则该值可能是目标值，并且左边的值都是小于posArr[R]的，不用看左边了；大于等于则右边的值都是大于posArr[R]的，右边不用看了。这样就可以一直二分下去直到找到为止。
           
        * 解决方案2：
        
            ```
             /**
                 * 第二个解决方法。二分查找优化
                 * @return
                 */
             public static TreeNode processToBST2(int[] posArr) {
                    return process2(0, posArr.length - 1, posArr);
                }
             
             /**
                 * 第二种方案。使用二分查找优化
                 * @param n
                 * @param m
                 * @param posArr
                 * @return
                 */
                public static TreeNode process2(int n, int m, int[] posArr) {
                    /* 递归结束条件*/
                    if (n > m) {
                        return null;
                    }
            
                    /* 数组的n-1号索引一定是重构的根节点，重构根节点 */
                    TreeNode head = new TreeNode();
                    head.node = posArr[m];
            
                    // 如果左右指针相等，直接返回head。同时还可以解决数组长度为一的问题
                    if (n == m) {
                        return head;
                    }
            
            
                    /* 循环重构左树 */
                    // 找到左树的范围
            
                    // 最为重要的一点，这个是为了防止没有左子树的情况，即左边没有小于posArr[m]的值。
                    // 一旦没有左子树，则temp=n-1进入section1(n, temp, posArr);，直接返回null
                    int temp = n - 1;
            
                    // 使用二分查找找到左子树范围
                    int l = n;
                    int r = m - 1;
                    while (l <= r) {
                        int mid = l + ((r - l) >> 1); // 等效于mid = n + (m - n) / 2。但是移位比除法底层要快。因此用移位。
                        if (posArr[mid] < posArr[m]) {
                            temp = mid;
                            l = mid + 1;
                        } else {
                            r = mid - 1;
                        }
                    }
            
                    // 构建左树
                    // 拼接
                    head.left = process2(n, temp, posArr);
            
                    /* 递归重构右树 */
                    // 根据找到的左树范围，推出右树范围。构建右树
                    head.right = process2(temp + 1, m - 1, posArr);
            
                    return head;
                }
            ```
            
        * 时间复杂度：O(nlogn)
        
        * 总结：二分查找不光能用于有顺序的集合进行查找，而且可以用于查找时左右任意一侧可以抛开不看的情况。
        
### 2. 题目二
2. 给定长度为m的字符串aim，以及一个长度为n的字符串str，问：能否在str中找到一个长度为m的连续子串，使得这个子串刚好由aim的m个字符组成，顺序无所谓，返回任意一个满足条件的一个子串的起始位置，未找到返回-1.

    * 解决方案1：
    
        * 思路：纯粹暴力匹配。没分，但是可以帮助理解
        
        * 时间复杂段：O(n^3*logn)
        
    * 解决方案2：
        * 思路：统计str中，所有长度为m的子串中，字符频率和aim中字符频率，以此来判断。稍微优化过的暴力破解。没分，但是比第一种要好一点点。
    
        * 时间复杂度：O(n*m)
        
    * 解决方案3：
        * 思路：
           通过aim建立一个字符还款表；建立一个长度为m的还款队列；建立一个统计无效还款的变量；
           先将str中第一个为m的子串放入还款队列，记录此时还款表情况，如果有任意还款字符变成负数，无效还款数+1。队列向后移动，记录每次进出队列的字符对还款表的修改情况和无效还款变化情况，直到遍历完整个str或者还款完毕无效还款数为0
        * 代码
            ```
            /**
                 * 第三种最好的方法
                 */
                public static int process3(String aim, String str) {
            
                    // 判断长度
                    if (aim.length() > str.length()) {
                        return -1;
                    }
            
                    // 将两个String转成char[]
                    char[] aimArr = aim.toCharArray();
                    char[] strArr = str.toCharArray();
            
                    // 建立还款表，即统计aim中每个字符的频率
                    int[] pay = new int[256];
                    for (int i = 0; i < aim.length(); i++) {
                        pay[ aimArr[i] ]++;
                    }
            
                    // 建立无效还款数量inValidTimes
                    int inValidTimes = 0;
            
                    // 初始化指针和队列长度
                    int m = 0;
                    int n = aim.length();
            
                    // 找到str的第一个n长度的子串
                    while (m < n) {
                        if (pay[ strArr[m] ]-- == 0) {
                            // 无效还款++
                            inValidTimes++;
                        }
            
                        m++;
                    }
            
                    // 继续遍历str
                    while (m < str.length()) {
            
                        if (inValidTimes == 0) {
                            // 还款完毕
                            return m - n;
                        }
            
                        if (pay[ strArr[m] ]-- <= 0) {
                            // 无效还款++
                            inValidTimes++;
                        }
            
                        if (pay[ strArr[m - n] ]++ < 0) {
                            // 本来就不需要，拿回去相当于有效拿钱，无效还款--】
                            inValidTimes--;
                        }
            
                        m++;
                    }
            
                    // str中最后一个为n的子串判断，还款完毕还是无法还款
                    return inValidTimes == 0 ? m - n : -1;
            
                }
            ```
        * 时间复杂度：O(n)
## day02
### 1. 题目一
1. 输入一个int类型的值N，构造一个长度为N的数组arr并返回。要求：对于任意的```i<k<j``, 满足arr[i] + arr[j] != arr[k] * 2

   * 解题思路：

     对于数组中的每个数，进行奇变换或者偶变换

     对于arr[i] + arr[j] != arr[k] * 2，它对于2arr[i] + 1 + 2arr[j] + 1 != 2(2arr[k] + 1)也完全成立

     因此我们可以认为，满足要求的arr，我们完全可以替换其中的所有数为，它们对应的奇数；根据上述的等式，替换完的数组仍然满足题目要求

     偶变换也同理

     因此，假设有一个满足题意的长度为N的数组，它全由奇数组成，同理还有一个全有偶数组成；假设将奇数的排在左边，偶数的排在右边，这个2N的新数组一定也满足题意，因为左边满足，右边满足，奇数+偶数一定等于奇数，中间也满足。

     至此，我们将N的数组一分为2，前一半都由索引对应的奇数组成，后边都由索引对应的偶数组成，拼接起来的N就是题意要求的N

  * 代码实现：
      ```
      /**
           * 传入num，返回满足要求的数组
           * @param num
           * @return
           */
          public static int[] process(int num) {
      
              if (num == 1) {
                  return new int[] { 1 };
              }
      
              // 初始化目标数组
              int[] target = new int[num];
      
              // 获取中间值。(num + 1) / 2，让num / 2做到了向上取整，保证2 * half > num即target数组一定能全部装填
              int half = (num + 1) / 2;
      
              // 获取长度为(num + 1) / 2的达标数组
              int[] base = process(half);
      
              // 奇变换
              for (int i = 0; i < half; i++) {
                  target[i] = 2 * base[i] + 1;
              }
      
              // 偶变换
              int temp = 0;
              for (int j = half; j < num; j++) {
                  target[j] = 2 * base[temp];
                  temp++;
              }
      
              return target;
          }
      ```
      * 时间复杂度：O(n)
  * 检验函数：
      ```
      /**
           * 检验函数
           */
          public static boolean isValid(int[] arr) {
              int N = arr.length;
              for (int i = 0; i < N; i++) {
                  for (int k = i + 1; k < N; k++) {
                      for (int j = k + 1; j < N; j++) {
                          if (arr[i] + arr[j] == arr[k]) {
                              return false;
                          }
                      }
                  }
              }
              return true;
          }
      ```
### 2. 题目二
2. 长度为N的数组arr，一定可以组成N^2个数值对。例如arr=[3, 1, 2]，数值对为(3,3)(3,1)(3,2)(1,3)(1,1)(1,2)(2,3)(2,1)(2,2)，也就是任意两个树都有数值对，而且自己和自己也算数值对。数值对如何排序？规定，第一维数据从小到大，第二维数据也从小到大，因此上述的变为(1,1)(1,2)(1,3)(2,1)(2,2)(2,3)(3,1)(3,2)(3,3)。要求：给定一个数组arr和整数k，返回第k小的数值对
   * 解题方案1：纯暴力破解，即将数组的全部数值对排好序，然后选择第k个
     
       * 时间复杂度：O(n^2 * logn^2)
       
   * 解题方案2：
       * 思路：先将arr排序。对于k来说，排序后的数值对，第一维数值为arr[(k - 1) / arr.length]。第二维数值如何求得？比第一维小的数假设有n个，它们产生的数值对数量m = n * arr.length，a = k - m。我们求出和第一维数字相同数字的个数，假设有i个。第二维数字arr[ a - 1 / i ]
       * 代码：
       
           ```
           /**
            * 长度为N的数组arr，一定可以组成N^2个数值对。例如arr=[3, 1, 2]，数值对为(3,3)(3,1)(3,2)(1,3)(1,1)(1,2)(2,3)(2,1)(2,2)，也就是任意两个树都有数值对，而且自己和自己也算数值对。数值对如何排序？
            * 规定，第一维数据从小到大，第二维数据也从小到大，因此上述的变为(1,1)(1,2)(1,3)(2,1)(2,2)(2,3)(3,1)(3,2)(3,3)。
            * 要求：给定一个数组arr和整数k，返回第k小的数值对
            */
           public class Section2 {
               public static void main(String[] args) {
                   int[] arr = {1, 3, 1, 3, 3, 3, 4, 4};
                   List<Integer> integers = process2(arr, 5);
                   System.out.println(integers.toString());
               }
           
               /**
                * 排序arr后求数值对
                * @param arr
                * @param k
                * @return
                */
               public static List<Integer> process2(int[] arr, int k) {
           
                   if (k > arr.length * arr.length) {
                       return null;
                   }
           
                   // 排序arr。使用快速排序
                   quick(arr, 0, arr.length - 1);
           
                   // 获取第一维索引
                   int firstNum = arr[ (k - 1) / arr.length ];
           
                   // 比firstNum小的数
                   int less = 0;
                   // 和firstNum相等的数
                   int equal = 0;
                   for (int i = 0;  i < arr.length && arr[i] <= firstNum; i++) {
                       if (arr[i] < firstNum) {
                           less++;
                       } else {
                           equal++;
                       }
                   }
           
                   // 第二维数获得
                   int rest = k - (less * arr.length);
                   int secondNum = arr[ (rest - 1) / equal ];
           
                   List<Integer> result = new ArrayList<>();
                   result.add(firstNum);
                   result.add(secondNum);
                   return result;
               }
           
               /**
                * 快速排序arr
                * @param arr
                * @return
                */
               public static void quick(int[] arr, int left, int right) {
                   int l = left;
                   int r = right;
                   int temp = 0;
                   int pivot = arr[ left + (right - left) / 2];
           
                   while (l < r) {
                       while (arr[l] < pivot) {
                           l++;
                       }
                       while (arr[r] > pivot) {
                           r--;
                       }
           
                       if (l >= r) {
                           break;
                       } else {
                           temp = arr[r];
                           arr[r] = arr[l];
                           arr[l] = temp;
                       }
           
                       if (arr[l] == pivot) {
                           l++;
                       }
                       if (arr[r] == right) {
                           r--;
                       }
                   }
           
                   if (l == r) {
                       r--;
                       l++;
                   }
           
                   if (left < r) {
                       quick(arr, left, r);
                   }
           
                   if (right > l) {
                       quick(arr, l, right);
                   }
           
               }
           }
           ```
           
       * 时间复杂度：只和排序算法的时间复杂度有关。这里是O(n*logn)
       
   * 解题方案3：
   
       * 思路：因为第一维和第二维数，都是只需要找到在无序数组中第x小的数。因此假设我们有个方法，方法传入arr，x，就可以得到该arr中第x小的数。已知，bfprt算法可以做到在arr中找到第x小的数
       * 代码：
       * 时间复杂度：取决于bfprt算法复杂度，bfprt算法复杂度为O(n)
### 3. 算法面试题两大类：主业务和主技巧
3. 算法面试题两大类：主业务和主技巧
    > 1. 主业务的题：不涉及到数据结构，纯粹看数学能力和聪明程度
    > 
    > 2. 主技巧的题：通过别的算法，得到或者启发得到题目最优解
    > 
    > 3. 笔试阶段，主业务 / 主技巧 = 6 / 4，因为要淘汰掉很多人；面试阶段，主业务 / 主技巧 = 3 / 7，因为要考察数据结构和一些经典算法
    > 
    > 4. 刷LeetCode，一定要先做主技巧的题目，因为要总结优秀的算法和复习数据结构。如何寻找主技巧的题？点比踩多很多的题
