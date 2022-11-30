## Transformation between matrix & 1d array

- mapping a ```m * n``` matrix to a ```r * c``` matrix (assume ```m * n == r * c```)
- assume matrix is stored & accessed in **row-major order**
- idea
  - first convert the 2d coordinate (i, j) in m * n matrix to 1d index ```(n * i + j)```
    - ```n * i``` gives us the start of the row in which the element exist; this only depends on the ***no. of elements in a row in the source matrix (i.e., n)***
    - ```+ j``` gives us the offset from the start of that row
  - then map the 1d index to the new 2d coordinate in r * c matrix; this onlly depends on the ***no. of elements in a row in the target matrix (i.e., c)***
    - ```idx / c``` gives us the index of the row in which the element exist
    - ```idx % c``` gives us the remainder of the division of the number of elements in a row, which is the offset from the start of a row which is the index of the column in which the element exists
```
public int[][] matrixReshape(int[][] mat, int r, int c) {

        int m = mat.length, n = mat[0].length;
        int[][] res = new int[r][c];
        
        int idx, a, b;
        for (int i = 0; i < m; i++) {
            for (int j = 0; j < n; j++) {
                idx = n * i + j; //index in 1d array
                a = idx / c; //row index in r * c matrix
                b = idx % c; //column index in r * c matrix
                res[a][b] = mat[i][j];
            }
        }
        
        return res;
    }
}
```

## Example questions
- [Search in 2D matrix](https://leetcode.com/problems/search-a-2d-matrix/?envType=study-plan&id=data-structure-i)
- [Reshape the matrix](https://leetcode.com/problems/reshape-the-matrix/?envType=study-plan&id=data-structure-i)
