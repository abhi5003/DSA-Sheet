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



## **Q:-** [**Majority Element (N/2)**](https://leetcode.com/problems/majority-element/)

![](https://33333.cdn.cke-cs.com/kSW7V9NHUXugvhoQeFaf/images/16a0bd01789f3e1ca73a7bb73857182b3009a5d00051dc2a.png)

### **Naive Approach**:

### **Approach:**

The steps are as follows:

1.  We will run a loop that will select the elements of the **array one by one**.
2.  Now, for each element, we will **run another loop and count its occurrence** in the given array.
3.  If any element occurs more than the floor of (N/2), we will simply return it.

```java
 public static int majorityElement(int []v) {
        //size of the given array:
        int n = v.length;

        for (int i = 0; i < n; i++) {
            //selected element is v[i]
            int cnt = 0;
            for (int j = 0; j < n; j++) {
                // counting the frequency of v[i]
                if (v[j] == v[i]) {
                    cnt++;
                }
            }

            // check if frquency is greater than n/2:
            if (cnt > (n / 2))
                return v[i];
        }

        return -1;
    }
```


### **Solution 2 (Better):**

### **Intuition**:

Use a better data structure to reduce the number of look-up operations and hence the time complexity. Moreover, we have been calculating the count of the same element again and again – so we have to reduce that also.

### **Approach**:

1.  Use a hashmap and store as _(key,_ value) pairs. (Can also use frequency array based on the size of nums) 
2.  Here the key will be the element of the array and the value will be the number of times it occurs. 
3.  Traverse the array and update the value of the key. Simultaneously check if the value is greater than the **floor of N/2**. 
    1.  If yes, return the key 
    2.  Else iterate forward.

```java
 public static int majorityElement(int []v) {
        //size of the given array:
        int n = v.length;

        //declaring a map:
        HashMap<Integer, Integer> mpp = new HashMap<>();

        //storing the elements with its occurnce:
        for (int i = 0; i < n; i++) {
            int value = mpp.getOrDefault(v[i], 0);
            mpp.put(v[i], value + 1);
        }

        //searching for the majority element:
        for (Map.Entry<Integer, Integer> it : mpp.entrySet()) {
            if (it.getValue() > (n / 2)) {
                return it.getKey();
            }
        }

        return -1;
    }
```


### 👌 **Optimal Approach**: **Moore’s Voting Algorithm:**

### **Approach:**

1.  Initialize 2 variables:  
    **Count** –  for tracking the count of element  
    **Element** – for which element we are counting
2.  Traverse through the given array.
    1.  If **Count** is 0 then store the current element of the array as **Element**.
    2.  If the current element and **Element** are the same increase the **Count** by 1.
    3.  If they are different decrease the **Count** by 1.
3.  The integer present in **Element** should be the result we are expecting

```java
class Solution {
    public int majorityElement(int[] nums) {
        int majorEle=nums[0]; // Element
        int ctn=1;  // count

         // //applying the algorithm:
         for(int i=1; i<nums.length; i++){
             if(ctn==0){
                ctn++;
                majorEle=nums[i]; 
             }else if(majorEle==nums[i]){
                 ctn++;
             }else ctn--;
         }

        //checking if the stored element
        // is the majority element:

         int ctn1=0;
         for(int i=0; i<nums.length; i++){
             if(nums[i]==majorEle) ctn1++;
         }

         if(ctn1>nums.length/2) return majorEle; 
        return -1;
    }
}
```



## **Q: -** [**Majority Element (N/3)**](https://leetcode.com/problems/majority-element-ii/)

![](https://33333.cdn.cke-cs.com/kSW7V9NHUXugvhoQeFaf/images/3bab49e93717d49b797aea4aa7a6a27c1fc553674e434c9d.png)

**Observation:** How many integers can occur more than floor(N/3) times in the given array:

> _**there can be a maximum of 2 majority elements.**_

### **Naive Approach (Brute-force)**:

1.  We will run a loop that will select the elements of the array one by one.
2.  Now, for each unique element, we will run another loop and count its occurrence in the given array.
3.  If any element occurs more than the floor of (N/3), we will include it in our answer. 
4.  While traversing if we find any element that is already included in our answer, we will just skip it.

```java
    public static List<Integer> majorityElement(int []v) {
        int n = v.length; //size of the array
        List<Integer> ls = new ArrayList<>(); // list of answers

        for (int i = 0; i < n; i++) {
            //selected element is v[i]:
            // Checking if v[i] is not already
            // a part of the answer:
            if (ls.size() == 0 || ls.get(0) != v[i]) {
                int cnt = 0;
                for (int j = 0; j < n; j++) {
                    // counting the frequency of v[i]
                    if (v[j] == v[i]) {
                        cnt++;
                    }
                }

                // check if frquency is greater than n/3:
                if (cnt > (n / 3))
                    ls.add(v[i]);
            }

            if (ls.size() == 2) break;
        }

        return ls;
    }
```


### **Better Approach (Using Hashing)**: 

### **Intuition:**

Use a better data structure to reduce the number of look-up operations and hence the time complexity. Moreover, we have been calculating the count of the same element again and again – so we have to reduce that also.

### **Approach:** 

The steps are as follows:

1.  Use a hashmap and store the elements as \<key, value> pairs. (Can also use frequency array based on the size of nums).  
    Here the key will be the element of the array and the value will be the number of times it occurs. 
2.  Traverse the whole array and update the occurrence of each element. 
3.  After that, check the map if the value for any element is greater than the **floor of N/3**. 
    1.  If yes, include it in the list. 
    2.  Else iterate forward.
4.  Finally, return the list.

```java
    public static List<Integer> majorityElement(int []v) {
        int n = v.length; //size of the array
        List<Integer> ls = new ArrayList<>(); // list of answers

        //declaring a map:
        HashMap<Integer, Integer> mpp = new HashMap<>();

        // least occurrence of the majority element:
        int mini = (int)(n / 3) + 1;

        //storing the elements with its occurnce:
        for (int i = 0; i < n; i++) {
            int value = mpp.getOrDefault(v[i], 0);
            mpp.put(v[i], value + 1);

            //checking if v[i] is
            // the majority element:
            if (mpp.get(v[i]) == mini) {
                ls.add(v[i]);
            }
            if (ls.size() == 2) break;
        }

        return ls;
    }
```


### 👌 **Optimal Approach (Extended Boyer Moore’s Voting Algorithm)**:

### **Approach:**

1.  Initialize 4 variables:  
    **cnt1 & cnt2** –  for tracking the counts of elements  
    **el1 & el2** – for storing the majority of elements.
2.  Traverse through the given array.
    1.  If **cnt1** is 0 and the current element is not el2 then store the current element of the array as **el1 along with increasing the cnt1 value by 1**.
    2.  If **cnt2** is 0 and the current element is not el1 then store the current element of the array as **el2 along with increasing the cnt2 value by 1**.
    3.  If the current element and **el1** are the same increase the **cnt1** by 1.
    4.  If the current element and **el2** are the same increase the **cnt2** by 1.
    5.  Other than all the above cases: decrease cnt1 and cnt2 by 1.
3.  The integers present in **el1 & el2** should be the result we are expecting. So, using another loop, we will manually check their counts if they are greater than the floor(N/3).

![](https://33333.cdn.cke-cs.com/kSW7V9NHUXugvhoQeFaf/images/6fa38cc5f4e85f20c4431558101ec1cd42472396d4ec53ea.png)

```java
class Solution {
    public List<Integer> majorityElement(int[] nums) {
        int n = nums.length; //size of the array

        int cnt1 = 0, cnt2 = 0; // counts
        int el1 = Integer.MIN_VALUE; // element 1
        int el2 = Integer.MIN_VALUE; // element 2

        // applying the Extended Boyer Moore's numsoting Algorithm:
        for (int i = 0; i < n; i++) {
            if (cnt1 == 0 && el2 != nums[i]) {
                cnt1 = 1;
                el1 = nums[i];
            } else if (cnt2 == 0 && el1 != nums[i]) {
                cnt2 = 1;
                el2 = nums[i];
            } else if (nums[i] == el1) cnt1++;
            else if (nums[i] == el2) cnt2++;
            else {
                cnt1--; cnt2--;
            }
        }

        List<Integer> ls = new ArrayList<>(); // list of answers

        // Manually check if the stored elements in
        // el1 and el2 are the majority elements:
        cnt1 = 0; cnt2 = 0;
        for (int i = 0; i < n; i++) {
            if (nums[i] == el1) cnt1++;
            if (nums[i] == el2) cnt2++;
        }

        int mini = (int)(n / 3) + 1;
        if (cnt1 >= mini) ls.add(el1);
        if (cnt2 >= mini) ls.add(el2);

        // Uncomment the following line
        // if it is told to sort the answer array:
        //Collections.sort(ls); //TC --> O(2*log2) ~ O(1);

        return ls;
    }
}
```

## Q:- [**Unique Paths**](https://leetcode.com/problems/unique-paths/)

![](https://33333.cdn.cke-cs.com/kSW7V9NHUXugvhoQeFaf/images/99d6a64d499b0bab277f8d821f24bae43f7ce8184519ee4a.png)

**Explanation: \*\* From the top left corner there is a total o**f 3\*\* ways to reach the bottom right corner.

**Step 1:** Right ->> Right ->> Down


**Step 2:** Right ->> Down ->> Right


**Step 3:** Down ->> Right->> Right


## **Solution**

_**Disclaimer**_: _Don’t jump directly to the solution, try it out yourself first._

**Approach**: The problem states that we can either move rightward or downward direction. So we recursively try out all the possible combinations.

1.  At first, we are at the **(0,0)** index let’s assume this state as **(i,j).** From here we can move towards the bottom as well as towards the right and we recursively move until we hit the base case.


2\. At any point of time when the recursive call goes out of the matrix boundary **(example: let’s assume m = 2, n= 2, and the current position of i and j is (2,0) which is out of matrix boundary)**, we’ll return zero because from here there are no possible paths beyond and that is the first base case.


3\. Whenever the recursive call reaches the end we’ll return **1** because we have found one possible path to reach the end.


4\. In the recursive tree what result we have got from the left transition and the right transition will sum it up and return the answer.

![](https://33333.cdn.cke-cs.com/kSW7V9NHUXugvhoQeFaf/images/ae0ecf20804eeebc01b12a533b7c5d268354bb04c2619b5d.png)

```java
public class Solution {
    public int countPaths(int i, int j, int n, int m) {
        if (i == (n - 1) && j == (m - 1))
            return 1;
        if (i >= n || j >= m)
            return 0;
        else
            return countPaths(i + 1, j, n, m) + countPaths(i, j + 1, n, m);
    }

    public int uniquePaths(int m, int n) {
        return countPaths(0, 0, m, n);
    }

    public static void main(String[] args) {
        Solution obj = new Solution();
        int totalCount = obj.uniquePaths(3, 7);
        System.out.println("The total number of Unique Paths are " + totalCount);
    }
}
```


###  👌 **Solution 2:** [**Dynamic Programming**](https://takeuforward.org/data-structure/dynamic-programming-introduction/) **Solution**

**Intuition**: The dynamic programming solution of this problem is a bit extension of the recursive solution. In recursive solutions, there are many overlapping subproblems. In this solution instead of traversing all the possible paths, whenever we get the result we’ll store it in a matrix for future use. Whenever we encounter the same subproblem we directly get the value from the matrix instead of recomputing it. By this memorization technique, we can avoid the recomputation and the time complexity will be drastically reduced. This is the main intuition behind this dynamic programming solution.

**Approach**: 

**Step 1**: Take a dummy matrix **A\[ \]\[ \]** of size **m X n** and fill it with **\-1.**

**Step 2**: At first, we are at the **(0,0)** index let’s assume this state as **(i,j).** From here we can move towards the bottom as well as towards the right and we recursively move until we hit the base case.


**Step 3:** At any point of time when the recursive call goes out of the boundary **(example: let’s assume m = 2, n= 3, and the current position of i and j is (2,0) which is out of matrix boundary)**, we will return zero because from here there are no possible paths beyond and that is the first base case.


**Step 4:** Whenever the recursive call reaches the end we’ll return **1** because we have found one possible path to reach the end.


**Step 5:** The only change in the dynamic programming solution is whenever we are returning answers we store them in the matrix **A\[i\]\[j\]** and wherever we are making a recursive call we simply check if that state is already visited or not in other words we’ll check if **A\[i\]\[j\]** is **\-1** or not if it is not **\-1** that means that there is a subproblem which is repeating. Now instead of recomputing the subproblem, we’ll return the value at **A\[i\]\[j\].**

![](https://33333.cdn.cke-cs.com/kSW7V9NHUXugvhoQeFaf/images/ae0ecf20804eeebc01b12a533b7c5d268354bb04c2619b5d.png)
```java
import java.util.Arrays;

public class Solution {
    public int countPaths(int i, int j, int n, int m, int[][] dp) {
        if (i == (n - 1) && j == (m - 1)) return 1;
        if (i >= n || j >= m) return 0;
        if (dp[i][j] != -1) return dp[i][j];
        else return dp[i][j] = countPaths(i + 1, j, n, m, dp) + countPaths(i, j + 1, n, m, dp);
    }

    public int uniquePaths(int m, int n) {
        int[][] dp = new int[m][n];
        for (int[] row : dp) Arrays.fill(row, -1);

        int num = countPaths(0, 0, m, n, dp);
        if (m == 1 && n == 1)
            return num;
        return dp[0][0];
    }

    public static void main(String[] args) {
        Solution obj = new Solution();
        int totalCount = obj.uniquePaths(3, 7);
        System.out.println("The total number of Unique Paths are " + totalCount);
    }
}
```



## Q:- [**Reverse Pairs**](https://leetcode.com/problems/reverse-pairs/)

![](https://33333.cdn.cke-cs.com/kSW7V9NHUXugvhoQeFaf/images/3536eff62daa5ad43b67220b6cab756d89e5ab3796b3fae9.png)

**Solution:**

This question is slightly different from the question [count inversion](https://takeuforward.org/data-structure/count-inversions-in-an-array/) where the condition was a\[i\] > a\[j\] but here in this question, the condition is a\[i\] > 2\*a\[j\]. In both questions, the index i \< j.

### **Naive Approach (Brute-force)**:

### **Approach:**

The steps are as follows:

1.  First, we will run a loop(say i) from 0 to N-1 to select the a\[i\].
2.  As index j should be greater than index i, inside loop i, we will run another loop i.e. j from i+1 to N-1, and select the element a\[j\].
3.  Inside this second loop, we will check if a\[i\] > 2\*a\[j\] i.e. if a\[i\] and a\[j\] can be a pair. If they satisfy the condition, we will increase the count by 1.
4.  Finally, we will return the count i.e. the number of such pairs.

### **Intuition:**

The naive approach is pretty straightforward. We will use nested loops to generate all possible pairs. We know index i must be smaller than index j. So, we will fix i at one index at a time through a loop, and with another loop, we will check(_the condition a\[i\] > 2\*a\[j\]_) the elements from index i+1 to N-1  if they can form a pair with a\[i\].

```java

import java.util.*;

public class tUf {

    public static int countPairs(int[] a, int n) {

        // Count the number of pairs:
        int cnt = 0;
        for (int i = 0; i < n; i++) {
            for (int j = i + 1; j < n; j++) {
                if (a[i] > 2 * a[j])
                    cnt++;
            }
        }
        return cnt;
    }

    public static int reversPair(int[] skill, int n) {
        return countPairs(skill, n);
    }

    public static void main(String[] args) {
        int[] a = {4, 1, 2, 3, 1};
        int n = 5;
        int cnt = team(a, n);
        System.out.println("The number of reverse pair is: " + cnt);
    }
}  
```


### 👌 **Optimal Approach**:

### **Intuition:**

In order to solve this problem we will use the merge sort algorithm like we used in the problem [count inversion](https://takeuforward.org/data-structure/count-inversions-in-an-array/) with a slight modification of the merge() function. But in this case, **the same logic will not work**. In order to understand this, we need to deep dive into the merge() function.

**Why the same logic of** [**count inversion**](https://takeuforward.org/data-structure/count-inversions-in-an-array/) **will not work?**

*   The merge function works by comparing two elements from two halves i.e. arr\[left\] and arr\[right\]. Now, the condition in the question was arr\[i\] > arr\[j\]. That is why we merged the logic. While comparing the elements, we counted the number of pairs.
*   But in this case, the condition is arr\[i\] > 2\*arr\[j\]. And, we cannot change the condition of comparing the elements in the merge() function. If **we change the condition, the merge() function will fail to merge the elements**. So, we need to check this **condition and count the number of pairs separately**.

```java
class Solution {
    public int reversePairs(int[] nums) {
         return invCount(nums, 0, nums.length-1);
    }

    private int invCount(int[] A, int start, int end) {
    int MOD = 1000000007;
    if (start == end)
      return 0;

    int mid = (start + end) / 2;
    int x = invCount(A, start, mid) % MOD;
    int y = invCount(A, mid + 1, end) % MOD;
    int z = merge(A, start, mid, end) % MOD;
    return (x + y + z) % MOD;
  }

  private int merge(int[] A, int start, int mid, int end) {
    int count = 0;
    int p1 = start;
    int p2 = mid + 1;
    int p3 = 0;
    int[] c = new int[end - start + 1];
    while(p1<=mid && p2<=end){
        if((long)A[p1]>(long)2*A[p2]){
             count += (mid - p1 + 1);
             p2++;
        }else{
            p1++;
        }
    }
    p1=start;
    p2=mid+1;
    while (p1 <= mid && p2 <= end) {
      if (A[p1] <= A[p2]) {
        c[p3] = A[p1];
        p1++;
        p3++;
      } else {
        c[p3] = A[p2];
        p2++;
        p3++;
        
      }
    }
    while (p1 <= mid) {
      c[p3] = A[p1];
      p1++;
      p3++;
    }

    while (p2 <= end) {
      c[p3] = A[p2];
      p2++;
      p3++;
    }

    p3 = 0;
    for (int i = start; i <= end; i++) {
      A[i] = c[p3];
      p3++;
    }
    return count;

  }
}
```

