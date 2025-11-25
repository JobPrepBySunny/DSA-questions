Here’s **Problem 3** formatted for GitHub (save as `03-smallest-missing-positive.md`).

---

# Smallest Missing Positive Number (Hard)

## Navigation

* [Problem Statement](#problem-statement)
* [Why is it Hard?](#why-is-it-hard)
* [Intuition](#intuition)
* [Algorithm](#algorithm)
* [Complexity](#complexity)
* [Java Code](#java-code)
* [Sample Input--Output](#sample-input--output)

---

## Problem Statement

Given an unsorted array `nums`, return the **smallest missing positive** integer.

Only positive integers matter (1, 2, 3, …).
Your solution **must** run in:

* **O(n)** time
* **O(1)** extra space

> **Task:**
> Implement a function to find the smallest missing positive number.

---

**Example 1**

```text
Input:  nums = [1,2,0]
Output: 3
Explanation:
1 and 2 exist, smallest missing = 3
```

**Example 2**

```text
Input:  nums = [3,4,-1,1]
Output: 2
```

**Example 3**

```text
Input:  nums = [7,8,9,11,12]
Output: 1
```

Constraints:

* `1 ≤ nums.length ≤ 10^5`
* `-10^5 ≤ nums[i] ≤ 10^5`

---

## Why is it Hard?

You must solve with:

* **No sorting**
* **No hash sets**
* Constant space
  → Forces a smart in-place approach!

---

## Intuition

The smallest missing positive number must be in **range** `[1, n+1]`
(where `n = nums.length`) because:

* If array contains `1..n` → answer = `n + 1`
* Else → the first missing between `1..n`

So we place each number **x** in index `x-1`
Example:

```
Index: 0 1 2 3
Nums:  3 4 -1 1
Goal:  1 2 3 4
```

We keep swapping until each valid number is in its correct spot.

Then scan array:
First index where `nums[i] != i + 1` → answer is `i + 1`.

---

## Algorithm

1. Let `n = nums.length`
2. For each index `i`:

   * While `nums[i]` is in `[1, n]` and not in correct index,
     swap `nums[i]` ↔ `nums[nums[i] - 1]`
3. Scan array:

   * First index `i` where `nums[i] != i + 1` → return `i + 1`
4. If all matched → return `n + 1`

---

## Complexity

| Type  | Complexity |
| ----- | ---------- |
| Time  | `O(n)`     |
| Space | `O(1)`     |

---

## Java Code

```java
public class SmallestMissingPositive {

    // Returns the smallest missing positive
    public static int firstMissingPositive(int[] nums) {
        int n = nums.length;

        // Place each number at index x-1 if possible
        for (int i = 0; i < n; i++) {
            while (nums[i] > 0 && nums[i] <= n 
                   && nums[nums[i] - 1] != nums[i]) {
                int correctIndex = nums[i] - 1;
                // Swap nums[i] with nums[correctIndex]
                int temp = nums[i];
                nums[i] = nums[correctIndex];
                nums[correctIndex] = temp;
            }
        }

        // Identify the first missing positive
        for (int i = 0; i < n; i++) {
            if (nums[i] != i + 1) {
                return i + 1;
            }
        }

        // If 1..n are present
        return n + 1;
    }

    // Demo
    public static void main(String[] args) {
        int[] nums1 = {3,4,-1,1};
        int[] nums2 = {1,2,0};
        int[] nums3 = {7,8,9,11,12};

        System.out.println(firstMissingPositive(nums1)); // 2
        System.out.println(firstMissingPositive(nums2)); // 3
        System.out.println(firstMissingPositive(nums3)); // 1
    }
}
```

---

## Sample Input–Output

```text
Input:  nums = [3,4,-1,1]
Output: 2
```

```text
Input:  nums = [1,2,0]
Output: 3
```

```text
Input:  nums = [7,8,9,11,12]
Output: 1
```

---

Reply **“next”** for
**4. Jump Game** 🚀
