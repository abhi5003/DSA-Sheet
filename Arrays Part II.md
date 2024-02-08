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



## **Q:-** [**Merge Without Extra Space**](https://www.geeksforgeeks.org/problems/merge-two-sorted-arrays-1587115620/1)

**Example 1:**

**Input**: 

n = 4,  arr1\[\] = \[1 3 5 7\] 

m = 5,  arr2\[\] = \[0 2 6 8 9\]

**Output**: 

arr1\[\] = \[0 1 2 3\]

arr2\[\] = \[5 6 7 8 9\]

**Approach 1 :-** Two pointer technique 

### 👌 Intuition -

> 1.  We will declare two pointers i.e. **left** and **right.** T**he left pointer will point to the last index of the arr1\[\]** (_i.e. Basically the maximum element of the array_). **The right pointer will point to the first index of the arr2\[\]** (_i.e. Basically the minimum element of the array_).
> 2.  Now, the left pointer will move toward index 0 and the right pointer will move towards the index m-1. While moving the two pointers we will face 2 different cases like the following:
>     1.  **If arr1\[left\] > arr2\[right\]:** In this case, we will swap the elements and move the pointers to the next positions.
>     2.  **If arr1\[left\] \<= arr2\[right\]:** In this case, we will stop moving the pointers as arr1\[\] and arr2\[\] are containing correct elements.
> 3.  Thus, after step 2, arr1\[\] will contain all smaller elements and arr2\[\] will contain all bigger elements. Finally, we will sort the two arrays.

![](https://33333.cdn.cke-cs.com/kSW7V9NHUXugvhoQeFaf/images/65bf02727eda34c62b4252c934bd7de3d594d7c2e098af1f.png)

```java


//User function Template for Java

class Solution
{
    //Function to merge the arrays.
    public static void merge(long arr1[], long arr2[], int n, int m) 
    {
        int left=n-1;
        int right=0;
        
        // Swap the elements until arr1[left] is
        // smaller than arr2[right]:
        
        while(left>=0 && right < m){
            
            if(arr1[left] > arr2[right]){
                long temp=arr1[left];
                arr1[left]=arr2[right];
                arr2[right]=temp;
                left--;
                right++;
            }else{
                break;
            }
        }
        
        Arrays.sort(arr1);
        Arrays.sort(arr2);
    }
}
```



**Approach 2 (Using gap method)**: 

### **👌 Intuition:**

> Similar to optimal approach 1, in this approach, we will use two pointers i.e. left and right, and swap the elements if the element at the left pointer is greater than the element at the right pointer. 
> 
> But the placing of the pointers will be based on the gap value calculated. The formula to calculate the initial gap is the following:
> 
> **Initial gap = ceil((size of arr1\[\] + size of arr2\[\]) / 2)**
> 
> **Assume the two arrays as a single continuous array and initially**, we will place the **left pointer at the first index** and the **right pointer at the (left+gap) index** of that continuous array.
> 
> Now, we will compare the elements at the left and right pointers and move them by 1 place each time after comparison. While comparing we will swap the elements if _**the element at the left pointer > the element at the right pointer**_. After some steps, the right pointer will reach the end and the iteration will be stopped.
> 
> After each iteration, we will decrease the gap and will follow the same procedure until the iteration for gap = 1 gets completed. Now, after each iteration, the gap will be the following:
> 
> **gap = ceil( previous gap / 2)**
> 
> The whole process will be applied to the imaginary continuous array constructed using arr1\[\] and arr2\[\].

```java


//User function Template for Java

class Solution
{
    //Function to merge the arrays.
    public static void merge(long arr1[], long arr2[], int n, int m) 
    {
        // len of the imaginary single array:
       int len=n+m;
       
        // Initial gap:
       int gap=(len/2) + (len%2); //  module either gives 0 or 1 in case of even or odd
       
       while(gap>0){
           
           // inisalize two pointers
           int left=0;
           int right=left+gap;
           
           // check for right pointer only because right will first goes out of the boundary
           while(right < len){
               
               // case 1: left in arr1[]
                //and right in arr2[]:
                if(left < n && right >= n){
                    swapIfGreater(arr1, arr2, left, right - n);
                }
                 // case 2: both pointers in arr2[]:
                else if(left >= n){
                    swapIfGreater(arr2, arr2, left - n, right - n);
                }
                
                // case 3: both pointers in arr1[]:
                else{
                     swapIfGreater(arr1, arr1, left, right);
                }
                
                left++;
                right++;
           }
           
            // break if iteration gap=1 is completed:
            if (gap == 1) break;

            // Otherwise, calculate new gap:
            gap = (gap / 2) + (gap % 2);
       }
    }
    
    public static void swapIfGreater(long[] arr1, long[] arr2, int ind1, int ind2) {
        if (arr1[ind1] > arr2[ind2]) {
            long temp = arr1[ind1];
            arr1[ind1] = arr2[ind2];
            arr2[ind2] = temp;
        }
    }
}
```




## **Q:-** [**Find the Duplicate Number**](https://leetcode.com/problems/find-the-duplicate-number/)

**Example 1:**

**Input:** nums = \[1,3,4,2,2\]

**Output:** 2

**Example 2:**

**Input:** nums = \[3,1,3,4,2\]

**Output:** 3

## **Brute Force (2 Loops)**

```java
    // 2 Loops
    public static int findDuplicate_2loops(int[] nums) {
        int len = nums.length;
        for (int i = 0; i < len; i++) {
            for (int j = i + 1; j < len; j++) {
                if (nums[i] == nums[j]) {
                    return nums[i];
                }
            }
        }

        return len;
    }
```

## **Analysis**

*   **Time Complexity**: O(n2)O(n^2)_O_(_n_2)
*   **Space Complexity**: O(1)O(1)_O_(1)

## **Count**

Count the frequency of the num in the array.

With extra O(n)O(n)_O_(_n_) space, without modifying the input.

```java
    public static int findDuplicate(int[] nums) {
        int len = nums.length;
        int[] cnt = new int[len + 1];
        for (int i = 0; i < len; i++) {
            cnt[nums[i]]++;
            if (cnt[nums[i]] > 1) {
                return nums[i];
            }
        }

        return len;
    }
```

## **Analysis**

*   **Time Complexity**: O(n)O(n)_O_(_n_)
*   **Space Complexity**: O(n)O(n)_O_(_n_)

## **Hash**

```java
    public static int findDuplicate_set(int[] nums) {
        Set<Integer> set = new HashSet<>();
        int len = nums.length;
        for (int i = 0; i < len; i++) {
            if (!set.add(nums[i])) {
                return nums[i];
            }
        }

        return len;
    }
```

## **Analysis**

*   **Time Complexity**: O(n)O(n)_O_(_n_)
*   **Space Complexity**: O(n)O(n)_O_(_n_)

## **Marking visited value within the array**

```java
    // Visited
    public static int findDuplicate_mark(int[] nums) {
        int len = nums.length;
        for (int num : nums) {
            int idx = Math.abs(num);
            if (nums[idx] < 0) {
                return idx;
            }
            nums[idx] = -nums[idx];
        }

        return len;
    }
```

## **Analysis**

*   **Time Complexity**: O(n)O(n)_O_(_n_)
*   **Space Complexity**: O(1)O(1)_O_(1)

## **Sort**

```java
    public static int findDuplicate_sort(int[] nums) {
        Arrays.sort(nums);
        int len = nums.length;
        for (int i = 1; i < len; i++) {
            if (nums[i] == nums[i - 1]) {
                return nums[i];
            }
        }

        return len;
    }
```

## **Analysis**

*   **Time Complexity**: O(nlogn)O(nlogn)_O_(_nlogn_)
*   **Space Complexity**: O(logn)O(logn)_O_(_logn_)

## **Index Sort**

```java
    // Index Sort
    // n + 1 numbers in n.
    public static int findDuplicate_index_sort(int[] nums) {
        int len = nums.length;
        for (int i = 0; i < len; ) {
            int n = nums[i];
            if (n == i + 1) {
                i++;
            } else if (n == nums[n - 1]) {
                return n;
            } else {
                nums[i] = nums[n - 1];
                nums[n - 1] = n;
            }
        }

        return 0;
    }
```

## **Analysis**

*   **Time Complexity**: O(n)O(n)_O_(_n_)
*   **Space Complexity**: O(1)O(1)_O_(1)

## **Binary Search**

```java
    public static int findDuplicate_bs(int[] nums) {
        int len = nums.length;
        int low = 1;
        int high = len - 1;
        while (low < high) {
            int mid = low + (high - low) / 2;
            int cnt = 0;
            for (int i = 0; i < len; i++) {
                if (nums[i] <= mid) {
                    cnt++;
                }
            }

            if (cnt <= mid) {
                low = mid + 1;
            } else {
                high = mid;
            }
        }

        return low;
    }
```

## **Analysis**

*   **Time Complexity**: O(nlogn)O(nlogn)_O_(_nlogn_)
*   **Space Complexity**: O(1)O(1)_O_(1)

## **Bit**

|   | 1 | 3 | 4 | 2 | 2 | x | y |
| --- | --- | --- | --- | --- | --- | --- | --- |
| Bit 0 | 1 | 1 | 0 | 0 | 0 | 2 | 2 |
| Bit 1 | 1 | 0 | 1 | 1 | 1 | 3 | 2 |
| Bit 2 | 0 | 0 | 1 | 0 | 0 | 1 | 1 |

```java
    public static int findDuplicate_bit(int[] nums) {
        int n = nums.length;
        int ans = 0;
        int bit_max = 31;
        while (((n - 1) >> bit_max) == 0) {
            bit_max -= 1;
        }

        for (int bit = 0; bit <= bit_max; ++bit) {
            int x = 0, y = 0;
            for (int i = 0; i < n; ++i) {
                if ((nums[i] & (1 << bit)) != 0) {
                    x += 1;
                }
                if (i >= 1 && ((i & (1 << bit)) != 0)) {
                    y += 1;
                }
            }
            if (x > y) {
                ans |= 1 << bit;
            }
        }

        return ans;
    }
```

## **Analysis**

*   **Time Complexity**: O(nlogn)O(nlogn)_O_(_nlogn_)
*   **Space Complexity**: O(1)O(1)_O_(1)

## **Fast Slow Pointers**

![Link List](https://assets.leetcode.com/users/images/aeb3e536-9615-466c-a2de-88a9a59ef255_1648626512.666411.png)

With extra O(n)O(n)_O_(_n_) space, without modifying the input.

```java
    public int findDuplicate_fastSlow(int[] nums) {
        int slow = 0;
        int fast = 0;
        do {
            slow = nums[slow];
            fast = nums[nums[fast]];
        } while (slow != fast);

        slow = 0;
        while (slow != fast) {
            slow = nums[slow];
            fast = nums[fast];
        }
        
        return slow;
    }
```

## **Analysis**

*   **Time Complexity**: O(n)O(n)_O_(_n_)
*   **Space Complexity**: O(1)O(1)_O_(1)



## Q:- [**Find Missing And Repeating**](https://www.geeksforgeeks.org/problems/find-missing-and-repeating2512/1)

**Example 1:**

**Input:** N = 2 Arr\[\] = {2, 2}

**Output:** 2 1

**Explanation:** Repeating number is 2 and smallest positive missing number is 1.

**Example 2:**

**Input:** N = 3 Arr\[\] = {1, 3, 3}

**Output:** 3 2

**Explanation:** Repeating number is 3 and smallest positive missing number is 2.

### **Approach:** Using HashMap

```java
// Java program to find the
// repeating and missing elements
// using Maps

import java.util.*;

public class Test1 {

	public static void main(String[] args)
	{

		int[] arr = { 4, 3, 6, 2, 1, 1 };

		Map<Integer, Boolean> numberMap
			= new HashMap<>();

		int max = arr.length;

		for (Integer i : arr) {

			if (numberMap.get(i) == null) {
				numberMap.put(i, true);
			}
			else {
				System.out.println("Repeating = " + i);
			}
		}
		for (int i = 1; i <= max; i++) {
			if (numberMap.get(i) == null) {
				System.out.println("Missing = " + i);
			}
		}
	}
}
```

### **Approach:** 

**Use elements as Index and mark the visited places**

```java
// Java program to Find the repeating
// and missing elements

import java.io.*;

class GFG {

	static void printTwoElements(int arr[], int size)
	{
		int i;
		System.out.print("The repeating element is ");

		for (i = 0; i < size; i++) {
			int abs_val = Math.abs(arr[i]);
			if (arr[abs_val - 1] > 0)
				arr[abs_val - 1] = -arr[abs_val - 1];
			else
				System.out.println(abs_val);
		}

		System.out.print("and the missing element is ");
		for (i = 0; i < size; i++) {
			if (arr[i] > 0)
				System.out.println(i + 1);
		}
	}

	// Driver code
	public static void main(String[] args)
	{
		int arr[] = { 7, 3, 4, 5, 5, 6, 2 };
		int n = arr.length;
		printTwoElements(arr, n);
	}
}


```


### Approach

**Using cycle sort**

```java
import java.util.*;

class FindCorruptPair {
    public static int[] findCorruptPair(int[] nums) {
        // Initialize missing and duplicated
        int duplicated = 0;
        int missing = 0;
        int[] pair = {
            0,
            0
        };
        // Apply cyclic sort on the array
        // Traversing the whole array
        int i = 0;
        while (i < nums.length) {
            // Determining what position the specific element should be at
            int correct = nums[i] - 1;
            //Check if the number is at wrong position
            if (nums[i] != nums[correct]) {
                // Swapping the number to it's correct position
                swap(nums, i, correct);
            } else {
                i = i + 1;
            }
        }
        // Finding the corrupt pair(missing, duplicated)     
        for (int j = 0; j < nums.length; j++) {
            if (nums[j] != j + 1) {
                duplicated = nums[j];
                missing = j + 1;
            }

        }
        pair[0] = missing;
        pair[1] = duplicated;

        return pair;
    }
    // Function for swapping
    public static void swap(int[] arr, int first, int second) {
        int temp = arr[first];
        arr[first] = arr[second];
        arr[second] = temp;
    }
    // Driver code
    public static void main(String[] args) {

        int[][] array = {{3, 1, 2, 5, 2},
                        {3, 1, 2, 3, 6, 4},
                        {4, 1, 2, 1, 6, 3},
                        {4, 3, 4, 5, 1},
                        {5, 3, 5, 6, 2, 1}};
        
        for (int i = 0; i < array.length; i++) {
            System.out.print(i + 1);
            System.out.print(".\tGiven array: " + Arrays.toString(array[i]));
            System.out.print("\n\tCorrupt pair: ");
            System.out.print(findCorruptPair(array[i])[0]);
            System.out.print(", ");
            System.out.println(findCorruptPair(array[i])[1]);
            System.out.println(new String(new char[100]).replace('\0', '-'));
        }

    }

}
```



### Approach

**Using Bit Manipulation**

```java
public class Solution {
    public static int[] findMissingRepeatingNumbers(int []a) {
        // Write your code here
        int n = a.length; // size of the array
        int xr = 0;

        //Step 1: Find XOR of all elements:
        for (int i = 0; i < n; i++) {
            xr = xr ^ a[i];
            xr = xr ^ (i + 1);
        }

        //Step 2: Find the differentiating bit number:
        int number = (xr & ~(xr - 1));

        //Step 3: Group the numbers:
        int zero = 0;
        int one = 0;
        for (int i = 0; i < n; i++) {
            //part of 1 group:
            if ((a[i] & number) != 0) {
                one = one ^ a[i];
            }
            //part of 0 group:
            else {
                zero = zero ^ a[i];
            }
        }

        for (int i = 1; i <= n; i++) {
            //part of 1 group:
            if ((i & number) != 0) {
                one = one ^ i;
            }
            //part of 0 group:
            else {
                zero = zero ^ i;
            }
        }

        // Last step: Identify the numbers:
        int cnt = 0;
        for (int i = 0; i < n; i++) {
            if (a[i] == zero) cnt++;
        }

        if (cnt == 2) return new int[] {zero, one};
        return new int[] {one, zero};
    }
}
```


## Merge Sort Algorithm

![](https://33333.cdn.cke-cs.com/kSW7V9NHUXugvhoQeFaf/images/cedfb4d7c17a79820625a1923dae0466a36d7d21905edbe6.png)

_Lets consider an array **arr\[\] = {38, 27, 43, 10}**_

*   _Initially divide the array into two equal halves:_

![Merge Sort: Divide the array into two halves](https://media.geeksforgeeks.org/wp-content/uploads/20230530153635/img1drawio.png)

_Merge Sort: Divide the array into two halves_

*   _These subarrays are further divided into two halves. Now they become array of unit length that can no longer be divided and array of unit length are always sorted._

![Merge Sort: Divide the subarrays into two halves (unit length subarrays here)](https://media.geeksforgeeks.org/wp-content/uploads/20230530153654/img2drawio.png)

_Merge Sort: Divide the subarrays into two halves (unit length subarrays here)_

_These sorted subarrays are merged together, and we get bigger sorted subarrays._

![Merge Sort: Merge the unit length subarrys into sorted subarrays](https://media.geeksforgeeks.org/wp-content/uploads/20230530153714/img3drawio.png)

_Merge Sort: Merge the unit length subarrys into sorted subarrays_

_This merging process is continued until the sorted array is built from the smaller subarrays._

![Merge Sort: Merge the sorted subarrys to get the sorted array](https://media.geeksforgeeks.org/wp-content/uploads/20230530153747/img4drawio.png)

```java
import java.util.*;

class Solution {
    private static void merge(int[] arr, int low, int mid, int high) {
        ArrayList<Integer> temp = new ArrayList<>(); // temporary array
        int left = low;      // starting index of left half of arr
        int right = mid + 1;   // starting index of right half of arr

        //storing elements in the temporary array in a sorted manner//

        while (left <= mid && right <= high) {
            if (arr[left] <= arr[right]) {
                temp.add(arr[left]);
                left++;
            } else {
                temp.add(arr[right]);
                right++;
            }
        }

        // if elements on the left half are still left //

        while (left <= mid) {
            temp.add(arr[left]);
            left++;
        }

        //  if elements on the right half are still left //
        while (right <= high) {
            temp.add(arr[right]);
            right++;
        }

        // transfering all elements from temporary to arr //
        for (int i = low; i <= high; i++) {
            arr[i] = temp.get(i - low);
        }
    }

    public static void mergeSort(int[] arr, int low, int high) {
        if (low >= high) return;
        int mid = (low + high) / 2 ;
        mergeSort(arr, low, mid);  // left half
        mergeSort(arr, mid + 1, high); // right half
        merge(arr, low, mid, high);  // merging sorted halves
    }
}
public class tUf {
    public static void main(String args[]) {
        Scanner sc = new Scanner(System.in);
        int n = 7;
        int arr[] = { 9, 4, 7, 6, 3, 1, 5 };
        System.out.println("Before sorting array: ");
        for (int i = 0; i < n; i++) {
            System.out.print(arr[i] + " ");
        }
        System.out.println();
        Solution.mergeSort(arr, 0, n - 1);
        System.out.println("After sorting array: ");
        for (int i = 0; i < n; i++) {
            System.out.print(arr[i] + " ");
        }
        System.out.println();
    }

}
```
