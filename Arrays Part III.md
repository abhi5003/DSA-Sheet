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

![](https://lh5.googleusercontent.com/IAzEYfiKE6DchH4oK2tf6P2omhORyaei5olLJ3Domv8Ky8N_hrG-6w4tquKM17FLZQvtKaioA4UkgDj1U8zcWOYkzVL_tgjFIwTjQpB_f1ouIp_yQrnazgd2-ieV5-sKp2lrEhzt)

**Step 2:** Right ->> Down ->> Right

![](https://lh6.googleusercontent.com/rcKQmVQ7jtcGsO98t7cZiA6S1VzZAV4YXW5v_C4hWC6led7bTOjaPmsZXeQEG2r7BaSKqMw0CTce6_919COT9qNeFYQHK_gt1SToefOov7kVAn_sJwZTH69izuepyTTo9sAiNehM)

**Step 3:** Down ->> Right->> Right

![](https://lh3.googleusercontent.com/0fJMMZyDIe-z7EcpiIpAzIPovX5C3WJUGsAqfwHHGIxXc5yrxUqK8Y87P4yeWVEVbjIynLxN142751YKmfsnpguBTyjNn1aNzDoDtR2oo39o1cPjJuaXKqWbo9ghx9BbxgLc0n9M)

## **Solution**

_**Disclaimer**_: _Don’t jump directly to the solution, try it out yourself first._

**Approach**: The problem states that we can either move rightward or downward direction. So we recursively try out all the possible combinations.

1.  At first, we are at the **(0,0)** index let’s assume this state as **(i,j).** From here we can move towards the bottom as well as towards the right and we recursively move until we hit the base case.

![](https://lh4.googleusercontent.com/SEgVVsDHe3SNZ0GtYHBhSNIxfBV1bjQYXJ_zbLh3hQJO0GuWt8ruWoLx_3vWzNZJlR5VnTBk0KXgblCd11Mu1NZdJfhr8t7fO8JevbvSI0ODvYeLqfToI_hZOwjfM_U3RDTUb885)

![](https://lh6.googleusercontent.com/bu9FxbN-MrDGUY73zhok8X7u-dv421b6PdmVgULIETlLjVQ1u3xM83tWD4y1I0hR0AgJ68GkgMptD94XfONLEPlrnA_gfg44SYKL6RnVBAyhiIE1I2P325IrYPsV-rANi3F9EvOk)

2\. At any point of time when the recursive call goes out of the matrix boundary **(example: let’s assume m = 2, n= 2, and the current position of i and j is (2,0) which is out of matrix boundary)**, we’ll return zero because from here there are no possible paths beyond and that is the first base case.

![](https://lh6.googleusercontent.com/wrmMhfQRdsMQCOydlQ517m0nJ2wUKMWbHP9GMbUuj6CSQV1aWXa1lM7wbIGTHMbzh6rfThYT2fCZSSS6fOIdaTdi38v1oDHMRLFv1dZE_QTIfP0eGh8V7Fulq57NbLOiEik5_yXJ)

![](https://lh6.googleusercontent.com/StgnXvK44FzWWltaFmHqw7Vj9WoRTbgzqSYjfocwKk6r9yxtoTKWTxoQ5m8dmabjD2laGNq0qh0EKhuNU6s5u_8Z6Fp0qg5OLG5jI8L2qpAGYOQWNCfP1g416bGfTQ2EIwe28Sti)

3\. Whenever the recursive call reaches the end we’ll return **1** because we have found one possible path to reach the end.

![](https://lh5.googleusercontent.com/uxUYTFyxi7M1zNkBQa3oc1Gc30nieewflWGj62Xw0sQo-bRFcnqmddF-Q9x8aO9P703DKOo0oYoUP5sG2kXKA_dDIHaC_yqrhXOUolRnmDr9MvU1HNH0vmyNPt3pn5WS3zJH8F2K)

![](https://lh6.googleusercontent.com/fRX-ys65xTQwL5naNe6krcGr8_slCefQbYLGrsGAzevwkMiQctJcxSlB2YB1rjDW4W99gcABIMnsvTgZcDrFHrXaAvNBETzAufDA3rcsxeotNGhkr0lwLqonfQMf6Nf_vSCgMfKo)

4\. In the recursive tree what result we have got from the left transition and the right transition will sum it up and return the answer.

![](https://lh6.googleusercontent.com/bEE5PtHZP3E7z105IY431kD8n1WtQiLzNWc8dI6ig3eJc_9ge1G8F0o9PGcXb45kvRiiZ90h7jZaxhP_Q85B6_bmCiJVCepX9T8QGHJo6cGWnOBFKKj88lM3cQCf8qT2WpSxygXf)

![](https://lh6.googleusercontent.com/KFPX5YL0-B5bIYsC5zO0Z9vffVKOQL3aYRNzRzmvPZz-JlAZX-zg41-HDnnZ50dd-zu45VTDvmZO_WhpstARiQj64TWpbirMzP2_YzpIuU7qOp3ekghQ6ps3RDWx1nFsRq05yoTY)

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

![](https://lh3.googleusercontent.com/JbkiIcUEFeQ8HXy5TNF1BEH_ZleehN5RnUIUcXPb2AjMVw9tjE6V25L8aftQ5nQ6_vgNEzAYnKzsUgWJY3hmDmW2nVv5_BjsQ-Q9moK6YBEJCNjaUH1qtW11ovWtv6AEf7sWUlQm)

![](https://lh6.googleusercontent.com/bu9FxbN-MrDGUY73zhok8X7u-dv421b6PdmVgULIETlLjVQ1u3xM83tWD4y1I0hR0AgJ68GkgMptD94XfONLEPlrnA_gfg44SYKL6RnVBAyhiIE1I2P325IrYPsV-rANi3F9EvOk)

**Step 3:** At any point of time when the recursive call goes out of the boundary **(example: let’s assume m = 2, n= 3, and the current position of i and j is (2,0) which is out of matrix boundary)**, we will return zero because from here there are no possible paths beyond and that is the first base case.

![](https://lh5.googleusercontent.com/ev2JMXBF8xOucsE0EmohQmbsF5w7eO9dgjJnIiIT-iNfhttpO-PGAh1DfKd0bh8of_CVjyjWBFoiQAhvIyXtXkDyklUX96B5ntKc7Gv-qZva__nexG3j3ErUzQgUhVr6_dY2fv7p)

![](https://lh6.googleusercontent.com/StgnXvK44FzWWltaFmHqw7Vj9WoRTbgzqSYjfocwKk6r9yxtoTKWTxoQ5m8dmabjD2laGNq0qh0EKhuNU6s5u_8Z6Fp0qg5OLG5jI8L2qpAGYOQWNCfP1g416bGfTQ2EIwe28Sti)

**Step 4:** Whenever the recursive call reaches the end we’ll return **1** because we have found one possible path to reach the end.

![](https://lh4.googleusercontent.com/R3CRSmmUq26ErlnwW8TGPIHPEuBB8qtJQ3guCjCvJ2iX12_pFASM7exyumTQOpS8E5y1O8GvLFIkkCADoDyaY2QLP1fj-QMUpX3bv47jxPc0e0HF83EqDxKHcm5nXu3aPUX4NSJW)

![](https://lh4.googleusercontent.com/rGBaBSkM2YQktqwc-D5dvvL36TdYKV38Yu-kXFDqG9RZheXY1nA7lz5UbNxdbSz1RAX_Me9MJs-Q6KefTWypE7JJkF_OcxQjD_32DxdLkxR3iMqr39xb_DJFWzuZazd3m--Q4-pe)

**Step 5:** The only change in the dynamic programming solution is whenever we are returning answers we store them in the matrix **A\[i\]\[j\]** and wherever we are making a recursive call we simply check if that state is already visited or not in other words we’ll check if **A\[i\]\[j\]** is **\-1** or not if it is not **\-1** that means that there is a subproblem which is repeating. Now instead of recomputing the subproblem, we’ll return the value at **A\[i\]\[j\].**

**![](https://lh4.googleusercontent.com/DwOp_Jd_aEVUCWI93KeRKAF0JdwAFNkOf66exqQ93Iv_6GhEk6Gkz0gJEhCfrY0eo7Ps6Wge2zDqBsDHsdx4P9DjO92l4dG1fCecMH_aZzQ0wwI89Z322VxLqsg5EH_jgBnpH2YF)**

**![](https://lh4.googleusercontent.com/JNtfoDqNrcSvpCyBW8SxIu6vR94SzZrAKkfO83CJ8kPJLA1cM9LiE7TszPWRFINxduB5cpx55v7E--k-EKnfa-igqr3ylx1vbVARY4YVSe3xYVdSFP81dRBqaqUgJizhkwUW-kuz)**

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

