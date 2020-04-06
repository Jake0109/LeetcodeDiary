# Leetcode No.74 Search a 2D Matrix

## Description

Write an efficient algorithm that searches for a value in an *m* x *n* matrix. This matrix has the following properties:

- Integers in each row are sorted from left to right.
- The first integer of each row is greater than the last integer of the previous row.

**Example 1:**

```
Input:
matrix = [
  [1,   3,  5,  7],
  [10, 11, 16, 20],
  [23, 30, 34, 50]
]
target = 3
Output: true
```

**Example 2:**

```
Input:
matrix = [
  [1,   3,  5,  7],
  [10, 11, 16, 20],
  [23, 30, 34, 50]
]
target = 13
Output: false
```

## My Solution

```python
class Solution:
    def searchMatrix(self, matrix: List[List[int]], target: int) -> bool:
        for line in matrix:
            if target in line:
                return True
        
        return False	
```

## Reflection & Polishing-up

```python
class Solution:
    def searchMatrix(self, matrix: List[List[int]], target: int) -> bool:
        if not matrix or not matrix[0]:
            return False

        height = len(matrix)
        length = len(matrix[0])

        end = height * length - 1
        start = 0
        while end >= start:
            mid = (end + start) // 2
            point = matrix[mid//length][mid%length]

            if point > target:
                end = mid - 1
            elif point < target:
                start = mid + 1
            else:
                return True

        return False

```

将数组降维后的二分法。

另一种分段的解法：

```python
def searchMatrix(self, matrix: List[List[int]], target: int) -> bool:
	if not matrix:
		return False
	row, col = 0, len(matrix[0]) - 1
	while row < len(matrix) and col >= 0:
		if matrix[row][col] == target:
			return True
		elif matrix[row][col] < target:
			row += 1
		else:
			col -= 1
	return False
```

我个人觉得挺好的。

别的基本都大同小异

## Conclusion

- 前天晚上发烧，昨天就直接没怎么好好做了。但是今天也突然感觉没什么可以做的，补了一个扁平化的二分法。就有点无奈。