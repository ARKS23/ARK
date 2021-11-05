# LRU算法
- LRU 缓存算法的核心数据结构就是哈希链表，双向链表和哈希表的结合体。
- 算法大致逻辑
![](LRU算法逻辑.jpeg)

- **146 LRU缓存机制**
思路: 构建哈希链表解决.
```C++
//双向链表数据结构
struct Node{
        int key;
        int value;
        Node *next;
        Node *prev;
};

class LRUCache {
public:
    LRUCache(int capacity) : capacity_(capacity){
        init();
    }
    
    int get(int key) {
        if (my_hash.count(key)) {
            Node *node = my_hash[key];
            remove_to_last(node);
            return node->value;
        }
        return -1;
    }
    
    void put(int key, int value) {
        if (my_hash.count(key)) {
            Node* node = my_hash[key];
            node->value = value;
            remove_to_last(node);
        }
        else {
            if (my_hash.size() == capacity_) {
                int k = remove();
                my_hash.erase(k);
                Node *node = new Node();
                node->key = key;
                node->value = value;
                my_hash[key] = node;
                add_to_last(node);
            }
            else {
                Node *node = new Node();
                node->key = key;
                node->value = value;
                my_hash[key] = node;
                add_to_last(node);
            }
        }
    }

protected:
    //头尾结点初始化
    void init() {
        head = new Node();
        tail = new Node();
        head->next = tail;
        tail->prev = head;
    }

    //LRU把结点移到尾部
    void remove_to_last(Node *node) {
        Node *pre = node->prev;
        Node *nxt = node->next;
        pre->next = nxt;
        nxt->prev = pre;

        add_to_last(node);
    }
    
    //添加新结点到尾部
    void add_to_last(Node *node) {
        node->prev = tail->prev;
        tail->prev->next = node;
        node->next = tail;
        tail->prev = node;
    }

    //移除头部结点
    int remove() {
        Node *node = head->next;
        head->next = node->next;
        node->next->prev = head;
        int k = node->key;
        delete node;
        return k;
    }

private:
    int capacity_;
    unordered_map<int, Node*> my_hash;
    Node *head;
    Node *tail;
};
```

--- 

- **895 最大频率栈**
https://labuladong.gitee.io/algo/2/18/40/
```C++
class FreqStack {
    /*
     * 用一个变量维护当前最大频率
     * 用一个map维护值-频对
     * 用一个map维护频-栈对
     * push和pop都要维护两张表和一个变量
     */
public:
    FreqStack() {
        max_freq = 0;
    }
    
    void push(int val) {
        int freq = ++value_freq[val];        //更新值-频对并且取得当前频率
        freq_stack[freq].push(val);          //更新频-栈对
        max_freq = max(max_freq, freq);      //更新最大频率变量
    }
    
    int pop() {
        int cur_top = freq_stack[max_freq].top();   //取得目标元素
        freq_stack[max_freq].pop();                 //更新频-栈对
        value_freq[cur_top]--;                      //更新值-频对
        if (freq_stack[max_freq].empty())           //如果最高频栈已空,则不存在这频率元素了,减少1
            max_freq--;
        return cur_top;
    }

private:
    int max_freq;                               //记录最大频率
    unordered_map<int, int> value_freq;         //值-频对
    unordered_map<int, stack<int>> freq_stack;  //频-栈对
};
```