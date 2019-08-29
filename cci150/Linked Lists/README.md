# Linked Lists

- slow indexing
- fast remove/insert
- when you're discussing a linked list in an interview, you must understand whether it is a singly linked list or a doubly linked list.



## Create a Linked List

```java
class Node {
	// member
	Node next= null;
	int data;
	
    // function
	public Node(int d) {
		data= d;
	}
    
	void appendToTail(int d) {
		Node end= new Node(d);
		Node n = this;
        while(n.next != null){
            n = n.next;
        }
        n.next = end;
    }
}
```



### Problem

Be careful: What if multiple objects need a reference to the linked list, and then the head of the linked list changes? Some objects might still be pointing to the old head.



### Solution

We could, if we chose, implement a **Linked List** class that wraps the **Node** class. This would essentially just have a single member variable: the head Node. This would largely resolve the earlier issue.



## Deleting a Node from a Singly Linked List

- Given a node `n`, we find the previous node `prev` and set `prev.next` equal to `n.next`. 

- If the list is doubly linked, we must also update `n.next` to set `n.next.prev` equal to `n. prev`. 

- The important things to remember are 
  - (1) to check for the `null` pointer
  - (2) to update the `head` or `tail` pointer as necessary.

```java
Node deleteNode(Node head, int d) {
	Node n = head;
	if (n.data == d) {
		return head.next; */ m oved head*/
	}
	while (n.next != null) {
		if (n.next.data == d) {
			n.next = n.next.next;
			return head; /* head didn't change*/
		}
		n = n.next;
	}
	return head;
}
```



## The Runner Technique (Fast-Slow Pointers)

- The runner technique means that you iterate through the linked list with two pointers simultaneously, with one ahead of the other. 
- The "fast" node might be ahead by a fixed amount, or it might be hopping multiple nodes for each one node that the "slow" node iterates through.

```
Example
Input: a1 - >a2 -> ••• ->an - >b1 ->b2 -> ••• ->bn
Output: a1 ->b1 ->a2 - >b2 -> ••• - >an - >bn

You do not know the length of the linked list (but you do know that the length is an even number).
```



### Solution

- `p1` (the fast pointer) move every two elements for every one move that `p2` makes.

- When `p1` hits the end of the linked list, `p2` will be at the midpoint.
- Then, move `p1` back to the front and begin "weaving" the elements. On each iteration, `p2` selects an element and inserts it after `p1`.



## Recursive Problems

- A number of linked list problems rely on recursion. If you're having trouble solving a linked list problem, you should explore if a recursive approach will work.
- However, you should remember that recursive algorithms take at least O (n) space, where n is the depth of the recursive call.