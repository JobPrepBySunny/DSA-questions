Here’s **Problem 8 – Count Subarrays with K Distinct Elements**
(Save as `08-count-subarrays-k-distinct.md`).

---

# Count Subarrays with K Distinct Elements (Hard – Sliding Window)

## Navigation

* [Problem Statement](#problem-statement)
* [Intuition](#intuition)
* [Key Idea: AtMost(K) Trick](#key-idea-atmostk-trick)
* [Algorithm](#algorithm)
* [Complexity](#complexity)
* [Java Code](#java-code)
* [Sample Input--Output](#sample-input--output)

---

## Problem Statement

You are given an integer array `nums` and an integer `k`.

A **subarray** is a contiguous part of the array.

> **Task:**
> Return the **number of subarrays** that contain **exactly `k` distinct integers**.

---

**Example 1**

```text
Input:  nums = [1, 2, 1, 2, 3], k = 2
Output: 7

Explanation:
The 7 subarrays are:
[1,2], [2,1], [1,2], [2,1,2], [1,2,3], [2,1,2], [1,2]
```

**Example 2**

```text
Input:  nums = [1, 2, 1, 3, 4], k = 3
Output: 3

Subarrays with exactly 3 distinct: 
[1,2,1,3], [2,1,3], [1,3,4]
```

Constraints (typical):

* `1 ≤ nums.length ≤ 2 * 10^5`
* `1 ≤ nums[i] ≤ 10^9`
* `1 ≤ k ≤ nums.length`

---

## Intuition

Counting subarrays with **exactly** `k` distinct directly is hard.

But counting subarrays with **at most** `k` distinct is easier using a sliding window.

Then we use this identity:

```text
#subarrays with exactly k distinct
  = #subarrays with at most k distinct
  - #subarrays with at most (k - 1) distinct
```

So the main problem reduces to:

> How do we count subarrays with **at most X distinct**?

---

## Key Idea: AtMost(K) Trick

We maintain a sliding window `[left, right]` and a frequency map:

* Expand `right` step by step.
* For each `right`, add `nums[right]` to map.
* If distinct count becomes more than `k`, shrink from `left` until distinct ≤ `k` again.
* For each `right`, the number of valid subarrays ending at `right` is:

  ```text
  (right - left + 1)
  ```

  because any starting index from `left` to `right` gives a valid subarray.

We implement a helper:

```java
int atMost(int[] nums, int k)
```

Then answer is:

```java
atMost(nums, k) - atMost(nums, k - 1)
```

---

## Algorithm

1. Implement `atMost(nums, k)`:

   * Use `Map<Integer, Integer> freq` for element counts.
   * `left = 0`, `count = 0`.
   * For each `right` from `0` to `n-1`:

     * Increase freq of `nums[right]`.
     * If freq of this element becomes 1 → new distinct → `k--`.
     * While `k < 0`:

       * Decrease freq of `nums[left]`.
       * If freq becomes 0 → one distinct removed → `k++`.
       * Move `left++`.
     * Add `(right - left + 1)` to `count`.
   * Return `count`.

2. Main function:

   ```java
   return atMost(nums, k) - atMost(nums, k - 1);
   ```

---

## Complexity

| Type  | Complexity                                            |
| ----- | ----------------------------------------------------- |
| Time  | `O(n)` (each index enters and leaves the window once) |
| Space | `O(n)` in worst case (for frequency map)              |

---

## Java Code

```java
import java.util.*;

public class CountSubarraysKDistinct {

    // Main function: exactly k distinct
    public static int subarraysWithKDistinct(int[] nums, int k) {
        return atMost(nums, k) - atMost(nums, k - 1);
    }

    // Helper: count subarrays with at most k distinct integers
    private static int atMost(int[] nums, int k) {
        Map<Integer, Integer> freq = new HashMap<>();
        int left = 0;
        int count = 0;

        for (int right = 0; right < nums.length; right++) {
            int x = nums[right];
            freq.put(x, freq.getOrDefault(x, 0) + 1);

            // New distinct element
            if (freq.get(x) == 1) {
                k--;
            }

            // Shrink window until we have at most k distinct
            while (k < 0) {
                int y = nums[left];
                freq.put(y, freq.get(y) - 1);
                if (freq.get(y) == 0) {
                    k++;
                }
                left++;
            }

            // All subarrays ending at right with valid distinct count
            count += right - left + 1;
        }

        return count;
    }

    // Demo
    public static void main(String[] args) {
        int[] nums1 = {1, 2, 1, 2, 3};
        int k1 = 2;
        System.out.println(subarraysWithKDistinct(nums1, k1)); // 7

        int[] nums2 = {1, 2, 1, 3, 4};
        int k2 = 3;
        System.out.println(subarraysWithKDistinct(nums2, k2)); // 3
    }
}
```

---

## Sample Input–Output

```text
Input:
nums = [1, 2, 1, 2, 3], k = 2
Output:
7
```

```text
Input:
nums = [1, 2, 1, 3, 4], k = 3
Output:
3
```

---

Say **“next”** for
**9. Next Smallest Palindrome** 🔢✨
