Here’s **Problem 4 – Jump Game** (save as `04-jump-game.md`).

---

# Jump Game (Hard / Greedy)

## Navigation

* [Problem Statement](#problem-statement)
* [Intuition](#intuition)
* [Algorithm](#algorithm)
* [Complexity](#complexity)
* [Java Code](#java-code)
* [Sample Input--Output](#sample-input--output)

---

## Problem Statement

You are given an integer array `nums` where `nums[i]` represents the **maximum jump length** from index `i`.

You start at index `0`.
Your task is to determine if you can reach the **last index** of the array.

> **Task:**
> Return `true` if you can reach the last index, otherwise return `false`.

---

**Example 1**

```text
Input:  nums = [2,3,1,1,4]
Output: true

Explanation:
Start at index 0:
- jump 1 step to index 1 (value 3),
- jump 3 steps to index 4 (last index).
```

**Example 2**

```text
Input:  nums = [3,2,1,0,4]
Output: false

Explanation:
You’ll get stuck at index 3 (value 0) and cannot reach index 4.
```

Constraints:

* `1 ≤ nums.length ≤ 10^5`
* `0 ≤ nums[i] ≤ 10^5`

---

## Intuition

Instead of trying all possible jumps (which is exponential), we think **greedily**:

* Maintain the **farthest index** we can reach so far, called `maxReach`.
* Iterate from left to right:

  * If at some index `i`, `i > maxReach`, it means we cannot even reach `i` → so we definitely can’t reach the end.
  * Otherwise, update `maxReach = max(maxReach, i + nums[i])`.

If after traversing the array we never get stuck, we can reach the last index.

---

## Algorithm

1. Initialize:

   ```text
   maxReach = 0
   ```
2. Loop `i` from `0` to `n - 1`:

   * If `i > maxReach` → return `false`.
   * Update:

     ```text
     maxReach = max(maxReach, i + nums[i])
     ```
3. If loop finishes without returning `false`, return `true`.

---

## Complexity

| Type  | Complexity |
| ----- | ---------- |
| Time  | `O(n)`     |
| Space | `O(1)`     |

---

## Java Code

```java
public class JumpGame {

    // Returns true if we can reach the last index
    public static boolean canJump(int[] nums) {
        int maxReach = 0;

        for (int i = 0; i < nums.length; i++) {
            // If current index is beyond our farthest reach, we are stuck
            if (i > maxReach) {
                return false;
            }

            // Update the farthest we can reach from here
            maxReach = Math.max(maxReach, i + nums[i]);
        }

        // If we never got stuck, we can reach the end
        return true;
    }

    // Demo
    public static void main(String[] args) {
        int[] nums1 = {2, 3, 1, 1, 4};
        int[] nums2 = {3, 2, 1, 0, 4};

        System.out.println(canJump(nums1)); // true
        System.out.println(canJump(nums2)); // false
    }
}
```

---

## Sample Input–Output

```text
Input:  nums = [2,3,1,1,4]
Output: true
```

```text
Input:  nums = [3,2,1,0,4]
Output: false
```

---

Say **“next”** for
**5. Closest Subsequence Sum** 🔍📏
