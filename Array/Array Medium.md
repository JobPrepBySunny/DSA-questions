
---
# Medium Array Coding Problems (Java Solutions)

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

> You can wrap all methods inside a single class like
> `public class MediumArrayProblems { ... }`
> for practice / reuse.

---

## Next Permutation

**Logic:**
Find first index `i` from right where `nums[i] < nums[i+1]`.
Find just greater element to the right and swap, then reverse suffix.
If no such `i`, just reverse entire array (last permutation → first).

```java
public static void nextPermutation(int[] nums) {
    int n = nums.length;
    int i = n - 2;

    while (i >= 0 && nums[i] >= nums[i + 1]) {
        i--;
    }

    if (i >= 0) {
        int j = n - 1;
        while (nums[j] <= nums[i]) j--;
        swap(nums, i, j);
    }

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

**Complexity:** `O(n)` time, `O(1)` space.

---

## Majority Element

> Element appearing **more than n/2 times** (guaranteed to exist).

**Logic:**
Boyer–Moore voting: maintain candidate + count.

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

**Complexity:** `O(n)` time, `O(1)` space.

---

## Majority Element II

> All elements appearing **more than n/3 times** (at most 2 such elements).

**Logic:**
Extended Boyer–Moore with 2 candidates, then verify.

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

> Given heights `arr[]` and integer `k`, increase or decrease **each element by at most k**.
> Minimize max height − min height.

**Logic:**
Sort array, assume initial diff = last − first. Then try modifying prefix as +k, suffix as −k and track min diff.

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

**Logic:**
Kadane’s Algorithm: running sum reset when it goes below 0.

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

**Logic:**
Because negative can flip sign, keep both maxProd and minProd at each step.

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

**Logic:**
Prefix product pass, then suffix product pass (no division).

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

> Count subarrays where product `< k`.

**Logic:**
Sliding window with product, shrink when product ≥ k.

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

> Number of ways to split into 3 contiguous parts having equal sum.

**Logic:**
Total sum must be divisible by 3.
Count prefix positions where prefixSum == sum/3 and where prefixSum == 2*sum/3 (for later indices).

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

    // last cut must be before last element
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

> Find max consecutive 1s if you can flip **at most `k` zeros** to 1.

**Logic:**
Sliding window tracking count of zeros; shrink when zeros > k.

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

> Plank of length `n`. Ants moving left or right at 1 unit/sec.
> `left[]` = positions of ants moving left, `right[]` = positions of ants moving right.
> Find the last moment when an ant is still on the plank.

**Logic:**
Ants are indistinguishable after collisions → just max of:

* farthest left ant time = its position
* farthest right ant time = `n - position`

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

> In a binary array, find index of `0` to flip to `1` to get **longest contiguous 1s**.

**Logic:**
Track index of previous zero and previous-to-previous zero to maintain longest window with at most 1 zero.

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
            bestIndex = prevZero; // flip this zero
        }
    }
    return bestIndex;
}
```

---

## Intersection of Interval Lists

> Given two sorted lists of disjoint intervals, return their intersections.

**Logic:**
Two pointers; overlap = `max(start1, start2)` to `min(end1, end2)` if valid.

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

> Rearrange so that positives and negatives **alternate**, starting with positive.
> (Assume equal count or difference ≤ 1).

**Logic:**
Store positives and negatives separately, then merge.

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

> Each person has available slots `[start, end]`.
> Find earliest common slot of at least `duration` minutes.

**Logic:**
Sort both slot lists, use two pointers, check overlap length.

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

> A mountain: strictly increasing then strictly decreasing, length ≥ 3.
> Return length of longest mountain.

**Logic:**
For each index, precompute increasing length from left and decreasing from right, then combine.

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

> Given `nums` and `a, b, c`, transform each element:
> `f(x) = a*x*x + b*x + c`, then return sorted result.

**Logic:**
Apply function; if `a == 0`, it’s linear; otherwise use two-pointer technique because f(x) is monotonic on each side of vertex when nums is sorted. (Assume `nums` sorted.)

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

> Minimum swaps needed to group all `1`s together (adjacent).

**Logic:**
Let `ones = count(1)`.
Use sliding window of size `ones` and minimize number of zeros inside.

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

> In one move, you can **increment or decrement any element by 1**.
> Find minimum moves to make all elements equal.

**Logic:**
Optimal target is the **median**; total moves = sum of absolute differences from median.

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

> Count indices such that **removing nums[i]** makes
> sum of elements at even indices == sum of elements at odd indices.

(After removal, indices to the right shift by 1, so parity changes.)

**Logic:**
Prefix sums for even/odd, then simulate removal.

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
        // sums on left side stay same
        int leftEven = prefEven[i];
        int leftOdd = prefOdd[i];

        // on right side, indices shift parity
        int rightEven = totalOdd - prefOdd[i + 1];
        int rightOdd = totalEven - prefEven[i + 1];

        if (leftEven + rightEven == leftOdd + rightOdd) {
            count++;
        }
    }
    return count;
}
```

---

If you want next, I can:

* Combine **all easy + medium** into a single big GitHub-ready README, or
* Add a small **problem statement line** above each code block for quick recall.
