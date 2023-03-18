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

