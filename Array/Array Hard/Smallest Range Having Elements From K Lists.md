Here’s **Problem 7 – Smallest Range Having Elements From K Lists**
(Save as `07-smallest-range-k-lists.md`).

---

# Smallest Range Having Elements From K Lists (Hard – Heap + Sliding Window)

## Navigation

* [Problem Statement](#problem-statement)
* [Intuition](#intuition)
* [Algorithm](#algorithm)
* [Complexity](#complexity)
* [Java Code](#java-code)
* [Sample Input--Output](#sample-input--output)

---

## Problem Statement

You are given `k` **sorted** lists of integers.

Your task is to find the **smallest range** `[L, R]` such that **at least one number from each of the `k` lists** lies in this range.

If there are multiple smallest ranges, you can return any one of them.

---

**Example 1**

```text
Input:
lists = [
  [4, 10, 15, 24, 26],
  [0,  9, 12, 20],
  [5, 18, 22, 30]
]

Output:
[20, 24]

Explanation:
One number from each list within this range:
- 24 from list 1
- 20 from list 2
- 22 from list 3
Range length = 24 - 20 = 4 (which is minimum).
```

---

You may assume:

* `k ≥ 1`
* Each list is sorted in non-decreasing order.
* Each list has at least 1 element.

---

## Intuition

Think of it like merging `k` sorted lists, but we always maintain **one active element from each list**.

We want a window that contains:

* **Exactly k elements**, one from each list.
* The **difference** between `max` and `min` in this window should be **minimum**.

Key idea:

* Use a **min-heap** to always know the current **minimum** element.
* Track a variable `currentMax` to know the current **maximum** among chosen elements.
* The current range is `[minFromHeap, currentMax]`.
  Try to minimize this.

At every step:

1. Pop the smallest element from heap → this element defines the current `min`.
2. Update best range using `[min, currentMax]`.
3. Move **forward in the same list** from which you popped.
4. Push the next element from that list to the heap and update `currentMax`.
5. If any list runs out of elements, you **cannot** have a complete set containing all lists → stop.

---

## Algorithm

1. Use a min-heap storing:

   ```text
   (value, listIndex, indexInThatList)
   ```
2. Initialize:

   * Push the **first element** of each list into the heap.
   * Track `currentMax` = maximum among these first elements.
   * `bestStart = 0`, `bestEnd = +∞` initially.
3. While heap size is `k`:

   * Pop root → this gives `minVal` and which list it came from.
   * Current range = `[minVal, currentMax]`

     * If `(currentMax - minVal) < (bestEnd - bestStart)`, update best range.
   * Move to **next element** in that same list:

     * If no next element exists → break the loop (we can’t cover all lists anymore).
     * Otherwise:

       * Push the next element in that list into heap.
       * Update `currentMax = max(currentMax, nextVal)`.
4. Return `[bestStart, bestEnd]`.

---

## Complexity

Let `N` = total number of elements across all lists, and `k` = number of lists.

| Type  | Complexity                                |
| ----- | ----------------------------------------- |
| Time  | `O(N log k)` (heap operations)            |
| Space | `O(k)` (heap stores one element per list) |

---

## Java Code

```java
import java.util.*;

public class SmallestRangeKLists {

    // Helper class to store value + its location
    static class Node {
        int value;
        int row; // which list
        int idx; // index within that list

        Node(int value, int row, int idx) {
            this.value = value;
            this.row = row;
            this.idx = idx;
        }
    }

    // Returns [L, R] as the smallest range
    public static int[] smallestRange(List<List<Integer>> nums) {
        // Min-heap based on node.value
        PriorityQueue<Node> pq = new PriorityQueue<>(
            Comparator.comparingInt(a -> a.value)
        );

        int currentMax = Integer.MIN_VALUE;

        // Step 1: Put first element of each list into heap
        for (int i = 0; i < nums.size(); i++) {
            int val = nums.get(i).get(0);
            pq.offer(new Node(val, i, 0));
            currentMax = Math.max(currentMax, val);
        }

        int rangeStart = 0;
        int rangeEnd = Integer.MAX_VALUE;

        // Step 2: Process heap
        while (pq.size() == nums.size()) {
            Node curr = pq.poll();
            int minVal = curr.value;

            // Update best range if current one is smaller
            if (currentMax - minVal < rangeEnd - rangeStart) {
                rangeStart = minVal;
                rangeEnd = currentMax;
            }

            // Move to the next element in the same list
            int nextIdx = curr.idx + 1;
            if (nextIdx < nums.get(curr.row).size()) {
                int nextVal = nums.get(curr.row).get(nextIdx);
                pq.offer(new Node(nextVal, curr.row, nextIdx));

                // Update current max
                if (nextVal > currentMax) {
                    currentMax = nextVal;
                }
            } else {
                // One of the lists is exhausted -> can't maintain k elements
                break;
            }
        }

        return new int[]{rangeStart, rangeEnd};
    }

    // Demo
    public static void main(String[] args) {
        List<List<Integer>> lists = new ArrayList<>();
        lists.add(Arrays.asList(4, 10, 15, 24, 26));
        lists.add(Arrays.asList(0, 9, 12, 20));
        lists.add(Arrays.asList(5, 18, 22, 30));

        int[] ans = smallestRange(lists);
        System.out.println("[" + ans[0] + ", " + ans[1] + "]"); // [20, 24]
    }
}
```

---

## Sample Input–Output

```text
Input:
lists = [
  [4, 10, 15, 24, 26],
  [0,  9, 12, 20],
  [5, 18, 22, 30]
]

Output:
[20, 24]
```

---

Say **“next”** for
**8. Count Subarrays with K Distinct Elements** 🧮📊
