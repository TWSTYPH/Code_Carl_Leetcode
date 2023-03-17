# Code_Carl_Leetcode

# Day1:二分法查找、移除元素

## 二分查找

1、给定一个有序的数组

2、数组中无重复元素

3、确定写程序的时候数组引用是什么区间；一般分为左闭右开、左闭右闭

case1：左闭右开

```c++
class Solution {
public:
    int search(vector<int>& nums, int target) {
        int left = 0;
        int right = nums.size();//此时选择作为右边开区间
        while (left < right) {//表示不用等到这两个数相等的时候即可，相等之后没有意义
            int middle = left + (right - left)/2;
            if (nums[middle] < target) {
                left = middle + 1;//左边是闭区间，此时num[middle]已经不符合要求了，所以将其舍去直接去下一个
            }
            else if (nums[middle] > target){
                right = middle;  //此时区间是右边开区间，即这个元素无法得到
            }
            else if (nums[middle] == target){
                return middle;
            }
            
        }
        return -1;
    }
};
```

case2:左闭右闭

```c++
class Solution {
public:
    int search(vector<int>& nums, int target) {
    int left = 0;
    int right = nums.size() - 1;//定义的是闭区间

    while(left <= right){//由于定义的是闭区间，所以当left=right 这个区间任然合法
    int middle = left + (right - left) / 2;//防止数据溢出
    if(nums[middle] > target){
    right = middle - 1;//此时middle这个值已经不符合我们要寻找的目标，所以不包括
    }
    else if(nums[middle] < target){
    left = middle + 1;
    }
    else if(nums[middle] = target)
    return middle;
    }
    return -1;
    

    }
};
```

注意：

1、在进行二分法程序编写过程中一定注意左右区间的情况，看清是否闭合

2、二分法进行查找的时候while循环过程中一定要注意循环的终止条件，如果区间作为双闭合，即可以 left <= right  因为此时在他们两个相等的条件下仍然有意义；

如果右边是开区间的话，循环终止条件应该是 left < right,如果相等的话不能最后数据没有意义，超出了数组的范围。

3、在进行left、right更新的过程中，一定要根据以上两种情况进行分析，确定是否是middle+-1。

## 移除元素

在进行数组元素移除的过程中只能够进行一个覆盖另一个，不能够单独把其中一个元素进行移除。

针对数组元素移除的常规解法：

首先构造两个循环，一个用于遍历所有数组的元素，另外一个用于数组的更新迭代。

```c++
class Solution {
public:
    int removeElement(vector<int>& nums, int val) {
        int size = nums.size();//计算出原始数组的大小
        for (int i = 0; i < size; i++){//遍历所有数组元素
            if (nums[i] == val){//寻找有没有想要移除元素
                for (int j = i+1; j < size; j++){//此循环用于将原来数组进行更新迭代，其中j = i+1; 表示将要移除元素的下一个元素
                    nums[j - 1] = nums[j];//把将要移除的元素用下一个元素覆盖，紧接着在其之后的所有元素都要进行覆盖，此时数组的长度减少一个
                }
                i--;//表示这个元素要重新进行检查，因为这个位置上已经是一个新的元素
                size--;//该语句包含在if中，每运行一次数组总长度减一
            }
        }
return size;
    }
};
```

此方法可以实现求解，占用资源过多，时间复杂度为O（n*n）

### 双指针法

双指针法指的是进行数组元素移除的过程中定义两个指针，其中快的指针用于寻找新数组中的元素；慢指针进行新数组中下标的更新

```c++
class Solution {
public:
    int removeElement(vector<int>& nums, int val) {
        int slowindex = 0;//定义慢指针，用于指向新数组下标
        for (int fastindex = 0; fastindex < nums.size(); fastindex++){//定义快指针，在这个循环中主要用于寻找新数组中的元素
            if (nums[fastindex] != val )//如果不想等的话就是新数组中的元素
            nums[slowindex++] = nums[fastindex];//把这个元素的值赋值给新数组，同时慢指针进行新数组元素下标的更新
        }
        return slowindex;
    }
};
```

通过双指针法可以更迅速地找到我们想要的新数组中的元素，有效的节约了时间复杂度

注意一定搞清楚快慢指针的含义。

# DAY2 有序数组的平方

## 有序数组的平方

在给定一个有序数组之后对其进行平方后然后整体排序，对于一个数组来说，他可能是正数或者负数，因此在对其进行平方之后最大的元素只可能出现在数组的两端，不可能出现在中间部分。

双指针法可以有效地进行应用，定义一个指针指向原始数组的初始位置，定义另外一个指针指向其终止位置，这个时候两个指向的元素进行比较大小，把较大的元素放入在新的数组中的最后位置。

具体代码如下所示

```c++
class Solution {
public:
    vector<int> sortedSquares(vector<int>& nums) {
        int k = nums.size() - 1;//定义一个指针序列指向新数组
        vector<int> result(nums.size(),0);//定义一个新数组
        for(int i = 0, j = k; i <= j; ){//该循环中要完成原始数据平方后的比较，i和j分别作为两个指针指向原始数组的起始位置和终止位置（注意，这个循环的使用只用到了两个后便必须加冒号）
            if (nums[i]*nums[i] < nums[j]*nums[j]){
                result[k--] = nums[j]*nums[j];
                j--;
            }
            else {
                result[k--] = nums[i]*nums[i];
                i++; 
            }
        }
        return result;
    }
};
```

## 长度最小的子数组

本题在求解的过程中采用原始的方法比较困难，在此选用滑动窗口法，即为定义两个指针，一个指向起始位置一个指向终止位置。在这两个位置之间的数据为我们想要的数据。

其中注意：for循环中的这个指针选用终止位置指针

​					在进入for循环之后sum满足条件之后起始位置指针要进行移动，注意这个过程中要放在一个while循环中，不能够放在一个if判断中进行计算。因为要一直减去起始指针指向的元素，直到符合条件。

```c++
class Solution {
public:
    int minSubArrayLen(int target, vector<int>& nums) {
        int result = nums.size() + 1;//定义一个超出数组长度范围的数据，作为其初始长度大小（已开始直接选取数组长度作为初始值的时候，在最短子数组正好是原来的数组长度时，返回结果有误）
        int length = 0;//选择取得数组结果长度
        int sum = 0;
        int i = 0;//起始指针，指向滑动窗口的起始处
        for (int j =0; j < nums.size(); j++){//j作为终点指针，始终指向滑动窗口的末尾处，在这个循环过程中直接把末尾指针一直循环到原来数组最后停止
            sum = sum + nums[j];//计算出来末尾指针所经历过的所有元素值得和
            while(sum >= target){//通过一个while循环计算出满足要求的长度
                length = j - i + 1;//计算符合条件的数组长度
                sum = sum - nums[i];//sum不断减去滑动窗口的前端数值
                result = result > length ? length : result;//选取最小的长度
                i++;//此时窗口的前端不断的向前滑动
            }
        }
        return result = result == (nums.size() + 1) ? 0 : result;

    }
}; 
```

## 螺旋矩阵

首先要明白什么叫做螺旋矩阵，就是一个二维数组，但是他的排列方式按照螺旋的方式进行排列，其中是一个矩阵的形式

注意：在处理这类问题的时候一定要搞清楚边界点的处理问题，即为每次循环赋值一条边的时候我们要始终保持四条边的处理方式一致。

**循环不变量：**在这个问题中保证每条边赋值的过程中都是左闭右开的形式，即为，每条边的两个端点在每次处理过程中，只处理这条边的起始位置，不用处理他的末尾位置，因为只个时候末尾未知的这一个元素可以作为下一个边的起始位置。

处理此类问题时候转几圈一定要分清楚：n/2

同时，如果为奇数，对于循环末尾的最后一个元素要单独处理

```c++
class Solution {
public:
    vector<vector<int>> generateMatrix(int n) {
        vector<vector<int>> nums(n, vector<int>(n));
        int startx = 0;
        int starty = 0;
        int loop = n/2;
        int offset = 1;
        int count = 1;
        int mid = n/2;
        int i, j;//此处定义作为全局变量
        while(loop--){
            i = startx;
            j = starty;
            for(j = starty; j < n - offset; j++){
                nums[startx][j] = count++;
            }
            for(i = startx; i < n-offset; i++){
                nums[i][j] = count++;
            }
            for(; j > starty; j--){
                nums[i][j] = count++;
            }
            for(; i > startx; i--){
                nums[i][j] = count++;
            }
            startx++;//每循环一轮，起始坐标和边界值都要加一
            starty++;
            offset++;
        }
        if (n % 2 == 1)//这个判断不能放在while循环内部，因为如果n==1，直接进不去这个循环，所以没办法对其赋值
        nums[mid][mid] = count;
        return nums;

    }
};
```

# 数组总结：

## 1、二分法查找

主要针对一个有序的数组并且没有重复元素。

在进行写的时候一定要注意该数组区间的定义，看清楚这个区间是**左闭右开**还是**左闭右闭**

对于这两种区间的差别我们要采用的while循环的终止条件也有不同，分别对应**left <right和left<=right**

同时在进行left和right更新的过程中，一定注意是等于middle+1或者middle-1或者middle。

## 2、数组元素的移除

在这个过程中主要数组元素是连续的

学习到了**双指针法**其中定义一个快指针和一个慢指针，快指针指向原来数组中的元素，用于遍历所有的元素；慢指针用于指向移除目标元素后的新数组。

双指针法很大程度减少了程序运行的时间复杂度。

## 3、有序数组的平方

主要利用双指针法思想，因为本身该数组为有序数组，所以平方后组成新数组的最大值以定位于原来数组的两端，所以本题目思想中定义两指针分别指向原来数组的首尾。

注意：在进行这两个指针指向的数组元素平方后比较大下之后，较大的那一个一定位于新数组的末端。

## 4、长度最小的子数组

**注意**：本例中学习到了一个重要思想，**滑动窗口法**

滑动窗口类似于双指针法，注意在for**循环的过程中这个用于循环的指针一定是指向窗口末端的指针**

在得到sum满足大小之后要在while循环中进行迭代，**此时窗口的前项指针**开始逐渐向前移动，一直到找到最小的长度数组。

## 5、螺旋矩阵

**这个例子中主要学习到了循环不变量**

# DAY3 移除链表、设计链表、反转链表

## 链表理论基础

链表是一种通过指针串联在一起的线性结构，每一个节点由两部分组成，**一个是数据区域**，**另外一个是指针区域**，用于存放下一个节点的指针。最后一个节点的指针指向空指针NULL。

**链表的入口节点成为链表的头结点**head。

**链表主要分为**：单链表、双链表（双链表中主要有每个节点有一个数值区域，两个指针区域，分别指向前一个指针和后一个指针）、循环链表，链表的首尾相连。**约瑟夫环问题**

链表的存储方式有别于数组，它的存储并不连续，而是通过每个节点的指针进行指引到下一个节点。

链表的定义：

```c++
struct ListNode{
    int val;//节点上存储的数值元素
    ListNode * next;//定义指向下一个节点的指针
}
//默认构造函数
//实例化
ListNode* head = new ListNode();
head -> val = 1;
```

## 移除链表元素

针对该类问题首先如果要移除链表中的一个节点元素的话直接将他的上一个节点的next指针指向该节点的下一个。cur ->next = cur ->next -->next;同时释放掉该节点所占用的内存。

**注意**：在进行移除过程中head节点如果他的数值是要移除的节点，我们并不能直接找到他的上一个节点指针地址。

**引入虚拟头指针法**：通过定义一个虚拟头节点，这个虚拟头结点的next指针指向原本链表的头结点，此时如果要移除头结点直接将虚拟头节点的next更改赋值。

```c++
dummyHead -> next = head;//定义虚拟头指针
cur = dummyHead;
```

```c++
class Solution {
public:
    ListNode* removeElements(ListNode* head, int val) {
        ListNode* dummyHead = new ListNode(0);//定义虚拟头指针
        dummyHead->next = head;//虚拟头指针的next指针赋值为头结点
        ListNode* cur = dummyHead;
        while(cur->next != NULL){
            if (cur->next->val == val){
                ListNode* temp = cur -> next;
                cur ->next = cur ->next -> next;
                delete temp;
            }
            else cur = cur -> next;
        }
        head = dummyHead -> next;
        delete dummyHead;
        return head;

    }
};
```

