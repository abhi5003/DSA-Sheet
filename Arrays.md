# Q:- [Set Matrix Zeroes](https://leetcode.com/problems/set-matrix-zeroes/)

![](https://33333.cdn.cke-cs.com/kSW7V9NHUXugvhoQeFaf/images/01505ee25bab15db0c8a3c21ad1406c462cbf534a10947c4.png)

**Brute Force -**

The steps are the following:

1.  First, we will use two loops(_nested loops_) to traverse all the cells of the matrix.
2.  If any cell (i,j) contains the value 0, we will mark all cells in row i and column j with -1 except those which contain 0.
3.  We will perform step 2 for every cell containing 0.
4.  Finally, we will mark all the cells containing -1 with 0.
5.  Thus the given matrix will be modified according to the question.

**Note:** _Here, we are assuming that the matrix does not contain any negative numbers. But if it contains negatives, we need to find some other ways to mark the cells instead of marking them with -1._

```java
import java.util.*;

public class Solution{
    static void markRow(int[][] matrix, int n, int m, int i) {
        // set all non-zero elements as -1 in the row i:
        for (int j = 0; j < m; j++) {
            if (matrix[i][j] != 0) {
                matrix[i][j] = -1;
            }
        }
    }

    static void markCol(int[][] matrix, int n, int m, int j) {
        // set all non-zero elements as -1 in the col j:
        for (int i = 0; i < n; i++) {
            if (matrix[i][j] != 0) {
                matrix[i][j] = -1;
            }
        }
    }

    static int[][] zeroMatrix(int[][] matrix, int n, int m) {
        // Set -1 for rows and cols that contains 0. Don't mark any 0 as -1:
        for (int i = 0; i < n; i++) {
            for (int j = 0; j < m; j++) {
                if (matrix[i][j] == 0) {
                    markRow(matrix, n, m, i);
                    markCol(matrix, n, m, j);
                }
            }
        }
        // Finally, mark all -1 as 0:
        for (int i = 0; i < n; i++) {
            for (int j = 0; j < m; j++) {
                if (matrix[i][j] == -1) {
                    matrix[i][j] = 0;
                }
            }
        }
        return matrix;
    }

    public static void main(String[] args) {
        int[][] matrix = {
            {1, 1, 1},
            {1, 0, 1},
            {1, 1, 1}
        };


        int n = matrix.length;
        int m = matrix[0].length;

        int[][] ans = zeroMatrix(matrix, n, m);

        System.out.println("The Final matrix is: ");
        for (int[] row : ans) {
            for (int ele : row) {
                System.out.print(ele + " ");
            }
            System.out.println();
        }
    }
}
```
**Time Complexity:** `O((N*M)*(N + M)) + O(N*M)`, where N = no. of rows in the matrix and M = no. of columns in the matrix.  
**Reason:** Firstly, we are traversing the matrix to find the cells with the value 0. It takes O(N\*M). Now, whenever we find any such cell we mark that row and column with -1. This process takes O(N+M). So, combining this the whole process, finding and marking, takes O((N\*M)\*(N + M)).  
Another O(N\*M) is taken to mark all the cells with -1 as 0 finally.

**Space Complexity:** `O(1)` as we are not using any extra space.

### Better Approach :-

**Approach (Using two extra arrays):**

The steps are as follows:

1.  First, we will declare two arrays: a **row** array of size N and a **col** array of size M and both are initialized with 0.
2.  Then, we will use two loops(_nested loops_) to traverse all the cells of the matrix.
3.  If any cell (i,j) contains the value 0, we will mark ith index of row array i.e. row\[i\] and jth index of col array col\[j\] as 1. It signifies that all the elements in the ith row and jth column will be 0 in the final matrix.
4.  We will perform step 3 for every cell containing 0.
5.  Finally, we will again traverse the entire matrix and we will put 0 into all the cells (i, j) for which either row\[i\] or col\[j\] is marked as 1.
6.  Thus we will get our final matrix.

```java
import java.util.*;

public class Solution{
    static int[][] zeroMatrix(int[][] matrix, int n, int m) {
        int[] row = new int[n]; // row array
        int[] col = new int[m]; // col array

        // Traverse the matrix:
        for (int i = 0; i < n; i++) {
            for (int j = 0; j < m; j++) {
                if (matrix[i][j] == 0) {
                    // mark ith index of row with 1:
                    row[i] = 1;

                    // mark jth index of col with 1:
                    col[j] = 1;
                }
            }
        }

        // Finally, mark all (i, j) as 0
        // if row[i] or col[j] is marked with 1.
        for (int i = 0; i < n; i++) {
            for (int j = 0; j < m; j++) {
                if (row[i] == 1 || col[j] == 1) {
                    matrix[i][j] = 0;
                }
            }
        }

        return matrix;
    }

    }
}
```

**Time Complexity:** `O(2*(N*M))`, where N = no. of rows in the matrix and M = no. of columns in the matrix.  
**Reason:** We are traversing the entire matrix 2 times and each traversal is taking O(N\*M) time complexity.

**Space Complexity:** `O(N) + O(M)`, where N = no. of rows in the matrix and M = no. of columns in the matrix.  
**Reason:** O(N) is for using the row array and O(M) is for using the col array.

### 👌 Optimal Approach :-

> Using two extra variable type Boolean 

```java
import java.util.*;

public class Solution {
    public void setZeroes(int[][] matrix) {
        boolean isZeroCol = false;
        boolean isZeroRow = false;

        for (int i = 0; i < matrix.length; i++) { // Check the first column
            if (matrix[i][0] == 0) {
                isZeroCol = true;
                break;
            }
        }

        for (int i = 0; i < matrix[0].length; i++) { // Check the first row
            if (matrix[0][i] == 0) {
                isZeroRow = true;
                break;
            }
        }

        for (int i = 1; i < matrix.length; i++) { // Check except the first row and column
            for (int j = 1; j < matrix[0].length; j++) {
                if (matrix[i][j] == 0) {
                    matrix[i][0] = 0;
                    matrix[0][j] = 0;
                }
            }
        }

        for (int i = 1; i < matrix.length; i++) { // Process except the first row and column
            for (int j = 1; j < matrix[0].length; j++) {
                if (matrix[i][0] == 0 || matrix[0][j] == 0)
                    matrix[i][j] = 0;
            }
        }

        if (isZeroCol) { // Handle the first column
            for (int i = 0; i < matrix.length; i++)
                matrix[i][0] = 0;
        }

        if (isZeroRow) { // Handle the first row
            for (int i = 0; i < matrix[0].length; i++)
                matrix[0][i] = 0;
        }
    }

    public static void main(String[] args) {
        Solution sol = new Solution();
        int[][] matrix = {
            {1, 1, 1},
            {1, 0, 1},
            {1, 1, 1}
        };
        sol.setZeroes(matrix);
        System.out.println("The Final matrix is: ");
        for (int[] row : matrix) {
            for (int ele : row) {
                System.out.print(ele + " ");
            }
            System.out.println();
        }
    }
}
```
**Time Complexity:** `O(2*(N*M))`, where N = no. of rows in the matrix and M = no. of columns in the matrix.  
**Reason:** In this approach, we are also traversing the entire matrix 2 times and each traversal is taking O(N\*M) time complexity.

**Space Complexity:** `O(1)` as we are not using any extra space.
