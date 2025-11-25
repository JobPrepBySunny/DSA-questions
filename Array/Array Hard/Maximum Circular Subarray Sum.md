Here’s **Problem 2** in markdown (you can save as `02-maximum-circular-subarray-sum.md`).

---

# Maximum Circular Subarray Sum (Hard)

## Navigation

* [Problem Statement](#problem-statement)
* [Intuition](#intuition)
* [Algorithm](#algorithm)
* [Complexity](#complexity)
* [Java Code](#java-code)
* [Sample Input--Output](#sample-input--output)

---

## Problem Statement

You are given a **circular** integer array `nums`. A subarray can wrap around the end of the array to the beginning (because it is circular).

Your task is to find the **maximum possible sum** of a non-empty subarray of this circular array.

> **Task:**
> Given an integer array `nums`, return the **maximum circular subarray sum**.

**Example 1:**

```text
Input:  nums = [1, -2, 3, -2]
Output: 3

Explanation:
Maximum subarray is [3] with sum = 3.
```

**Example 2:**

```text
Input:  nums = [5, -3, 5]
Output: 10

Explanation:
Maximum sum subarray is [5, 5] (wraps around) with sum = 10.
```

**Example 3:**

```text
Input:  nums = [-3, -2, -3]
Output: -2

Explanation:
All numbers are negative, choose the single largest element.
```

You may assume:

* `1 <= nums.length <= 2 * 10^5`
* `-10^4 <= nums[i] <= 10^4`

---

## Intuition

There are **two** possible types of maximum subarrays:

1. **Normal (non-circular) subarray**

   * Just use standard **Kadane’s Algorithm** to get `maxSubarraySum`.

2. **Circular subarray (wrap-around case)**

   * If the max subarray wraps, it is equivalent to:

     * **Total sum of array** minus the **minimum subarray sum** in the middle.
   * So if `totalSum` is the sum of all elements and `minSubarraySum` is the minimum (non-empty) subarray sum, then:

     ```text
     maxCircular = totalSum - minSubarraySum
     ```

Final answer:

```text
answer = max(maxSubarraySum, totalSum - minSubarraySum)
```

**Important edge case:**
If **all numbers are negative**, then:

* `maxSubarraySum` will be the largest (least negative) number.
* `totalSum - minSubarraySum` becomes 0, which is invalid because subarray must be non-empty.
  So in that case, we just return `maxSubarraySum`.

---

## Algorithm

1. Initialize:

   * `total = 0`
   * `curMax = 0`, `maxSum = -∞`
   * `curMin = 0`, `minSum = +∞`

2. For each element `n` in `nums`:

   * Update total sum:

     ```text
     total += n
     ```
   * Standard Kadane for max subarray:

     ```text
     curMax = max(n, curMax + n)
     maxSum = max(maxSum, curMax)
     ```
   * Kadane variation for min subarray:

     ```text
     curMin = min(n, curMin + n)
     minSum = min(minSum, curMin)
     ```

3. If `maxSum < 0` (all numbers negative), return `maxSum`.

4. Otherwise return:

   ```text
   max(maxSum, total - minSum)
   ```

---

## Complexity

* **Time:** `O(n)` (single pass through the array)
* **Space:** `O(1)` (constant extra space)

---

## Java Code

```java
public class MaxCircularSubarraySum {

    // Returns the maximum circular subarray sum
    public static int maxSubarraySumCircular(int[] nums) {
        int total = 0;

        int curMax = 0;
        int maxSum = Integer.MIN_VALUE;

        int curMin = 0;
        int minSum = Integer.MAX_VALUE;

        for (int n : nums) {
            total += n;

            // Kadane for maximum subarray
            curMax = Math.max(n, curMax + n);
            maxSum = Math.max(maxSum, curMax);

            // Kadane for minimum subarray
            curMin = Math.min(n, curMin + n);
            minSum = Math.min(minSum, curMin);
        }

        // If all numbers are negative, total - minSum would be 0 (invalid),
        // so just return the normal max subarray sum
        if (maxSum < 0) {
            return maxSum;
        }

        // Otherwise, take the best of non-circular and circular cases
        return Math.max(maxSum, total - minSum);
    }

    // Small demo
    public static void main(String[] args) {
        int[] nums1 = {1, -2, 3, -2};
        int[] nums2 = {5, -3, 5};
        int[] nums3 = {-3, -2, -3};

        System.out.println(maxSubarraySumCircular(nums1)); // 3
        System.out.println(maxSubarraySumCircular(nums2)); // 10
        System.out.println(maxSubarraySumCircular(nums3)); // -2
    }
}
```

---

## Sample Input–Output

```text
Input:
nums = [1, -2, 3, -2]
Output:
3
```

```text
Input:
nums = [5, -3, 5]
Output:
10
```

```text
Input:
nums = [-3, -2, -3]
Output:
-2
```

---

Say **“next”** when you’re ready for
**3. Smallest Missing Positive Number** in the same format.
