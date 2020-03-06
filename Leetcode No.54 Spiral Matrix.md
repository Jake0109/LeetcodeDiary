# Leetcode No.54 Spiral Matrix

## Description

Given a matrix of *m* x *n* elements (*m* rows, *n* columns), return all elements of the matrix in spiral order.

**Example 1:**

```
Input:
[
 [ 1, 2, 3 ],
 [ 4, 5, 6 ],
 [ 7, 8, 9 ]
]
Output: [1,2,3,6,9,8,7,4,5]
```

**Example 2:**

```
Input:
[
  [1, 2, 3, 4],
  [5, 6, 7, 8],
  [9,10,11,12]
]
Output: [1,2,3,4,8,12,11,10,9,5,6,7]
```

## My Solution

```python
class Solution:
    def spiralOrder(self, matrix):
        res = []

        while matrix:
            res.extend(matrix.pop(0))
            if not matrix:
                break

            for line in matrix:
                res.append(line.pop())
            if not matrix[0]:
                break

            res.extend(reversed(matrix.pop()))
            if not matrix:
                break

            temp = []
            for line in matrix:
                temp.append(line.pop(0))
            res.extend(reversed(temp))
            if not matrix[0]:
                break

        return res
```

写是写出来了，但总感觉不怎么优雅...

## Reflection & Polishing-up

额，看了一下，大同小异，还有不少是枚举法做的，大多数人好像都不用pop()。而且用枚举法的更多...我就不太明白了...

```python
class Solution:
	def spiralOrder(self, matrix: List[List[int]]) -> List[int]:
		if not matrix:
			return []
		top,left=0,0
		right,buttom=len(matrix[0])-1,len(matrix)-1
		newMatrix=[]
		while top<=buttom and left<=right:
			#Step 1 :- From Top left to Top right
			for i in range(left,right+1):
				newMatrix.append(matrix[top][i])
			top+=1
           #Step 2 :-From right Top to right Buttom
			for i in range(top,buttom+1):
				newMatrix.append(matrix[i][right])
			right-=1
			
			#Step 3 :- from Buttom right To Buttom left
			if top<=buttom:
				for i in range(right,left-1,-1):
					newMatrix.append(matrix[buttom][i])
				buttom-=1
				
				#Step 4 :- from left Buttom to left Top
			if left<=right:
				for i in range(buttom,top-1,-1):
					newMatrix.append(matrix[i][left])
				left+=1
		return newMatrix
```

这个算是我看的比较好的了但是我仍然觉得，一个个append是低效的，给我我会改成这样：

```python
class Solution:
	def spiralOrder(self, matrix: List[List[int]]) -> List[int]:
		if not matrix:
			return []
		top,left=0,0
		right,buttom=len(matrix[0])-1,len(matrix)-1
		newMatrix=[]
		while top<=buttom and left<=right:
			#Step 1 :- From Top left to Top right
			newMatrix.extend(matrix[top][left:right+1])
			top+=1
            
           #Step 2 :-From right Top to right Buttom
			for i in range(top,buttom+1):
				newMatrix.append(matrix[i][right])
			right-=1
			
			#Step 3 :- from Buttom right To Buttom left
			if top<=buttom:
				newMatrix.extend(matrix[bottom][right:left-1:-1])
				buttom-=1
				
				#Step 4 :- from left Buttom to left Top
			if left<=right:
				for i in range(buttom,top-1,-1):
					newMatrix.append(matrix[i][left])
				left+=1
		return newMatrix
```

反正内置的extend和循环append时间复杂度是一样的，为什么不简洁一点呢。

## Conclusion

- 我记得两年前一个程序比赛里面有过类似的题目，当时我没做出来...现在算是比两年前强了吧。