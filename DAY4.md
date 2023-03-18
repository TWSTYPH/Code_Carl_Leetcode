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

