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

注意一定搞清楚快慢指针的含义
