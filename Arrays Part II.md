## **Q:-** [**Rotate Image**](https://leetcode.com/problems/rotate-image/)

You are given an `n x n` 2D `matrix` representing an image, rotate the image by **90** degrees (clockwise).

You have to rotate the image [**in-place**](https://en.wikipedia.org/wiki/In-place_algorithm), which means you have to modify the input 2D matrix directly. **DO NOT** allocate another 2D matrix and do the rotation.

![](https://33333.cdn.cke-cs.com/kSW7V9NHUXugvhoQeFaf/images/8464e9c5b329530a91e5fac557515e019b368eb64766437b.png)

### **👌 Intuition :-**

> 1.  First take transpose of the matrix
> 2.  Then reverse each row

```java
class Solution {
    public void rotate(int[][] matrix) {

        int n=matrix.length;
        int m=matrix[0].length;

        // First take transpose of matrix

        for(int i = 0; i < n; i++) {
            for(int j = i + 1; j < m; j++) {
                int tmp = matrix[i][j];
                matrix[i][j] = matrix[j][i];
                matrix[j][i] = tmp;
            }
        }

           // Reverse the each row
       
            for(int i=0; i<n; i++){
                for(int j=0; j<m/2; j++){
                   int temp=matrix[i][j];
                   matrix[i][j]=matrix[i][m - 1 -j];
                   matrix[i][m -1 -j]=temp;


                }
            }
        
        
    }
}
```



## Q:- [**Merge Intervals**](https://leetcode.com/problems/merge-intervals/)

Given an array of `intervals` where `intervals[i] = [starti, endi]`, merge all overlapping intervals, and return _an array of the non-overlapping intervals that cover all the intervals in the input_.

**Example 1:**

**Input:** intervals = \[\[1,3\],\[2,6\],\[8,10\],\[15,18\]\] **Output:** \[\[1,6\],\[8,10\],\[15,18\]\] **Explanation:** Since intervals \[1,3\] and \[2,6\] overlap, merge them into \[1,6\].

**Example 2:**

**Input:** intervals = \[\[1,4\],\[4,5\]\] **Output:** \[\[1,5\]\] **Explanation:** Intervals \[1,4\] and \[4,5\] are considered overlapping.

### 👌 Intuition:-

> 1.  Sort the intervals by the starting point **Arrays.sort(intervals, (a, b) -> a\[0\] - b\[0\]);**
> 2.  ```java
>     // Inisalize first interval start & end
>             int start = intervals[0][0];
>             int end = intervals[0][1];
>     ```
>     
> 3.  Iterate over intervals
> 4.  **Overlap case -** if current start is less then equal to previous end **take max of both**
> 5.  else insert interval and take their start and end
> 6.  At that last one interval will remain after iteration insert that interval in ans list

```java
class Solution {
    public int[][] merge(int[][] intervals) {

        List<int[]> res = new ArrayList<>();
        // Check for edge case
        if(intervals.length == 0 || intervals == null) return res.toArray(new int[0][]);
        
        // Sort the intervals by the starting point

        Arrays.sort(intervals, (a, b) -> a[0] - b[0]);
        
        // Inisalize first interval start & end
        int start = intervals[0][0];
        int end = intervals[0][1];
        
        for(int[] interval : intervals) {
            
            // If previous interval end is grater than
            // current interval start then take max from bot end
            // and assign to previous end 
            // and do continous this operate if true

            if(interval[0] <= end) {
                end = Math.max(end, interval[1]);
            }
            else {
                // if above condition false then
                // add previous interval in result
                // and assign new start and end from current interval

                res.add(new int[]{start, end});
                start = interval[0];
                end = interval[1];
            }
        }

        // At the one interval has rest
        // also add that interval in result

        res.add(new int[]{start, end});
       return res.toArray(new int[0][]);
         
    }
}
```
