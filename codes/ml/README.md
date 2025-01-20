# 单调队列 （Monotonic List）

单调队列可以简单理解为单调递增或者单调递减的队列。

解题模板一般如下：

- `pop_back`：队列末尾元素出列，当有新的元素加入进来但是不满足单调性要求可以删除掉影响的元素
- `pop_front`：队列首元素出列，当达到条件限制的时候来删除掉不符合条件的元素，比如`k`个elements这种条件
- `front`：取出队列中的第一个元素，一般为我们需要的内容拍那个。

在`c++`中因为没有单调队列，我们可以使用`list<T>`或者`deque`来替换，可以达到相同的效果，但是效率貌似不是特别高。

```c++
void MonotonicQueue(vector<int>& nums, int k) {
    deque<int> q;
    vector<int> res;
    
    for (int i = 0; i < nums.size(); i ++) {
        while (!q.empty() && i - q.front() >= k) q.pop_front(); // remove from head, ensure k
        while (!q.empty() && nums[q.back()] <= nums[i]) q.pop_back(); // remove from tail, ensure monotonic
        q.push_back(i);
        q.front(); // using the front element
    }
    
    return res;
}
```