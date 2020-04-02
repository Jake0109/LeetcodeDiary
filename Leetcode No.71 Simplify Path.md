# Leetcode No.71 Simplify Path

## Description

Given an **absolute path** for a file (Unix-style), simplify it. Or in other words, convert it to the **canonical path**.

In a UNIX-style file system, a period `.` refers to the current directory. Furthermore, a double period `..` moves the directory up a level.

Note that the returned canonical path must always begin with a slash `/`, and there must be only a single slash `/` between two directory names. The last directory name (if it exists) **must not** end with a trailing `/`. Also, the canonical path must be the **shortest** string representing the absolute path.

 

**Example 1:**

```
Input: "/home/"
Output: "/home"
Explanation: Note that there is no trailing slash after the last directory name.
```

**Example 2:**

```
Input: "/../"
Output: "/"
Explanation: Going one level up from the root directory is a no-op, as the root level is the highest level you can go.
```

**Example 3:**

```
Input: "/home//foo/"
Output: "/home/foo"
Explanation: In the canonical path, multiple consecutive slashes are replaced by a single one.
```

**Example 4:**

```
Input: "/a/./b/../../c/"
Output: "/c"
```

**Example 5:**

```
Input: "/a/../../b/../c//.//"
Output: "/c"
```

**Example 6:**

```
Input: "/a//b////c/d//././/.."
Output: "/a/b/c"
```

## My Solution

```python
class Solution:
    def simplifyPath(self, path: str) -> str:
        path = path.replace("//", "/")
		
        # remove additional "/"
        l = path.split("/")

        # for "." does not work, so remove them
        while "." in l:
            l.remove(".")
		
        # remove empty element
        while "" in l:
            l.remove("")
		
       	# return to father dir
        while ".." in l:
            index = l.index("..")
            if index > 0:
                l.pop(index-1)
                l.pop(index-1)
            else:
                l.pop(0)

        return "/"+"/".join(l)
```

自己首先想到的就是这个了。

## Reflection & Polishing-up

在评论区看到关键字"stack"那就自己想办法实现一下吧：

```python
class Solution:
    def simplifyPath(self, path: str) -> str:
        stack = []
        for item in path.split('/'):
            if item == "..":
                if stack:
                    stack.pop()
            elif item == "." or item == "":
                continue
            else:
                stack.append(item)
        return '/'+'/'.join(stack)
```

我想着如果用直接把‘/’放进去也挺糟糕的，就用split分离了一下。

然后这位就真的很厉害了

```python
class Solution:
    def simplifyPath(self, path: str) -> str:
        r = []
        for s in path.split('/'):
            r = {'':r, '.':r, '..':r[:-1]}.get(s, r + [s])
        return '/'+'/'.join(r)
```

开眼界了，原来dict是可以当switch使的。以后可以试试。

## Conclusion

- 有脚本风格的编程题，还挺中意的。
- 没能想到stack，从而大量使用了pop(0)这种很费时间的操作，有点傻。