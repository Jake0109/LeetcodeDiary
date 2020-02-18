# Leetcode No.40 Combination Sum II

## Description

Given a collection of candidate numbers (`candidates`) and a target number (`target`), find all unique combinations in `candidates` where the candidate numbers sums to `target`.

Each number in `candidates` may only be used **once** in the combination.

**Note:**

- All numbers (including `target`) will be positive integers.
- The solution set must not contain duplicate combinations.

**Example 1:**

```
Input: candidates = [10,1,2,7,6,1,5], target = 8,
A solution set is:
[
  [1, 7],
  [1, 2, 5],
  [2, 6],
  [1, 1, 6]
]
```

**Example 2:**

```
Input: candidates = [2,5,2,1,2], target = 5,
A solution set is:
[
  [1,2,2],
  [5]
]
```

## My Solution

今天这道题，准备试一试回溯算法：

```python
class Solution:
    def combinationSum2(self,candidates,target):
        res = []
        candidates.sort()

        def findcombination(candidates,temp_list=[]):  # trackback function
            for item in candidates:
                if item > target:
                    return
                pass

        return res
```

然后试着做完：

```python
class Solution:
    def combinationSum2(self, candidates, target):
        res = []
        candidates.sort()

        # backtrack function
        def findcombination(candidates, target, temp_list=[]):
            for i, item in enumerate(candidates):
                if item > target:
                    return
                temp_list.append(item)
                if item == target:
                    res.append(temp_list)
                    temp_list.pop()  # backtrack
                else:
                    findcombination(candidates[i + 1:], target - item, temp_list=temp_list)
                    temp_list.pop()  # backtrack

        findcombination(candidates,target)
        return res
```

然后没对...出现了一个很诡异的情况啊，就是输出的都是空列表就像这样`[[], [], [], [], [], []]`嗯，关于这个，我们Reflection板块再解释。然后就是忽略了一点啊，就是输出列表的互异性。

```python
class Solution:
    def combinationSum2(self, candidates, target):
        res = []
        candidates.sort()

        # backtrack function
        def findcombination(candidates, target, temp_list=[]):
            for i, item in enumerate(candidates):
                if item > target:
                    return
                temp_list.append(item)
                if item == target and temp_list not in res:
                    res.append(temp_list.copy())
                    temp_list.pop()  # backtrack
                else:
                    findcombination(candidates[i + 1:], target - item, temp_list=temp_list)
                    temp_list.pop()  # backtrack

        findcombination(candidates,target)
        return res
```

## Reflection & Polishing-up

首先我们看一下之前那个输出全是空列表的那个，假设我的输入是`candidates = [10,1,2,7,6,1,5], target = 8`那么我们在这个地方加一个输出看看结果是什么。

```python
# 原来的line 12 的位置
if item == target:
    res.append(temp_list)
    print(temp_list)
    temp_list.pop()  # backtrack

# output
[1, 1, 6]
[1, 2, 5]
[1, 7]
[1, 2, 5]
[1, 7]
[2, 6]
[[], [], [], [], [], []]
```

可以看到，明明在条件成立，也就是sum(temp_list)的值等于target的时候，输出是正确的~~虽然会有重复~~，但是为什么到最后输出的时候，就全是空列表呢。

首先是关于python的赋值机制，python的赋值的过程是，将当前的变量指向一个object，这个object可以是int，可以是list，可以是class，然后当赋值的时候，比如上面的`res.append(temp_list)`就是将res[appended_index]也指向temp_list指向的对象，在这里就是temp_list这个列表，假设它是[1,7]。

那么当赋值结束以后，执行`temp_list.pop()`这个操作不会改变temp_list的指针，所以res[appended_index]也不会特地记录这个内容，所以现在它的值就是[1]。再当下一次pop()结束后，它就只剩下空壳了。

但是用copy()就可以解决这个问题，那么关于copy相关的一些东西，我自己做了一个示例，来解释二者的一点区别。

```python
# code
candidates = [10, 1, 2, 7, 6, 1, 5]
copyC1 = candidates
copyC2 = candidates.copy()
print(copyC1 == candidates,copyC2 == candidates)
print(copyC1 is candidates,copyC2 is candidates)
candidates.append('append')
print(copyC1, copyC2)

# print(copyC1 == candidates,copyC2 == candidates) output
True True

# print(copyC1 is candidates,copyC2 is candidates) output
True False

# print(copyC1, copyC2) output
[10, 1, 2, 7, 6, 1, 5, 'append'] [10, 1, 2, 7, 6, 1, 5]
```

也就是说，copyC2和candidates并不指向同一个对象，只是当前对象内元素指向同一对象。

可能听上去比较绕口，但是就是这么一个事实，再进一步就需要深度拷贝了。那么我先把源码中的文档放在这，有兴趣的可以自己看一看。

```python
Interface summary:

        import copy

        x = copy.copy(y)        # make a shallow copy of y
        x = copy.deepcopy(y)    # make a deep copy of y

For module specific errors, copy.Error is raised.

The difference between shallow and deep copying is only relevant for
compound objects (objects that contain other objects, like lists or
class instances).

- A shallow copy constructs a new compound object and then (to the
  extent possible) inserts *the same objects* into it that the
  original contains.

- A deep copy constructs a new compound object and then, recursively,
  inserts *copies* into it of the objects found in the original.
```

## Conclusion

- 继续练习backtrack，争取做到：能使用backtrack提升效率的时候，能够自发想到，而不是看到别人有这种solution的时候才意识到。
- 学会自己看源码，开发者对于代码的态度和专业程度远超过我，所以他们为了不让我们迷惑做的努力，我们自己不能辜负这些人。

