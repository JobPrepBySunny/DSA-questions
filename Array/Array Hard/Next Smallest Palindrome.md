Here’s **Problem 9 – Next Smallest Palindrome**
(Save as `09-next-smallest-palindrome.md`).

---

# Next Smallest Palindrome (Hard)

## Navigation

* [Problem Statement](#problem-statement)
* [Intuition](#intuition)
* [Algorithm](#algorithm)
* [Complexity](#complexity)
* [Java Code](#java-code)
* [Sample Input--Output](#sample-input--output)

---

## Problem Statement

You are given a number as an array of digits `num[]`, where:

* `num[0]` is the **most significant digit**
* `num[n-1]` is the **least significant digit**
* The number **does not** have leading zeros (except 0 itself).

> **Task:**
> Find the **next smallest palindrome** which is **strictly greater** than the given number.
> Return it as a new array of digits.

---

**Example 1**

```text
Input:  num = [1, 2, 3, 4]
Output: [1, 3, 3, 1]
```

**Example 2**

```text
Input:  num = [9, 4, 1, 8, 7, 9, 7, 8, 3, 2]
Output: [9, 4, 1, 8, 8, 0, 8, 8, 3, 2]
```

**Example 3**

```text
Input:  num = [9, 9, 9]
Output: [1, 0, 0, 1]
```

Constraints (typical):

* `1 ≤ n ≤ 10^5`
* `0 ≤ num[i] ≤ 9`

---

## Intuition

We want the **smallest palindrome greater than the original number**.

High level idea:

1. **Mirror left half to right half**

   * Copy each `num[i]` to `num[n-1-i]`.
   * This makes a palindrome `P`.
2. If `P` is **already greater** than original → done.
3. Otherwise, we need to **increment the middle** (like adding 1), and then mirror again.

Edge case:

* When number is like `9 9 9`:

  * After incrementing middle with carry, we get a longer palindrome: `1 0 0 1`.

---

## Algorithm

Let `n = num.length`.

1. **Copy original number** into `res[]`.
2. **Mirror left to right**:

   ```text
   for i = 0 to n/2 - 1:
       res[n-1-i] = res[i]
   ```
3. If `res` is **greater** than original `num` → return `res`.
4. Else, we need to **increment the middle**:

   * Let `midLeft = (n - 1) / 2`  (middle index for odd, left middle for even).
   * Start with `carry = 1`, `i = midLeft`.
   * While `i ≥ 0` and `carry > 0`:

     * `val = res[i] + carry`
     * `res[i] = val % 10`
     * `carry = val / 10`
     * `i--`
5. After increment:

   * If `carry > 0`, e.g., original was all 9s like `9 9 9`:

     * Create new array `ans` of length `n+1`
     * Set `ans[0] = ans[n] = 1`, rest 0 → return `ans`.
6. If no extra carry:

   * **Mirror again** (left to right) to ensure palindrome.
7. Return `res`.

---

## Complexity

| Type  | Complexity                           |
| ----- | ------------------------------------ |
| Time  | `O(n)`                               |
| Space | `O(1)` extra (ignoring output array) |

---

## Java Code

```java
import java.util.Arrays;

public class NextSmallestPalindrome {

    // Returns the next smallest palindrome strictly greater than num[]
    public static int[] nextPalindrome(int[] num) {
        int n = num.length;

        // Copy input to result array
        int[] res = Arrays.copyOf(num, n);

        // Step 1: Mirror left half to right half
        for (int i = 0; i < n / 2; i++) {
            res[n - 1 - i] = res[i];
        }

        // Step 2: If this mirrored number is already greater, return it
        if (isGreater(res, num)) {
            return res;
        }

        // Step 3: Increment the middle and handle carry
        int carry = 1;
        int midLeft = (n - 1) / 2;         // middle index (or left middle)
        int i = midLeft;

        while (i >= 0 && carry > 0) {
            int val = res[i] + carry;
            res[i] = val % 10;
            carry = val / 10;
            i--;
        }

        // Step 4: If we still have carry, it was like 999 -> 1001
        if (carry > 0) {
            int[] ans = new int[n + 1];
            ans[0] = 1;
            ans[n] = 1;
            return ans;
        }

        // Step 5: Mirror again after incrementing the left/middle
        for (int j = 0; j < n / 2; j++) {
            res[n - 1 - j] = res[j];
        }

        return res;
    }

    // Compare a and b as numbers represented by digit arrays
    // Return true if a > b
    private static boolean isGreater(int[] a, int[] b) {
        if (a.length != b.length) return a.length > b.length;
        for (int i = 0; i < a.length; i++) {
            if (a[i] != b[i]) {
                return a[i] > b[i];
            }
        }
        return false; // equal
    }

    // Demo
    public static void main(String[] args) {
        int[] num1 = {1, 2, 3, 4};
        int[] num2 = {9, 4, 1, 8, 7, 9, 7, 8, 3, 2};
        int[] num3 = {9, 9, 9};

        System.out.println(Arrays.toString(nextPalindrome(num1))); // [1, 3, 3, 1]
        System.out.println(Arrays.toString(nextPalindrome(num2))); // next palindrome
        System.out.println(Arrays.toString(nextPalindrome(num3))); // [1, 0, 0, 1]
    }
}
```

---

## Sample Input–Output

```text
Input:  num = [1, 2, 3, 4]
Output: [1, 3, 3, 1]
```

```text
Input:  num = [9, 9, 9]
Output: [1, 0, 0, 1]
```

---

Reply **“next”** for the last one:
**10. Maximum Sum Among All Rotations** 🔁📈
