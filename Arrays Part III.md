## Q:- [**Search a 2D Matrix**](https://leetcode.com/problems/search-a-2d-matrix/)

**Example 1:**

![](https://assets.leetcode.com/uploads/2020/10/05/mat.jpg)

**Input:** matrix = \[\[1,3,5,7\],\[10,11,16,20\],\[23,30,34,60\]\], target = 3

**Output:** true

**Example 2:**

![](https://assets.leetcode.com/uploads/2020/10/05/mat2.jpg)

**Input:** matrix = \[\[1,3,5,7\],\[10,11,16,20\],\[23,30,34,60\]\], target = 13

**Output:** false

### Approaches : -

#### Brute Force ( Using two nested loop) 

```java
 public static boolean searchMatrix(ArrayList<ArrayList<Integer>> matrix, int target) {
        int n = matrix.size(), m = matrix.get(0).size();

        // traverse the matrix:
        for (int i = 0; i < n; i++) {
            for (int j = 0; j < m; j++) {
                if (matrix.get(i).get(j) == target)
                    return true;
            }
        }
        return false;
    }
```


#### Better Approach

**👌 Intuition**

> Here matrix is sorted by the row. So we can apply binary search on row.
> 
> So applying binary search on particular row first we have find that like target exist in that row. Here we know row is sorted so we can check 
> 
> **If matrix\[i\]\[0\] \<= target && target \<= matrix\[i\]\[m-1\]** then apply **binary search** on that particular row.

```java
public class MatrixSearch {

    public static boolean binarySearch(int[] nums, int target) {
        int n = nums.length; // size of the array
        int low = 0, high = n - 1;

        // Perform the steps:
        while (low <= high) {
            int mid = (low + high) / 2;
            if (nums[mid] == target) return true;
            else if (target > nums[mid]) low = mid + 1;
            else high = mid - 1;
        }
        return false;
    }

    public static boolean searchMatrix(int[][] matrix, int target) {
        int n = matrix.length;
        int m = matrix[0].length;

        for (int i = 0; i < n; i++) {
            if (matrix[i][0] <= target && target <= matrix[i][m - 1]) {
                return binarySearch(matrix[i], target);
            }
        }
        return false;
    }

    public static void main(String[] args) {
        int[][] matrix = {
                {1, 2, 3, 4},
                {5, 6, 7, 8},
                {9, 10, 11, 12}
        };

        boolean result = searchMatrix(matrix, 8);
        System.out.println(result ? "true" : "false");
    }
}
```

#### **Optimal Approach**

**👌 Intuition**

> **Here applying binary search on the matrix without flattening.**
> 
> For that we taken range of binary search is **low=0 to high= ((n\*m) - 1)** where n is row length and m is col length

**How to convert 1D array index to the corresponding cell of the 2D matrix:**

We will use the following formula:

If index = i, and no. of columns in the matrix = m, the index i corresponds to the cell with  
**row = i / m** and **col = i % m**. More formally, the cell is (i / m, i % m)(_0-based indexing_).

![](https://33333.cdn.cke-cs.com/kSW7V9NHUXugvhoQeFaf/images/c16c7813165a4698a5557034d3540c8444eb4d3d5a76a327.png)

The range of the indices of the imaginary 1D array is \[0, (NxM)-1\] and in this range, we will apply binary search.

```java
public static boolean searchMatrix(int[][] matrix, int target) {
    int n = matrix.length;
    int m = matrix[0].length;

    // Apply binary search:
    int low = 0, high = n * m - 1;
    while (low <= high) {
        int mid = (low + high) / 2;
        int row = mid / m, col = mid % m;
        if (matrix[row][col] == target) return true;
        else if (matrix[row][col] < target) low = mid + 1;
        else high = mid - 1;
    }
    return false;
}
```


## Q:- [**Pow(x, n)**](https://leetcode.com/problems/powx-n/)

Implement [pow(x, n)](http://www.cplusplus.com/reference/valarray/pow/), which calculates `x` raised to the power `n` (i.e., `xn`).

**Example 1:**

**Input:** x = 2.00000, n = 10

**Output:** 1024.00000

**Example 2:**

**Input:** x = 2.10000, n = 3

**Output:** 9.26100

**Example 3:**

**Input:** x = 2.00000, n = -2

**Output:** 0.25000

**Explanation:** 2-2 = 1/22 = 1/4 = 0.25

**What we are doing here :-** 

![](https://33333.cdn.cke-cs.com/kSW7V9NHUXugvhoQeFaf/images/8482777fc49da6abb451980eac1f7e75fb026a0cb45ff407.png)

```java
 public double myPow(double x, int n) {
        
        if(n==0) return 1;
        else if(n<0){
            return 1/x * myPow(1/x, -(n+1));
        }
        else{
           double ans =myPow(x, n/2);
           if(n%2!=0){
               return (ans*ans)*x;
           }else{
               return (ans*ans);
           }
        }
    }
```
