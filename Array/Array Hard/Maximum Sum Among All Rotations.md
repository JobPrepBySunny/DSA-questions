Here’s **Problem 10 – Maximum Sum Among All Rotations**
(Save as `10-maximum-sum-all-rotations.md`).

---

# Maximum Sum Among All Rotations (Hard)

## Navigation

* [Problem Statement](#problem-statement)
* [Example](#example)
* [Intuition](#intuition)
* [Algorithm](#algorithm)
* [Complexity](#complexity)
* [Java Code](#java-code)
* [Sample Input--Output](#sample-input--output)

---

## Problem Statement

You are given an array `arr` of size `n`.

Consider all **rotations** of the array. For each rotation, define:

[
F(k) = \sum_{i=0}^{n-1} i \cdot arr_k[i]
]

where `arr_k` is the array after rotating `k` times (clockwise or anticlockwise, but consistently one way), and `i` is the index in that rotated array.

> **Task:**
> Return the **maximum** value of `F(k)` among all possible rotations.

---

## Example

**Input**

```text
arr = [8, 3, 1, 2]
```

All rotations and their F(k):

1. Rotation 0: `[8, 3, 1, 2]`
   (F(0) = 0*8 + 1*3 + 2*1 + 3*2 = 0 + 3 + 2 + 6 = 11)

2. Rotation 1: `[3, 1, 2, 8]`
   (F(1) = 0*3 + 1*1 + 2*2 + 3*8 = 0 + 1 + 4 + 24 = 29)

3. Rotation 2: `[1, 2, 8, 3]`
   (F(2) = 0*1 + 1*2 + 2*8 + 3*3 = 0 + 2 + 16 + 9 = 27)

4. Rotation 3: `[2, 8, 3, 1]`
   (F(3) = 0*2 + 1*8 + 2*3 + 3*1 = 0 + 8 + 6 + 3 = 17)

**Output**

```text
29
```

---

## Intuition

Brute force:

* Generate each rotation,
* Compute F(k) in `O(n)`
  → Total `O(n^2)` — too slow for large `n`.

We need a **relation** between `F(k)` and `F(k+1)`.

Let:

* `sum = arr[0] + arr[1] + ... + arr[n-1]`
* `F(0) = 0*arr[0] + 1*arr[1] + ... + (n-1)*arr[n-1]`

When we rotate **right by 1** (or equivalently consider sequence of rotations in a fixed direction), we can derive:

[
F(k+1) = F(k) + sum - n \cdot arr[n - 1 - k]
]

This formula lets us compute next rotation value in **O(1)** from previous.

So we:

1. Compute `sum` and `F(0)` in `O(n)`.
2. Use the recurrence to get all `F(k)` in `O(n)`.
3. Track the maximum.

---

## Algorithm

1. Let `n = arr.length`.
2. Compute:

   ```text
   sum = Σ arr[i]
   F = Σ i * arr[i]      // this is F(0)
   maxVal = F
   ```
3. For each `k` from `1` to `n-1`:

   * Use recurrence:

     ```text
     F = F + sum - n * arr[n - k]
     maxVal = max(maxVal, F)
     ```
4. Return `maxVal`.

Note:
Here we are effectively considering rotations one by one in a consistent direction, using the formula derived for that direction.

---

## Complexity

| Type  | Complexity |
| ----- | ---------- |
| Time  | `O(n)`     |
| Space | `O(1)`     |

---

## Java Code

```java
public class MaxSumAllRotations {

    // Returns maximum value of F(k) among all rotations
    public static int maxSum(int[] arr) {
        int n = arr.length;

        int sum = 0;      // sum of all elements
        int F = 0;        // F(0)
        
        for (int i = 0; i < n; i++) {
            sum += arr[i];
            F += i * arr[i];
        }

        int maxVal = F;

        // Compute F(k) from F(k-1) using the recurrence
        for (int k = 1; k < n; k++) {
            // F(k) = F(k-1) + sum - n * arr[n - k]
            F = F + sum - n * arr[n - k];
            if (F > maxVal) {
                maxVal = F;
            }
        }

        return maxVal;
    }

    // Demo
    public static void main(String[] args) {
        int[] arr1 = {8, 3, 1, 2};
        System.out.println(maxSum(arr1)); // 29

        int[] arr2 = {1, 2, 3, 4, 5};
        System.out.println(maxSum(arr2)); // Some max rotation sum
    }
}
```

---

## Sample Input–Output

```text
Input:
arr = [8, 3, 1, 2]
Output:
29
```

```text
Input:
arr = [1, 2, 3]
Possible F values:
[1,2,3] -> 0*1 + 1*2 + 2*3 = 8
[2,3,1] -> 0*2 + 1*3 + 2*1 = 5
[3,1,2] -> 0*3 + 1*1 + 2*2 = 5
Output:
8
```

---

All 10 hard problems are now ready in clean markdown + Java for your GitHub.
If you want, I can also give you a **README navigation** linking all these files.
