# DAY6:哈希表基础、有效字母异位词

![扫描全能王 2023-03-20 21 55_1](https://user-images.githubusercontent.com/103561103/226363357-89f2fef1-8dc2-49eb-a6bc-99f33cd0cd98.jpg)

## 有效字母异位词



```c++
class Solution {
public:
    bool isAnagram(string s, string t) {
        int hash[26] = {0};//定义初始化一个hash表
        for(int i = 0; i < s.size(); i++){//第一个循环记录第一个字符串出现的字母频率
            hash[s[i] - 'a']++;//注意此时hash表中字母顺序按照正常顺序排列，同时每出现一次该字母位置上边就加一
        }
        for(int j = 0; j < t.size(); j++){
            hash[t[j] - 'a']--;
        }
        for (int i = 0; i < 26; i++){
            if(hash[i] != 0)
            return false;
        }
        return true;
    }
};
```

## 求两个数组的交集
![扫描全能王 2023-03-20 21 55_2](https://user-images.githubusercontent.com/103561103/226363530-7680569b-2b44-40cd-ba13-d0ba6cccc756.jpg)



```c++
class Solution {
public:
    vector<int> intersection(vector<int>& nums1, vector<int>& nums2) {
        unordered_set<int> result;
        unordered_set<int> num1_set(nums1.begin(), nums1.end());//定义把nums转换为undered_set哈希表
        for (int num : nums2){
            if(num1_set.find(num) != num1_set.end()){//该容器在查找时如果找到了返回指向该值的迭代器，没找到则返回只想容器末尾迭代器的
                result.insert(num);
            }
        }
        return vector<int>(result.begin(), result.end());
    }
};
```

## 快乐数



## 两数之和

![扫描全能王 2023-03-20 21 55_3](https://user-images.githubusercontent.com/103561103/226363585-4e175db6-87c5-46e3-ac9e-b83d5019e65e.jpg)


```c++
class Solution {
public:
    vector<int> twoSum(vector<int>& nums, int target) {
        std::unordered_map<int,int> map;
        for(int i = 0; i < nums.size(); i++){
            auto iter = map.find(target - nums[i]);
            if(iter != map.end())
            return {iter -> second, i};
            map.insert(pair<int, int> (nums[i],i));
        }
        return {};
        
    }
};
```

