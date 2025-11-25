
---

# Medium Array Coding Problems (with Questions + Java Solutions)

## 📌 Navigation

### Medium Problems

* [Next Permutation](#next-permutation)
* [Majority Element](#majority-element)
* [Majority Element II](#majority-element-ii)
* [Minimize the Heights II](#minimize-the-heights-ii)
* [Maximum Subarray Sum](#maximum-subarray-sum)
* [Maximum Product Subarray](#maximum-product-subarray)
* [Product of Array Except Self](#product-of-array-except-self)
* [Subarrays with Product Less Than K](#subarrays-with-product-less-than-k)
* [Split Into Three Equal Sum Segments](#split-into-three-equal-sum-segments)
* [Maximum Consecutive 1s After Flipping 0s](#maximum-consecutive-1s-after-flipping-0s)
* [Last Moment Before Ants Fall Out of Plank](#last-moment-before-ants-fall-out-of-plank)
* [Find 0 with Farthest 1s in a Binary](#find-0-with-farthest-1s-in-a-binary)
* [Intersection of Interval Lists](#intersection-of-interval-lists)
* [Rearrange Array Elements by Sign](#rearrange-array-elements-by-sign)
* [Meeting Scheduler for Two Persons](#meeting-scheduler-for-two-persons)
* [Longest Mountain Subarray](#longest-mountain-subarray)
* [Transform and Sort Array](#transform-and-sort-array)
* [Minimum Swaps To Group All Ones](#minimum-swaps-to-group-all-ones)
* [Minimum Moves To Equalize Array](#minimum-moves-to-equalize-array)
* [Minimum Indices To Equal Even-Odd Sums](#minimum-indices-to-equal-even-odd-sums)

---

> You can put all methods inside a class like
> `public class MediumArrayProblems { ... }` for practice.

---

## Next Permutation

**Problem:**
You are given an array `nums` representing a permutation of numbers. Rearrange `nums` into the **next lexicographically greater permutation**.
If such arrangement is not possible (already highest), rearrange it to the **lowest possible order** (sorted ascending).
You must do this in-place.

**Logic (Steps):**

1. From the right, find the first index `i` such that `nums[i] < nums[i + 1]`.
2. From the right, find the first number greater than `nums[i]` and swap them.
3. Reverse the part of the array from `i + 1` to the end.

**Java Solution:**

```java
public static void nextPermutation(int[] nums) {
    int n = nums.length;
    int i = n - 2;

    // 1. Find first decreasing element from right
    while (i >= 0 && nums[i] >= nums[i + 1]) {
        i--;
    }

    // 2. If found, find element just greater than nums[i] from right and swap
    if (i >= 0) {
        int j = n - 1;
        while (nums[j] <= nums[i]) j--;
        swap(nums, i, j);
    }

    // 3. Reverse from i+1 to end
    reverse(nums, i + 1, n - 1);
}

private static void swap(int[] nums, int i, int j) {
    int temp = nums[i];
    nums[i] = nums[j];
    nums[j] = temp;
}

private static void reverse(int[] nums, int l, int r) {
    while (l < r) {
        swap(nums, l++, r--);
    }
}
```

---

## Majority Element

**Problem:**
You are given an integer array `nums`. A **majority element** is the element that appears **more than n/2 times**.
Assume such element **always exists**. Return that element.

**Logic:**
Use **Boyer–Moore Voting**:

* Maintain a candidate and a counter.
* When counter is 0, choose current element as candidate.
* Increase counter if current equals candidate, else decrease.

**Java Solution:**

```java
public static int majorityElement(int[] nums) {
    int candidate = 0, count = 0;
    for (int x : nums) {
        if (count == 0) {
            candidate = x;
        }
        count += (x == candidate) ? 1 : -1;
    }
    return candidate;
}
```

---

## Majority Element II

**Problem:**
You are given an integer array `nums`. Find all elements that appear **more than ⌊n/3⌋ times**.
There can be **at most 2** such elements.

**Logic:**

* Use extended Boyer–Moore with **two candidates**.
* First pass: find two potential candidates.
* Second pass: count their actual frequencies and add valid ones.

**Java Solution:**

```java
import java.util.*;

public static List<Integer> majorityElementII(int[] nums) {
    Integer cand1 = null, cand2 = null;
    int c1 = 0, c2 = 0;

    for (int x : nums) {
        if (cand1 != null && x == cand1) c1++;
        else if (cand2 != null && x == cand2) c2++;
        else if (c1 == 0) {
            cand1 = x; c1 = 1;
        } else if (c2 == 0) {
            cand2 = x; c2 = 1;
        } else {
            c1--; c2--;
        }
    }

    // verify
    c1 = c2 = 0;
    for (int x : nums) {
        if (cand1 != null && x == cand1) c1++;
        else if (cand2 != null && x == cand2) c2++;
    }

    List<Integer> res = new ArrayList<>();
    int n = nums.length;
    if (cand1 != null && c1 > n / 3) res.add(cand1);
    if (cand2 != null && c2 > n / 3) res.add(cand2);
    return res;
}
```

---

## Minimize the Heights II

**Problem:**
Given an array `arr[]` of `n` tower heights and a value `k`. You can **add or subtract k** to each height **at most once**.
Find the **minimum possible difference** between the tallest and shortest tower after these operations.

**Logic:**

1. Sort the array.
2. Initial difference = `arr[n-1] - arr[0]`.
3. Assume we make some prefix smaller and suffix larger, update min diff by checking new min and new max.

**Java Solution:**

```java
import java.util.*;

public static int minimizeHeightsII(int[] arr, int k) {
    int n = arr.length;
    if (n == 1) return 0;

    Arrays.sort(arr);
    int diff = arr[n - 1] - arr[0];

    int small = arr[0] + k;
    int big = arr[n - 1] - k;

    if (small > big) {
        int temp = small;
        small = big;
        big = temp;
    }

    for (int i = 1; i < n - 1; i++) {
        int subtract = arr[i] - k;
        int add = arr[i] + k;

        if (subtract >= small || add <= big) continue;

        if (big - subtract <= add - small) {
            small = subtract;
        } else {
            big = add;
        }
    }
    return Math.min(diff, big - small);
}
```

---

## Maximum Subarray Sum

**Problem:**
Given an integer array `nums`, find a **contiguous subarray** with the **maximum sum** and return that sum.

**Logic (Kadane’s Algorithm):**

* Keep a running sum `curr`.
* If `curr` becomes negative, reset it to current element.
* Track maximum seen so far.

**Java Solution:**

```java
public static int maxSubArray(int[] nums) {
    int maxSoFar = nums[0];
    int curr = nums[0];

    for (int i = 1; i < nums.length; i++) {
        curr = Math.max(nums[i], curr + nums[i]);
        maxSoFar = Math.max(maxSoFar, curr);
    }
    return maxSoFar;
}
```

---

## Maximum Product Subarray

**Problem:**
Given an integer array `nums`, find a contiguous subarray within `nums` that has the **largest product** and return that product.

**Logic:**

* Maintain `maxProd` and `minProd` at each step because a negative can flip sign.
* If current number is negative, swap `maxProd` and `minProd`.

**Java Solution:**

```java
public static int maxProductSubarray(int[] nums) {
    int maxProd = nums[0];
    int minProd = nums[0];
    int ans = nums[0];

    for (int i = 1; i < nums.length; i++) {
        int x = nums[i];
        if (x < 0) {
            int tmp = maxProd;
            maxProd = minProd;
            minProd = tmp;
        }
        maxProd = Math.max(x, maxProd * x);
        minProd = Math.min(x, minProd * x);
        ans = Math.max(ans, maxProd);
    }
    return ans;
}
```

---

## Product of Array Except Self

**Problem:**
Given integer array `nums`, return an array `answer` such that
`answer[i] = product of all elements of nums except nums[i]`.
You must not use division and must run in `O(n)`.

**Logic:**

* First pass (left to right): store prefix product.
* Second pass (right to left): multiply with suffix product.

**Java Solution:**

```java
public static int[] productExceptSelf(int[] nums) {
    int n = nums.length;
    int[] res = new int[n];

    int prefix = 1;
    for (int i = 0; i < n; i++) {
        res[i] = prefix;
        prefix *= nums[i];
    }

    int suffix = 1;
    for (int i = n - 1; i >= 0; i--) {
        res[i] *= suffix;
        suffix *= nums[i];
    }

    return res;
}
```

---

## Subarrays with Product Less Than K

**Problem:**
Given an array of positive integers `nums` and an integer `k`,
return the **number of contiguous subarrays** where the product of all elements is **less than k**.

**Logic (Sliding Window):**

* Maintain a window `[left, right]` and current product.
* Expand right; while product ≥ k, move left and divide.
* At each step, number of valid subarrays ending at `right` = `right - left + 1`.

**Java Solution:**

```java
public static int numSubarrayProductLessThanK(int[] nums, int k) {
    if (k <= 1) return 0;
    int left = 0;
    long prod = 1;
    int count = 0;

    for (int right = 0; right < nums.length; right++) {
        prod *= nums[right];

        while (prod >= k && left <= right) {
            prod /= nums[left++];
        }
        count += right - left + 1;
    }
    return count;
}
```

---

## Split Into Three Equal Sum Segments

**Problem:**
Given an array `nums`, count the number of ways to split it into **three non-empty contiguous parts** such that the **sum of each part is equal**.

**Logic:**

1. Compute `totalSum`. If not divisible by 3 → 0 ways.
2. Let `target = totalSum / 3`, `target2 = 2 * target`.
3. Traverse and count positions where prefix sum equals `target`,
   and when prefix sum equals `target2`, add how many `target` positions seen before.

**Java Solution:**

```java
public static long splitIntoThreeEqualSums(int[] nums) {
    long total = 0;
    for (int x : nums) total += x;
    if (total % 3 != 0) return 0;

    long target1 = total / 3;
    long target2 = 2 * target1;

    long prefix = 0;
    long countTarget1 = 0;
    long ways = 0;

    // last cut before last element
    for (int i = 0; i < nums.length - 1; i++) {
        prefix += nums[i];
        if (prefix == target2) {
            ways += countTarget1;
        }
        if (prefix == target1) {
            countTarget1++;
        }
    }
    return ways;
}
```

---

## Maximum Consecutive 1s After Flipping 0s

**Problem:**
Given a binary array `nums` and an integer `k`, you can flip at most `k` zeros to 1.
Find the **maximum length of consecutive 1s** you can get.

**Logic (Sliding Window):**

* Maintain window `[left, right]` with at most `k` zeros.
* Expand `right`, if zeros > k, move `left` forward until zeros ≤ k.

**Java Solution:**

```java
public static int maxConsecutiveOnesAfterFlips(int[] nums, int k) {
    int left = 0, zeros = 0, best = 0;
    for (int right = 0; right < nums.length; right++) {
        if (nums[right] == 0) zeros++;
        while (zeros > k) {
            if (nums[left] == 0) zeros--;
            left++;
        }
        best = Math.max(best, right - left + 1);
    }
    return best;
}
```

---

## Last Moment Before Ants Fall Out of Plank

**Problem:**
A plank of length `n`.
Some ants move to the **left** (their positions in `left[]`) and some to the **right** (`right[]`).
All move at speed 1. When two ants meet, they just pass through (equivalent to swapping identities).
Return the **last moment when any ant is on the plank**.

**Logic:**

* The last time is just the maximum of:

  * Time for left-moving ants to fall → their positions.
  * Time for right-moving ants to fall → `n - position`.

**Java Solution:**

```java
public static int lastMoment(int n, int[] left, int[] right) {
    int maxTime = 0;
    for (int x : left) {
        maxTime = Math.max(maxTime, x);
    }
    for (int x : right) {
        maxTime = Math.max(maxTime, n - x);
    }
    return maxTime;
}
```

---

## Find 0 with Farthest 1s in a Binary

**Problem:**
You are given a binary array (only 0s and 1s).
You can flip **exactly one `0` to `1`**.
Return the **index of the 0** to flip so that the resulting array has the **longest contiguous sequence of 1s**.
If multiple, return any one.

**Logic:**

* Keep track of:

  * `prevZero` = index of last zero.
  * `prevPrevZero` = index of zero before that.
* Window with at most 1 zero gives longest sequence; its zero index = answer.

**Java Solution:**

```java
public static int indexOfZeroToFlip(int[] nums) {
    int prevZero = -1, prevPrevZero = -1;
    int bestLen = 0, bestIndex = -1;

    for (int i = 0; i < nums.length; i++) {
        if (nums[i] == 0) {
            prevPrevZero = prevZero;
            prevZero = i;
        }
        int start = prevPrevZero + 1;
        int len = i - start + 1;

        if (len > bestLen) {
            bestLen = len;
            bestIndex = prevZero;
        }
    }
    return bestIndex;
}
```

---

## Intersection of Interval Lists

**Problem:**
You are given two lists of **disjoint sorted intervals**, `A` and `B`.
Return the list of intervals that represent the **intersection** of these two.

**Logic:**

* Use two pointers `i` and `j`.
* For `A[i]` and `B[j]`, intersection start = `max(startA, startB)`, end = `min(endA, endB)`.
* If `start <= end`, add it.
* Move the pointer whose interval ends first.

**Java Solution:**

```java
import java.util.*;

public static int[][] intervalIntersection(int[][] A, int[][] B) {
    List<int[]> res = new ArrayList<>();
    int i = 0, j = 0;

    while (i < A.length && j < B.length) {
        int start = Math.max(A[i][0], B[j][0]);
        int end = Math.min(A[i][1], B[j][1]);

        if (start <= end) {
            res.add(new int[]{start, end});
        }

        if (A[i][1] < B[j][1]) i++;
        else j++;
    }

    return res.toArray(new int[res.size()][]);
}
```

---

## Rearrange Array Elements by Sign

**Problem:**
You are given an integer array `nums` containing positive and negative numbers.
Rearrange the array so that **positive and negative numbers alternate**, starting with a positive.
If extra positives/negatives remain, they can go at the end.

**Logic:**

* Store positives and negatives in separate arrays.
* Then fill back into `nums` alternating between positive and negative.

**Java Solution:**

```java
public static int[] rearrangeBySign(int[] nums) {
    int n = nums.length;
    int[] pos = new int[n];
    int[] neg = new int[n];
    int p = 0, q = 0;

    for (int x : nums) {
        if (x >= 0) pos[p++] = x;
        else neg[q++] = x;
    }

    int i = 0, pi = 0, ni = 0;
    boolean turnPos = true;

    while (pi < p && ni < q) {
        nums[i++] = turnPos ? pos[pi++] : neg[ni++];
        turnPos = !turnPos;
    }
    while (pi < p) nums[i++] = pos[pi++];
    while (ni < q) nums[i++] = neg[ni++];

    return nums;
}
```

---

## Meeting Scheduler for Two Persons

**Problem:**
Two people have lists of available time slots: `slots1` and `slots2`.
Each slot is `[start, end]` (in minutes).
Given a required `duration`, find the **earliest time slot** that is common to both persons and has at least that duration.
Return `[start, start + duration]`, or empty list if not possible.

**Logic:**

* Sort both slot lists by start time.
* Use two pointers; for each pair of slots, find overlap:

  * `start = max(start1, start2)`
  * `end = min(end1, end2)`
* If `end - start >= duration`, answer found.

**Java Solution:**

```java
import java.util.*;

public static List<Integer> minAvailableDuration(int[][] slots1, int[][] slots2, int duration) {
    Arrays.sort(slots1, (a, b) -> Integer.compare(a[0], b[0]));
    Arrays.sort(slots2, (a, b) -> Integer.compare(a[0], b[0]));

    int i = 0, j = 0;
    while (i < slots1.length && j < slots2.length) {
        int start = Math.max(slots1[i][0], slots2[j][0]);
        int end = Math.min(slots1[i][1], slots2[j][1]);

        if (end - start >= duration) {
            return Arrays.asList(start, start + duration);
        }

        if (slots1[i][1] < slots2[j][1]) i++;
        else j++;
    }
    return Collections.emptyList();
}
```

---

## Longest Mountain Subarray

**Problem:**
An array forms a **mountain** if:

* It strictly increases to a peak,
* Then strictly decreases after that,
* And length is at least 3.

Given `arr`, return the **length of the longest mountain**. If none, return 0.

**Logic:**

* `up[i]` = length of increasing sequence ending at `i`.
* `down[i]` = length of decreasing sequence starting at `i`.
* For each `i`, if `up[i] > 0` and `down[i] > 0`, mountain length = `up[i] + down[i] + 1`.

**Java Solution:**

```java
public static int longestMountain(int[] arr) {
    int n = arr.length;
    if (n < 3) return 0;

    int[] up = new int[n];
    int[] down = new int[n];

    for (int i = 1; i < n; i++) {
        if (arr[i] > arr[i - 1]) {
            up[i] = up[i - 1] + 1;
        }
    }

    for (int i = n - 2; i >= 0; i--) {
        if (arr[i] > arr[i + 1]) {
            down[i] = down[i + 1] + 1;
        }
    }

    int best = 0;
    for (int i = 0; i < n; i++) {
        if (up[i] > 0 && down[i] > 0) {
            best = Math.max(best, up[i] + down[i] + 1);
        }
    }
    return best;
}
```

---

## Transform and Sort Array

**Problem:**
You are given a **sorted** array `nums` and three integers `a`, `b`, `c`.
Apply the function `f(x) = a*x*x + b*x + c` to each element `x` in `nums`.
Return the **resulting array in sorted order**.

**Logic:**

* `f(x)` is a quadratic.
* When `nums` is sorted:

  * If `a >= 0`, largest values are at the ends (like a U shape).
  * If `a < 0`, smallest values are at the ends (inverted U).
* Use two-pointer technique to fill result from appropriate side.

**Java Solution:**

```java
public static int[] sortTransformedArray(int[] nums, int a, int b, int c) {
    int n = nums.length;
    int[] res = new int[n];
    int left = 0, right = n - 1;
    int index = a >= 0 ? n - 1 : 0;

    while (left <= right) {
        int valLeft = transform(nums[left], a, b, c);
        int valRight = transform(nums[right], a, b, c);

        if (a >= 0) {
            if (valLeft > valRight) {
                res[index--] = valLeft;
                left++;
            } else {
                res[index--] = valRight;
                right--;
            }
        } else {
            if (valLeft < valRight) {
                res[index++] = valLeft;
                left++;
            } else {
                res[index++] = valRight;
                right--;
            }
        }
    }
    return res;
}

private static int transform(int x, int a, int b, int c) {
    return a * x * x + b * x + c;
}
```

---

## Minimum Swaps To Group All Ones

**Problem:**
Given a binary array `nums`, find the **minimum number of swaps** needed to group **all 1s together** in a contiguous block.

**Logic:**

1. Count total number of `1`s = `ones`.
2. Consider every window of size `ones`.
3. Minimum swaps = minimum number of `0`s present in any such window.

**Java Solution:**

```java
public static int minSwapsToGroupOnes(int[] nums) {
    int n = nums.length;
    int ones = 0;
    for (int x : nums) if (x == 1) ones++;
    if (ones <= 1) return 0;

    int zerosInWindow = 0;
    int left = 0;
    int minZeros = Integer.MAX_VALUE;

    for (int right = 0; right < n; right++) {
        if (nums[right] == 0) zerosInWindow++;

        if (right - left + 1 > ones) {
            if (nums[left] == 0) zerosInWindow--;
            left++;
        }

        if (right - left + 1 == ones) {
            minZeros = Math.min(minZeros, zerosInWindow);
        }
    }
    return minZeros;
}
```

---

## Minimum Moves To Equalize Array

**Problem:**
Given `nums`, in one move you can **increase or decrease any element by 1**.
Return the **minimum number of moves** to make all array elements equal.

**Logic:**

* Best value to make all elements equal to is the **median**.
* Total moves = sum of `|nums[i] − median|`.

**Java Solution:**

```java
import java.util.*;

public static int minMovesToEqualize(int[] nums) {
    Arrays.sort(nums);
    int n = nums.length;
    int median = nums[n / 2];
    long moves = 0;
    for (int x : nums) {
        moves += Math.abs(x - median);
    }
    return (int) moves;
}
```

---

## Minimum Indices To Equal Even-Odd Sums

**Problem:**
Given `nums`, consider removing exactly one element at index `i`.
After removal, elements to the right shift left, so their index parity (even/odd) changes.
Count how many indices `i` we can remove such that **sum of elements at even indices = sum of elements at odd indices** in the new array.

**Logic:**

* Precompute prefix sums for even and odd indices.
* For each index `i`, compute:

  * Left even sum, left odd sum (unchanged).
  * Right even sum and odd sum (but parity flips after removal).
* Check if final even sum == final odd sum.

**Java Solution:**

```java
public static int countIndicesToMakeEvenOddSumsEqual(int[] nums) {
    int n = nums.length;
    int[] prefEven = new int[n + 1];
    int[] prefOdd = new int[n + 1];

    for (int i = 0; i < n; i++) {
        prefEven[i + 1] = prefEven[i];
        prefOdd[i + 1] = prefOdd[i];
        if (i % 2 == 0) prefEven[i + 1] += nums[i];
        else prefOdd[i + 1] += nums[i];
    }

    int totalEven = prefEven[n];
    int totalOdd = prefOdd[n];
    int count = 0;

    for (int i = 0; i < n; i++) {
        int leftEven = prefEven[i];
        int leftOdd = prefOdd[i];

        int rightEven = totalOdd - prefOdd[i + 1]; // becomes even after shift
        int rightOdd = totalEven - prefEven[i + 1]; // becomes odd after shift

        if (leftEven + rightEven == leftOdd + rightOdd) {
            count++;
        }
    }
    return count;
}
```

---

If you want, next I can:

* Combine **Easy + Medium** into one big GitHub DSA repo layout, or
* Add a **small example** under each problem to make it even easier to understand.
