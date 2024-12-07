# 146. LRU Cache

Design a data structure that follows the constraints of a **[Least Recently Used (LRU) cache](https://en.wikipedia.org/wiki/Cache_replacement_policies#LRU)**.

Implement the `LRUCache` class:

- `LRUCache(int capacity)` Initialize the LRU cache with **positive** size `capacity`.
- `int get(int key)` Return the value of the `key` if the key exists, otherwise return `-1`.
- `void put(int key, int value)` Update the value of the `key` if the `key` exists. Otherwise, add the `key-value` pair to the cache. If the number of keys exceeds the `capacity` from this operation, **evict** the least recently used key.

The functions `get` and `put` must each run in `O(1)` average time complexity.

 

**Example 1:**

```
Input
["LRUCache", "put", "put", "get", "put", "get", "put", "get", "get", "get"]
[[2], [1, 1], [2, 2], [1], [3, 3], [2], [4, 4], [1], [3], [4]]
Output
[null, null, null, 1, null, -1, null, -1, 3, 4]

Explanation
LRUCache lRUCache = new LRUCache(2);
lRUCache.put(1, 1); // cache is {1=1}
lRUCache.put(2, 2); // cache is {1=1, 2=2}
lRUCache.get(1);    // return 1
lRUCache.put(3, 3); // LRU key was 2, evicts key 2, cache is {1=1, 3=3}
lRUCache.get(2);    // returns -1 (not found)
lRUCache.put(4, 4); // LRU key was 1, evicts key 1, cache is {4=4, 3=3}
lRUCache.get(1);    // return -1 (not found)
lRUCache.get(3);    // return 3
lRUCache.get(4);    // return 4
```

 

**Constraints:**

- `1 <= capacity <= 3000`
- `0 <= key <= 104`
- `0 <= value <= 105`
- At most `2 * 105` calls will be made to `get` and `put`.

## Solution

### **工作逻辑**

1. **缓存初始化**：设定一个固定大小的缓存空间。

2. **访问数据**：

   - 如果数据在缓存中（**命中**），将其标记为最近使用的。

   - 如果数据不在缓存中（

     未命中

     ），则需要将数据加载到缓存。

     - 如果缓存未满，直接将数据加入。
     - 如果缓存已满，淘汰最久未使用的项，然后将新数据加入。

3. **关键操作**：

   - **更新最近使用状态**：每次访问某个数据时，需要将其更新为最近使用的。
   - **淘汰机制**：当缓存已满且新数据需要加入时，移除最久未使用的数据。

逻辑很简单，就是双向链表比较麻烦。GPT代写一部分

```c++
using namespace std;

struct Node {
    int key;
    int val;
    Node* next;
    Node* prev;
    Node(int k, int v) : key(k), val(v), next(nullptr), prev(nullptr) {}
};

class LRUCache {
private:
    Node* head;
    Node* tail;
    unordered_map<int, Node*> maps;
    int capacity;
    int size;

    void moveToHead(Node* node) {
        removeNode(node);
        addToHead(node);
    }

    void removeNode(Node* node) {
        node->prev->next = node->next;
        node->next->prev = node->prev;
    }

    void addToHead(Node* node) {
        node->next = head->next;
        node->prev = head;
        head->next->prev = node;
        head->next = node;
    }

    Node* removeTail() {
        Node* node = tail->prev;
        removeNode(node);
        return node;
    }

public:
    LRUCache(int capacity) {
        this->capacity = capacity;
        this->size = 0;

        head = new Node(0, 0);
        tail = new Node(0, 0);
        head->next = tail;
        tail->prev = head;
    }
    
    int get(int key) {
        if (maps.find(key) == maps.end()) {
            return -1;
        }
        Node* node = maps[key];
        moveToHead(node);
        return node->val;
    }
    
    void put(int key, int value) {
        if (maps.find(key) != maps.end()) {
            Node* node = maps[key];
            node->val = value;
            moveToHead(node);
        } else {
            Node* newNode = new Node(key, value);
            maps[key] = newNode;
            addToHead(newNode);
            size++;

            if (size > capacity) {
                Node* tailNode = removeTail();
                maps.erase(tailNode->key);
                delete tailNode;
                size--;
            }
        }
    }

    ~LRUCache() {
        Node* current = head;
        while (current != nullptr) {
            Node* temp = current;
            current = current->next;
            delete temp;
        }
    }
};
```

