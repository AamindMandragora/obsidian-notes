# Problem

Design a `HashSet` without using any built-in hash table libraries. Your class must have the following methods:
- `void add(int key)` inserts `key` into the `HashSet`
- `bool contains(int key)` returns whether `key` exists in the `HashSet` or not
- `void remove(int key)` removes `key` from the `HashSet` if it existed

**Constraints**: All keys will be between $0$ and $10^6$, inclusive. At most $10^4$ calls will be made to the three methods.
# Solution

The simplest solution is to simply use a `vector<int>` and ensure you never have a duplicate entry:

```cpp
class HashSet {
	vector<int> set;
public:
	HashSet() {
		set = vector<int>();
	}
	
	bool contains(int key) {
		for (int k : set) {
			if (k == key) return true;
		}
		return false;
	}
	
	void add(int key) {
		if (!contains(key)) {
			set.push_back(key);
		}
	}
	
	void remove(int key) {
		for (auto it = set.begin(); it != set.end(); it++) {
			if (*it == key) {
				set.erase(it);
				break;
			}
		}
	}
};
```

This has time complexity $O(n)$ for `add`, `remove`, and `contains`. However, we can do better. Since our keys are always really small numbers, we can use them as indices instead of as elements, and store our set as a `bool[]`:

```cpp
class HashSet {
	vector<bool> set;
public:
	HashSet() {
		set = vector<bool>(1000001, false);
	}
	
	bool contains(int key) {
		return set[key];
	}
	
	void add(int key) {
		set[key] = true;
	}
	
	void remove(int key) {
		set[key] = false;
	}
};
```

This has $O(1)$ time complexity for all three operations, but has $O(k)$ space complexity, where $k$ is the largest possible key, which means while it is faster than the previous approach, it isn't the most optimal solution. Maybe we should try using an actual hash function:

```cpp
class Node {
public:
	int key;
	Node* next;
	
	Node(int k) {
		key = k;
		next = nullptr;
	}
};

class HashSet {
    vector<Node*> set;
	int k = 1000;
public:
    HashSet() {
        set = vector<Node*>(k);
    }
    
    bool contains(int key) {
		Node* curr = set[key % k];
		while (curr) {
			if (curr->key == key) {
				return true;
			}
			curr = curr->next;
		}
		return false;
	}
	
	void add(int key) {
		Node* curr = set[key % k];
        if (curr == nullptr) {
            set[key % n] = new Node(key);
            return;
        }
		while (curr) {
			if (curr->key == key) {
				break;
			}
			if (curr->next) {
				curr = curr->next;
			} else {
				curr->next = new Node(key);
			}
		}
	}
	
	void remove(int key) {
		Node* curr = set[key % k];
        if (curr == nullptr) return;
        if (curr->key == key) {
            set[key % n] = curr->next;
            delete curr;
            return;
        }
        Node* next = curr->next;
		while (next) {
			if (next->key == key) {
				curr->next = next->next;
				delete next;
				break;
			}
			curr = next;
            next = next->next;
		}
	}
};
```

In the average case, this is $O(1)$ time for each operation and it' generalized to any key constraints.