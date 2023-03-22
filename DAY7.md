# DAY7：四数相加、救赎信、三数之和、四数之和

## 四数相加


![ae21bc9e530cd6df190766f35981128](https://user-images.githubusercontent.com/103561103/226859248-f989c4bb-afc0-4c37-8c48-005245ebf2b8.jpg)

```
class Solution {
public:
    int fourSumCount(vector<int>& nums1, vector<int>& nums2, vector<int>& nums3, vector<int>& nums4) {
        unordered_map<int,int> map;
        for(int a : nums1){//遍历前两个数组中相加等于固定数据的次数，map数据结构中，没有特定顺序，key和value关联起来，通过key可以快速查找
            for(int b : nums2){
                map[a + b] ++;
            }
        }
        int count = 0;
        for(int c : nums3){
            for(int d : nums4){
                if (map.find(0 - (c+d)) != map.end()){//遍历后两个数组中两个数相加等于强两个数组的为零的数据，并且记录相应的次数，注意每次count加的都是前两个数组中那两个数出现的次数
                    count += map[0 - (c+d)];
                }
            }
        }
        return count;

    }
};
```



## 救赎信
![a13f1ad9f94ea3784ef9753ccfb5a68](https://user-images.githubusercontent.com/103561103/226859318-20e0f083-aa9b-48f8-9b53-91650c2739f6.jpg)

```
class Solution {
public:
    bool canConstruct(string ransomNote, string magazine) {
        int record[26] = {0};
        //add
        if (ransomNote.size() > magazine.size()) {
            return false;
        }
        for (int i = 0; i < magazine.length(); i++) {
            // 通过recode数据记录 magazine里各个字符出现次数
            record[magazine[i]-'a'] ++;
        }
        for (int j = 0; j < ransomNote.length(); j++) {
            // 遍历ransomNote，在record里对应的字符个数做--操作
            record[ransomNote[j]-'a']--;
            // 如果小于零说明ransomNote里出现的字符，magazine没有
            if(record[ransomNote[j]-'a'] < 0) {
                return false;
            }
        }
        return true;
    }
};
```

## 三数之和


![b1d5c516818c8b31dd0f29ec851e9c1](https://user-images.githubusercontent.com/103561103/226859180-88f1fe09-e76d-410b-9847-e63bb66461bb.jpg)

```c++
class Solution {
public:
    vector<vector<int>> threeSum(vector<int>& nums) {
        vector<vector<int>> result;
        sort(nums.begin(), nums.end());
        for (int i = 0; i < nums.size(); i++) {// 排序之后如果第一个元素已经大于零，那么无论如何组合都不可能凑成三元组，直接返回结果就可以了
            if (nums[i] > 0) {
                return result;
            }
            // 对于a去重方法
            if (i > 0 && nums[i] == nums[i - 1]) {
                continue;
            }
            int left = i + 1;
            int right = nums.size() - 1;
            while (right > left) {
                // 去重复逻辑如果放在这里，0，0，0 的情况，可能直接导致 right<=left 了，从而漏掉了 0,0,0 这种三元组
                /*
                while (right > left && nums[right] == nums[right - 1]) right--;
                while (right > left && nums[left] == nums[left + 1]) left++;
                */
                if (nums[i] + nums[left] + nums[right] > 0) right--;
                else if (nums[i] + nums[left] + nums[right] < 0) left++;
                else {
                    result.push_back(vector<int>{nums[i], nums[left], nums[right]});
                    // 去重逻辑应该放在找到一个三元组之后，对b 和 c去重
                    while (right > left && nums[right] == nums[right - 1]) right--;
                    while (right > left && nums[left] == nums[left + 1]) left++;

                    // 找到答案时，双指针同时收缩
                    right--;
                    left++;
                }
            }

        }
        return result;
    }
};
```

## 四数之和

![31b9cdac3c77a9468721ce997b3fe77](https://user-images.githubusercontent.com/103561103/226859373-3bd721be-8cc0-4dad-a11a-8f9f5a765f86.jpg)


```c++
class Solution {
public:
    vector<vector<int>> fourSum(vector<int>& nums, int target) {
        vector<vector<int>> result;
        sort(nums.begin(), nums.end());
        for (int k = 0; k < nums.size(); k++) {
            if (nums[k] > target && nums[k] >= 0) {// 剪枝处理，主要区别于三数之和，target可能为负数
            	break; 
            }
            if (k > 0 && nums[k] == nums[k - 1]) {
                continue;
            }
            for (int i = k + 1; i < nums.size(); i++) {
                if (nums[k] + nums[i] > target && nums[k] + nums[i] >= 0) {// 剪枝处理，参考上边，作类比
                    break;
                }
                if (i > k + 1 && nums[i] == nums[i - 1]) {
                    continue;
                }
                int left = i + 1;
                int right = nums.size() - 1;
                while (right > left) {
                    // nums[k] + nums[i] + nums[left] + nums[right] > target 会溢出
                    if ((long) nums[k] + nums[i] + nums[left] + nums[right] > target) {
                        right--;
                    // nums[k] + nums[i] + nums[left] + nums[right] < target 会溢出
                    } else if ((long) nums[k] + nums[i] + nums[left] + nums[right]  < target) {
                        left++;
                    } else {
                        result.push_back(vector<int>{nums[k], nums[i], nums[left], nums[right]});
                        // 对nums[left]和nums[right]去重
                        while (right > left && nums[right] == nums[right - 1]) right--;
                        while (right > left && nums[left] == nums[left + 1]) left++;

                        // 找到答案时，双指针同时收缩
                        right--;
                        left++;
                    }
                }

            }
        }
        return result;
    }
};
```

