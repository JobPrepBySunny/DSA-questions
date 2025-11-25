Here you go, Sahil — **Medium Linked List Problems (Interview Mode)** with:

* Navigation at top
* Each problem: **Question → Example → Logic → Time & Space Complexity → Java Code**
* “Back to Top” links

You can save this as: `LinkedList/Medium-Problems.md`

---

# Navigation – Medium Linked List Problems

* [Detect Loop](#1-detect-loop)
* [Length of the Loop](#2-length-of-the-loop)
* [Reverse in groups](#3-reverse-in-groups)
* [Intersection Point](#4-intersection-point)
* [Delete without Head pointer](#5-delete-without-head-pointer)
* [Merge two sorted linked lists](#6-merge-two-sorted-linked-lists)
* [Sort a List of 0s, 1s and 2s](#7-sort-a-list-of-0s-1s-and-2s)
* [Palindrome Linked List](#8-palindrome-linked-list)
* [Remove all occurrences of a given list](#9-remove-all-occurrences-of-a-given-list)
* [Split a Circular Linked List into two halves](#10-split-a-circular-linked-list-into-two-halves)
* [Pair Sum in Doubly Linked List](#11-pair-sum-in-doubly-linked-list)
* [Add two numbers represented by Linked lists](#12-add-two-numbers-represented-by-linked-lists)
* [Multiply two numbers represented by Linked Lists](#13-multiply-two-numbers-represented-by-linked-lists)
* [Swap Kth nodes from beginning and end](#14-swap-kth-nodes-from-beginning-and-end)
* [Rotate Doubly linked list by N nodes](#15-rotate-doubly-linked-list-by-n-nodes)
* [Binary Tree to Doubly Linked List](#16-binary-tree-to-doubly-linked-list)
* [Linked List from a 2D matrix](#17-linked-list-from-a-2d-matrix)
* [Reverse a Sublist](#18-reverse-a-sublist)
* [Delete N nodes after M](#19-delete-n-nodes-after-m)
* [Rearrange a given linked list in-place](#20-rearrange-a-given-linked-list-in-place)
* [Partition around a given value](#21-partition-around-a-given-value)

---

## 1. Detect Loop

### Problem

Given head of a singly linked list, **detect if there is a cycle (loop)**.

### Logic / Approach (Floyd’s Cycle Detection)

* Use two pointers:

  * `slow` moves 1 step, `fast` moves 2 steps.
* If at any time `slow == fast`, there is a loop.
* If `fast` or `fast.next` becomes null → no loop.

**Time Complexity:** O(n)
**Space Complexity:** O(1)

### Java Code

```java
public class DetectLoop {

    static class Node {
        int data;
        Node next;
        Node(int d) { data = d; }
    }

    public static boolean hasLoop(Node head) {
        if (head == null) return false;

        Node slow = head, fast = head;
        while (fast != null && fast.next != null) {
            slow = slow.next;
            fast = fast.next.next;

            if (slow == fast) return true;
        }
        return false;
    }
}
```

🔝 [Back to Top](#navigation--medium-linked-list-problems)

---

## 2. Length of the Loop

### Problem

If a loop exists in a linked list, find **number of nodes in the loop**.

### Logic / Approach

* First detect meeting point using Floyd’s algorithm.
* From meeting node, move once around the cycle counting nodes until you come back.

**Time Complexity:** O(n)
**Space Complexity:** O(1)

### Java Code

```java
public class LengthOfLoop {

    static class Node {
        int data;
        Node next;
        Node(int d) { data = d; }
    }

    public static int loopLength(Node head) {
        Node slow = head, fast = head;

        while (fast != null && fast.next != null) {
            slow = slow.next;
            fast = fast.next.next;

            if (slow == fast) {
                // count nodes in loop
                int len = 1;
                Node cur = slow.next;
                while (cur != slow) {
                    len++;
                    cur = cur.next;
                }
                return len;
            }
        }
        return 0; // no loop
    }
}
```

🔝 [Back to Top](#navigation--medium-linked-list-problems)

---

## 3. Reverse in groups

### Problem

Given a linked list and integer `k`, reverse nodes of the list in **groups of size k**.

### Example

`1 → 2 → 3 → 4 → 5 → 6`, k = 3
Output: `3 → 2 → 1 → 6 → 5 → 4`

### Logic / Approach

* Reverse first `k` nodes like normal reverse.
* Recursively/iteratively process rest list; connect tail of reversed part to head of next reversed part.

**Time Complexity:** O(n)
**Space Complexity:** O(1) iterative, O(n/k) recursion stack if recursive

### Java Code

```java
public class ReverseInGroups {

    static class Node {
        int data;
        Node next;
        Node(int d) { data = d; }
    }

    public static Node reverseKGroup(Node head, int k) {
        if (head == null || k <= 1) return head;

        Node curr = head;
        int count = 0;

        // check if there are at least k nodes
        while (curr != null && count < k) {
            curr = curr.next;
            count++;
        }
        if (count < k) return head;

        // reverse first k nodes
        curr = head;
        Node prev = null, next = null;
        count = 0;
        while (curr != null && count < k) {
            next = curr.next;
            curr.next = prev;
            prev = curr;
            curr = next;
            count++;
        }

        // head is now tail of reversed group
        head.next = reverseKGroup(curr, k);
        return prev; // new head
    }
}
```

🔝 [Back to Top](#navigation--medium-linked-list-problems)

---

## 4. Intersection Point

### Problem

Given heads of two singly linked lists, find the **intersection node** (same reference), if it exists.

### Logic / Approach (2-pointer trick)

* Let pointers `p1 = head1`, `p2 = head2`.
* Move both by 1 node, when one reaches end, redirect it to other list’s head.
* If they meet, that node is intersection; if both become null → no intersection.

**Time Complexity:** O(m + n)
**Space Complexity:** O(1)

### Java Code

```java
public class IntersectionPoint {

    static class Node {
        int data;
        Node next;
        Node(int d) { data = d; }
    }

    public static Node getIntersection(Node headA, Node headB) {
        if (headA == null || headB == null) return null;

        Node p1 = headA, p2 = headB;

        while (p1 != p2) {
            p1 = (p1 == null) ? headB : p1.next;
            p2 = (p2 == null) ? headA : p2.next;
        }
        return p1; // either intersection or null
    }
}
```

🔝 [Back to Top](#navigation--medium-linked-list-problems)

---

## 5. Delete without Head pointer

### Problem

Given only a **pointer to a node** (not head), delete that node from the singly linked list.

> Assume node is **not the last node**.

### Logic / Approach

* Copy `node.next.data` to `node.data`.
* Bypass next node: `node.next = node.next.next`.

**Time Complexity:** O(1)
**Space Complexity:** O(1)

### Java Code

```java
public class DeleteWithoutHeadPointer {

    static class Node {
        int data;
        Node next;
        Node(int d) { data = d; }
    }

    public static void deleteNode(Node node) {
        if (node == null || node.next == null) return; // can't delete last

        node.data = node.next.data;
        node.next = node.next.next;
    }
}
```

🔝 [Back to Top](#navigation--medium-linked-list-problems)

---

## 6. Merge two sorted linked lists

### Problem

Given two **sorted** singly linked lists, merge them into one sorted list.

### Logic / Approach (Iterative merge)

* Use dummy node and tail pointer.
* Compare head nodes, attach smaller to tail and move that list’s pointer.
* Continue until one list ends; attach remaining part.

**Time Complexity:** O(m + n)
**Space Complexity:** O(1) extra

### Java Code

```java
public class MergeTwoSortedLists {

    static class Node {
        int data;
        Node next;
        Node(int d) { data = d; }
    }

    public static Node merge(Node a, Node b) {
        Node dummy = new Node(0);
        Node tail = dummy;

        while (a != null && b != null) {
            if (a.data <= b.data) {
                tail.next = a;
                a = a.next;
            } else {
                tail.next = b;
                b = b.next;
            }
            tail = tail.next;
        }
        if (a != null) tail.next = a;
        else tail.next = b;

        return dummy.next;
    }
}
```

🔝 [Back to Top](#navigation--medium-linked-list-problems)

---

## 7. Sort a List of 0s, 1s and 2s

### Problem

Given a linked list with nodes containing only `0`, `1`, and `2`, sort the list.

### Logic / Approach (counting)

* First pass: count number of 0s, 1s, 2s.
* Second pass: overwrite node data in order `0s`, then `1s`, then `2s`.

**Time Complexity:** O(n)
**Space Complexity:** O(1)

### Java Code

```java
public class Sort012List {

    static class Node {
        int data;
        Node next;
        Node(int d) { data = d; }
    }

    public static Node sort012(Node head) {
        int[] count = new int[3];
        Node curr = head;

        while (curr != null) {
            count[curr.data]++;
            curr = curr.next;
        }

        curr = head;
        int i = 0;
        while (curr != null) {
            while (i < 3 && count[i] == 0) i++;
            curr.data = i;
            count[i]--;
            curr = curr.next;
        }
        return head;
    }
}
```

🔝 [Back to Top](#navigation--medium-linked-list-problems)

---

## 8. Palindrome Linked List

### Problem

Check if a singly linked list is a **palindrome**.

### Logic / Approach

* Find middle using slow/fast.
* Reverse second half.
* Compare first half and reversed second half.
* (Optional: restore list.)

**Time Complexity:** O(n)
**Space Complexity:** O(1)

### Java Code

```java
public class PalindromeLinkedList {

    static class Node {
        int data;
        Node next;
        Node(int d) { data = d; }
    }

    public static boolean isPalindrome(Node head) {
        if (head == null || head.next == null) return true;

        Node slow = head, fast = head;

        while (fast != null && fast.next != null) {
            slow = slow.next;
            fast = fast.next.next;
        }

        Node second = reverse(slow);
        Node first = head;

        Node copySecond = second; // if you want to restore later

        boolean result = true;
        while (second != null) {
            if (first.data != second.data) {
                result = false;
                break;
            }
            first = first.next;
            second = second.next;
        }

        // Optional: reverse(copySecond) to restore list
        return result;
    }

    private static Node reverse(Node head) {
        Node prev = null, curr = head;
        while (curr != null) {
            Node next = curr.next;
            curr.next = prev;
            prev = curr;
            curr = next;
        }
        return prev;
    }
}
```

🔝 [Back to Top](#navigation--medium-linked-list-problems)

---

## 9. Remove all occurrences of a given list

> I’ll assume this means:
> **Given a list `head` and a pattern list `patHead`, remove all occurrences of the pattern sublist from the main list.**

### Example

Main: `1 → 2 → 3 → 2 → 3 → 4`, Pattern: `2 → 3`
Output: `1 → 4`

### Logic / Approach

* Use dummy node before head.
* From each node `prev.next`, check if pattern matches starting there.
* If it matches:

  * Skip those nodes: `prev.next = endNext`.
* Else:

  * `prev = prev.next`.

**Time Complexity:** O(n * m) in worst case (n=list length, m=pattern length)
**Space Complexity:** O(1)

### Java Code

```java
public class RemoveAllOccurrencesOfList {

    static class Node {
        int data;
        Node next;
        Node(int d) { data = d; }
    }

    private static boolean match(Node start, Node pat) {
        while (start != null && pat != null) {
            if (start.data != pat.data) return false;
            start = start.next;
            pat = pat.next;
        }
        return pat == null; // pattern fully matched
    }

    public static Node removeAll(Node head, Node patHead) {
        if (head == null || patHead == null) return head;

        Node dummy = new Node(0);
        dummy.next = head;
        Node prev = dummy;

        while (prev.next != null) {
            if (match(prev.next, patHead)) {
                // skip len(pattern) nodes
                Node temp = prev.next;
                Node p = patHead;
                while (p != null && temp != null) {
                    temp = temp.next;
                    p = p.next;
                }
                prev.next = temp;
            } else {
                prev = prev.next;
            }
        }
        return dummy.next;
    }
}
```

🔝 [Back to Top](#navigation--medium-linked-list-problems)

---

## 10. Split a Circular Linked List into two halves

### Problem

Given a **circular singly linked list**, split it into **two circular lists** of (almost) equal size.

### Logic / Approach

* Use slow/fast pointers (like middle).
* `slow` at mid, `fast` at last.
* First half: `head1 = head`, last node of first = `slow`.
* Second half: `head2 = slow.next`.
* Fix `slow.next = head1` and `fast.next = head2`.

**Time Complexity:** O(n)
**Space Complexity:** O(1)

### Java Code

```java
public class SplitCircularList {

    static class Node {
        int data;
        Node next;
        Node(int d) { data = d; }
    }

    static class Result {
        Node head1, head2;
    }

    public static Result split(Node head) {
        Result res = new Result();
        if (head == null || head.next == head) {
            res.head1 = head;
            res.head2 = null;
            return res;
        }

        Node slow = head, fast = head;

        while (fast.next != head && fast.next.next != head) {
            slow = slow.next;
            fast = fast.next.next;
        }

        Node head1 = head;
        Node head2 = slow.next;

        // make first half circular
        slow.next = head1;

        // make second half circular
        if (fast.next == head) fast.next = head2;
        else fast.next.next = head2;

        res.head1 = head1;
        res.head2 = head2;
        return res;
    }
}
```

🔝 [Back to Top](#navigation--medium-linked-list-problems)

---

## 11. Pair Sum in Doubly Linked List

### Problem

Given a **sorted doubly linked list** and integer `x`, find all **pairs of nodes** with sum `x`.

### Logic / Approach

* Use two pointers:

  * `left` at head, `right` at tail.
* While `left != right` and `left.prev != right`:

  * If `left.data + right.data == x` → record pair, move both.
  * If sum < x → move `left`.
  * If sum > x → move `right`.

**Time Complexity:** O(n)
**Space Complexity:** O(1) (apart from output)

### Java Code

```java
import java.util.*;

public class PairSumDLL {

    static class Node {
        int data;
        Node prev, next;
        Node(int d) { data = d; }
    }

    public static List<int[]> pairSum(Node head, int x) {
        List<int[]> res = new ArrayList<>();
        if (head == null) return res;

        Node left = head;
        Node right = head;
        while (right.next != null) right = right.next; // go to tail

        while (left != null && right != null && left != right && right.next != left) {
            int sum = left.data + right.data;
            if (sum == x) {
                res.add(new int[]{left.data, right.data});
                left = left.next;
                right = right.prev;
            } else if (sum < x) {
                left = left.next;
            } else {
                right = right.prev;
            }
        }
        return res;
    }
}
```

🔝 [Back to Top](#navigation--medium-linked-list-problems)

---

## 12. Add two numbers represented by Linked lists

### Problem

Each linked list represents a non-negative integer where **digits are stored in reverse order** (1’s digit first).
Add them and return sum as a linked list (also in reverse).

### Example

`(2 → 4 → 3)` + `(5 → 6 → 4)` = `342 + 465 = 807`
Output: `7 → 0 → 8`

### Logic / Approach

* Use carry like column-wise addition.
* Traverse both lists, sum digits + carry; create new nodes.

**Time Complexity:** O(max(m, n))
**Space Complexity:** O(max(m, n))

### Java Code

```java
public class AddTwoNumbersLists {

    static class Node {
        int data;
        Node next;
        Node(int d) { data = d; }
    }

    public static Node addTwoNumbers(Node l1, Node l2) {
        Node dummy = new Node(0);
        Node tail = dummy;
        int carry = 0;

        while (l1 != null || l2 != null || carry != 0) {
            int x = (l1 != null) ? l1.data : 0;
            int y = (l2 != null) ? l2.data : 0;

            int sum = x + y + carry;
            carry = sum / 10;

            tail.next = new Node(sum % 10);
            tail = tail.next;

            if (l1 != null) l1 = l1.next;
            if (l2 != null) l2 = l2.next;
        }

        return dummy.next;
    }
}
```

🔝 [Back to Top](#navigation--medium-linked-list-problems)

---

## 13. Multiply two numbers represented by Linked Lists

### Problem

Two linked lists represent two non-negative integers (digits in forward or reverse).
To keep it simple and safe for large numbers, we’ll:

* Convert to strings
* Use `BigInteger` to multiply
* Return result as a linked list (digits in forward order).

### Logic / Approach

* Read digits into a string.
* Use `BigInteger` for multiplication.
* Create list from result string.

**Time Complexity:** O(m + n) for reading + BigInteger multiply cost
**Space Complexity:** O(m + n)

### Java Code

```java
import java.math.BigInteger;

public class MultiplyTwoNumbersLists {

    static class Node {
        int data;
        Node next;
        Node(int d) { data = d; }
    }

    private static String listToString(Node head) {
        StringBuilder sb = new StringBuilder();
        while (head != null) {
            sb.append(head.data);
            head = head.next;
        }
        return sb.toString();
    }

    private static Node stringToList(String s) {
        if (s.length() == 0) return null;
        Node head = new Node(s.charAt(0) - '0');
        Node curr = head;
        for (int i = 1; i < s.length(); i++) {
            curr.next = new Node(s.charAt(i) - '0');
            curr = curr.next;
        }
        return head;
    }

    public static Node multiply(Node l1, Node l2) {
        String a = listToString(l1);
        String b = listToString(l2);

        BigInteger A = new BigInteger(a);
        BigInteger B = new BigInteger(b);
        BigInteger C = A.multiply(B);

        return stringToList(C.toString());
    }
}
```

🔝 [Back to Top](#navigation--medium-linked-list-problems)

---

## 14. Swap Kth nodes from beginning and end

### Problem

Given a singly linked list and integer `k`, **swap Kth node from start with Kth node from end**.

For simplicity, we will **swap data** (not whole nodes).

### Logic / Approach

* Find length `n`.
* If `k > n`, do nothing.
* `kthFromStart` is k-th node.
* `kthFromEnd` is (n-k+1)-th node.
* Swap their data.

**Time Complexity:** O(n)
**Space Complexity:** O(1)

### Java Code

```java
public class SwapKthNodes {

    static class Node {
        int data;
        Node next;
        Node(int d) { data = d; }
    }

    public static Node swapKth(Node head, int k) {
        if (head == null || k <= 0) return head;

        int n = 0;
        Node curr = head;
        while (curr != null) {
            n++;
            curr = curr.next;
        }
        if (k > n) return head;
        if (2 * k - 1 == n) return head; // same node

        Node x = head, y = head;
        for (int i = 1; i < k; i++) x = x.next;
        for (int i = 1; i < n - k + 1; i++) y = y.next;

        int temp = x.data;
        x.data = y.data;
        y.data = temp;

        return head;
    }
}
```

🔝 [Back to Top](#navigation--medium-linked-list-problems)

---

## 15. Rotate Doubly linked list by N nodes

### Problem

Given a doubly linked list, rotate it **left by N nodes**.

### Example

`1 ⇄ 2 ⇄ 3 ⇄ 4 ⇄ 5`, N=2
Output: `3 ⇄ 4 ⇄ 5 ⇄ 1 ⇄ 2`

### Logic / Approach

* Find length and tail.
* Do `N = N % len`.
* Move `curr` N nodes from head (this becomes new head).
* Adjust pointers:

  * `curr.prev.next = null`, `curr.prev = null`
  * `tail.next = oldHead`, `oldHead.prev = tail`

**Time Complexity:** O(n)
**Space Complexity:** O(1)

### Java Code

```java
public class RotateDLL {

    static class Node {
        int data;
        Node prev, next;
        Node(int d) { data = d; }
    }

    public static Node rotate(Node head, int N) {
        if (head == null || N == 0) return head;

        Node tail = head;
        int len = 1;
        while (tail.next != null) {
            tail = tail.next;
            len++;
        }

        N = N % len;
        if (N == 0) return head;

        Node curr = head;
        for (int i = 0; i < N; i++) {
            curr = curr.next;
        }

        Node newHead = curr;
        Node prev = curr.prev;

        prev.next = null;
        newHead.prev = null;

        tail.next = head;
        head.prev = tail;

        return newHead;
    }
}
```

🔝 [Back to Top](#navigation--medium-linked-list-problems)

---

## 16. Binary Tree to Doubly Linked List

### Problem

Convert a **binary tree** to a **sorted doubly linked list** using **inorder traversal**.
Left pointer = `prev`, right pointer = `next`.

### Logic / Approach

* Inorder traversal:

  * Maintain `prev` node.
  * Connect `prev.right = current`, `current.left = prev`.
* Track head (leftmost node).

**Time Complexity:** O(n)
**Space Complexity:** O(h) recursion stack (h = height)

### Java Code

```java
public class BinaryTreeToDLL {

    static class Node {
        int data;
        Node left, right; // left = prev, right = next in DLL
        Node(int d) { data = d; }
    }

    private static Node prev = null;
    private static Node head = null;

    public static Node bToDLL(Node root) {
        prev = null;
        head = null;
        inorder(root);
        return head;
    }

    private static void inorder(Node root) {
        if (root == null) return;

        inorder(root.left);

        if (prev == null) {
            head = root;
        } else {
            prev.right = root;
            root.left = prev;
        }
        prev = root;

        inorder(root.right);
    }
}
```

🔝 [Back to Top](#navigation--medium-linked-list-problems)

---

## 17. Linked List from a 2D matrix

### Problem

Given an `n x m` matrix, create a **linked structure** where each node has:

* `right` pointer to next element in row
* `down` pointer to element in next row (same column)

### Logic / Approach

* Create `Node` for each cell.
* For each cell `(i, j)`:

  * `node[i][j].right = node[i][j+1]` (if j+1 < m)
  * `node[i][j].down  = node[i+1][j]` (if i+1 < n)

**Time Complexity:** O(n * m)
**Space Complexity:** O(n * m)

### Java Code

```java
public class LinkedListFrom2DMatrix {

    static class Node {
        int data;
        Node right, down;
        Node(int d) { data = d; }
    }

    public static Node construct(int[][] mat) {
        int n = mat.length;
        if (n == 0) return null;
        int m = mat[0].length;

        Node[][] nodes = new Node[n][m];

        // create all nodes
        for (int i = 0; i < n; i++) {
            for (int j = 0; j < m; j++) {
                nodes[i][j] = new Node(mat[i][j]);
            }
        }

        // set right and down pointers
        for (int i = 0; i < n; i++) {
            for (int j = 0; j < m; j++) {
                if (j + 1 < m) nodes[i][j].right = nodes[i][j + 1];
                if (i + 1 < n) nodes[i][j].down = nodes[i + 1][j];
            }
        }
        return nodes[0][0];
    }
}
```

🔝 [Back to Top](#navigation--medium-linked-list-problems)

---

## 18. Reverse a Sublist

### Problem

Reverse the nodes of a singly linked list from position `left` to `right` (1-based), in-place.

### Example

List: `1 → 2 → 3 → 4 → 5`, left=2, right=4
Output: `1 → 4 → 3 → 2 → 5`

### Logic / Approach

* Use dummy before head.
* Move `prev` to node before `left`.
* Reverse `right-left+1` nodes using standard reverse logic.
* Connect `prev.next` to new sublist head; original `start` to remaining list.

**Time Complexity:** O(n)
**Space Complexity:** O(1)

### Java Code

```java
public class ReverseSublist {

    static class Node {
        int data;
        Node next;
        Node(int d) { data = d; }
    }

    public static Node reverseBetween(Node head, int left, int right) {
        if (head == null || left == right) return head;

        Node dummy = new Node(0);
        dummy.next = head;

        Node prev = dummy;
        for (int i = 1; i < left; i++) {
            prev = prev.next;
        }

        Node curr = prev.next;
        Node subTail = curr;
        Node next = null;
        Node revPrev = null;

        int count = right - left + 1;
        while (count-- > 0 && curr != null) {
            next = curr.next;
            curr.next = revPrev;
            revPrev = curr;
            curr = next;
        }

        prev.next = revPrev;
        subTail.next = curr;

        return dummy.next;
    }
}
```

🔝 [Back to Top](#navigation--medium-linked-list-problems)

---

## 19. Delete N nodes after M

### Problem

Given a linked list and integers `M` and `N`, **skip M nodes**, then **delete N nodes**, and repeat the process till end.

### Logic / Approach

* Use a pointer `curr = head`.
* While `curr` not null:

  * Skip `M-1` nodes.
  * Then delete next `N` nodes by moving and adjusting links.

**Time Complexity:** O(n)
**Space Complexity:** O(1)

### Java Code

```java
public class DeleteNAfterM {

    static class Node {
        int data;
        Node next;
        Node(int d) { data = d; }
    }

    public static Node deleteNafterM(Node head, int M, int N) {
        if (head == null || N == 0) return head;
        if (M == 0) return null; // delete all

        Node curr = head;

        while (curr != null) {
            // skip M-1 nodes
            for (int i = 1; i < M && curr != null; i++) {
                curr = curr.next;
            }
            if (curr == null) break;

            // delete next N nodes
            Node temp = curr.next;
            for (int i = 0; i < N && temp != null; i++) {
                temp = temp.next;
            }
            curr.next = temp;
            curr = temp;
        }
        return head;
    }
}
```

🔝 [Back to Top](#navigation--medium-linked-list-problems)

---

## 20. Rearrange a given linked list in-place

### Problem

Rearrange list as:
`L0 → L1 → ... → Ln-1 → Ln` → `L0 → Ln → L1 → Ln-1 → L2 → Ln-2 → ...`

### Example

`1 → 2 → 3 → 4 → 5` → `1 → 5 → 2 → 4 → 3`

### Logic / Approach

* Find middle.
* Reverse second half.
* Merge first half and reversed second half alternately.

**Time Complexity:** O(n)
**Space Complexity:** O(1)

### Java Code

```java
public class RearrangeLinkedListInPlace {

    static class Node {
        int data;
        Node next;
        Node(int d) { data = d; }
    }

    public static void rearrange(Node head) {
        if (head == null || head.next == null) return;

        // find middle
        Node slow = head, fast = head;
        while (fast != null && fast.next != null && fast.next.next != null) {
            slow = slow.next;
            fast = fast.next.next;
        }

        // reverse second half
        Node second = reverse(slow.next);
        slow.next = null;

        // merge
        Node first = head;
        while (first != null && second != null) {
            Node fNext = first.next;
            Node sNext = second.next;

            first.next = second;
            second.next = fNext;

            first = fNext;
            second = sNext;
        }
    }

    private static Node reverse(Node head) {
        Node prev = null, curr = head;
        while (curr != null) {
            Node next = curr.next;
            curr.next = prev;
            prev = curr;
            curr = next;
        }
        return prev;
    }
}
```

🔝 [Back to Top](#navigation--medium-linked-list-problems)

---

## 21. Partition around a given value

### Problem

Given a linked list and value `x`, rearrange list so that:

* All nodes `< x` come before nodes `>= x`
* **Relative order** within each group is preserved.

### Logic / Approach (stable partition)

* Maintain two dummy lists:

  * `before` for `< x`
  * `after` for `>= x`
* Traverse original list, append to correct list.
* Finally connect `beforeTail.next = afterHead`.

**Time Complexity:** O(n)
**Space Complexity:** O(1) extra (just a few pointers)

### Java Code

```java
public class PartitionListAroundValue {

    static class Node {
        int data;
        Node next;
        Node(int d) { data = d; }
    }

    public static Node partition(Node head, int x) {
        Node beforeDummy = new Node(0);
        Node afterDummy = new Node(0);

        Node before = beforeDummy;
        Node after = afterDummy;

        Node curr = head;
        while (curr != null) {
            if (curr.data < x) {
                before.next = curr;
                before = before.next;
            } else {
                after.next = curr;
                after = after.next;
            }
            curr = curr.next;
        }

        after.next = null;
        before.next = afterDummy.next;

        return beforeDummy.next;
    }
}
```

🔝 [Back to Top](#navigation--medium-linked-list-problems)

---

If you want, next step we can:

* Do **Hard Linked List Problems** in same format
* Or build a **main `README.md`** linking all Easy/Medium/Hard for Arrays, Strings, Linked Lists so your GitHub looks like a full DSA sheet.
