

---

# Navigation – Easy String Problems

* [Palindrome Check](#1-palindrome-check)
* [Reverse a String](#2-reverse-a-string)
* [Reverse Words](#3-reverse-words)
* [Check for Rotation](#4-check-for-rotation)
* [First Non Repeating](#5-first-non-repeating)
* [Roman to Integer](#6-roman-to-integer)
* [Implement Atoi](#7-implement-atoi)
* [Encrypt the String – II](#8-encrypt-the-string--ii)
* [Equal Point in Brackets](#9-equal-point-in-brackets)
* [Anagram Checking](#10-anagram-checking)
* [Panagram Checking](#11-panagram-checking)
* [Validate IP Address](#12-validate-ip-address)
* [Add Binary Strings](#13-add-binary-strings)

---

## 1. Palindrome Check

### Problem

Check if a given string is a **palindrome** (reads same forwards and backwards).

### Example

```text
Input:  "madam"
Output: true

Input:  "hello"
Output: false
```

### Logic / Approach

* Use **two pointers**: `i` at start, `j` at end.
* While `i < j`:

  * If `s.charAt(i) != s.charAt(j)` → not a palindrome.
  * Else move `i++`, `j--`.
* If loop ends, it’s a palindrome.
* (You can convert to lowercase if you want case-insensitive.)

⏱ Time: O(n)
📦 Space: O(1)

### Java Code

```java
public class PalindromeCheck {

    public static boolean isPalindrome(String s) {
        int i = 0, j = s.length() - 1;

        while (i < j) {
            if (s.charAt(i) != s.charAt(j)) {
                return false;
            }
            i++;
            j--;
        }
        return true;
    }
}
```

🔝 [Back to Top](#navigation--easy-string-problems)

---

## 2. Reverse a String

### Problem

Given a string, return its **reverse**.

### Example

```text
Input:  "sahil"
Output: "lih as" (lol just "lihas" actually 😄)
```

### Logic / Approach

* Use a **StringBuilder** and reverse. OR
* Manual two-pointer swap on a `char[]`.
* Returning new string is fine.

⏱ Time: O(n)
📦 Space: O(n) (or O(1) on char array in-place)

### Java Code

```java
public class ReverseString {

    public static String reverse(String s) {
        StringBuilder sb = new StringBuilder(s);
        return sb.reverse().toString();
    }
}
```

🔝 [Back to Top](#navigation--easy-string-problems)

---

## 3. Reverse Words

### Problem

Given a sentence, **reverse the order of words**.

### Example

```text
Input:  "the sky is blue"
Output: "blue is sky the"
```

### Logic / Approach

* Split by spaces → array of words.
* Reverse the words array.
* Join back with spaces.
* Can also handle extra spaces by trimming and skipping empty parts.

⏱ Time: O(n)
📦 Space: O(n)

### Java Code

```java
public class ReverseWords {

    public static String reverseWords(String s) {
        String[] parts = s.trim().split("\\s+");
        StringBuilder sb = new StringBuilder();

        for (int i = parts.length - 1; i >= 0; i--) {
            sb.append(parts[i]);
            if (i != 0) sb.append(" ");
        }
        return sb.toString();
    }
}
```

🔝 [Back to Top](#navigation--easy-string-problems)

---

## 4. Check for Rotation

### Problem

Given two strings `s1` and `s2`, check if `s2` is a **rotation** of `s1`.

### Example

```text
Input:  s1 = "abcd", s2 = "cdab"
Output: true    (rotation)

Input:  s1 = "abcd", s2 = "acbd"
Output: false
```

### Logic / Approach

* If lengths differ → false.
* Otherwise, check if `s2` is a substring of `s1 + s1`.

  * E.g. `"abcdabcd".contains("cdab")` → true.

⏱ Time: O(n) (average, depends on `contains` implementation)
📦 Space: O(n)

### Java Code

```java
public class CheckRotation {

    public static boolean isRotation(String s1, String s2) {
        if (s1.length() != s2.length()) return false;
        String doubled = s1 + s1;
        return doubled.contains(s2);
    }
}
```

🔝 [Back to Top](#navigation--easy-string-problems)

---

## 5. First Non Repeating

### Problem

Find the **first non-repeating character** in a string.
(If none, you can return something like `'-'` or index `-1`.)

### Example

```text
Input:  "aabbcde"
Output: 'c'

Input:  "aabb"
Output: -1 (no non-repeating)
```

### Logic / Approach

* First pass: frequency of each character (assuming ASCII, size 256 or just 26 for lowercase).
* Second pass: first char whose frequency is 1 is answer.

⏱ Time: O(n)
📦 Space: O(1) (fixed-size freq array)

### Java Code

```java
public class FirstNonRepeating {

    public static int firstNonRepeatingIndex(String s) {
        int[] freq = new int[256];

        for (int i = 0; i < s.length(); i++) {
            freq[s.charAt(i)]++;
        }

        for (int i = 0; i < s.length(); i++) {
            if (freq[s.charAt(i)] == 1) {
                return i;
            }
        }
        return -1;
    }
}
```

🔝 [Back to Top](#navigation--easy-string-problems)

---

## 6. Roman to Integer

### Problem

Convert a Roman numeral string to its **integer** value.

Valid symbols: `I,V,X,L,C,D,M`.

### Example

```text
Input:  "IX"
Output: 9

Input:  "MCMIV"
Output: 1904  (1000 + 900 + 4)
```

### Logic / Approach

* Map each Roman char to value.
* Scan from **right to left**:

  * Keep `sum` and `prev`.
  * If current value < `prev` → subtract.
  * Else → add.
* This handles cases like `IV` (5-1), `IX` (10-1), etc.

⏱ Time: O(n)
📦 Space: O(1)

### Java Code

```java
import java.util.*;

public class RomanToInteger {

    private static int value(char c) {
        switch (c) {
            case 'I': return 1;
            case 'V': return 5;
            case 'X': return 10;
            case 'L': return 50;
            case 'C': return 100;
            case 'D': return 500;
            case 'M': return 1000;
        }
        return 0;
    }

    public static int romanToInt(String s) {
        int sum = 0;
        int prev = 0;

        for (int i = s.length() - 1; i >= 0; i--) {
            int curr = value(s.charAt(i));
            if (curr < prev) {
                sum -= curr;
            } else {
                sum += curr;
            }
            prev = curr;
        }
        return sum;
    }
}
```

🔝 [Back to Top](#navigation--easy-string-problems)

---

## 7. Implement Atoi

### Problem

Convert a string to integer (`atoi`-like).

Handle:

* Leading spaces
* Optional `+` or `-`
* Stop on non-digit
* 32-bit overflow (return `Integer.MAX_VALUE` / `Integer.MIN_VALUE`)

### Example

```text
Input:  "   -42"
Output: -42

Input:  "4193 with words"
Output: 4193
```

### Logic / Approach

* Skip leading spaces.
* Read sign.
* For each digit:

  * Before adding, check for overflow:

    * `if (res > (MAX - digit)/10)` → clamp.
  * Update `res = res*10 + digit`.
* Return `sign * res`.

⏱ Time: O(n)
📦 Space: O(1)

### Java Code

```java
public class ImplementAtoi {

    public static int myAtoi(String s) {
        int i = 0, n = s.length();

        // 1. Skip spaces
        while (i < n && s.charAt(i) == ' ') i++;

        // 2. Sign
        int sign = 1;
        if (i < n && (s.charAt(i) == '+' || s.charAt(i) == '-')) {
            if (s.charAt(i) == '-') sign = -1;
            i++;
        }

        // 3. Convert digits
        int res = 0;
        int MAX = Integer.MAX_VALUE;
        int MIN = Integer.MIN_VALUE;

        while (i < n && Character.isDigit(s.charAt(i))) {
            int d = s.charAt(i) - '0';

            // Overflow check
            if (res > (MAX - d) / 10) {
                return (sign == 1) ? MAX : MIN;
            }

            res = res * 10 + d;
            i++;
        }

        return res * sign;
    }
}
```

🔝 [Back to Top](#navigation--easy-string-problems)

---

## 8. Encrypt the String – II

### Problem (from GFG – “Encrypt the string - 2”)

Given a lowercase string `s`:

1. Replace each **contiguous group** of the same character with:
   `character + (length of group in lowercase hexadecimal)`
2. Reverse the whole resulting string.
   Return the final string.

### Example

```text
Input:  "aaaaaaaaaaa"
Step1:  "a11"      (11 times)
Step2:  11 in hex → "b"  → "ab"
Step3:  reverse → "ba"
Output: "ba"
```

```text
Input:  "abc"
Step1:  "a1b1c1"
Step2:  hex(1) = "1" → same
Step3:  reverse → "1c1b1a"
Output: "1c1b1a"
```

### Logic / Approach

* Iterate string and count consecutive same characters.
* For each group:

  * Convert count to **hex string** (lowercase).
  * Append `char + hex` to a `StringBuilder`.
* At end, reverse the built string and return.

⏱ Time: O(n)
📦 Space: O(n)

### Java Code

```java
public class EncryptString2 {

    private static String toHex(int num) {
        StringBuilder sb = new StringBuilder();
        while (num != 0) {
            int rem = num % 16;
            if (rem < 10) {
                sb.append((char) ('0' + rem));
            } else {
                sb.append((char) ('a' + (rem - 10)));
            }
            num /= 16;
        }
        return sb.reverse().toString();
    }

    public static String encryptString(String s) {
        int n = s.length();
        StringBuilder ans = new StringBuilder();

        for (int i = 0; i < n; i++) {
            char ch = s.charAt(i);
            int count = 0;

            while (i < n && s.charAt(i) == ch) {
                count++;
                i++;
            }
            i--; // adjust

            String hex = toHex(count);
            ans.append(ch).append(hex);
        }

        return ans.reverse().toString();
    }
}
```

🔝 [Back to Top](#navigation--easy-string-problems)

---

## 9. Equal Point in Brackets

### Problem

Given a string of only `'('` and `')'`, find the **index** such that:

> Number of `'('` before index = number of `')'` from index (inclusive or exclusive depending on definition; we’ll use `before` vs `after`).

Common GFG version: index `i` (1-based) where count of `'('` in `s[0..i-1]` equals count of `')'` in `s[i..n-1]`.

### Example

```text
Input:  "(()))("
Index:   123456 (1-based)

At i = 3:
  '(' in [1..2] = 2
  ')' in [3..6] = 2  → equal point = 3
```

### Logic / Approach

* Precompute:

  * `openPrefix[i]` = count of `'('` from 0 to i-1
  * `closeSuffix[i]` = count of `')'` from i to n-1
* For each i (1..n), if `openPrefix[i] == closeSuffix[i]` → this i is equal point.

⏱ Time: O(n)
📦 Space: O(n)

### Java Code

```java
public class EqualPointInBrackets {

    // returns 1-based index, or -1 if none
    public static int equalPoint(String s) {
        int n = s.length();
        int[] openPrefix = new int[n + 1];
        int[] closeSuffix = new int[n + 1];

        // prefix '(' count
        for (int i = 0; i < n; i++) {
            openPrefix[i + 1] = openPrefix[i] + (s.charAt(i) == '(' ? 1 : 0);
        }

        // suffix ')' count
        for (int i = n - 1; i >= 0; i--) {
            closeSuffix[i] = closeSuffix[i + 1] + (s.charAt(i) == ')' ? 1 : 0);
        }

        for (int i = 0; i <= n; i++) {
            if (openPrefix[i] == closeSuffix[i]) {
                return i; // i is 0..n ; if want 1-based, return i
            }
        }
        return -1;
    }
}
```

🔝 [Back to Top](#navigation--easy-string-problems)

---

## 10. Anagram Checking

### Problem

Check if two strings are **anagrams** (same characters with same frequency, order doesn’t matter).

### Example

```text
Input:  "listen", "silent"
Output: true

Input:  "hello", "bello"
Output: false
```

### Logic / Approach

* If lengths differ → not anagram.
* Use frequency array `freq[26]`.

  * For s1: `freq[ch - 'a']++`
  * For s2: `freq[ch - 'a']--`
* Finally, if all `freq[i] == 0` → anagram.

⏱ Time: O(n)
📦 Space: O(1)

### Java Code

```java
public class AnagramChecking {

    public static boolean areAnagrams(String s1, String s2) {
        if (s1.length() != s2.length()) return false;

        int[] freq = new int[26];

        s1 = s1.toLowerCase();
        s2 = s2.toLowerCase();

        for (int i = 0; i < s1.length(); i++) {
            freq[s1.charAt(i) - 'a']++;
            freq[s2.charAt(i) - 'a']--;
        }

        for (int x : freq) {
            if (x != 0) return false;
        }
        return true;
    }
}
```

🔝 [Back to Top](#navigation--easy-string-problems)

---

## 11. Panagram Checking

### Problem

Check if a string is a **pangram** → contains **all 26 letters** of the English alphabet at least once.

### Example

```text
Input:  "The quick brown fox jumps over a lazy dog"
Output: true
```

### Logic / Approach

* Convert to lowercase.
* Use boolean array `seen[26]`.
* For each character:

  * If `a <= ch <= z` → mark `seen[ch - 'a'] = true`.
* At end, if any `seen[i]` is false → not pangram.

⏱ Time: O(n)
📦 Space: O(1)

### Java Code

```java
public class PangramChecking {

    public static boolean isPangram(String s) {
        boolean[] seen = new boolean[26];
        s = s.toLowerCase();

        for (char c : s.toCharArray()) {
            if (c >= 'a' && c <= 'z') {
                seen[c - 'a'] = true;
            }
        }

        for (boolean b : seen) {
            if (!b) return false;
        }
        return true;
    }
}
```

🔝 [Back to Top](#navigation--easy-string-problems)

---

## 12. Validate IP Address

### Problem

Given a string, check if it is a valid **IPv4 address**.

Rules:

* 4 parts separated by `.`
* Each part is **0–255**
* No leading zeros (except the number "0" itself)
* Only digits allowed.

### Example

```text
"192.168.1.1"      → valid
"256.0.0.1"        → invalid (256 > 255)
"01.1.1.1"         → invalid (leading zero)
"1.1.1"            → invalid (only 3 parts)
```

### Logic / Approach

* Split by `"\\."`.
* Must have exactly 4 parts.
* For each part:

  * Non-empty, only digits.
  * If length > 1 and starts with '0' → invalid.
  * Parse int, must be in [0, 255].

⏱ Time: O(n)
📦 Space: O(1)

### Java Code

```java
public class ValidateIPAddress {

    public static boolean isValidIPv4(String ip) {
        String[] parts = ip.split("\\.");
        if (parts.length != 4) return false;

        for (String part : parts) {
            if (part.length() == 0) return false;

            // digits only
            for (char c : part.toCharArray()) {
                if (!Character.isDigit(c)) return false;
            }

            // leading zero check
            if (part.length() > 1 && part.charAt(0) == '0') return false;

            int val;
            try {
                val = Integer.parseInt(part);
            } catch (NumberFormatException e) {
                return false;
            }

            if (val < 0 || val > 255) return false;
        }
        return true;
    }
}
```

🔝 [Back to Top](#navigation--easy-string-problems)

---

## 13. Add Binary Strings

### Problem

Given two binary strings `a` and `b`, return their **sum** as a binary string.

### Example

```text
Input:  a = "11", b = "1"
Output: "100"

Input:  a = "1010", b = "1011"
Output: "10101"
```

### Logic / Approach

* Start from end of both strings with indices `i`, `j` and `carry = 0`.
* While `i >= 0` or `j >= 0` or `carry != 0`:

  * Get bits `x`, `y` from `a[i]`, `b[j]` if in bounds.
  * `sum = x + y + carry`.
  * Result bit = `sum % 2`, new carry = `sum / 2`.
* Append bits to `StringBuilder` and finally reverse.

⏱ Time: O(max(len(a), len(b)))
📦 Space: O(max(len(a), len(b)))

### Java Code

```java
public class AddBinaryStrings {

    public static String addBinary(String a, String b) {
        int i = a.length() - 1;
        int j = b.length() - 1;
        int carry = 0;

        StringBuilder sb = new StringBuilder();

        while (i >= 0 || j >= 0 || carry != 0) {
            int x = (i >= 0) ? a.charAt(i) - '0' : 0;
            int y = (j >= 0) ? b.charAt(j) - '0' : 0;

            int sum = x + y + carry;
            sb.append(sum % 2);
            carry = sum / 2;

            i--;
            j--;
        }

        return sb.reverse().toString();
    }
}
```

🔝 [Back to Top](#navigation--easy-string-problems)

---

If you want, next I can:

* Add **time & space complexity bullet points** under each (already hinted but we can formalize).
* Create **separate `.java` files** list for each class for direct copy into an IDE.
* Do **Medium String Problems** in the exact same GitHub style.
