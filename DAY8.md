# DAY8 反转字符串、反转字符串Ⅱ、替换空格、旋转单词、左旋转字符串

## 反转字符串

![fd6e1bf9dcf8ddb0068fb77ff6e2f74](C:\Users\DELL\Desktop\fd6e1bf9dcf8ddb0068fb77ff6e2f74.jpg)

```c++
class Solution {
public:
    void reverseString(vector<char>& s) {//双指针法,i从0开始，j从尾部开始
        int len = s.size();
        for (int i = 0,j = len - 1; i < len/2; i++, j--){
            swap(s[i],s[j]);
        }
    }
};
```



## 反转字符串Ⅱ



![33590a345301742b7d165f7e909bbfe](C:\Users\DELL\Desktop\33590a345301742b7d165f7e909bbfe.jpg)

```c++
class Solution {
public:
    string reverseStr(string s, int k) {
        for (int i = 0; i < s.size(); i += 2*k ){//新思路，在循环中不一定每次都是i++，可以用需求来决定
            if(i + k <= s.size()){//判断是否剩下的是大于k个
                reverse(s.begin() + i, s.begin() + i + k);
            }
            else {//当小于k个时候直接反转当前值和末尾的值
                reverse(s.begin() + i, s.end());
            }
        }
        return s;

    }
};
```



## 替换空格

![623c2c9ab1c4484dd4175387a16bad3](C:\Users\DELL\Desktop\623c2c9ab1c4484dd4175387a16bad3.jpg)

```c++
class Solution {
public:
    string replaceSpace(string s) {
        int count = 0;
        for(int i = 0; i<s.size(); i++){//计算原来数组中空格的数目
            if(s[i] == ' '){
            count++;
            }
        }
        int old_size = s.size();
        int new_size = s.size() + 2*count;//重新设置新数组的大小
        s.resize(new_size);
        for(int i = new_size - 1, j = old_size - 1; j < i; j--, i-- ){//双指针法，i指向新数组的末尾，j指向旧数组末尾
            if(s[j] != ' ')//如果为空格
            s[i] = s[j];
            else {
                s[i] = '0';
                s[i -1] = '2';
                s[i - 2] = '%';
                i -= 2;
            }
        }
    return s;
    }
};
```



## 反转单词

![bc737fd88a0c8272546bb681bc069de](C:\Users\DELL\Desktop\bc737fd88a0c8272546bb681bc069de.jpg)

```
class Solution {
public:
    void reverse(string& s, int start, int end){ //翻转，区间写法：左闭右闭 []
        for (int i = start, j = end; i < j; i++, j--) {
            swap(s[i], s[j]);
        }
    }

    void removeExtraSpaces(string& s) {//去除所有空格并在相邻单词之间添加空格, 快慢指针。
        int slow = 0;   
        for (int i = 0; i < s.size(); ++i) { //
            if (s[i] != ' ') { //遇到非空格就处理，即删除所有空格。
                if (slow != 0) s[slow++] = ' '; //手动控制空格，给单词之间添加空格。slow != 0说明不是第一个单词，需要在单词前添加空格。
                while (i < s.size() && s[i] != ' ') { //补上该单词，遇到空格说明单词结束。
                    s[slow++] = s[i++];
                }
            }
        }
        s.resize(slow); //slow的大小即为去除多余空格后的大小。
    }

    string reverseWords(string s) {
        removeExtraSpaces(s); //去除多余空格，保证单词之间之只有一个空格，且字符串首尾没空格。
        reverse(s, 0, s.size() - 1);
        int start = 0; //removeExtraSpaces后保证第一个单词的开始下标一定是0。
        for (int i = 0; i <= s.size(); ++i) {
            if (i == s.size() || s[i] == ' ') { //到达空格或者串尾，说明一个单词结束。进行翻转。
                reverse(s, start, i - 1); //翻转，注意是左闭右闭 []的翻转。
                start = i + 1; //更新下一个单词的开始下标start
            }
        }
        return s;
    }
};
```



## 左旋转字符串

![2504f328511e554783134212c1b6fa0](C:\Users\DELL\Desktop\2504f328511e554783134212c1b6fa0.jpg)

```c++
class Solution {
public:
    string reverseLeftWords(string s, int n) {
        reverse(s.begin(), s.begin() + n);//一定注意在C++官方给的库中反转指的是左闭右开区间
        reverse(s.begin() + n, s.end());
        reverse(s.begin(), s.end());
return s;
    }
};
```

