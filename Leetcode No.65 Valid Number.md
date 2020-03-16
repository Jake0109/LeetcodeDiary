# Leetcode No.65 Valid Number

## Description

Validate if a given string can be interpreted as a decimal number.

Some examples:
`"0"` => `true`
`" 0.1 "` => `true`
`"abc"` => `false`
`"1 a"` => `false`
`"2e10"` => `true`
`" -90e3  "` => `true`
`" 1e"` => `false`
`"e3"` => `false`
`" 6e-1"` => `true`
`" 99e2.5 "` => `false`
`"53.5e93"` => `true`
`" --6 "` => `false`
`"-+3"` => `false`
`"95a54e53"` => `false`

**Note:** It is intended for the problem statement to be ambiguous. You should gather all requirements up front before implementing one. However, here is a list of characters that can be in a valid decimal number:

- Numbers 0-9
- Exponent - "e"
- Positive/negative sign - "+"/"-"
- Decimal point - "."

Of course, the context of these characters also matters in the input.

**Update (2015-02-10):**
The signature of the `C++` function had been updated. If you still see your function signature accepts a `const char *` argument, please click the reload button to reset your code definition.

## My Solution

None

我没做出来。别的地方我不太晓得，但是在江苏生活了20多年，我真的没有怎么接触到用e来做科学计数的，所以概念真的不明确，自然也会出现很多我个人理解不了的情况，所以这题我是真的做不出来，可能不是因为技术原因。

## Reflection & Polishing-up

做都没做出来，自然也不存在什么反思和提高了。看看人家怎么做的吧。

```python
def isNumber(self, s: str) -> bool:
        return re.match("^((([-+]?\d+)(\.\d+)?)|([-+]?\.\d+)|([-+]?\d+\.))(e[-+]?\d+)?$", s.strip()) is not None
```

从结构上，比我写的简单，但是我是被这么一个数据卡住的：

Input			`"46.e3"`

Output		`false`

Expected	`true`

我不太理解这个数是多少呢，e不是用来表示：前面的数*10的多少次方的吗，放在小数点后面又是啥？

## Conclusion

- 又一个没写出来的题目。
- 想起前段时间特别流行的一句话叫：我们永远无法赚到超过自身认知的钱。或者说类似的意思。这个东西就超过了当前我的认知，所以我写不出来，可能不是我思维能力不够，而是我认知跟不上。