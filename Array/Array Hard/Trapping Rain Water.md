Here’s **Problem 1** in a clean markdown format you can put directly in GitHub (e.g., `01-trapping-rain-water.md`).

---

# Trapping Rain Water (Hard)

## Navigation

* [Problem Statement](#problem-statement)
* [Intuition](#intuition)
* [Algorithm](#algorithm)
* [Complexity](#complexity)
* [Java Code](#java-code)
* [Sample Input--Output](#sample-input--output)

---

## Problem Statement

You are given an array `height` of length `n` where each element represents the height of a vertical bar and the width of each bar is 1.

Rainwater can be trapped between bars after it rains. You need to compute how much water is trapped.

> **Task:**
> Given an integer array `height`, return the total amount of trapped rainwater.

**Example 1:**

```text
Input:  height = [0,1,0,2,1,0,1,3,2,1,2,1]
Output: 6
Explanation:
6 units of water are trapped between the bars.
```

**Example 2:**

```text
Input:  height = [4,2,0,3,2,5]
Output: 9
```

You may assume:

* `0 <= height[i] <= 10^5`
* `1 <= n <= 2 * 10^5`

---

## Intuition

Water above a bar depends on:

* The **tallest bar to its left**.
* The **tallest bar to its right**.

Water at index `i` =
`min(maxLeft[i], maxRight[i]) - height[i]` (if this is positive).

Instead of storing full arrays of `maxLeft` and `maxRight`, we can use **two pointers** and track:

* `leftMax`: max height from the left.
* `rightMax`: max height from the right.

Always move the pointer with **smaller height**, because that side is the limiting factor.

---

## Algorithm

1. Initialize:

   * `left = 0`, `right = n - 1`
   * `leftMax = 0`, `rightMax = 0`, `water = 0`
2. While `left < right`:

   * If `height[left] <= height[right]`:

     * If `height[left] >= leftMax`, update `leftMax`.
     * Else, add `leftMax - height[left]` to `water`.
     * Move `left++`.
   * Else:

     * If `height[right] >= rightMax`, update `rightMax`.
     * Else, add `rightMax - height[right]` to `water`.
     * Move `right--`.
3. Return `water`.

---

## Complexity

* **Time:** `O(n)` (single pass with two pointers)
* **Space:** `O(1)` (no extra arrays)

---

## Java Code

```java
public class TrappingRainWater {

    // Returns total amount of trapped rain water
    public static int trap(int[] height) {
        if (height == null || height.length == 0) return 0;

        int left = 0;
        int right = height.length - 1;
        int leftMax = 0;
        int rightMax = 0;
        int water = 0;

        while (left < right) {
            if (height[left] <= height[right]) {
                // Process left side
                if (height[left] >= leftMax) {
                    leftMax = height[left];
                } else {
                    water += leftMax - height[left];
                }
                left++;
            } else {
                // Process right side
                if (height[right] >= rightMax) {
                    rightMax = height[right];
                } else {
                    water += rightMax - height[right];
                }
                right--;
            }
        }
        return water;
    }

    // Small demo
    public static void main(String[] args) {
        int[] height1 = {0,1,0,2,1,0,1,3,2,1,2,1};
        int[] height2 = {4,2,0,3,2,5};

        System.out.println(trap(height1)); // 6
        System.out.println(trap(height2)); // 9
    }
}
```

---

## Sample Input–Output

```text
Input:
height = [0,1,0,2,1,0,1,3,2,1,2,1]
Output:
6
```

```text
Input:
height = [4,2,0,3,2,5]
Output:
9
```

---

When you’re ready, say **“2”** and I’ll give you the markdown for
**Maximum Circular Subarray Sum** in the same style.
