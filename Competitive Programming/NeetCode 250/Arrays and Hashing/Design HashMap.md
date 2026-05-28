# Problem

Design a `HashMap` without using any built-in hash table libraries. Your class must have the following methods:
- `void put(int key, int value)` inserts a `(key, value)` pair into the `HashMap`. If `key` already exists, update the corresponding `value`.
- `int get(int key)` returns the `value` to which `key` is mapped, or `-1` if none exists.
- `void remove(int key)` removes the `(key, value)` pair from the `HashMap` if it exists.

**Constraints**: All keys will be between $0$ and $10^6$, inclusive. At most $10^4$ calls will be made to the three methods.
# Solution

We can maintain a `vector<int> keys` and a `vector<int> values` and ensure that each pair shares an index:

```cpp
class HashMap {
	vector<int> keys;
	vector<int> values;
public:
    HashMap() {
        keys = vector<int>();
        values = vector<int>();
    }
    
    void put(int key, int value) {
        for (int i = 0; i < keys.size(); i++) {
	        if (keys[i] == key) {
		        values[i] = value;
		        return;
	        }
        }
        keys.push_back(key);
        values.push_back(value);
    }
    
    int get(int key) {
        for (int i = 0; i < keys.size(); i++) {
	        if (keys[i] == key) {
		        return values[i];
		    }
	    }
	    return -1;
    }
    
    void remove(int key) {
        for (int i = 0; i < keys.size(); i++) {
	        if (keys[i] == key) {
		        keys.erase(keys.begin() + i);
		        values.erase(values.begin() + i);
	        }
        }
    }
};
```

This is $O(n)$ time in each operation, so we can definitely do better. Since we have an upper bound on the magnitude of the keys, we can use them as indices in a single `vector<int> values`, which will be efficient as the bound is quite small:

```cpp
class HashMap {
	vector<int> values;
public:
    HashMap() {
        values = vector<int>(1000001, -1);
    }
    
    void put(int key, int value) {
        values[key] = value;
    }
    
    int get(int key) {
        return values[key];
    }
    
    void remove(int key) {
        values[key] = -1;
    }
};
```

This is $O(1)$ time for all three operations, but requires the storage of a very large `vector<int>`. It's also not generalizable, so maybe we should use an actual hash function:

```cpp
class Node {
public:
	int key;
	int value;
	Node* next;
	
	Node(int k, int v) {
		key = k;
		value = v;
		next = nullptr;
	}
};

class HashMap {
	vector<Node*> map;
	int n = 1000;
public:
    HashMap() {
        map = vector<Node*>(n);
    }
    
    void put(int key, int value) {
        Node* curr = map[key % n];
        if (curr == nullptr) {
	        map[key % n] = new Node(key, value);
	        return;
        }
        while (curr) {
	        if (curr->key == key) {
		        curr->value = value;
		        return;
	        }
            if (curr->next) {
                curr = curr->next;
            } else {
                curr->next = new Node(key, value);
            }
        }
    }
    
    int get(int key) {
        Node* curr = map[key % n];
        while (curr) {
	        if (curr->key == key) {
		        return curr->value;
	        }
	        curr = curr->next;
        }
        return -1;
    }
    
    void remove(int key) {
        Node* curr = map[key % n];
        if (curr == nullptr) {
	        return;
        }
        if (curr->key == key) {
	        map[key % n] = curr->next;
	        delete curr;
	        return;
        }
        Node* next = curr->next;
        while (next) {
	        if (next->key == key) {
		        curr->next = next->next;
		        delete next;
		        return;
	        }
	        curr = next;
	        next = next->next;
        }
    }
};
```

This is now $O(1)$ for all operations in the average case due to our good hashing function.