Here you go Sahil — same structure as before, but now with **Java solutions** for each problem, ready to paste into GitHub.

You can save this as: `Matrix/Easy-Problems.md`.

---

# Navigation – Easy Matrix Problems

* [Rotate Matrix Elements](#1-rotate-matrix-elements)
* [Sort the Matrix](#2-sort-the-matrix)
* [Turn Image by 90-Degree](#3-turn-image-by-90-degree)
* [Multiply Two Matrices](#4-multiply-two-matrices)
* [Maximum Element of Each Row in a Matrix](#5-maximum-element-of-each-row-in-a-matrix)
* [Count Sorted Rows in a Matrix](#6-count-sorted-rows-in-a-matrix)
* [Common Elements in All Rows of Matrix](#7-common-elements-in-all-rows-of-matrix)
* [Print Matrix in Snake Pattern](#8-print-matrix-in-snake-pattern)
* [Sort a Matrix in All Way Increasing Order](#9-sort-a-matrix-in-all-way-increasing-order)
* [Row with Maximum 1s](#10-row-with-maximum-1s)
* [Sums of Diagonals of a Matrix](#11-sums-of-diagonals-of-a-matrix)
* [Array Subset Check](#12-array-subset-check)
* [Boundary Elements of a Matrix](#13-boundary-elements-of-a-matrix)
* [Magic Square](#14-magic-square)

---

## 1. Rotate Matrix Elements

### Problem

Rotate all elements of the matrix in **each ring (boundary)** by one step **clockwise**.

### Example

Input:

```text
1  2  3
4  5  6
7  8  9
```

One clockwise rotation of outer ring gives:

```text
4  1  2
7  5  3
8  9  6
```

### Java Solution

```java
public class RotateMatrixElements {

    public static void rotateClockwise(int[][] mat) {
        int m = mat.length;
        if (m == 0) return;
        int n = mat[0].length;

        int top = 0, bottom = m - 1;
        int left = 0, right = n - 1;

        while (top < bottom && left < right) {
            int prev = mat[top + 1][left];

            // top row: left -> right
            for (int j = left; j <= right; j++) {
                int temp = mat[top][j];
                mat[top][j] = prev;
                prev = temp;
            }
            top++;

            // right column: top -> bottom
            for (int i = top; i <= bottom; i++) {
                int temp = mat[i][right];
                mat[i][right] = prev;
                prev = temp;
            }
            right--;

            // bottom row: right -> left
            for (int j = right; j >= left; j--) {
                int temp = mat[bottom][j];
                mat[bottom][j] = prev;
                prev = temp;
            }
            bottom--;

            // left column: bottom -> top
            for (int i = bottom; i >= top; i--) {
                int temp = mat[i][left];
                mat[i][left] = prev;
                prev = temp;
            }
            left++;
        }
    }
}
```

🔝 [Back to Top](#navigation--easy-matrix-problems)

---

## 2. Sort the Matrix

### Problem

Sort all elements of the matrix in **non-decreasing order** and fill them back row-wise.

### Example

Input:

```text
9 8 7
6 5 4
3 2 1
```

Output:

```text
1 2 3
4 5 6
7 8 9
```

### Java Solution

```java
import java.util.Arrays;

public class SortMatrix {

    public static void sortMatrix(int[][] mat) {
        int m = mat.length;
        if (m == 0) return;
        int n = mat[0].length;

        int[] temp = new int[m * n];
        int idx = 0;
        for (int[] row : mat) {
            for (int val : row) {
                temp[idx++] = val;
            }
        }

        Arrays.sort(temp);

        idx = 0;
        for (int i = 0; i < m; i++) {
            for (int j = 0; j < n; j++) {
                mat[i][j] = temp[idx++];
            }
        }
    }
}
```

🔝 [Back to Top](#navigation--easy-matrix-problems)

---

## 3. Turn Image by 90-Degree

### Problem

Given an **N × N** matrix (image), rotate it **90° clockwise** in-place.

### Example

Input:

```text
1 2 3
4 5 6
7 8 9
```

Output:

```text
7 4 1
8 5 2
9 6 3
```

### Java Solution

```java
public class RotateImage90 {

    public static void rotate90Clockwise(int[][] mat) {
        int n = mat.length;

        // 1) Transpose
        for (int i = 0; i < n; i++) {
            for (int j = i + 1; j < n; j++) {
                int temp = mat[i][j];
                mat[i][j] = mat[j][i];
                mat[j][i] = temp;
            }
        }

        // 2) Reverse each row
        for (int i = 0; i < n; i++) {
            int left = 0, right = n - 1;
            while (left < right) {
                int temp = mat[i][left];
                mat[i][left] = mat[i][right];
                mat[i][right] = temp;
                left++;
                right--;
            }
        }
    }
}
```

🔝 [Back to Top](#navigation--easy-matrix-problems)

---

## 4. Multiply Two Matrices

### Problem

Multiply matrix **A (m × n)** with matrix **B (n × p)** and return result **C (m × p)**.

### Example

Input:

```text
A =
1 2
3 4

B =
2 0
1 2
```

Output:

```text
4 4
10 8
```

### Java Solution

```java
public class MatrixMultiplication {

    public static int[][] multiply(int[][] A, int[][] B) {
        int m = A.length;
        int n = A[0].length;
        int n2 = B.length;
        int p = B[0].length;

        if (n != n2) {
            throw new IllegalArgumentException("Invalid dimensions for multiplication");
        }

        int[][] C = new int[m][p];

        for (int i = 0; i < m; i++) {
            for (int k = 0; k < n; k++) {
                for (int j = 0; j < p; j++) {
                    C[i][j] += A[i][k] * B[k][j];
                }
            }
        }
        return C;
    }
}
```

🔝 [Back to Top](#navigation--easy-matrix-problems)

---

## 5. Maximum Element of Each Row in a Matrix

### Problem

Find the **maximum element for each row** and print/return them.

### Example

Input:

```text
1 4 2
9 7 3
5 6 8
```

Output:

```text
4 9 8
```

### Java Solution

```java
public class RowMaxElements {

    public static int[] rowMax(int[][] mat) {
        int m = mat.length;
        if (m == 0) return new int[0];
        int n = mat[0].length;

        int[] ans = new int[m];

        for (int i = 0; i < m; i++) {
            int max = Integer.MIN_VALUE;
            for (int j = 0; j < n; j++) {
                if (mat[i][j] > max) {
                    max = mat[i][j];
                }
            }
            ans[i] = max;
        }
        return ans;
    }
}
```

🔝 [Back to Top](#navigation--easy-matrix-problems)

---

## 6. Count Sorted Rows in a Matrix

### Problem

Count rows that are:

* either **non-decreasing** (ascending)
* or **non-increasing** (descending)

### Example

Input:

```text
1 2 3
9 7 5
4 2 1
```

All 3 rows are sorted (first ascending, others descending).
Output: `3`

### Java Solution

```java
public class CountSortedRows {

    public static int countSortedRows(int[][] mat) {
        int m = mat.length;
        if (m == 0) return 0;
        int n = mat[0].length;

        int count = 0;

        for (int i = 0; i < m; i++) {
            boolean asc = true, desc = true;

            for (int j = 1; j < n; j++) {
                if (mat[i][j] < mat[i][j - 1]) asc = false;
                if (mat[i][j] > mat[i][j - 1]) desc = false;
            }

            if (asc || desc) count++;
        }
        return count;
    }
}
```

🔝 [Back to Top](#navigation--easy-matrix-problems)

---

## 7. Common Elements in All Rows of Matrix

### Problem

Print elements that appear in **every row** of the matrix.

### Example

Input:

```text
1 2 3
4 2 5
6 2 9
```

Output:

```text
2
```

### Java Solution

```java
import java.util.*;

public class CommonElementsInRows {

    public static List<Integer> commonElements(int[][] mat) {
        int m = mat.length;
        if (m == 0) return List.of();

        Map<Integer, Integer> freq = new HashMap<>();

        // First row: put elements with count = 1
        for (int x : mat[0]) {
            freq.put(x, 1);
        }

        // For each next row, increase count if present
        for (int i = 1; i < m; i++) {
            Set<Integer> rowSet = new HashSet<>();
            for (int x : mat[i]) {
                rowSet.add(x);
            }
            for (int x : rowSet) {
                if (freq.containsKey(x) && freq.get(x) == i) {
                    freq.put(x, i + 1);
                }
            }
        }

        List<Integer> ans = new ArrayList<>();
        for (Map.Entry<Integer, Integer> e : freq.entrySet()) {
            if (e.getValue() == m) {
                ans.add(e.getKey());
            }
        }
        return ans;
    }
}
```

🔝 [Back to Top](#navigation--easy-matrix-problems)

---

## 8. Print Matrix in Snake Pattern

### Problem

Print matrix elements in a **snake pattern**:

* Even row → left to right
* Odd row → right to left

### Example

Input:

```text
1 2 3
4 5 6
7 8 9
```

Output:

```text
1 2 3 6 5 4 7 8 9
```

### Java Solution

```java
import java.util.*;

public class SnakePattern {

    public static List<Integer> snakePattern(int[][] mat) {
        int m = mat.length;
        if (m == 0) return List.of();
        int n = mat[0].length;

        List<Integer> ans = new ArrayList<>();

        for (int i = 0; i < m; i++) {
            if (i % 2 == 0) {
                // left to right
                for (int j = 0; j < n; j++) {
                    ans.add(mat[i][j]);
                }
            } else {
                // right to left
                for (int j = n - 1; j >= 0; j--) {
                    ans.add(mat[i][j]);
                }
            }
        }
        return ans;
    }
}
```

🔝 [Back to Top](#navigation--easy-matrix-problems)

---

## 9. Sort a Matrix in All Way Increasing Order

### Problem

Sort the entire matrix so elements are increasing:

* left → right inside a row
* top → bottom across rows

(Effectively same as Problem 2.)

### Example

Input:

```text
3 1
2 4
```

Output:

```text
1 2
3 4
```

### Java Solution

(Same approach as Problem 2)

```java
import java.util.Arrays;

public class SortMatrixAllIncreasing {

    public static void sortAllIncreasing(int[][] mat) {
        int m = mat.length;
        if (m == 0) return;
        int n = mat[0].length;

        int[] temp = new int[m * n];
        int idx = 0;

        for (int[] row : mat) {
            for (int val : row) {
                temp[idx++] = val;
            }
        }

        Arrays.sort(temp);

        idx = 0;
        for (int i = 0; i < m; i++) {
            for (int j = 0; j < n; j++) {
                mat[i][j] = temp[idx++];
            }
        }
    }
}
```

🔝 [Back to Top](#navigation--easy-matrix-problems)

---

## 10. Row with Maximum 1s

### Problem

Given a **binary matrix** where each row is **sorted** (all 0s then 1s), find the **index of the row** with maximum number of 1s.

### Example

Input:

```text
0 0 1 1
0 1 1 1
0 0 0 1
```

Output:

```text
Row index = 1
```

### Java Solution (O(m + n))

```java
public class RowWithMaxOnes {

    public static int rowWithMax1s(int[][] mat) {
        int m = mat.length;
        if (m == 0) return -1;
        int n = mat[0].length;

        int maxRow = -1;
        int j = n - 1;

        for (int i = 0; i < m; i++) {
            while (j >= 0 && mat[i][j] == 1) {
                j--;
                maxRow = i; // this row has more 1s
            }
        }
        return maxRow;
    }
}
```

🔝 [Back to Top](#navigation--easy-matrix-problems)

---

## 11. Sums of Diagonals of a Matrix

### Problem

For a **square matrix**, find:

* sum of primary (main) diagonal
* sum of secondary diagonal

### Example

Input:

```text
1 2 3
4 5 6
7 8 9
```

Primary = 1 + 5 + 9 = 15
Secondary = 3 + 5 + 7 = 15

### Java Solution

```java
public class DiagonalSums {

    public static int[] diagonalSums(int[][] mat) {
        int n = mat.length;
        int primary = 0;
        int secondary = 0;

        for (int i = 0; i < n; i++) {
            primary += mat[i][i];
            secondary += mat[i][n - 1 - i];
        }

        return new int[]{primary, secondary};
    }
}
```

🔝 [Back to Top](#navigation--easy-matrix-problems)

---

## 12. Array Subset Check

### Problem

Given two arrays **A** and **B**, check if **B is a subset of A** (every element of B appears in A, at least once).

### Example

```text
A = [1, 2, 3, 4, 5]
B = [2, 5, 4]

Output: Yes
```

### Java Solution

```java
import java.util.HashMap;
import java.util.Map;

public class ArraySubsetCheck {

    public static boolean isSubset(int[] A, int[] B) {
        Map<Integer, Integer> freq = new HashMap<>();

        for (int x : A) {
            freq.put(x, freq.getOrDefault(x, 0) + 1);
        }

        for (int x : B) {
            if (!freq.containsKey(x) || freq.get(x) == 0) {
                return false;
            }
            freq.put(x, freq.get(x) - 1);
        }
        return true;
    }
}
```

🔝 [Back to Top](#navigation--easy-matrix-problems)

---

## 13. Boundary Elements of a Matrix

### Problem

Print only the **boundary elements** of a matrix in clockwise order.

### Example

Input:

```text
1 2 3
4 5 6
7 8 9
```

Output:

```text
1 2 3 6 9 8 7 4
```

### Java Solution

```java
import java.util.*;

public class BoundaryTraversal {

    public static List<Integer> boundary(int[][] mat) {
        List<Integer> ans = new ArrayList<>();
        int m = mat.length;
        if (m == 0) return ans;
        int n = mat[0].length;

        // Single row
        if (m == 1) {
            for (int j = 0; j < n; j++) ans.add(mat[0][j]);
            return ans;
        }

        // Single column
        if (n == 1) {
            for (int i = 0; i < m; i++) ans.add(mat[i][0]);
            return ans;
        }

        // Top row
        for (int j = 0; j < n; j++) ans.add(mat[0][j]);
        // Right column
        for (int i = 1; i < m; i++) ans.add(mat[i][n - 1]);
        // Bottom row
        for (int j = n - 2; j >= 0; j--) ans.add(mat[m - 1][j]);
        // Left column
        for (int i = m - 2; i >= 1; i--) ans.add(mat[i][0]);

        return ans;
    }
}
```

🔝 [Back to Top](#navigation--easy-matrix-problems)

---

## 14. Magic Square

### Problem

A square matrix is a **magic square** if:

* All rows, columns and both diagonals have the **same sum**.
* (Sometimes also uses distinct numbers 1..n², but basic check is equal sums.)

### Example

```text
8 1 6
3 5 7
4 9 2
```

Row sums, column sums and diagonal sums are all 15 → **Magic Square**.

### Java Solution

```java
public class MagicSquareCheck {

    public static boolean isMagicSquare(int[][] mat) {
        int n = mat.length;
        if (n == 0 || mat[0].length != n) {
            return false; // must be square
        }

        int targetSum = 0;

        // Sum of first row
        for (int j = 0; j < n; j++) {
            targetSum += mat[0][j];
        }

        // Check all rows
        for (int i = 1; i < n; i++) {
            int rowSum = 0;
            for (int j = 0; j < n; j++) {
                rowSum += mat[i][j];
            }
            if (rowSum != targetSum) return false;
        }

        // Check all columns
        for (int j = 0; j < n; j++) {
            int colSum = 0;
            for (int i = 0; i < n; i++) {
                colSum += mat[i][j];
            }
            if (colSum != targetSum) return false;
        }

        // Primary diagonal
        int diag1 = 0, diag2 = 0;
        for (int i = 0; i < n; i++) {
            diag1 += mat[i][i];
            diag2 += mat[i][n - 1 - i];
        }
        if (diag1 != targetSum || diag2 != targetSum) return false;

        return true;
    }
}
```

🔝 [Back to Top](#navigation--easy-matrix-problems)

---

If you want, next I can:

* Convert this into **separate Java files** per problem
* Or add **time & space complexity notes** under each solution for interview prep.
