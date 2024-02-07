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
