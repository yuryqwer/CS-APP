# CS:APP Data Lab 

## 学生须知
### 第1步：仔细阅读以下说明。
通过编辑这个源文件中的函数来完成作业。

**整数编码规则**

  重写每个函数中的“return”语句，使用一行或多行C代码。你的代码必须符合以下风格：
```
  int Funct(arg1, arg2, ...) {
      /* 简要说明你的实现 */
      int var1 = Expr1;
      ...
      int varM = ExprM;

      varJ = ExprJ;
      ...
      varN = ExprN;
      return ExprR;
  }
```
  每个表达式只能使用以下内容：
  1. <span id="integerconstants">从0到255（0xFF）的整数常量，包括255。你不能使用大整数，比如0xffffffff。</span>
  2. 函数参数和局部变量（不能用全局变量）。
  3. 一元整数操作 !（逻辑非） ~（按位取反）
  4. 二元整数操作 &（按位与） ^（按位异或） |（按位或） +（加） <<（左移） >>（右移）

  某些问题对于允许使用的操作符会更加严格。每个表达式可以由多个操作符组成。
  并不是一行只能使用一个操作符。

  明确禁止：
  1. 使用任何控制结构，例如if、do、while、for、switch等等。
  2. 定义或者使用任何宏。
  3. 在本文件中定义任何额外的函数。
  4. 调用任何函数。
  5. 使用任何其他的操作符，例如 &&、||、-、以及?:
  6. 使用任何形式的强制类型转换。
  7. 使用除了int以外的数据类型。 这意味着你不能使用数组、结构体、共用体。


  你可以假设你的电脑：
  1. 使用二进制补码，用32位（4字节）来代表整数。
  2. 使用算术右移。
  3. 当移位的数量小于0或者大于31时的行为不可预测。


可接受的编码风格：
```
  /*
   * pow2plus1 - returns 2^x + 1, where 0 <= x <= 31
   */
  int pow2plus1(int x) {
     /* 利用移位来计算2的次幂 */
     return (1 << x) + 1;
  }

  /*
   * pow2plus4 - returns 2^x + 4, where 0 <= x <= 31
   */
  int pow2plus4(int x) {
     /* 利用移位来计算2的次幂 */
     int result = (1 << x);
     result += 4;
     return result;
  }
```
**浮点数编码规则**

对于需要你实现浮点操作的问题，编码规则没那么严格。你可以使用循环和条件控制。可以使用
int类型和unsigned类型。可以使用任意的整数和无符号数常量。可以对int类型或者unsigned类型
使用任何算术、逻辑或者比较操作。

明确禁止：
  1. 定义或者使用任何宏。
  2. 在本文件中定义任何额外的函数。
  3. 调用任何函数。
  4. 使用任何形式的强制类型转换。
  5. 使用除了int和unsigned以外的数据类型。 这意味着你不能使用数组、结构体、共用体。
  6. 使用任何浮点数据类型、操作或者常量。


提示：
  1. 使用dlc（data lab checker）编译器（在讲义中提到过）来检查提交是否合法。
  2. 每个函数允许使用的操作符（数字，逻辑或者比较）有上限。对使用的操作符计数的工作
     由dlc来完成。注意赋值（'='）不会被计数；你想用多少就用多少。
  3. 使用btest测试工具来检查你写的函数的正确性。
  4. 使用BDD检查器来验证你写的函数（译注：BBD检查器是给老师使用的，学生用btest即可）
  5. 每个函数允许使用的最大操作符数量在上方的注释中给出。如果作业和本文件中规定的
     最大操作符数量有任何不一致，以本文件为准。


### 第2步：根据编码规则修改以下函数。

  重要。为了得到更好的成绩：
  1. 使用dlc编译器来检查你的提交符合编码规则。
  2. 使用BDD检查器来验证你提交了正确的答案。（译注：BBD检查器是给老师使用的，
     学生用btest来验证即可）


## 题目列表
|名称|描述|难度|最大操作符|
|----|----|----|----|
|int bitXor(int x, int y)|只使用`~`和`&`实现`^`|1|14|
|int tmin(void)|返回最小的补码|1|4|
|int isTmax(int x)|判断是否最大的补码|1|10|
|int allOddBits(int x)|判断补码的所有奇数位是否都是1|2|12|
|int negate(int x)|不使用负号`-`实现`-x`|2|5|
|int isAsciiDigit(int x)|判断是否是ASCII码0到9|3|15|
|int conditional(int x, int y, int z)|实现三元条件运算|3|16|

## 题解
### int bitXor(int x, int y)
> 描述：只使用按位非、按位与实现异或
> 
> 示例：bitXor(4, 5) = 1
>
> 可用操作符：~ &
>
> 最大操作符：14
>
> 难度：1
 * 思路

关于异或的基本知识是`x ^ y = (~x & y) | (~y & x)`，这边唯一不能使用的是按位或`|`，那么我们想办法用按位非、按位与来实现按位或即可：`x | y = ~((~x) & (~y))`，然后综合两个式子，即可得到最终结果。
 * 代码
```c
/* 
 * bitXor - x^y using only ~ and & 
 *   Example: bitXor(4, 5) = 1
 *   Legal ops: ~ &
 *   Max ops: 14
 *   Rating: 1
 */
int bitXor(int x, int y) {
    return ~((~((~x) & y)) & (~((~y) & x)));
}
```
### int tmin(void)
> 描述：返回最小的补码
>
> 可用操作符：! ~ & ^ | + << >>
>
> 最大操作符：4
>
> 难度：1
 * 思路

这边的操作符明显比第一题多了很多，但是限制了最多能使用的操作符只有四个。对于int类型，我们知道最小的补码的十六进制形式是`0x80000000`，但是能使用的[整数范围](#integerconstants)只有0到255，于是我们只能想办法构造超出范围的整数，这边可以用左移来得到上面的十六进制形式。
 * 代码
```c
/* 
 * tmin - return minimum two's complement integer 
 *   Legal ops: ! ~ & ^ | + << >>
 *   Max ops: 4
 *   Rating: 1
 */
int tmin(void)
{
    int a = 1;
    a = a << 31;
    return a;
}
```
### int isTmax(int x)
> 描述：如果x是最大的补码返回1，否则返回0
>
> 可用操作符：! ~ & ^ | +
>
> 最大操作符：10
>
> 难度：1
 * 思路

最大的补码是`0x7fffffff`，碰到判断是某个值的时候一般可以用异或这个值来解决，但是这边不允许使用移位运算来从某个小一点的值来构造大值。那么只能从x本身出发。可以发现，假如x是最大的补码，那么它加1等于最小的补码，然后用最大的补码加上最小的补码正好能得到`0xffffffff`的形式，它的每一位都是1，只有对这样的形式进行按位反运算才能得到0，然后用逻辑非将0转为1，将所有不是0的值都转为0。最后，再根据实际需要考虑是否要在上一步的结果上再加一个逻辑非运算即可。
 * 代码
```c
/*
 * isTmax - returns 1 if x is the maximum, two's complement number,
 *     and 0 otherwise 
 *   Legal ops: ! ~ & ^ | +
 *   Max ops: 10
 *   Rating: 1
 */
int isTmax(int x) {
    int tmin = x + 1;
    int negOne = tmin + x;
    return !(~negOne);
}
```
### int allOddBits(int x)
> 描述：判断补码的所有奇数位是否都是1，位的编号从0到31
>
> 示例：allOddBits(0xFFFFFFFD) = 0, allOddBits(0xAAAAAAAA) = 1
> 
> 可用操作符：! ~ & ^ | + << >>
>
> 最大操作符：12
>
> 难度：2
 * 思路

这边提供了移位运算，我们可以构造一个只有所有奇数位都是1，所有偶数位都是0的补码，然后让x经过转换变成这样的补码，这样做一次异或运算就可以判断结果。能够使用的最大整数是一个字节，而我们想要构造的补码形式为`0xAAAAAAAA`，所以要想办法从`0xAA`这样的数出发去构造成`0xAAAAAAAA`。
 * 代码
```c
/* 
 * allOddBits - return 1 if all odd-numbered bits in word set to 1
 *   where bits are numbered from 0 (least significant) to 31 (most significant)
 *   Examples allOddBits(0xFFFFFFFD) = 0, allOddBits(0xAAAAAAAA) = 1
 *   Legal ops: ! ~ & ^ | + << >>
 *   Max ops: 12
 *   Rating: 2
 */
int allOddBits(int x) {
    int a = 0xAA;
    int b = (a << 8) | a;
    int c = (b << 16) | b;
    return !((x & c) ^ c);
}
```
### int negate(int x)
> 描述：不使用负号`-`实现`-x`
>
> 示例：negate(1) = -1
> 
> 可用操作符：! ~ & ^ | + << >>
>
> 最大操作符：5
>
> 难度：2
 * 思路

可以先试试按位取反，对于一个正数来说，取反后的值正好是除了符号位以外的不为1的位的和减去`pow(2, w-1)`的结果（这边用伪代码形式表示2的w-1次幂），也就是`~x + x = 0x7fffffff - pow(2, w-1) = pow(2, w-1) - 1 - pow(2, w-1) = -1`，这样取反后再+1得到的就是-x。对于一个负数来说，取反后的值和这个负数不是符号位加起来的和正好是0x7fffffff的形式，也就是`~x + x = 0x7fffffff - pow(2, w-1)`，和正数的例子完全一样。
 * 代码
```c
/* 
 * negate - return -x 
 *   Example: negate(1) = -1.
 *   Legal ops: ! ~ & ^ | + << >>
 *   Max ops: 5
 *   Rating: 2
 */
int negate(int x) {
    return ~x + 1;
}
```
### int isAsciiDigit(int x)
> 描述：如果0x30 <= x <= 0x39，也就是在ASCII码的0到9之间，则返回1
>
> 示例：isAsciiDigit(0x35) = 1.
> isAsciiDigit(0x3a) = 0.
> isAsciiDigit(0x05) = 0.
> 
> 可用操作符：! ~ & ^ | + << >>
>
> 最大操作符：15
>
> 难度：3
 * 思路

要判断的东西从一个值变成区间以后，感觉难度一下子上去了。一个想法是构造两个边界，属于这两个边界内的值加上边界以后得到的是一种情况，不属于边界的值加上边界以后得到的是另一种情况。假设属于边界内的值加上边界得到0，边界外的值加上边界得到1（可以是任何非0的值），那么最终可以通过将两个边界的结果进行按位与再配合逻辑非来得到。

对于加法来说，一种情况变为另一种情况最有可能发生在符号位的进位上。属于边界内的值加上边界以后是否发生进位可以用`0x80000000 & res`来计算，这两个边界可以尝试用按位反来对`0x30`和`0x39`进行某些操作得到。

下面详细讨论一下两个边界各自的情况。首先考虑一个超过`0x39`的数，加上上边界以后，发生正数溢出，使得符号位变为1的过程。此时上边界应该是类似`0x7FFFFFC6`的形式。再考虑一个小于`0x30`的数，加上下边界以后，不足以发生溢出，此时下边界应该是类似`0x7FFFFFCF`的形式。注意，下边界的不溢出和上边界的溢出应当返回一致的结果。
 * 代码
```c
/* 
 * isAsciiDigit - return 1 if 0x30 <= x <= 0x39 (ASCII codes for characters '0' to '9')
 *   Example: isAsciiDigit(0x35) = 1.
 *            isAsciiDigit(0x3a) = 0.
 *            isAsciiDigit(0x05) = 0.
 *   Legal ops: ! ~ & ^ | + << >>
 *   Max ops: 15
 *   Rating: 3
 */
int isAsciiDigit(int x) {
    int tmin = 1 << 31;
    int upperBound = (~0x39) ^ tmin;
    int lowerBound = (~0x30) ^ tmin;
    int addUpper = (upperBound + x) >> 31;
    int addLower = ~((lowerBound + x + 1) >> 31);
    return !(addUpper | addLower);
}
```
### int conditional(int x, int y, int z)
> 描述：实现x ? y : z ，也就是x为真则返回y，否则返回z
>
> 示例：conditional(2,4,5) = 4
> 
> 可用操作符：! ~ & ^ | + << >>
>
> 最大操作符：16
>
> 难度：3
 * 思路

考虑从x出发构造全为0或者全为1的形式，这样可以通过分别将x与y，~x与z进行按位与，再把结果按位或得到y或者z。当x全为0时，(x & y) | (~x & z) = 0 | z = z；当x全为1时，(x & y) | (~x & z) = y | 0 = y。

所以结果变成了当x为0时让它变为全0，当x不为0时让它变为全1的形式。用逻辑非运算可以把x变为0和1这两种特殊情况，分别取反后得到0xFFFFFFFF和0xFFFFFFFE，再加1后得到0x00000000和0xFFFFFFFF，可以转化成我们想要的形式。
 * 代码
```c
/* 
 * conditional - same as x ? y : z 
 *   Example: conditional(2,4,5) = 4
 *   Legal ops: ! ~ & ^ | + << >>
 *   Max ops: 16
 *   Rating: 3
 */
int conditional(int x, int y, int z) {
    x = ~(!x) + 1;
    return (x & z) | (~x & y);
}
```