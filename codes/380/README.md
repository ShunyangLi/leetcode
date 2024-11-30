# 380. Insert Delete GetRandom O(1)

Implement the `RandomizedSet` class:

- `RandomizedSet()` Initializes the `RandomizedSet` object.
- `bool insert(int val)` Inserts an item `val` into the set if not present. Returns `true` if the item was not present, `false` otherwise.
- `bool remove(int val)` Removes an item `val` from the set if present. Returns `true` if the item was present, `false` otherwise.
- `int getRandom()` Returns a random element from the current set of elements (it's guaranteed that at least one element exists when this method is called). Each element must have the **same probability** of being returned.

You must implement the functions of the class such that each function works in **average** `O(1)` time complexity.

 

**Example 1:**

```
Input
["RandomizedSet", "insert", "remove", "insert", "getRandom", "remove", "insert", "getRandom"]
[[], [1], [2], [2], [], [1], [2], []]
Output
[null, true, false, true, 2, true, false, 2]

Explanation
RandomizedSet randomizedSet = new RandomizedSet();
randomizedSet.insert(1); // Inserts 1 to the set. Returns true as 1 was inserted successfully.
randomizedSet.remove(2); // Returns false as 2 does not exist in the set.
randomizedSet.insert(2); // Inserts 2 to the set, returns true. Set now contains [1,2].
randomizedSet.getRandom(); // getRandom() should return either 1 or 2 randomly.
randomizedSet.remove(1); // Removes 1 from the set, returns true. Set now contains [2].
randomizedSet.insert(2); // 2 was already in the set, so return false.
randomizedSet.getRandom(); // Since 2 is the only number in the set, getRandom() will always return 2.
```

 

**Constraints:**

- `-231 <= val <= 231 - 1`
- At most `2 * ``105` calls will be made to `insert`, `remove`, and `getRandom`.
- There will be **at least one** element in the data structure when `getRandom` is called.

## Solution

通过使用`hashmap`和`vector`来实现，hashmap来记录数字在vector中index的位置。当需要remove的时候，我们可以把最后该数字放到vector的末尾，然后删除掉最后一个数字，这样的话就只需要`O(1)`的复杂度。remove的时候只需要更新index位置就行。

```c++
class RandomizedSet {
private:
    unordered_map<int, int> maps;
    vector<int> vec;
    std::mt19937 gen;
public:
    RandomizedSet() : gen(std::random_device{}()) {}

    bool insert(int val) {
        if (maps.count(val)) return false;

        maps[val] = vec.size();
        vec.push_back(val);

        return true;
    }

    bool remove(int val) {
        auto it = maps.find(val);
        if (it == maps.end()) return false;

        int idx = it->second;
        int last = vec.back();

        vec[idx] = last;
        maps[last] = idx;

        vec.pop_back();
        maps.erase(it);

        return true;
    }

    int getRandom() {
        std::uniform_int_distribution<> dis(0, vec.size() - 1);
        return vec[dis(gen)];
    }
};
```

