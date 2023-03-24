# DAY10: 用栈来实现队列、用队列来实现栈

## 用栈来实现队列



```c++
class MyQueue {
public:
    MyQueue() {

    }
    stack<int> Sl_In;
    stack<int> Sl_Out;
    
    void push(int x) {
        Sl_In.push(x);//直接在进栈中push
    }
    
    int pop() {
        if(Sl_Out.empty()){//先判断是否出栈中是否为空
            while(!Sl_In.empty()){//如果出栈为空，同时进展不为空的话，直接把所有进展的元素放入出栈中
                Sl_Out.push(Sl_In.top());
                Sl_In.pop();//同时pop出去进栈中的元素
            }
        }
        int result = Sl_Out.top();
        Sl_Out.pop();
        return result;

    }
    
    int peek() {
        int result = this -> pop();//直接复用定义的pop函数，用到了this指针
        Sl_Out.push(result);
        return result;

    }
    
    bool empty() {
        return Sl_In.empty() && Sl_Out.empty();

    }
};
```



## 用队列来实现栈



```c++
class MyStack {
public:
    MyStack() {

    }
    queue<int> que;
    
    void push(int x) {
        que.push(x);

    }
    
    int pop() {
        int size = que.size();
        size--;
        while(size--){//依次先弹出队列中前size- 1 个元素
            que.push(que.front());
            que.pop();
        }
        int result = que.front();//返回结果之后一定记住把返回的这个结果一块pop
        que.pop();
        return result;

    }
    
    int top() {
        return que.back();

    }
    
    bool empty() {
        return que.empty();

    }
};
```

