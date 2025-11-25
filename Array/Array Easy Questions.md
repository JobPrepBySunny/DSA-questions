
## 📌 Navigation

1. [Second Largest Element](#1-second-largest-element)
2. [Third Largest Element](#2-third-largest-element)
3. [Reverse an Array](#3-reverse-an-array)
4. [Reverse Array in Groups](#4-reverse-array-in-groups)
5. [Rotate Array (Right by k)](#5-rotate-array-right-by-k)
6. [Three Great Candidates (Max Product of 3 Numbers)](#6-three-great-candidates-max-product-of-3-numbers)
7. [Max Consecutive Ones](#7-max-consecutive-ones)
8. [Move All Zeroes To End (Stable)](#8-move-all-zeroes-to-end-stable)
9. [Wave Array (Sort + Swap Adjacent)](#9-wave-array-sort--swap-adjacent)
10. [Plus One (Array of Digits)](#10-plus-one-array-of-digits)
11. [Stock Buy and Sell – One Transaction](#11-stock-buy-and-sell--one-transaction)
12. [Stock Buy and Sell – Multiple Transactions](#12-stock-buy-and-sell--multiple-transactions)
13. [Remove Duplicates from Sorted Array (In-Place)](#13-remove-duplicates-from-sorted-array-in-place)
14. [Alternate Positive Negative](#14-alternate-positive-negative)
15. [Array Leaders](#15-array-leaders)
16. [Missing and Repeating in Array (1n)](#16-missing-and-repeating-in-array-1n)
17. [Missing Ranges of Numbers](#17-missing-ranges-of-numbers)
18. [Sum of all Subarrays](#18-sum-of-all-subarrays)

---

## 1. Second Largest Element

**Logic:**
Scan once, track `largest` and `secondLargest`. Update as you go.

```java
public static int secondLargest(int[] arr) {
    if (arr == null || arr.length < 2) {
        throw new IllegalArgumentException("Need at least 2 elements");
    }
    int max = Integer.MIN_VALUE;
    int second = Integer.MIN_VALUE;

    for (int x : arr) {
        if (x > max) {
            second = max;
            max = x;
        } else if (x > second && x != max) {
            second = x;
        }
    }
    if (second == Integer.MIN_VALUE) {
        throw new IllegalArgumentException("No second distinct largest element");
    }
    return second;
}
```

---

## 2. Third Largest Element

**Logic:**
Same idea, but keep top 3 values.

```java
public static int thirdLargest(int[] arr) {
    if (arr == null || arr.length < 3) {
        throw new IllegalArgumentException("Need at least 3 elements");
    }
    int first = Integer.MIN_VALUE;
    int second = Integer.MIN_VALUE;
    int third = Integer.MIN_VALUE;

    for (int x : arr) {
        if (x > first) {
            third = second;
            second = first;
            first = x;
        } else if (x > second && x != first) {
            third = second;
            second = x;
        } else if (x > third && x != second && x != first) {
            third = x;
        }
    }
    if (third == Integer.MIN_VALUE) {
        throw new IllegalArgumentException("No third distinct largest element");
    }
    return third;
}
```

---

## 3. Reverse an Array

**Logic:**
Two-pointer swap, start and end.

```java
public static void reverse(int[] arr) {
    int i = 0, j = arr.length - 1;
    while (i < j) {
        int temp = arr[i];
        arr[i] = arr[j];
        arr[j] = temp;
        i++;
        j--;
    }
}
```

---

## 4. Reverse Array in Groups

**Logic:**
For each block of size `k`, reverse that segment.

```java
public static void reverseInGroups(int[] arr, int k) {
    int n = arr.length;
    for (int i = 0; i < n; i += k) {
        int left = i;
        int right = Math.min(i + k - 1, n - 1);
        while (left < right) {
            int temp = arr[left];
            arr[left] = arr[right];
            arr[right] = temp;
            left++;
            right--;
        }
    }
}
```

---

## 5. Rotate Array (Right by k)

**Logic:**
Normalize `k`, reverse full array, then reverse two parts.

```java
public static void rotateRight(int[] arr, int k) {
    int n = arr.length;
    if (n == 0) return;
    k = k % n;
    if (k < 0) k += n;  // handle negative if needed

    reverseRange(arr, 0, n - 1);
    reverseRange(arr, 0, k - 1);
    reverseRange(arr, k, n - 1);
}

private static void reverseRange(int[] arr, int i, int j) {
    while (i < j) {
        int tmp = arr[i];
        arr[i] = arr[j];
        arr[j] = tmp;
        i++;
        j--;
    }
}
```

---

## 6. Three Great Candidates (Max Product of 3 Numbers)

**Logic:**
Sorted view: answer is max of
`max1 * max2 * max3` OR `max1 * min1 * min2` (for negatives).
Track 3 largest and 2 smallest in one pass.

```java
public static int maxProductOfThree(int[] arr) {
    int max1 = Integer.MIN_VALUE, max2 = Integer.MIN_VALUE, max3 = Integer.MIN_VALUE;
    int min1 = Integer.MAX_VALUE, min2 = Integer.MAX_VALUE;

    for (int x : arr) {
        // largest three
        if (x > max1) {
            max3 = max2;
            max2 = max1;
            max1 = x;
        } else if (x > max2) {
            max3 = max2;
            max2 = x;
        } else if (x > max3) {
            max3 = x;
        }

        // smallest two
        if (x < min1) {
            min2 = min1;
            min1 = x;
        } else if (x < min2) {
            min2 = x;
        }
    }

    int prod1 = max1 * max2 * max3;
    int prod2 = max1 * min1 * min2;
    return Math.max(prod1, prod2);
}
```

---

## 7. Max Consecutive Ones

**Logic:**
Count current run of 1s, track max.

```java
public static int maxConsecutiveOnes(int[] arr) {
    int max = 0, curr = 0;
    for (int x : arr) {
        if (x == 1) {
            curr++;
            max = Math.max(max, curr);
        } else {
            curr = 0;
        }
    }
    return max;
}
```

---

## 8. Move All Zeroes To End (Stable)

**Logic:**
Two indices: one to place next non-zero; then fill remaining with 0.

```java
public static void moveZeroesToEnd(int[] arr) {
    int index = 0;
    for (int x : arr) {
        if (x != 0) {
            arr[index++] = x;
        }
    }
    while (index < arr.length) {
        arr[index++] = 0;
    }
}
```

---

## 9. Wave Array (Sort + Swap Adjacent)

**Logic:**
Sort, then swap (0,1), (2,3), …

```java
import java.util.Arrays;

public static void waveArray(int[] arr) {
    Arrays.sort(arr);
    for (int i = 0; i + 1 < arr.length; i += 2) {
        int temp = arr[i];
        arr[i] = arr[i + 1];
        arr[i + 1] = temp;
    }
}
```

---

## 10. Plus One (Array of Digits)

**Logic:**
Add one from last index; handle carry. If carry remains, create new array with extra digit.

```java
public static int[] plusOne(int[] digits) {
    int n = digits.length;
    for (int i = n - 1; i >= 0; i--) {
        if (digits[i] < 9) {
            digits[i]++;
            return digits;
        }
        digits[i] = 0;
    }
    int[] res = new int[n + 1];
    res[0] = 1;
    return res;
}
```

---

## 11. Stock Buy and Sell – One Transaction

**Logic:**
Track min price so far; at each price compute profit = price – minSoFar.

```java
public static int maxProfitOneTransaction(int[] prices) {
    if (prices == null || prices.length == 0) return 0;
    int minPrice = prices[0];
    int maxProfit = 0;

    for (int p : prices) {
        if (p < minPrice) {
            minPrice = p;
        } else {
            maxProfit = Math.max(maxProfit, p - minPrice);
        }
    }
    return maxProfit;
}
```

---

## 12. Stock Buy and Sell – Multiple Transactions

**Logic:**
Sum all positive differences of consecutive days.

```java
public static int maxProfitMultipleTransactions(int[] prices) {
    int profit = 0;
    for (int i = 1; i < prices.length; i++) {
        if (prices[i] > prices[i - 1]) {
            profit += prices[i] - prices[i - 1];
        }
    }
    return profit;
}
```

---

## 13. Remove Duplicates from Sorted Array (In-Place)

**Logic:**
Two pointers: `i` = index of last unique, `j` scan.

```java
public static int removeDuplicates(int[] nums) {
    if (nums.length == 0) return 0;
    int i = 0;
    for (int j = 1; j < nums.length; j++) {
        if (nums[j] != nums[i]) {
            i++;
            nums[i] = nums[j];
        }
    }
    // length of unique part = i + 1
    return i + 1;
}
```

---

## 14. Alternate Positive Negative

**Logic (simple version):**
First collect positives and negatives in two arrays, then merge alternately (starting with positive).

```java
import java.util.*;

public static int[] rearrangeAlternatePosNeg(int[] arr) {
    List<Integer> pos = new ArrayList<>();
    List<Integer> neg = new ArrayList<>();
    for (int x : arr) {
        if (x >= 0) pos.add(x);
        else neg.add(x);
    }
    int i = 0, p = 0, n = 0;
    boolean turnPos = true; // start with positive

    while (p < pos.size() && n < neg.size()) {
        arr[i++] = turnPos ? pos.get(p++) : neg.get(n++);
        turnPos = !turnPos;
    }
    // append remaining
    while (p < pos.size()) arr[i++] = pos.get(p++);
    while (n < neg.size()) arr[i++] = neg.get(n++);
    return arr;
}
```

---

## 15. Array Leaders

**Logic:**
Traverse from right, track current max. If element ≥ maxSoFar, it is a leader.

```java
import java.util.*;

public static List<Integer> arrayLeaders(int[] arr) {
    List<Integer> leaders = new ArrayList<>();
    int n = arr.length;
    int maxFromRight = Integer.MIN_VALUE;

    for (int i = n - 1; i >= 0; i--) {
        if (arr[i] >= maxFromRight) {
            leaders.add(arr[i]);
            maxFromRight = arr[i];
        }
    }
    Collections.reverse(leaders);
    return leaders;
}
```

---

## 16. Missing and Repeating in Array (1..n)

**Logic:**
Use math:
`sum = Σa[i]`, `sumSq = Σ(a[i]^2)` vs theoretical sums. Solve for missing (M) and repeating (R).

```java
public static int[] findMissingAndRepeating(int[] arr) {
    int n = arr.length;
    long sum = 0, sumSq = 0;
    long expectedSum = (long) n * (n + 1) / 2;
    long expectedSumSq = (long) n * (n + 1) * (2L * n + 1) / 6;

    for (int x : arr) {
        sum += x;
        sumSq += (long) x * x;
    }

    long diff = expectedSum - sum;           // M - R
    long diffSq = expectedSumSq - sumSq;     // M^2 - R^2 = (M - R)(M + R)

    long sumMR = diffSq / diff;             // M + R

    long missing = (diff + sumMR) / 2;
    long repeating = sumMR - missing;

    return new int[]{(int) missing, (int) repeating};
}
```

---

## 17. Missing Ranges of Numbers

**Problem style:**
Given sorted array `nums` and `lower`, `upper`, print missing ranges within [lower, upper].
Return like `"5"` or `"2->4"`.

**Logic:**
Use `prev`, `curr` approach with sentinels.

```java
import java.util.*;

public static List<String> findMissingRanges(int[] nums, int lower, int upper) {
    List<String> res = new ArrayList<>();
    long prev = (long) lower - 1;  // to avoid overflow

    for (int i = 0; i <= nums.length; i++) {
        long curr = (i < nums.length) ? nums[i] : (long) upper + 1;
        if (curr - prev >= 2) {
            res.add(formatRange(prev + 1, curr - 1));
        }
        prev = curr;
    }
    return res;
}

private static String formatRange(long a, long b) {
    if (a == b) return String.valueOf(a);
    return a + "->" + b;
}
```

---

## 18. Sum of all Subarrays

**Logic (contribution):**
Each `arr[i]` appears in `(i + 1) * (n - i)` subarrays.
Total sum = Σ `arr[i] * (i+1) * (n-i)`.

```java
public static long sumOfAllSubarrays(int[] arr) {
    int n = arr.length;
    long total = 0;
    for (int i = 0; i < n; i++) {
        long contributionCount = (long) (i + 1) * (n - i);
        total += (long) arr[i] * contributionCount;
    }
    return total;
}
```

---

If you want, next I can:

* Convert all of these into one `DSAArrayUtils` class for you, or
* Add **time & space complexity** notes for each (good for interviews).
