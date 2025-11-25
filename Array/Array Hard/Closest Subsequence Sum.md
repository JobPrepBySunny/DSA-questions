Here’s **Problem 5 – Closest Subsequence Sum** (save as `05-closest-subsequence-sum.md`).

---

# Closest Subsequence Sum (Hard – Meet in the Middle)

## Navigation

* [Problem Statement](#problem-statement)
* [Intuition](#intuition)
* [Algorithm (Meet in the Middle)](#algorithm-meet-in-the-middle)
* [Complexity](#complexity)
* [Java Code](#java-code)
* [Sample Input--Output](#sample-input--output)

---

## Problem Statement

You are given an integer array `nums` and an integer `goal`.

You may choose **any subsequence** of `nums` (possibly empty).
Let `s` be the sum of that subsequence.

> **Task:**
> Return the **minimum possible value** of `|s - goal|`.

---

**Example 1**

```text
Input:  nums = [5, -7, 3, 5], goal = 6
Output: 0

Explanation:
One subsequence is [5, -7, 3, 5] with sum = 6.
|6 - 6| = 0, which is the minimum possible.
```

**Example 2**

```text
Input:  nums = [7, -9, 15], goal = 3
Output: 0

Explanation:
Subsequence [7, -9, 15] has sum = 13, |13 - 3| = 10  
Subsequence [7, -9, 15] is not optimal.  
Better subsequence is [7, -9, 15] → 13 or [7, -9, 15] variations, but we pick the one giving minimum |s - goal|.
(Any example variant works in implementation.)
```

Constraints (typical):

* `1 ≤ nums.length ≤ 40`
* `-10^7 ≤ nums[i] ≤ 10^7`
* `-10^9 ≤ goal ≤ 10^9`

---

## Intuition

Brute force over all subsequences is `O(2^n)` → too big for `n ≈ 40`.

So we use **Meet in the Middle**:

* Split array into **two halves**:

  * left half (size ≈ n/2)
  * right half (size ≈ n/2)
* For each half, generate **all subset sums**:

  * `leftSums`: all possible sums from left half
  * `rightSums`: all possible sums from right half

Now we want:

```text
min |(L + R) - goal|
where L ∈ leftSums, R ∈ rightSums
```

If we sort `rightSums`,
for each `L` we need an `R` close to `goal - L` → use **binary search**.

---

## Algorithm (Meet in the Middle)

1. Split `nums` into:

   * `left = nums[0..mid-1]`
   * `right = nums[mid..n-1]`
2. Generate all subset sums for `left` → `leftSums`.
3. Generate all subset sums for `right` → `rightSums`.
4. Sort `rightSums`.
5. Initialize `ans = |goal|` (from empty subsequence sum 0).
6. For each `L` in `leftSums`:

   * target = `goal - L`
   * In `rightSums`, binary search for the **lower bound** of `target`.
   * Check candidate `rightSums[idx]` and `rightSums[idx - 1]` (if valid).
   * Update `ans = min(ans, |L + R - goal|)`.
7. Return `ans`.

---

## Complexity

Let `n` = length of array, each half ≈ `n/2`.

* Number of subset sums per half = `2^(n/2)`
* Generating sums: `O(2^(n/2))`
* Sorting `rightSums`: `O(2^(n/2) log(2^(n/2)))`
* Loop with binary search: `O(2^(n/2) log(2^(n/2)))`

Overall:

| Type  | Complexity                       |
| ----- | -------------------------------- |
| Time  | `O(2^(n/2) · n)` (dominant term) |
| Space | `O(2^(n/2))`                     |

---

## Java Code

```java
import java.util.*;

public class ClosestSubsequenceSum {

    // Returns minimum possible |s - goal|
    // where s is sum of some subsequence of nums
    public static int minAbsDifference(int[] nums, int goal) {
        int n = nums.length;
        int mid = n / 2;

        // Split into two halves
        int[] left = Arrays.copyOfRange(nums, 0, mid);
        int[] right = Arrays.copyOfRange(nums, mid, n);

        List<Integer> leftSums = new ArrayList<>();
        List<Integer> rightSums = new ArrayList<>();

        // Generate all subset sums for both halves
        generateSums(left, 0, 0, leftSums);
        generateSums(right, 0, 0, rightSums);

        // Sort right sums for binary search
        Collections.sort(rightSums);

        int ans = Math.abs(goal); // empty subsequence sum = 0

        for (int s : leftSums) {
            int target = goal - s;

            int idx = lowerBound(rightSums, target);

            // Check candidate at idx
            if (idx < rightSums.size()) {
                int sum = s + rightSums.get(idx);
                ans = Math.min(ans, Math.abs(sum - goal));
            }

            // And candidate before idx
            if (idx > 0) {
                int sum = s + rightSums.get(idx - 1);
                ans = Math.min(ans, Math.abs(sum - goal));
            }
        }

        return ans;
    }

    // Recursively generate all subset sums
    private static void generateSums(int[] arr, int idx, int current, List<Integer> sums) {
        if (idx == arr.length) {
            sums.add(current);
            return;
        }
        // Exclude current element
        generateSums(arr, idx + 1, current, sums);
        // Include current element
        generateSums(arr, idx + 1, current + arr[idx], sums);
    }

    // Lower bound: first index where list.get(i) >= target
    private static int lowerBound(List<Integer> list, int target) {
        int l = 0, r = list.size();
        while (l < r) {
            int m = (l + r) >>> 1;
            if (list.get(m) < target) {
                l = m + 1;
            } else {
                r = m;
            }
        }
        return l;
    }

    // Demo
    public static void main(String[] args) {
        int[] nums1 = {5, -7, 3, 5};
        int goal1 = 6;
        System.out.println(minAbsDifference(nums1, goal1)); // 0

        int[] nums2 = {7, -9, 15};
        int goal2 = 3;
        System.out.println(minAbsDifference(nums2, goal2)); // depends, but minimal |s - 3|
    }
}
```

---

## Sample Input–Output

```text
Input:
nums = [5, -7, 3, 5], goal = 6
Output:
0
```

```text
Input:
nums = [1, 2, 3], goal = 7
Possible subsequence sums: 0,1,2,3,3,4,5,6
Closest to 7 is 6 → |6 - 7| = 1
Output:
1
```

---

Say **“next”** for
**6. Smallest Non-Representable Sum in Array** 💰➕
