

# DAY4:两两交换链表中的节点、删除链表的倒数第N个节点、链表相交、环形链表

## 两两交换链表中的节点

两两交换链表中节点注意

1、两节点相互交换一定要找到要交换节点的上一个节点

2、节点交换终止条件：链表长度是偶数时：cur -> next != nullptr  链表节点是奇数时： cur -> next -> next != nullptr

3、一定注意提前保存要改变的节点

```c++
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode() : val(0), next(nullptr) {}
 *     ListNode(int x) : val(x), next(nullptr) {}
 *     ListNode(int x, ListNode *next) : val(x), next(next) {}
 * };
 */
class Solution {
public:
    ListNode* swapPairs(ListNode* head) {

        ListNode* dummyhead = new ListNode(0);//定义一个虚拟头结点（意思就是新定义一个链表，这个链表作为虚拟头指针）
        dummyhead -> next = head;//链表的下一个指向头节点
        ListNode* cur = dummyhead;//当前节点指向要交换节点的前一个节点
        while(cur -> next != nullptr && cur -> next -> next != nullptr){//注意一定要把cur -> next 写在前边
            ListNode* temp1 = cur -> next;
            ListNode* temp2 = cur -> next -> next -> next;
            //交换操作
            cur -> next = cur -> next -> next;
            cur -> next -> next = temp1;
            cur -> next -> next -> next = temp2;

            cur = cur -> next -> next;//向后更新两个位置
        }
        return dummyhead -> next;
    }
};
```

## 删除链表中倒数第N个节点

**本问题的关键是找到倒数第N个节点**

采用双指针法，其中快指针和慢指针同时指向虚拟头结点；此时快指针先运动，当快指针运动N次时，慢指针同时开始运动，直到快指针运动到空指针时，慢指针指向第N个节点。

此时注意要删除第N个节点就必须指向第N-1个节点

一定判断N的合法性

```c++
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode() : val(0), next(nullptr) {}
 *     ListNode(int x) : val(x), next(nullptr) {}
 *     ListNode(int x, ListNode *next) : val(x), next(next) {}
 * };
 */
class Solution {
public:
    ListNode* removeNthFromEnd(ListNode* head, int n) {
        ListNode* dummyhead = new ListNode(0);
        dummyhead -> next = head;
        ListNode* fast = dummyhead;//快慢指针同时先指向虚拟头结点
        ListNode* slow = dummyhead;
        n++;
        while(n--){//快指针先执行n+1
            fast = fast -> next;
        }
        while(fast != nullptr){//快慢指针同时运动，此时当快指针运动到空指针时候，慢指针正好运动在倒数第n个节点的前一个
            fast = fast -> next;
            slow = slow -> next;
        }
        //已经找到了该节点，对其进行删除操作，同时释放内存
        slow -> next = slow -> next -> next;
        return dummyhead -> next;
    }
};
```

## 环形链表

解题关键点：

**1、判断是否有环（双指针法）**

**2、找到环的入口**

在本道题目中，快指针两步一次；慢指针一步一次。如果存在环形链表的话相当于快指针以相对于慢指针一步一次的距离进行追赶慢指针。

判断是否有环：即判断两个指针能否相等。一直循环，在循环中终止条件为快指针的下一个节点和下下一个节点不为空指针。

有环之后，环的入口是求解重点
![32e8022397fc99fc7610ec2dc23be33](https://user-images.githubusercontent.com/103561103/226171600-beb82901-0e2c-484a-a60a-947cfb557967.jpg)

```c++
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode(int x) : val(x), next(NULL) {}
 * };
 */
class Solution {
public:
    ListNode *detectCycle(ListNode *head) {
        ListNode* fast = head;//定义快指针
        ListNode* slow = head;//定义慢指针
        while(fast != NULL && fast->next != NULL) {//判断是否为环，快指针可以一直运动知道他为空指针
            slow = slow->next;
            fast = fast->next->next;
            // 快慢指针相遇，此时从head 和 相遇点，同时查找直至相遇
            if (slow == fast) {//此时快慢指针相遇，并且一定是在慢指针进入的第一圈相遇
                ListNode* index1 = fast;//相遇时候的地址节点
                ListNode* index2 = head;
                while (index1 != index2) {
                    index1 = index1->next;
                    index2 = index2->next;
                }
                return index2; // 返回环的入口
            }
        }
        return NULL;
    }
};
```

