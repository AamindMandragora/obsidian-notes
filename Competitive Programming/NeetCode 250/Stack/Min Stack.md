# Problem

Design a stack class that supports the `push`, `pop`, `top`, and `getMin` operations in $O(1)$ time. The latter three operations will always be called on non-empty stacks.
# Solution

For the first three operations, we simply use a singly-linked list with a `head` sentinel. However, we'd have to traverse the whole list to find the smallest element, which takes too long. We could try a `min` sentinel that gets moved whenever we push a smaller element, but we'd still have to traverse the whole list to find the new smallest when we pop `min`. A problem where we'll only ever do stuff with the latest element but we'll still need access to previous elements after we're done processing the latest is easily solvable by a stack. We'll maintain a regular stack and a second one that only gets pushed to when there's a new smallest element. When we pop, we may have to pop from one or both stacks.

```cpp
class MinStack {
	class Node {
	public:
		int val;
		Node* next;
		Node(int v) {
			val = v;
			next = nullptr;
		}
	};
	Node* stack_;
	Node* min_;
public:
    MinStack() {
        stack_ = nullptr;
        min_ = nullptr;
    }
    
    void push(int val) {
	    if (stack_ == nullptr) {
	        stack_ = new Node(val);
			min_ = new Node(val);
        } else {
	        auto* n = new Node(val);
			n->next = stack_;
			stack_ = n;
	        if (val <= min_->val) {
		        auto* n = new Node(val);
		        n->next = min_;
		        min_ = n;
	        }
        }
    }
    
    void pop() {
        if (stack_->val == min_->val) {
	        auto* n = min_->next;
	        delete min_;
	        min_ = n;
        }
        auto* n = stack_->next;
        delete stack_;
        stack_ = n;
    }
    
    int top() {
        return stack_->val;
    }
    
    int getMin() {
        return min_->val;
    }
};
```