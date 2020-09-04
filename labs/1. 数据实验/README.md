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

可以先试试按位取反，对于一个正数来说，取反后的值正好是不为1的位的和减去$2^{w-1}$的结果，也就是~x + x = 0x7fffffff - $2^{w-1}$ = $2^{w-1}$ - 1 - $2^{w-1}$ = -1，这样取反后再+1得到的就是-x。对于一个负数来说可以用同样的思路去计算。
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