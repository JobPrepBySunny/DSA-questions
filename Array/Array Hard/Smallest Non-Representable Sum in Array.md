Here’s **Problem 6 – Smallest Non-Representable Sum in Array**
(Save as `06-smallest-non-representable-sum.md`.)

---

# Smallest Non-Representable Sum in Array (Greedy / Hard)

## Navigation

* [Problem Statement](#problem-statement)
* [Intuition](#intuition)
* [Algorithm](#algorithm)
* [Complexity](#complexity)
* [Java Code](#java-code)
* [Sample Input--Output](#sample-input--output)

---

## Problem Statement

You are given a **sorted** array `arr` of positive integers.

You need to find the **smallest positive integer value** that **cannot** be represented as the sum of elements of any **subset** of `arr`.

> If the array is not sorted in the input, sort it first.

---

**Example 1**

```text
Input:  arr = [1, 2, 3, 10]
Output: 7

Explanation:
All values from 1 to 6 can be formed:
1, 2, 3, 1+2=3, 1+3=4, 2+3=5, 1+2+3=6
But 7 cannot be formed.
```

**Example 2**

```text
Input:  arr = [1, 1, 1, 1]
Output: 5
Explanation:
We can form 1,2,3,4 using subset sums, but not 5.
```

**Example 3**

```text
Input:  arr = [2, 3, 4]
Output: 1
Explanation:
1 itself cannot be formed, so answer is 1.
```

Constraints (typical):

* `1 ≤ n ≤ 10^5`
* `1 ≤ arr[i] ≤ 10^9`
* `arr` is sorted in non-decreasing order (or we sort before processing).

---

## Intuition

We maintain a value `res` that represents:

> “We can form **all** values in the range `[1, res)` using subset sums of elements processed so far.”

Initially:

```text
res = 1
```

Meaning:
Before looking at any element, we **cannot** form 1 yet.

Now, process array from left to right:

* Suppose `res` is current reachable range end, and we see next element `x = arr[i]`.
* Two cases:

### Case 1: `x <= res`

We can **extend** the reachable range.

* Since we already can form `[1, res)`,
* Using `x`, we can now form all sums up to `res + x - 1`.

So we update:

```text
res = res + x
```

### Case 2: `x > res`

There is a **gap** at `res`.

* We can form up to `res - 1`
* The next element `x` is bigger than `res`
* Therefore, we **cannot** form sum exactly equal to `res`.

So the **answer is `res`**.

If we finish the array without hitting Case 2, then the answer is the final `res`.

---

## Algorithm

1. If array is not sorted, sort it.
2. Initialize:

   ```text
   res = 1  // smallest sum we cannot form yet
   ```
3. For each element `x` in `arr`:

   * If `x > res`,
     → we **cannot** form `res`, so return `res`.
   * Else
     → we can extend range:

     ```text
     res = res + x
     ```
4. After loop, return `res`.

---

## Complexity

| Type  | Complexity                                            |
| ----- | ----------------------------------------------------- |
| Time  | `O(n)` after sorting (`O(n log n)` if sorting needed) |
| Space | `O(1)`                                                |

---

## Java Code

Version 1 – assumes **sorted** positive array (like GFG style):

```java
public class SmallestNonRepresentableSum {

    // Assumes arr is sorted and contains only positive numbers
    public static long smallestNonRepresentable(long[] arr) {
        long res = 1;  // We can form all sums in [1, res) initially none

        for (long x : arr) {
            // If current element is greater than res,
            // we cannot form 'res' using any subset
            if (x > res) {
                break;
            }
            // Otherwise, extend the range of representable sums
            res += x;
        }

        return res;
    }

    // Demo
    public static void main(String[] args) {
        long[] arr1 = {1, 2, 3, 10};
        long[] arr2 = {1, 1, 1, 1};
        long[] arr3 = {2, 3, 4};

        System.out.println(smallestNonRepresentable(arr1)); // 7
        System.out.println(smallestNonRepresentable(arr2)); // 5
        System.out.println(smallestNonRepresentable(arr3)); // 1
    }
}
```

---

### If Input Is Not Sorted (Optional Version)

```java
import java.util.Arrays;

public class SmallestNonRepresentableSumUnsorted {

    public static long smallestNonRepresentable(int[] nums) {
        Arrays.sort(nums);  // sort first

        long res = 1;

        for (int x : nums) {
            if (x > res) {
                break;
            }
            res += x;
        }
        return res;
    }
}
```

---

## Sample Input–Output

```text
Input:  arr = [1, 2, 3, 10]
Output: 7
```

```text
Input:  arr = [1, 1, 1, 1]
Output: 5
```

```text
Input:  arr = [2, 3, 4]
Output: 1
```

---

Say **“next”** for
**7. Smallest Range Having Elements From K Lists** 📚📏
