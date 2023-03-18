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

## 设计链表

首先要搞明白，设计一个链表的前提是先要有一个链表

首先在这个类中定义一个链表的结构体，单链表中这个结构体仅仅包含两个成员变量：**节点的值和下一个节点指针**注意链表的构造方式

在对链表进行处理的时候一定善于利用**虚拟头结点**这一个节点的定义

1、获取链表中，首先给定一个获取的索引，一定要先判断这个索引的合法性。注意：**链表的头结点编号为0**链表的**尾结点指向的是空指针**

2、头部插入节点

在进行链表的头部插入节点的时候一定注意，找到的是虚拟头结点，注意新节点和虚拟头节点的赋值顺序。

3、尾部插入节点时候注意，一定是先找到尾部节点。寻找尾部节点的条件为其next指向的是空指针。

4、在第n个节点前插入一个新节点。首先判断这个索引是否合法：a、如果索引小于零直接令他等于0。 b、如果索引等于size大小，要求在这个链表的尾结点后添加一个节点。 c、如果大于size直接退出.

5、删除一个节点，注意先判断这个索引的合法性，后找第n个节点的前一个节点。

```c++
class MyLinkedList {
public:
    //定义链表节点结构体
    struct LinkedNode {
    int val;
    LinkedNode* next;
    LinkedNode(int val):val(val), next(nullptr){}
    };
    //类的构造函数
    MyLinkedList() {
    dummyHead = new LinkedNode(0); // 这里定义的头结点 是一个虚拟头结点，而不是真正的链表头点  
    size = 0;
    }
    
    int get(int index) {
        LinkedNode* cur = dummyHead -> next;
        if (index < 0||index > (size - 1))
        return -1;
        while(index){
            cur = cur -> next;
            index--;
        }
        return cur -> val;

    }
    
    void addAtHead(int val) {
        LinkedNode* newnode = new LinkedNode(val);
        newnode -> next = dummyHead -> next;
        dummyHead -> next = newnode;
        size++;
    }
    
    void addAtTail(int val) {
        LinkedNode* newnode = new LinkedNode(val);
        LinkedNode* cur = dummyHead;
        while (cur -> next != nullptr){
            cur = cur -> next;
        }
        cur -> next = newnode;
        size++;
    }
    
    void addAtIndex(int index, int val) {//注意如果index等于size时候，此时在链表的末尾添加一个节点
        if(index > size) return;
        if(index < 0) index = 0;   
        LinkedNode* newnode = new LinkedNode(val);
        LinkedNode* cur = dummyHead;
        while(index){
            cur = cur -> next;
            index--;
        }
        newnode -> next = cur -> next;
        cur -> next = newnode;
        size++;
    }
    
    void deleteAtIndex(int index) {
        if (index >= size || index < 0) {
            return;
        }
        LinkedNode* cur = dummyHead;
        while(index){
            cur = cur -> next;
            index--;
        }
        cur -> next = cur -> next -> next;
        size--;

    }
    private:
    int size ;
    LinkedNode* dummyHead;
};

/**
 * Your MyLinkedList object will be instantiated and called as such:
 * MyLinkedList* obj = new MyLinkedList();
 * int param_1 = obj->get(index);
 * obj->addAtHead(val);
 * obj->addAtTail(val);
 * obj->addAtIndex(index,val);
 * obj->deleteAtIndex(index);
 */
```

## 翻转链表

双指针法：初始化的时候cur指向头结点，pre指向cur的前一个节点

注意循环结束条件：cur == nullptr；

递归法有点疑惑，不明白那个return返回到哪了。

```c++
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
    ListNode* reverse(ListNode* cur, ListNode* pre){
        if (cur == nullptr) return pre;
        ListNode* temp = cur -> next;
        cur -> next = pre;
        return reverse(temp, cur);
    }
    ListNode* reverseList(ListNode* head) {
        return reverse(head, nullptr);
    }
};
```

