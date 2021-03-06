
## ASCII码 随想

* 空间是 0 ~ 127, size 128
* 常规字符的范围 32 - 126, 以空格开始, ~ 结尾
* 特殊字符分布在 0 ~ 31, 结尾127还有一个\DEL

* 大写字母在小写字母前面, 两者相差并不是 26 (字母数量), 而是 26 + 6 = 32,
  刚好0010 0000, 判断 第3高位就可以识别是大写还是小写

``` haskell
# ghci
> :m+ Text.Printf
> f i = let x = fromEnum i in printf "%c: (%d, 0x%02x, %08b)\n" x x x x
> f 'z'
z: (122, 0x7a, 01111010)
```

(好久没碰曾经深爱的Haskell,都不知道什么时候开始可以在ghci中直接定义函数了...)

* A: (65, 0x41, 0100 0001)
* Z: (90, 0x5a, 0101 1010)
* a: (97, 0x61, 0110 0001) 
* 0: (48, 0x30, 0011 0000)
* 9: (57, 0x39, 0011 1001)
* [空格]: (32, 0x20, 0010 0000)

* 范围是0~127, 空间size 128, 32 - 127区间size 96, 字母占用了26 * 2 = 52,
  数字0-9 占用10个空间, 其他符号就只有 96 - 52 - 10 = 34

* print them in REPL with:
  * Lua: `for i = 32, 127 do io.stdout:write(string.char(i)) end io.stdout:write '\n'`
  * Haskell: `map toEnum [32 .. 127] :: String`

## 算术

* 整数的除法结果需要取整，取整方向可以采用向 正无穷大 0 或 负无穷大，不同的方式
也就定义了不同的运算结果。
Haskell中的整数除法定义了两种取整方向： 0 和 负无穷大

-5 / 3 -> -1.6666666666666667

向下(负无穷大)取整:

* -5 `divMod` 3 -> -2, 1
* -4 `divMod` 3 -> -2, 2
* -3 `divMod` 3 -> -1, 0

向上(0)取整:

* -5 `quotRem` 3 -> -1, -2
* -4 `quotRem` 3 -> -1, -1
* -3 `quotRem` 3 -> -1, 0

很明显，mod运算 除数为正时，不管被除数是否为正，余数都是正，这个性质在程序中很有用。

另外要注意，不同的语言中默认的整数除法定义的可能不同，很多语言只提供了一种方式。

C:

-5 % 3 = -2

Python:

-5 % 3 = 1

* 补码 0000 FFFF(-1)
* 移码 (Lua中,iAsBx格式的指令中,使用了移码来表示有符号整数)


