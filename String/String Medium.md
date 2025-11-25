Nice list, this is like a full mini–string DSA sheet 🧠
I’ll keep the format same as before:

* Navigation at top
* Each problem: **Question + Example + Logic + Java code**
* “Back to top” link after every problem

You can save this as:
`Strings/Medium-Problems.md`

---

# Navigation – Medium String Problems

* [Integer to Words](#1-integer-to-words)
* [Fizz Buzz](#2-fizz-buzz)
* [Palindromic Sentence Check](#3-palindromic-sentence-check)
* [Isomorphic Strings](#4-isomorphic-strings)
* [Check for k-anagram](#5-check-for-k-anagram)
* [Equal 0,1, and 2](#6-equal-01-and-2)
* [Find and replace in String](#7-find-and-replace-in-string)
* [Look and say Pattern](#8-look-and-say-pattern)
* [Minimum repetitions to make Substring](#9-minimum-repetitions-to-make-substring)
* [Excel Sheet – I](#10-excel-sheet--i)
* [Find the N-th character](#11-find-the-n-th-character)
* [Next Palindromic Number with same digits](#12-next-palindromic-number-with-same-digits)
* [Length of longest prefix suffix](#13-length-of-longest-prefix-suffix)
* [Longest K unique characters substring](#14-longest-k-unique-characters-substring)
* [Smallest window containing all](#15-smallest-window-containing-all)
* [Longest substring without repeating characters](#16-longest-substring-without-repeating-characters)
* [Substrings of length k with k-1 distinct elements](#17-substrings-of-length-k-with-k-1-distinct-elements)
* [Count number of substrings](#18-count-number-of-substrings)
* [Interleaved Strings](#19-interleaved-strings)
* [Print Anagrams together](#20-print-anagrams-together)
* [Rank the permutation](#21-rank-the-permutation)
* [A Special Keyboard](#22-a-special-keyboard)
* [Sum of two large numbers](#23-sum-of-two-large-numbers)

---

## 1. Integer to Words

### Problem

Convert a non-negative integer to its **English words** representation (e.g., `123 → "One Hundred Twenty Three"`).

### Example

```text
Input: 123
Output: "One Hundred Twenty Three"

Input: 1000010
Output: "One Million Ten"
```

### Logic / Approach

* Break number into chunks: **Billion, Million, Thousand, rest**.
* Convert each chunk (0–999) with helper:

  * Handle `<20` using small array.
  * Handle tens (`20–90`).
  * Add `"Hundred"` when needed.
* Append scale name (Thousand/Million/Billion) when chunk > 0.

⏱ O(1) (fixed number of chunks)
📦 O(1)

### Java Code

```java
public class IntegerToWords {

    private static final String[] BELOW_20 = {"", "One", "Two", "Three", "Four", "Five", "Six",
            "Seven", "Eight", "Nine", "Ten", "Eleven", "Twelve", "Thirteen", "Fourteen",
            "Fifteen", "Sixteen", "Seventeen", "Eighteen", "Nineteen"};

    private static final String[] TENS = {"", "", "Twenty", "Thirty", "Forty", "Fifty",
            "Sixty", "Seventy", "Eighty", "Ninety"};

    public static String numberToWords(int num) {
        if (num == 0) return "Zero";

        return helper(num).trim();
    }

    private static String helper(int num) {
        if (num == 0) return "";
        else if (num < 20) return BELOW_20[num] + " ";
        else if (num < 100)
            return TENS[num / 10] + " " + helper(num % 10);
        else if (num < 1000)
            return BELOW_20[num / 100] + " Hundred " + helper(num % 100);
        else if (num < 1_000_000)
            return helper(num / 1000) + "Thousand " + helper(num % 1000);
        else if (num < 1_000_000_000)
            return helper(num / 1_000_000) + "Million " + helper(num % 1_000_000);
        else
            return helper(num / 1_000_000_000) + "Billion " + helper(num % 1_000_000_000);
    }
}
```

🔝 [Back to Top](#navigation--medium-string-problems)

---

## 2. Fizz Buzz

### Problem

Print numbers from 1 to n, but:

* Multiples of 3 → `"Fizz"`
* Multiples of 5 → `"Buzz"`
* Multiples of both → `"FizzBuzz"`

### Logic / Approach

* Loop `i = 1..n` and check:

  * if `i % 15 == 0` → `"FizzBuzz"`
  * else if `%3 == 0` → `"Fizz"`
  * else if `%5 == 0` → `"Buzz"`
  * else → `i` as string.

### Java Code

```java
import java.util.*;

public class FizzBuzz {

    public static List<String> fizzBuzz(int n) {
        List<String> res = new ArrayList<>();

        for (int i = 1; i <= n; i++) {
            if (i % 15 == 0) res.add("FizzBuzz");
            else if (i % 3 == 0) res.add("Fizz");
            else if (i % 5 == 0) res.add("Buzz");
            else res.add(String.valueOf(i));
        }
        return res;
    }
}
```

🔝 [Back to Top](#navigation--medium-string-problems)

---

## 3. Palindromic Sentence Check

### Problem

Check if a **sentence** is palindrome **ignoring spaces, punctuation, and case**.

### Example

```text
Input:  "A man, a plan, a canal: Panama"
Output: true
```

### Logic / Approach

* Use two pointers `i`, `j`.
* Move `i` forward until alphanumeric; `j` backward similarly.
* Compare lowercase chars; if mismatch → false.

### Java Code

```java
public class PalindromicSentenceCheck {

    public static boolean isPalindromeSentence(String s) {
        int i = 0, j = s.length() - 1;

        while (i < j) {
            char left = s.charAt(i);
            char right = s.charAt(j);

            if (!Character.isLetterOrDigit(left)) {
                i++; continue;
            }
            if (!Character.isLetterOrDigit(right)) {
                j--; continue;
            }

            if (Character.toLowerCase(left) != Character.toLowerCase(right)) {
                return false;
            }
            i++;
            j--;
        }
        return true;
    }
}
```

🔝 [Back to Top](#navigation--medium-string-problems)

---

## 4. Isomorphic Strings

### Problem

Two strings `s` and `t` are **isomorphic** if characters in `s` can be replaced to get `t`, with:

* one-to-one mapping
* no two chars in `s` map to same char in `t`.

### Example

```text
"egg", "add"   → true
"foo", "bar"   → false
```

### Logic / Approach

* Maintain 2 maps:

  * `sChar -> tChar`
  * `tChar -> sChar`
* While scanning, ensure mapping is consistent both ways.

### Java Code

```java
import java.util.*;

public class IsomorphicStrings {

    public static boolean isIsomorphic(String s, String t) {
        if (s.length() != t.length()) return false;

        Map<Character, Character> mapST = new HashMap<>();
        Map<Character, Character> mapTS = new HashMap<>();

        for (int i = 0; i < s.length(); i++) {
            char c1 = s.charAt(i), c2 = t.charAt(i);

            if (mapST.containsKey(c1) && mapST.get(c1) != c2) return false;
            if (mapTS.containsKey(c2) && mapTS.get(c2) != c1) return false;

            mapST.put(c1, c2);
            mapTS.put(c2, c1);
        }
        return true;
    }
}
```

🔝 [Back to Top](#navigation--medium-string-problems)

---

## 5. Check for k-anagram

### Problem

Two strings are **k-anagrams** if:

* same length
* can be made anagrams by **changing at most k characters** in one string.

### Example

```text
s1 = "anagram", s2 = "grammar", k = 3 → true
```

### Logic / Approach

* Count frequencies of s1 and s2.
* For each char: if `freq1[c] > freq2[c]`, extras must be changed.
* Sum of extras = number of changes needed; check `<= k`.

### Java Code

```java
public class KAnagramCheck {

    public static boolean areKAnagrams(String s1, String s2, int k) {
        if (s1.length() != s2.length()) return false;

        int[] freq1 = new int[26];
        int[] freq2 = new int[26];

        for (char c : s1.toCharArray()) freq1[c - 'a']++;
        for (char c : s2.toCharArray()) freq2[c - 'a']++;

        int changes = 0;
        for (int i = 0; i < 26; i++) {
            if (freq1[i] > freq2[i]) {
                changes += freq1[i] - freq2[i];
            }
        }
        return changes <= k;
    }
}
```

🔝 [Back to Top](#navigation--medium-string-problems)

---

## 6. Equal 0,1, and 2

### Problem

Given a string of `0`, `1`, `2`, count **number of substrings** having **equal number of 0, 1, and 2**.

### Logic / Approach

* Let counts: `c0, c1, c2`.
* For any two indices `i < j`, substring `i..j` has equal 0,1,2 if:

  * `(c1-c0)` and `(c2-c1)` are same at i and j.
* So we store a map:

  * key = pair `(c1-c0, c2-c1)`
  * value = frequency of that pair so far
* For each position, add number of previous same pairs to answer.

### Java Code

```java
import java.util.*;

public class Equal012Substrings {

    private static class Pair {
        int a, b;
        Pair(int a, int b) { this.a = a; this.b = b; }
        @Override public boolean equals(Object o) {
            if (this == o) return true;
            if (!(o instanceof Pair)) return false;
            Pair p = (Pair) o;
            return a == p.a && b == p.b;
        }
        @Override public int hashCode() { return Objects.hash(a, b); }
    }

    public static long countSubstrings(String s) {
        int c0 = 0, c1 = 0, c2 = 0;
        Map<Pair, Integer> map = new HashMap<>();

        Pair start = new Pair(0, 0);
        map.put(start, 1);
        long ans = 0;

        for (char ch : s.toCharArray()) {
            if (ch == '0') c0++;
            else if (ch == '1') c1++;
            else if (ch == '2') c2++;

            Pair key = new Pair(c1 - c0, c2 - c1);
            int freq = map.getOrDefault(key, 0);
            ans += freq;
            map.put(key, freq + 1);
        }
        return ans;
    }
}
```

🔝 [Back to Top](#navigation--medium-string-problems)

---

## 7. Find and replace in String

### Problem (simple version)

Given a string `s`, a pattern `p`, and a replacement `r`, **replace all non-overlapping occurrences** of `p` in `s` with `r`.

### Example

```text
s = "abcabc", p = "ab", r = "x"
Output: "xcxc"
```

### Logic / Approach

* Iterate using `indexOf(p, start)` to find next match.
* Append substring before match and replacement.
* Move `start` after matched pattern.

### Java Code

```java
public class FindAndReplace {

    public static String findAndReplace(String s, String pattern, String repl) {
        StringBuilder sb = new StringBuilder();
        int start = 0;
        int idx;

        while ((idx = s.indexOf(pattern, start)) != -1) {
            sb.append(s, start, idx);
            sb.append(repl);
            start = idx + pattern.length();
        }
        sb.append(s.substring(start));
        return sb.toString();
    }
}
```

🔝 [Back to Top](#navigation--medium-string-problems)

---

## 8. Look and say Pattern

### Problem

Generate the **n-th term** of the look-and-say sequence:
1 → "1"
2 → "11" (one 1)
3 → "21" (two 1s)
4 → "1211" (one 2, one 1), etc.

### Logic / Approach

* Start with `"1"`.
* For each step:

  * Scan previous term, count consecutive same digits.
  * Append `count + digit`.

### Java Code

```java
public class LookAndSay {

    public static String lookAndSay(int n) {
        String s = "1";
        for (int i = 2; i <= n; i++) {
            s = next(s);
        }
        return s;
    }

    private static String next(String s) {
        StringBuilder sb = new StringBuilder();
        int i = 0, len = s.length();

        while (i < len) {
            char c = s.charAt(i);
            int count = 0;
            while (i < len && s.charAt(i) == c) {
                count++;
                i++;
            }
            sb.append(count).append(c);
        }
        return sb.toString();
    }
}
```

🔝 [Back to Top](#navigation--medium-string-problems)

---

## 9. Minimum repetitions to make Substring

### Problem

Given strings `A` and `B`, find **minimum number of times** we must repeat `A` such that `B` is a **substring** of that repeated string. If impossible, return `-1`.

### Example

```text
A = "abcd", B = "cdabcdab"
Output: 3   ("abcdabcdabcd" contains B)
```

### Logic / Approach

* Repeat `A` until length ≥ `B.length()`.
* Check if `B` is substring; if yes, return repeats.
* Also check one more repeat (for overlap).
* If still not found, return -1.

### Java Code

```java
public class MinRepeatsSubstring {

    public static int minRepeats(String A, String B) {
        StringBuilder sb = new StringBuilder(A);
        int count = 1;

        while (sb.length() < B.length()) {
            sb.append(A);
            count++;
        }
        if (sb.indexOf(B) != -1) return count;

        sb.append(A);
        count++;

        if (sb.indexOf(B) != -1) return count;
        return -1;
    }
}
```

🔝 [Back to Top](#navigation--medium-string-problems)

---

## 10. Excel Sheet – I

### Problem

Convert a **column number** to its Excel column title.
1 → A, 2 → B, …, 26 → Z, 27 → AA, etc.

### Logic / Approach

* Repeatedly:

  * `n--`, then `ch = 'A' + (n % 26)`.
  * `n = n / 26`.
* Build string in reverse.

### Java Code

```java
public class ExcelSheetColumnTitle {

    public static String convertToTitle(int n) {
        StringBuilder sb = new StringBuilder();

        while (n > 0) {
            n--; // adjust to 0-based
            char c = (char) ('A' + (n % 26));
            sb.append(c);
            n /= 26;
        }
        return sb.reverse().toString();
    }
}
```

🔝 [Back to Top](#navigation--medium-string-problems)

---

## 11. Find the N-th character

### Problem (one common version)

Given an **encoded string** with pattern `char + count` (e.g., `"a2b3"` → `"aabbb"`),
find the **N-th character** in the decoded string **without fully expanding** if possible.

### Example

```text
s = "a2b3", decoded = "aabbb"
N = 4 → 'b'
```

### Logic / Approach

* First pass: compute total decoded length:

  * For each pattern (c, num), add `num`.
* Second pass: again parse:

  * Keep running length; when `prefix + num >= N`, answer is that char.

### Java Code

```java
public class NthCharacterDecoded {

    public static char findNthChar(String s, int N) {
        int n = s.length();
        int total = 0;

        // first pass: total length
        for (int i = 0; i < n; ) {
            char c = s.charAt(i++);
            int num = 0;
            while (i < n && Character.isDigit(s.charAt(i))) {
                num = num * 10 + (s.charAt(i) - '0');
                i++;
            }
            total += num;
        }

        if (N > total) throw new IllegalArgumentException("N too large");

        // second pass
        int prefix = 0;
        for (int i = 0; i < n; ) {
            char c = s.charAt(i++);
            int num = 0;
            while (i < n && Character.isDigit(s.charAt(i))) {
                num = num * 10 + (s.charAt(i) - '0');
                i++;
            }
            if (prefix + num >= N) return c;
            prefix += num;
        }
        throw new IllegalStateException("Should not reach");
    }
}
```

🔝 [Back to Top](#navigation--medium-string-problems)

---

## 12. Next Palindromic Number with same digits

### Problem

Given a number as string, find the **next higher palindromic number** using **exactly the same digits**.
If not possible, return `"-1"`.

### Logic / Approach

* For single / two digits → usually no answer.
* For length ≥ 3:

  * Work on the **left half** (including middle if odd).
  * Try to generate **next permutation** of this half.
  * If none, return `-1`.
  * Mirror the updated half to form full palindrome.

### Java Code

```java
public class NextPalindromicSameDigits {

    public static String nextPalin(String s) {
        int n = s.length();
        if (n <= 3) return "-1";

        char[] arr = s.toCharArray();
        int mid = n / 2 - 1; // last index of left-half

        // next permutation on arr[0..mid]
        if (!nextPermutation(arr, 0, mid)) return "-1";

        // mirror left to right
        for (int i = 0; i <= mid; i++) {
            arr[n - 1 - i] = arr[i];
        }
        return new String(arr);
    }

    private static boolean nextPermutation(char[] arr, int l, int r) {
        int i = r - 1;
        while (i >= l && arr[i] >= arr[i + 1]) i--;
        if (i < l) return false;

        int j = r;
        while (arr[j] <= arr[i]) j--;
        swap(arr, i, j);

        reverse(arr, i + 1, r);
        return true;
    }

    private static void swap(char[] a, int i, int j) {
        char t = a[i]; a[i] = a[j]; a[j] = t;
    }

    private static void reverse(char[] a, int i, int j) {
        while (i < j) swap(a, i++, j--);
    }
}
```

🔝 [Back to Top](#navigation--medium-string-problems)

---

## 13. Length of longest prefix suffix

### Problem

Given a string, find the length of the **longest proper prefix** which is also a **proper suffix**.
(This is the **LPS** value from KMP.)

### Example

```text
"abab"   → "ab" length 2
"aaaa"   → "aaa" length 3
```

### Logic / Approach

* Use KMP prefix-function:

  * `lps[i]` = longest proper prefix of `s[0..i]` which is also suffix.
* Final answer: `lps[n-1]`.

### Java Code

```java
public class LongestPrefixSuffix {

    public static int lps(String s) {
        int n = s.length();
        int[] lps = new int[n];
        int len = 0, i = 1;

        while (i < n) {
            if (s.charAt(i) == s.charAt(len)) {
                len++;
                lps[i] = len;
                i++;
            } else {
                if (len != 0) {
                    len = lps[len - 1];
                } else {
                    lps[i] = 0;
                    i++;
                }
            }
        }
        return lps[n - 1];
    }
}
```

🔝 [Back to Top](#navigation--medium-string-problems)

---

## 14. Longest K unique characters substring

### Problem

Given a string and integer `k`, find the **length of longest substring** that contains **exactly k distinct characters**.

### Logic / Approach

* Use sliding window with `HashMap<char, count>`.
* Expand `right`, add chars;
* While distinct chars > k → shrink from `left`.
* Whenever distinct == k → update answer with window length.

### Java Code

```java
import java.util.*;

public class LongestKUniqueSubstring {

    public static int longestKUnique(String s, int k) {
        Map<Character, Integer> freq = new HashMap<>();
        int left = 0, ans = -1;

        for (int right = 0; right < s.length(); right++) {
            char c = s.charAt(right);
            freq.put(c, freq.getOrDefault(c, 0) + 1);

            while (freq.size() > k) {
                char lc = s.charAt(left++);
                freq.put(lc, freq.get(lc) - 1);
                if (freq.get(lc) == 0) freq.remove(lc);
            }

            if (freq.size() == k) {
                ans = Math.max(ans, right - left + 1);
            }
        }
        return ans;
    }
}
```

🔝 [Back to Top](#navigation--medium-string-problems)

---

## 15. Smallest window containing all

### Problem

Given string `s` and pattern `p`, find **smallest window in s that contains all characters of p** (including multiplicities).

### Logic / Approach

* Count frequencies of pattern chars.
* Use sliding window, keep `formed` count of how many required chars satisfied.
* When all required met, try to shrink from left and update minimum window.

### Java Code

```java
import java.util.*;

public class SmallestWindowContainingAll {

    public static String minWindow(String s, String t) {
        if (t.length() > s.length()) return "";

        int[] need = new int[128];
        for (char c : t.toCharArray()) need[c]++;

        int required = t.length();
        int left = 0, minLen = Integer.MAX_VALUE, start = 0;

        for (int right = 0; right < s.length(); right++) {
            char c = s.charAt(right);
            if (need[c] > 0) required--;
            need[c]--;

            while (required == 0) {
                if (right - left + 1 < minLen) {
                    minLen = right - left + 1;
                    start = left;
                }
                char lc = s.charAt(left++);
                need[lc]++;
                if (need[lc] > 0) required++;
            }
        }
        return (minLen == Integer.MAX_VALUE) ? "" : s.substring(start, start + minLen);
    }
}
```

🔝 [Back to Top](#navigation--medium-string-problems)

---

## 16. Longest substring without repeating characters

### Problem

Find length of **longest substring with all distinct characters**.

### Logic / Approach

* Sliding window + last index map.
* Move `right`, if char seen before inside window → move `left` after its last position.

### Java Code

```java
import java.util.*;

public class LongestSubstringNoRepeat {

    public static int lengthOfLongestSubstring(String s) {
        Map<Character, Integer> last = new HashMap<>();
        int left = 0, ans = 0;

        for (int right = 0; right < s.length(); right++) {
            char c = s.charAt(right);
            if (last.containsKey(c) && last.get(c) >= left) {
                left = last.get(c) + 1;
            }
            last.put(c, right);
            ans = Math.max(ans, right - left + 1);
        }
        return ans;
    }
}
```

🔝 [Back to Top](#navigation--medium-string-problems)

---

## 17. Substrings of length k with k-1 distinct elements

### Problem

Count substrings of length `k` that have **exactly k−1 distinct characters**
(i.e., one character is repeated twice, all others unique).

### Logic / Approach

* Sliding window of size `k`.
* Maintain frequency map;
* For each window:

  * if `distinct == k-1` → count++.
* Move window: add new char, remove old.

### Java Code

```java
import java.util.*;

public class SubstringsKWithKMinus1Distinct {

    public static int countSubstrings(String s, int k) {
        if (k > s.length()) return 0;
        Map<Character, Integer> freq = new HashMap<>();
        int distinct = 0, ans = 0;

        for (int i = 0; i < s.length(); i++) {
            char c = s.charAt(i);
            freq.put(c, freq.getOrDefault(c, 0) + 1);
            if (freq.get(c) == 1) distinct++;

            if (i >= k) {
                char out = s.charAt(i - k);
                freq.put(out, freq.get(out) - 1);
                if (freq.get(out) == 0) {
                    freq.remove(out);
                    distinct--;
                }
            }

            if (i >= k - 1 && distinct == k - 1) {
                ans++;
            }
        }
        return ans;
    }
}
```

🔝 [Back to Top](#navigation--medium-string-problems)

---

## 18. Count number of substrings

### Problem (version used here)

Count substrings of a string whose **first and last characters are the same**.

### Example

```text
"abcab"
Substrings with same first & last: "a","b","c","a","b","aba","bab","abcba" → 7 (depending on exact counting)
```

### Logic / Approach

* For each character, if it appears `f` times, then it contributes `f*(f+1)/2` substrings (choose start and end positions with same char).
* So count frequency of each character and apply formula.

### Java Code

```java
public class CountSubstringsSameEnds {

    public static long countSubstrings(String s) {
        long[] freq = new long[256];
        for (char c : s.toCharArray()) freq[c]++;

        long ans = 0;
        for (long f : freq) {
            ans += f * (f + 1) / 2;
        }
        return ans;
    }
}
```

🔝 [Back to Top](#navigation--medium-string-problems)

---

## 19. Interleaved Strings

### Problem

Given strings `A`, `B`, `C`, check if `C` is an **interleaving** of `A` and `B`.
Order of chars in A and B must be preserved.

### Logic / Approach

* If `lenC != lenA + lenB` → false.
* Use DP: `dp[i][j]` = whether `C[0..i+j-1]` can be formed by `A[0..i-1]` and `B[0..j-1]`.
* Transitions:

  * If `A[i-1] == C[i+j-1]` and `dp[i-1][j]` true → `dp[i][j] = true`.
  * If `B[j-1] == C[i+j-1]` and `dp[i][j-1]` true → `dp[i][j] = true`.

### Java Code

```java
public class InterleavedStrings {

    public static boolean isInterleave(String A, String B, String C) {
        int m = A.length(), n = B.length();
        if (C.length() != m + n) return false;

        boolean[][] dp = new boolean[m + 1][n + 1];
        dp[0][0] = true;

        for (int i = 0; i <= m; i++) {
            for (int j = 0; j <= n; j++) {
                if (i > 0 && A.charAt(i - 1) == C.charAt(i + j - 1)) {
                    dp[i][j] = dp[i][j] || dp[i - 1][j];
                }
                if (j > 0 && B.charAt(j - 1) == C.charAt(i + j - 1)) {
                    dp[i][j] = dp[i][j] || dp[i][j - 1];
                }
            }
        }
        return dp[m][n];
    }
}
```

🔝 [Back to Top](#navigation--medium-string-problems)

---

## 20. Print Anagrams together

### Problem

Given an array of strings, group all **anagrams** together.

### Logic / Approach

* For each word, sort its characters → use as **key**.
* Put words with same key in same list (HashMap).
* Finally print lists.

### Java Code

```java
import java.util.*;

public class PrintAnagramsTogether {

    public static List<List<String>> groupAnagrams(String[] words) {
        Map<String, List<String>> map = new HashMap<>();

        for (String w : words) {
            char[] arr = w.toCharArray();
            Arrays.sort(arr);
            String key = new String(arr);

            map.computeIfAbsent(key, k -> new ArrayList<>()).add(w);
        }

        return new ArrayList<>(map.values());
    }
}
```

🔝 [Back to Top](#navigation--medium-string-problems)

---

## 21. Rank the permutation

### Problem

Given a string with **distinct characters**, find its **lexicographic rank** among all permutations (1-based).

### Example

```text
"abc" → 1
"acb" → 2
"cba" → 6
```

### Logic / Approach

* For each position i:

  * Count chars smaller than `s[i]` in suffix.
  * Rank contribution = count * (factorial of remaining positions).
* Sum contributions + 1.

### Java Code

```java
public class RankOfPermutation {

    private static long fact(int n) {
        long f = 1;
        for (int i = 2; i <= n; i++) f *= i;
        return f;
    }

    public static long findRank(String s) {
        int n = s.length();
        long rank = 1;

        for (int i = 0; i < n; i++) {
            int smaller = 0;
            for (int j = i + 1; j < n; j++) {
                if (s.charAt(j) < s.charAt(i)) smaller++;
            }
            rank += smaller * fact(n - i - 1);
        }
        return rank;
    }
}
```

🔝 [Back to Top](#navigation--medium-string-problems)

---

## 22. A Special Keyboard

### Problem (classic DP version)

You have 4 keys:

* Key 1: print `'A'`
* Key 2: `Ctrl + A` (select all)
* Key 3: `Ctrl + C` (copy)
* Key 4: `Ctrl + V` (paste)

Given `N` key presses, find **maximum number of 'A's** that can be printed.

### Logic / Approach

* For `N <= 6`: best is pressing `'A'` every time → `N`.
* For larger N:

  * Last optimal sequence: some `'A'`s, then `Ctrl-A`, `Ctrl-C`, then a series of `Ctrl-V`.
  * Try all breakpoints `b` from `N-3` downwards:

    * At `b` we finish building base; then we can paste `(N - b - 1)` times.
    * Candidate = `dp[b] * (N - b - 1)`.
  * `dp[N]` = max over all b.

### Java Code

```java
public class SpecialKeyboard {

    public static int maxAs(int N) {
        if (N <= 6) return N;

        int[] dp = new int[N + 1];
        for (int i = 0; i <= 6 && i <= N; i++) dp[i] = i;

        for (int i = 7; i <= N; i++) {
            dp[i] = 0;
            for (int b = i - 3; b >= 1; b--) {
                int curr = dp[b] * (i - b - 1);
                dp[i] = Math.max(dp[i], curr);
            }
        }
        return dp[N];
    }
}
```

🔝 [Back to Top](#navigation--medium-string-problems)

---

## 23. Sum of two large numbers

### Problem

Add two **large non-negative integers** given as strings (may not fit in `long`) and return their sum as string.

### Logic / Approach

* Use indices from end of both strings.
* Keep `carry`;
* For each position:

  * `sum = digitA + digitB + carry`
  * Append `sum % 10`, update `carry = sum / 10`.
* Reverse the result at end.

### Java Code

```java
public class SumOfTwoLargeNumbers {

    public static String addStrings(String a, String b) {
        int i = a.length() - 1;
        int j = b.length() - 1;
        int carry = 0;
        StringBuilder sb = new StringBuilder();

        while (i >= 0 || j >= 0 || carry != 0) {
            int x = (i >= 0) ? a.charAt(i) - '0' : 0;
            int y = (j >= 0) ? b.charAt(j) - '0' : 0;

            int sum = x + y + carry;
            sb.append(sum % 10);
            carry = sum / 10;

            i--; j--;
        }
        return sb.reverse().toString();
    }
}
```

🔝 [Back to Top](#navigation--medium-string-problems)

---

If you want, next we can:

* Add **time/space complexity lines** explicitly under each question for interview mode,
* Or start **Hard string problems** in the exact same GitHub format.
