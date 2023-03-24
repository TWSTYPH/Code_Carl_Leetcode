# DAY10: 用栈来实现队列、用队列来实现栈

## 用栈来实现队列

![459c1148dfc1c7d165d225ecc8fe8e6](https://user-images.githubusercontent.com/103561103/227432552-3923b1e4-81c5-4e88-b67f-ef62a6fd6b63.jpg)

![35940df96c85e40c210a96b08cfc413](https://user-images.githubusercontent.com/103561103/227432596-8e52ce12-1c7b-4ee5-a802-65dc66b0d541.jpg)

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

![b3e5af1109a05e3be439d1fd9541694](https://user-images.githubusercontent.com/103561103/227432632-bf1e9b5b-bdda-4e51-a0b8-df974b5781e1.jpg)


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

