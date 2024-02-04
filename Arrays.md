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

## **Q:-** [**Pascal's Triangle**](https://leetcode.com/problems/pascals-triangle/)

![](https://upload.wikimedia.org/wikipedia/commons/0/0d/PascalTriangleAnimated2.gif)

**Example 1:**

**Input:** numRows = 5 **Output:** \[\[1\],\[1,1\],\[1,2,1\],\[1,3,3,1\],\[1,4,6,4,1\]\]

**Example 2:**

**Input:** numRows = 1 **Output:** \[\[1\]\]

### 👌Intuition :-

> Here at each level first and last column should to set 1.
> 
> And other middle column on that particular level would be sum of upper previous level left and right column at current column.

```java
class Solution {
    public List<List<Integer>> generate(int numRows) {
        
        List<List<Integer>> result = new ArrayList<>();
        
        for(int i = 0 ; i < numRows; i++) {
            List<Integer> list = new ArrayList<>();
            
            for(int j = 0; j < i+1 ; j++) {
                if(j == 0 || j == i) {
                    // adding 1 for first row
                    // adding 1 for first and last col of each row
                    list.add(1);
                }
                else {
                    
                    // if col is middele then get two previous elements
                    // from reslut and add them and push to list

                    int left = result.get(i - 1).get(j - 1);
                    int right = result.get(i - 1).get(j);
                    list.add(left + right);
                }
            }
            result.add(list);
         }
        return result;
      }
}
```


## Q:- [**Next Permutation**](https://leetcode.com/problems/next-permutation/)

*   For example, the next permutation of `arr = [1,2,3]` is `[1,3,2]`.
*   Similarly, the next permutation of `arr = [2,3,1]` is `[3,1,2]`.
*   While the next permutation of `arr = [3,2,1]` is `[1,2,3]` because `[3,2,1]` does not have a lexicographical larger rearrangement.

**Example 1:**

**Input:** nums = \[1,2,3\] **Output:** \[1,3,2\]

**Example 2:**

**Input:** nums = \[3,2,1\] **Output:** \[1,2,3\]

**Example 3:**

**Input:** nums = \[1,1,5\] **Output:** \[1,5,1\]

### 👌 Intuition

> ![](https://33333.cdn.cke-cs.com/kSW7V9NHUXugvhoQeFaf/images/7b9a78610607f8817ec1431ced32d2da05988287f8b1227b.png)
> 
> *   **Find the break-point, i:** Break-point means the _**first index i from the back of the given array**_ where arr\[i\] becomes smaller than arr\[i+1\].
> *   **If such a break-point does not exist i.e. if the array is sorted in decreasing order,** the given permutation is the last one in the sorted order of all possible permutations. So, the next permutation must be the first i.e. the permutation in increasing order.  
>     So, _**in this case, we will reverse the whole array and will return it as our answer.**_
> *   **If a break-point exists:**
>     *   Find the smallest number i.e. > arr\[i\] and in the right half of index i(i.e. from index i+1 to n-1) and swap it with arr\[i\].
>     *   Reverse the entire right half(i.e. from index i+1 to n-1) of index i. And finally, return the array.

```java
class Solution {
   public void nextPermutation(int[] nums) {
    int n = nums.length; // size of the array.

    // Step 1: Find the break point:
    int ind = -1; // break point
    for (int i = n - 2; i >= 0; i--) {
        if (nums[i] < nums[i + 1]) {
            // index i is the break point
            ind = i;
            break;
        }
    }

    // If break point does not exist:
    if (ind == -1) {
        // reverse the whole array:
        reverseArray(nums, 0, n - 1);
        return;
    }

    // Step 2: Find the next greater element and swap it with nums[ind]
    // Here traversing right left because array is sorted in decending order from break point
    // So we will get smallest grater element than break point element
    for (int i = n - 1; i > ind; i--) {
        if (nums[i] > nums[ind]) {
            // swapping
            int tmp = nums[i];
            nums[i] = nums[ind];
            nums[ind] = tmp;
            break;
        }
    }

    // Step 3: reverse the right half:
   // Reversing because make as much smallest element 
    reverseArray(nums, ind + 1, n - 1);
}

private void reverseArray(int[] nums, int start, int end) {
    while (start < end) {
        int temp = nums[start];
        nums[start] = nums[end];
        nums[end] = temp;
        start++;
        end--;
    }
}

}
```


## Q:- [**Maximum Subarray**](https://leetcode.com/problems/maximum-subarray/)

**Example 1:**

**Input:** nums = \[-2,1,-3,4,-1,2,1,-5,4\] **Output:** 6 **Explanation:** The subarray \[4,-1,2,1\] has the largest sum 6.

**Example 2:**

**Input:** nums = \[1\] **Output:** 1 **Explanation:** The subarray \[1\] has the largest sum 1.

**Example 3:**

**Input:** nums = \[5,4,-1,7,8\] **Output:** 23 **Explanation:** The subarray \[5,4,-1,7,8\] has the largest sum 23.

### **👌 Intuition:**

_**Kadane Algorithm**_

> _**A subarray with a sum less than 0 will always reduce our answer and so this type of subarray cannot be a part of the subarray with maximum sum.**_

```java
class Solution {
    public int maxSubArray(int[] nums) {
        int totalSum=Integer.MIN_VALUE;;
        int currSum=0;
        for(int i=0; i<nums.length; i++){
           currSum+=nums[i];
           
           totalSum=Math.max(currSum, totalSum);
           // if currSum becomes negative then set to 0
           if(currSum<0){
               currSum=0;
           }
        }

        return totalSum;
    }
}
```

## **Q:-** [**Sort Colors**](https://leetcode.com/problems/sort-colors/)

**Example 1:**

**Input:** nums = \[2,0,2,1,1,0\] **Output:** \[0,0,1,1,2,2\]

**Example 2:**

**Input:** nums = \[2,0,1\] **Output:** \[0,1,2\]

#### 👉 Better Approach

### **Intuition:**

> Keeping count of values
> 
>  Since in this case there are only 3 distinct values in the array so it’s easy to maintain the count of all, Like the count of 0, 1, and 2. This can be followed by overwriting the array based on the frequency(count) of the values.

```java
class Solution {
    public void sortColors(int[] nums) {

        int len=nums.length;
        int zeroCount=0;
        int oneCount=0;
        int twoCount=0;

        // count 0's, i's and 2 in array
        for(int i=0; i<len; i++){
            if(nums[i]==0) zeroCount++;
            else if(nums[i]==1) oneCount++;
            else twoCount++;
        }

        int idx=0;
        while(zeroCount>0){
            nums[idx++]=0;
            zeroCount--;
        }

         while(oneCount>0){
            nums[idx++]=1;
            oneCount--;
        }

        while(twoCount>0){
            nums[idx++]=2;
            twoCount--;
        }
    }
}
```


#### Optimal Approach

### **👌 Intuition**

> This algorithm contains 3 pointers i.e. low, mid, and high, and 3 main rules.  The rules are the following:
> 
> *   arr\[0….low-1\] contains 0. \[Extreme left part\]
> *   arr\[low….mid-1\] contains 1.
> *   arr\[high+1….n-1\] contains 2. \[Extreme right part\], n = size of the array

![](https://33333.cdn.cke-cs.com/kSW7V9NHUXugvhoQeFaf/images/35a4cfcbc72d453309a5622b1023bf98cb7e959214690bf2.png)

**The steps will be the following:**

1.  First, we will run a loop that will continue until **mid \<= high**.
2.  There can be three different values of mid pointer i.e. arr\[mid\]
    1.  **If arr\[mid\] == 0,** we will swap arr\[low\] and arr\[mid\] and will increment both low and mid. Now the subarray from index 0 to (low-1) only contains 0.
    2.  **If arr\[mid\] == 1,** we will just increment the mid pointer and then the index (mid-1) will point to 1 as it should according to the rules.
    3.  **If arr\[mid\] == 2,** we will swap arr\[mid\] and arr\[high\] and will decrement high. Now the subarray from index high+1 to (n-1) only contains 2.  
        In this step, we will do nothing to the mid-pointer as even after swapping, the subarray from mid to high(_after decrementing high_) might be unsorted. So, we will check the value of mid again in the next iteration.
3.  Finally, our array should be sorted.

```java
class Solution {
    public void sortColors(int[] nums) {
        int len=nums.length;

        int low=0;
        int mid=0;
        int high=len-1;

        while (mid <= high) {
            if(nums[mid]==0){
                swap(nums, low, mid);
                low++;
                mid++;
            }else if(nums[mid]==1){
                mid++;
            }else{
                swap(nums, mid, high);
                high--;
            }
        }
    }

    void swap(int[] arr, int index1, int index2) {
    int temp = arr[index1];
    arr[index1] = arr[index2];
    arr[index2] = temp;
 }
}
```
