
---

# Navigation – Easy Linked List Problems

* [Middle of a linked list](#1-middle-of-a-linked-list)
* [Reverse a Linked List](#2-reverse-a-linked-list)
* [Reverse a Doubly Linked List](#3-reverse-a-doubly-linked-list)
* [Rotate a linked list](#4-rotate-a-linked-list)
* [Nth node from End](#5-nth-node-from-end)
* [Delete Last Occurrence](#6-delete-last-occurrence)
* [Delete Middle](#7-delete-middle)
* [Merge Alternate Positions](#8-merge-alternate-positions)
* [Circular List Traversal](#9-circular-list-traversal)
* [Queue using Linked List](#10-queue-using-linked-list)
* [Stack using singly linked list](#11-stack-using-singly-linked-list)
* [Pairwise Swap](#12-pairwise-swap)
* [Count Occurrences](#13-count-occurrences)

---

## 1. Middle of a linked list

### Problem

Given the head of a singly linked list, return the **middle node**.
If there are two middles (even length), return the **second middle**.

### Example

List: `1 → 2 → 3 → 4 → 5`
Output: node with value `3`

List: `1 → 2 → 3 → 4 → 5 → 6`
Output: node with value `4` (second middle)

### Logic / Approach

* Use **slow and fast pointers**:

  * `slow` moves 1 step at a time.
  * `fast` moves 2 steps at a time.
* When `fast` reaches end, `slow` will be at middle.

**Time Complexity:** O(n)
**Space Complexity:** O(1)

### Java Code

```java
public class MiddleOfLinkedList {

    static class Node {
        int data;
        Node next;
        Node(int d) { data = d; }
    }

    public static Node middleNode(Node head) {
        if (head == null) return null;

        Node slow = head;
        Node fast = head;

        while (fast != null && fast.next != null) {
            slow = slow.next;          // 1 step
            fast = fast.next.next;     // 2 steps
        }
        return slow;
    }
}
```

🔝 [Back to Top](#navigation--easy-linked-list-problems)

---

## 2. Reverse a Linked List

### Problem

Reverse a **singly linked list**.

### Example

Input: `1 → 2 → 3 → 4`
Output: `4 → 3 → 2 → 1`

### Logic / Approach

* Iterative reversal using 3 pointers:

  * `prev`, `curr`, `next`.
* For each node:

  * Store `next = curr.next`
  * Change `curr.next = prev`
  * Move `prev = curr`, `curr = next`
* At end, `prev` is new head.

**Time Complexity:** O(n)
**Space Complexity:** O(1)

### Java Code

```java
public class ReverseLinkedList {

    static class Node {
        int data;
        Node next;
        Node(int d) { data = d; }
    }

    public static Node reverse(Node head) {
        Node prev = null;
        Node curr = head;

        while (curr != null) {
            Node next = curr.next; // store next
            curr.next = prev;      // reverse link
            prev = curr;           // move prev
            curr = next;           // move curr
        }
        return prev; // new head
    }
}
```

🔝 [Back to Top](#navigation--easy-linked-list-problems)

---

## 3. Reverse a Doubly Linked List

### Problem

Reverse a **doubly linked list**.

### Example

Input: `1 ⇄ 2 ⇄ 3 ⇄ 4`
Output: `4 ⇄ 3 ⇄ 2 ⇄ 1`

### Logic / Approach

* Traverse list and **swap `prev` and `next`** for each node.
* Keep track of last processed node; that will become the new head.

**Time Complexity:** O(n)
**Space Complexity:** O(1)

### Java Code

```java
public class ReverseDoublyLinkedList {

    static class Node {
        int data;
        Node prev, next;
        Node(int d) { data = d; }
    }

    public static Node reverse(Node head) {
        if (head == null) return null;

        Node curr = head;
        Node newHead = null;

        while (curr != null) {
            // swap prev and next
            Node temp = curr.prev;
            curr.prev = curr.next;
            curr.next = temp;

            newHead = curr;        // last processed becomes new head
            curr = curr.prev;      // move using prev (was next)
        }
        return newHead;
    }
}
```

🔝 [Back to Top](#navigation--easy-linked-list-problems)

---

## 4. Rotate a linked list

### Problem

Rotate a singly linked list **left by k nodes**.
(First k nodes are moved to the end.)

### Example

List: `10 → 20 → 30 → 40 → 50`, k = 2
Output: `30 → 40 → 50 → 10 → 20`

### Logic / Approach

* Find length `n` and last node (tail).
* Do `k = k % n`. If `k == 0`, no change.
* Move a pointer `k-1` steps from head → this is `kthPrev`.
* New head = `kthPrev.next`.
* Break `kthPrev.next = null` and connect tail.next to old head.

**Time Complexity:** O(n)
**Space Complexity:** O(1)

### Java Code

```java
public class RotateLinkedList {

    static class Node {
        int data;
        Node next;
        Node(int d) { data = d; }
    }

    public static Node rotateLeft(Node head, int k) {
        if (head == null || k == 0) return head;

        // find length and last node
        Node tail = head;
        int n = 1;
        while (tail.next != null) {
            tail = tail.next;
            n++;
        }

        k = k % n;
        if (k == 0) return head;

        // find (k-1)th node
        Node curr = head;
        for (int i = 1; i < k; i++) {
            curr = curr.next;
        }

        Node newHead = curr.next;
        curr.next = null;
        tail.next = head;

        return newHead;
    }
}
```

🔝 [Back to Top](#navigation--easy-linked-list-problems)

---

## 5. Nth node from End

### Problem

Given a singly linked list, return the **Nth node from the end** (1-based).
If N > length, return null or -1 based on requirement.

### Example

List: `1 → 2 → 3 → 4 → 5`, N = 2
Output: node with value `4`

### Logic / Approach

* Use **two pointers**:

  * Move `first` N steps ahead.
  * Then move both `first` and `second` until `first` becomes null.
  * `second` will be at Nth from end.

**Time Complexity:** O(n)
**Space Complexity:** O(1)

### Java Code

```java
public class NthFromEnd {

    static class Node {
        int data;
        Node next;
        Node(int d) { data = d; }
    }

    public static Node nthFromEnd(Node head, int n) {
        if (head == null || n <= 0) return null;

        Node first = head;
        for (int i = 0; i < n; i++) {
            if (first == null) return null; // n > length
            first = first.next;
        }

        Node second = head;
        while (first != null) {
            first = first.next;
            second = second.next;
        }
        return second;
    }
}
```

🔝 [Back to Top](#navigation--easy-linked-list-problems)

---

## 6. Delete Last Occurrence

### Problem

Delete the **last occurrence of a given key** in a singly linked list.

### Example

List: `1 → 2 → 3 → 2 → 4`, key = 2
Output: `1 → 2 → 3 → 4`

### Logic / Approach

* Traverse the list, keep track of:

  * `lastPrev` = previous node of last node with key
  * `last` = last node with key
* After traversal:

  * If no occurrence, return head.
  * If last is head → move head = head.next.
  * Else: `lastPrev.next = last.next`.

**Time Complexity:** O(n)
**Space Complexity:** O(1)

### Java Code

```java
public class DeleteLastOccurrence {

    static class Node {
        int data;
        Node next;
        Node(int d) { data = d; }
    }

    public static Node deleteLastOccurrence(Node head, int key) {
        if (head == null) return null;

        Node curr = head, prev = null;
        Node last = null, lastPrev = null;

        while (curr != null) {
            if (curr.data == key) {
                last = curr;
                lastPrev = prev;
            }
            prev = curr;
            curr = curr.next;
        }

        if (last == null) return head; // no key

        if (lastPrev == null) {
            // last occurrence is at head
            head = head.next;
        } else {
            lastPrev.next = last.next;
        }

        return head;
    }
}
```

🔝 [Back to Top](#navigation--easy-linked-list-problems)

---

## 7. Delete Middle

### Problem

Delete the **middle node** of a singly linked list.
If even length, delete the **second middle**.

### Example

List: `1 → 2 → 3 → 4 → 5` → delete `3`
Result: `1 → 2 → 4 → 5`

List: `1 → 2 → 3 → 4` → middle considered `3` (second middle)
Result: `1 → 2 → 4`

### Logic / Approach

* Use slow/fast + `prev`:

  * `fast` moves 2 steps, `slow` 1 step.
  * `prev` follows slow.
* When fast ends, `slow` = middle.
* Delete by `prev.next = slow.next`.
* Edge case: only one node → return null.

**Time Complexity:** O(n)
**Space Complexity:** O(1)

### Java Code

```java
public class DeleteMiddleNode {

    static class Node {
        int data;
        Node next;
        Node(int d) { data = d; }
    }

    public static Node deleteMiddle(Node head) {
        if (head == null || head.next == null) return null;

        Node slow = head, fast = head;
        Node prev = null;

        while (fast != null && fast.next != null) {
            prev = slow;
            slow = slow.next;
            fast = fast.next.next;
        }

        // slow is middle
        prev.next = slow.next;
        return head;
    }
}
```

🔝 [Back to Top](#navigation--easy-linked-list-problems)

---

## 8. Merge Alternate Positions

### Problem

Given two singly linked lists `a` and `b`, merge nodes of `b` into `a` at **alternate positions**.
Remaining nodes of `b` (if any) stay as a separate list.

### Example

`a: 1 → 2 → 3`
`b: 4 → 5 → 6 → 7`

After merge:
`a: 1 → 4 → 2 → 5 → 3 → 6`
`b: 7`

### Logic / Approach

* Use two pointers `p1` (on a) and `p2` (on b).
* While both non-null:

  * Store `next1 = p1.next`, `next2 = p2.next`.
  * Insert `p2` after `p1`.
  * Move `p1 = next1`, `p2 = next2`.
* Return modified `a` and remaining `b`.

**Time Complexity:** O(min(len(a), len(b)))
**Space Complexity:** O(1)

### Java Code

```java
public class MergeAlternatePositions {

    static class Node {
        int data;
        Node next;
        Node(int d) { data = d; }
    }

    // returns remaining head of list b
    public static Node mergeAlternate(Node a, Node b) {
        Node p1 = a;
        Node p2 = b;

        while (p1 != null && p2 != null) {
            Node next1 = p1.next;
            Node next2 = p2.next;

            // insert p2 after p1
            p1.next = p2;
            p2.next = next1;

            // move
            p1 = next1;
            p2 = next2;
        }
        // p2 is remaining list b head
        return p2;
    }
}
```

🔝 [Back to Top](#navigation--easy-linked-list-problems)

---

## 9. Circular List Traversal

### Problem

Given the head of a **circular singly linked list**, traverse and print all elements **once**.

### Example

List: `10 → 20 → 30 → (back to 10)`
Traversal: `10 20 30`

### Logic / Approach

* If head is null → nothing.
* Use `Node curr = head`;
* Use `do { ... } while (curr != head);` to ensure each node visited exactly once.

**Time Complexity:** O(n)
**Space Complexity:** O(1)

### Java Code

```java
public class CircularListTraversal {

    static class Node {
        int data;
        Node next;
        Node(int d) { data = d; }
    }

    public static void traverse(Node head) {
        if (head == null) return;

        Node curr = head;
        do {
            System.out.print(curr.data + " ");
            curr = curr.next;
        } while (curr != head);
    }
}
```

🔝 [Back to Top](#navigation--easy-linked-list-problems)

---

## 10. Queue using Linked List

### Problem

Implement a **Queue** (FIFO) using a linked list.

Operations:

* `enqueue(x)` → insert at rear
* `dequeue()` → remove from front

### Logic / Approach

* Maintain `front` and `rear` pointers.
* Enqueue:

  * New node at `rear.next`, move `rear`.
  * If queue empty, `front = rear = newNode`.
* Dequeue:

  * Remove `front`, move `front = front.next`.
  * If becomes null, set `rear = null`.

**Time Complexity:**

* Enqueue: O(1)
* Dequeue: O(1)

**Space Complexity:** O(n) for n elements

### Java Code

```java
public class QueueUsingLinkedList {

    static class Node {
        int data;
        Node next;
        Node(int d) { data = d; }
    }

    static class MyQueue {
        Node front, rear;

        boolean isEmpty() {
            return front == null;
        }

        void enqueue(int x) {
            Node node = new Node(x);
            if (rear == null) {
                front = rear = node;
                return;
            }
            rear.next = node;
            rear = node;
        }

        Integer dequeue() {
            if (front == null) return null;
            int val = front.data;
            front = front.next;
            if (front == null) rear = null;
            return val;
        }
    }
}
```

🔝 [Back to Top](#navigation--easy-linked-list-problems)

---

## 11. Stack using singly linked list

### Problem

Implement a **Stack** (LIFO) using a singly linked list.

Operations:

* `push(x)` → insert at top
* `pop()` → remove from top

### Logic / Approach

* Maintain `top` pointer.
* Push:

  * New node points to `top`, then `top = newNode`.
* Pop:

  * Remove `top`, move `top = top.next`.

**Time Complexity:**

* Push: O(1)
* Pop: O(1)

**Space Complexity:** O(n) for n elements

### Java Code

```java
public class StackUsingLinkedList {

    static class Node {
        int data;
        Node next;
        Node(int d) { data = d; }
    }

    static class MyStack {
        Node top;

        boolean isEmpty() {
            return top == null;
        }

        void push(int x) {
            Node node = new Node(x);
            node.next = top;
            top = node;
        }

        Integer pop() {
            if (top == null) return null;
            int val = top.data;
            top = top.next;
            return val;
        }

        Integer peek() {
            return (top == null) ? null : top.data;
        }
    }
}
```

🔝 [Back to Top](#navigation--easy-linked-list-problems)

---

## 12. Pairwise Swap

### Problem

Swap **nodes of a singly linked list in pairs**.
(1st with 2nd, 3rd with 4th, etc.)

### Example

Input: `1 → 2 → 3 → 4 → 5`
Output: `2 → 1 → 4 → 3 → 5`

### Logic / Approach (swap data – simpler)

* Traverse nodes two at a time: `curr` and `curr.next`.
* Swap their `data`.
* Move `curr = curr.next.next`.

**Time Complexity:** O(n)
**Space Complexity:** O(1)

*(Interviewers sometimes want link-level swap; data-swap is okay for basic)*

### Java Code (data swap)

```java
public class PairwiseSwap {

    static class Node {
        int data;
        Node next;
        Node(int d) { data = d; }
    }

    public static Node pairwiseSwap(Node head) {
        Node curr = head;

        while (curr != null && curr.next != null) {
            int temp = curr.data;
            curr.data = curr.next.data;
            curr.next.data = temp;

            curr = curr.next.next;
        }
        return head;
    }
}
```

🔝 [Back to Top](#navigation--easy-linked-list-problems)

---

## 13. Count Occurrences

### Problem

Count how many times a **given key** occurs in a singly linked list.

### Example

List: `1 → 2 → 2 → 3 → 2`, key = 2
Output: `3`

### Logic / Approach

* Traverse list, increment counter whenever `node.data == key`.

**Time Complexity:** O(n)
**Space Complexity:** O(1)

### Java Code

```java
public class CountOccurrences {

    static class Node {
        int data;
        Node next;
        Node(int d) { data = d; }
    }

    public static int count(Node head, int key) {
        int cnt = 0;
        Node curr = head;

        while (curr != null) {
            if (curr.data == key) cnt++;
            curr = curr.next;
        }
        return cnt;
    }
}
```

🔝 [Back to Top](#navigation--easy-linked-list-problems)

---

If you want, next we can:

* Do **Medium Linked List Problems** in same format, or
* Create a **`README` for Linked List section** that links Easy + Medium + Hard sheets.
