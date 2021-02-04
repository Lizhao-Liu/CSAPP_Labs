# Data Lab

## 说明

[Data Lab](http://csapp.cs.cmu.edu/3e/labs.html)

[README](http://csapp.cs.cmu.edu/3e/README-datalab)

[Instruction](http://csapp.cs.cmu.edu/3e/datalab.pdf)

[Recitation Video](https://www.youtube.com/watch?v=65-9NFv3Lws&list=PL4YtNpAhVHFJVlaD203_8JkUOUT8RYUhY&index=28)

## 结果
1. Use `./dlc bits.c` to check the legality of the code.
2. Compile the `bits.c` with the Makefile provided.
3. Use `./btest` to check the correctness of the functions and get the score with `./driver.pl`.

## Lab content

### bitXor
> 仅使用`~`和`&`运算符实现异或。
```c
/* 
 * bitXor - x^y using only ~ and & 
 *   Example: bitXor(4, 5) = 1
 *   Legal ops: ~ &
 *   Max ops: 14
 *   Rating: 1
 */
```
- solution
```c
int bitXor(int x, int y) 
{
  return (~(x&y) & ~((~x)&(~y)));
}
```

```c
int bitXor(int x, int y) 
{
  return ~(~(x & ~y) & ~(~x & y));
}
```

### tmin
> 返回最小的二进制补码。
```c
/* 
 * tmin - return minimum two's complement integer 
 *   Legal ops: ! ~ & ^ | + << >>
 *   Max ops: 4
 *   Rating: 1
 */
 ```
 
- solution
 ```c
 int tmin(void) {
  return (0x1 << 31); 
}
```

- Note: 
      `tMin == tMax+1`
      `tMin = Ox80000000 (32 bits)`
      

### isTmax
> 如果x是最大的二进制补码数返回1，否则返回0。
```c
/*
 * isTmax - returns 1 if x is the maximum, two's complement number,
 *     and 0 otherwise 
 *   Legal ops: ! ~ & ^ | +
 *   Max ops: 10
 *   Rating: 1
 */
 ```
 
- solution
 ```c
 int isTmax(int x) {
  return (!~((x+1)^(x)) & !(~x))
}
```
```c
/*补充istmin*/
int isTmin(int x) {
   return !(x+x)&!!(x);
}
```

- Note

    tmax等于`0x7fffffff`，加一之后会变成tmin`0x800000000`，tmin与tmax各位相反，所以可以用`（x+1)^x`再各位取反是否为零判断一个数是否为tmax.
    另一种方法是tMax加1后取反仍是tMax。我们可以通过`~(x + 1) ^ x`是否等于0来判断一个数是否是tMax. 但两种解法都存在一个特例，`0xffffffff`也满足
    两种条件，所以用`!(-x)`来排除特例。
    
    tmin等于`0x80000000`, 前面一部分用于判断左移一位后是否全部为0，后面一部分用来判断 x 值是否为 0.
  
    
### allOddBits
> 检查x二进制表示0-31位中是否奇数位全为1。
```c
/* 
 * allOddBits - return 1 if all odd-numbered bits in word set to 1
 *   where bits are numbered from 0 (least significant) to 31 (most significant)
 *   Examples allOddBits(0xFFFFFFFD) = 0, allOddBits(0xAAAAAAAA) = 1
 *   Legal ops: ! ~ & ^ | + << >>
 *   Max ops: 12
 *   Rating: 2
 */
 ```
 
- solution
 ```c
 int allOddBits(int x) {
  int t1 = (0xAA << 8) + 0xAA; // 0x0000AAAA
  int t2 = (t1 << 16) + t1;    // 0xAAAAAAAA
  return !((x & t2) ^ t2);
}
 ```
 
- Note 

    将`0xAAAAAAAA`这个奇数位全为1的`掩码`构出来与x进行先与后异或的比较。
      
    思考：不采用掩码可以使用移位，使用`!~(x>>1 | x)`的方法得到每隔一位存在1，但是无法得到确定奇数位还是偶数位。


### negate
> 返回-x。
```c
/* 
 * negate - return -x 
 *   Example: negate(1) = -1.
 *   Legal ops: ! ~ & ^ | + << >>
 *   Max ops: 5
 *   Rating: 2
 */
 ```
 
- solution
```c
int negate(int x) {
  return (~x) + 1;
}
```

- Note

    在二进制补码中，正数与负数的关系是：`正数按位取反 + 1 = 负数`


### isAsciiDigit
> 检查x是否是'0'字符与'9'字符之间的数。
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
 ```
 
 - solutions
 ```c
 int isAsciiDigit(int x) {
  int min=~0x30;
  int max=~0x39+1;
  return !((x+min)>>31) & ((x+max)>>31);
}

int isAsciiDigit(int x) {
  int flag1 = !!((0x2F + ~x + 1) >> 31);
  int flag2 = !((0x39 + ~x + 1) >> 31);
  return flag1 & flag2;
}

int isAsciiDigit(int x) {
  int sign = 0x1<<31;
  int upperBound = ~(sign|0x39);
  int lowerBound = ~0x30;
  upperBound = sign&(upperBound+x)>>31;
  lowerBound = sign&(lowerBound+1+x)>>31;
  return !(upperBound|lowerBound);
}
```

- Note

    `0x30 <= x <= 0x39`

> solution 2:

    分别得到`0x30 - x <= 0`和`0x39 - x >= 0`
    1. `(0x30 - 1) - x < 0` : `0x30 + (-x) = 0x30 + ~x + 1` 小于等于0， x 为integer
    2. `0x39 - x >= 0`
    3. 检查符号位即可

> solution 3:

    一个数是加上比0x39大的数后符号由正变负，另一个数是加上比0x30小的值时是负数。这两个数是代码中初始化的 `upperBound` 和 `lowerBound`，然后加法之后获取其符号位判断即可。

    所以我们便判断，x是否同时满足这两个条件即可，由于负数右移，实验要求的右移是`算术右移`(负数高位补1，非负数高位补0)，所以`(0x2F + ~x + 1) >> 31`后如果满足结果则是`0xffffffff`，我们使用     `!!`将结果转为逻辑值`1`，使用逻辑值和按位与来检查x是否同时满足2个条件更加方便。



### conditional
> 实现`x ? y : z`条件运算符。
```c
/* 
 * conditional - same as x ? y : z 
 *   Example: conditional(2,4,5) = 4
 *   Legal ops: ! ~ & ^ | + << >>
 *   Max ops: 16
 *   Rating: 3
 */
 ```
 
 - solution
 ```c
int conditional(int x, int y, int z) {
  x = !!x;
  x = ~x+1;
  return (x & y) | (~x & z);
}
```

- Note

> 错误：
    ```c
    int conditional(int x, int y, int z) {
      return (!!x & y) | (!x & z);
    }
    ```
> 原因：
    1 的表示并不是全1， 而是`00000001`， 这里掩码应该设置为全1，将x转为逻辑值，再取反加1构造成功。

> 分析：
    只需要根据x的值构造一个全1或者全0的掩码即可。


### isLessOrEqual
> 如果x <= y，返回1，否则返回0.
```c
/* 
 * isLessOrEqual - if x <= y  then return 1, else return 0 
 *   Example: isLessOrEqual(4,5) = 1.
 *   Legal ops: ! ~ & ^ | + << >>
 *   Max ops: 24
 *   Rating: 3
 */
 ```
 
 - solution
 ```c
int isLessOrEqual(int x, int y) {
  int xIsNeg = !!(x >> 31); // x是否为负数
  int yIsNeg = !!(y >> 31); // y是否为负数 
  int flag = xIsNeg ^ yIsNeg; // x和y是否异号
  x = ~x+1;
  return (flag & xIsNeg & !yIsNeg) | (!flag & !((y+x)>>31)); //第二项或用`x - 1 - y < 0 = x - 1 + ~y + 1 < 0 = x + ~y < 0`来判断，得到(!flag & !!((x + ~y) >> 31));
}
```

- Note

> 错误：
    ```c
     int isLessOrEqual(int x, int y) 
     {
      x = ~x+1;
      return !((y+x)>>31);
     }
    ```
> 原因：
    没有考虑异号溢出的情况。

> 分析：
`   x + ~y`会在x、y异号时可能出现溢出从而导致无法判断。而在x和y异号的时候，只有`x < 0, y >= 0`条件满足，并且无需检查x和y的绝对大小。

    1. 如果`x < 0 , y >= 0`，则返回1。
    2. 在1不满足的基础下，如果`y-x>=0`，则返回1，否则返回0。

    在x等于0的情况，可以在条件2中检测。


### logicalNeg
> 实现逻辑非，非1为0，0为1。

```c
/* 
 * logicalNeg - implement the ! operator, using all of 
 *              the legal operators except !
 *   Examples: logicalNeg(3) = 0, logicalNeg(0) = 1
 *   Legal ops: ~ & ^ | + << >>
 *   Max ops: 12
 *   Rating: 4 
 */
 ```
 
 - solution
 ```c
int logicalNeg(int x) {
  flag = (((~x+1)|x)>>31);
  return flag + 1;
}

int logicalNeg(int x) {
  int minusX = ~x + 1;
  return ((x | minusX) >> 31) + 1;
}
```

- Note

> solution 1:
    一个数取反符号不同，除了tmin和0，0或非0的符号位仍未0，而tmin符号或后为1.

> solution 2:
    也就是要构造出一个特征区分是否为0。一个数或自己的相反数的结果是-1， 这个数非0.


### howManyBits
 > 求一个数最少需要多少位二进制表示
```c
/* howManyBits - return the minimum number of bits required to represent x in
 *             two's complement
 *  Examples: howManyBits(12) = 5
 *            howManyBits(298) = 10
 *            howManyBits(-5) = 4
 *            howManyBits(0)  = 1
 *            howManyBits(-1) = 1
 *            howManyBits(0x80000000) = 32
 *  Legal ops: ! ~ & ^ | + << >>
 *  Max ops: 90
 *  Rating: 4
 */
 ```
 
 - solution
 ```c
 int howManyBits(int x) {
  int b16, b8, b4, b2, b1, b0, sign = x >> 31;
  x = (sign & ~x) | (~sign & x); /*利用符号位，正数不变，负数取反，位数取决于最高位的1.*/
  b16 = !!(x >> 16) << 4; /*高16位是否含有1*/  /*确定下次二分的目的范围*/
  x = x >> b16;
  b8 = !!(x >> 8) << 3; /*剩余高8位1的位置*2的3次方*/
  x = x >> b8;
  b4 = !!(x >> 4) << 2;
  x = x >> b4;
  b2 = !!(x >> 2) << 1;
  x = x >> b2; 
  b1 = !!(x >> 1);
  x = x >> b1;
  b0 = x;
  return b16 + b8 + b4 + b2 + b1 + b0 + 1;/*计算位置 + 符号位*/
}
```

- Note

   正数的最少表示位数，应该是最高的1位加上符号位0。
   负数的最少表示位数，应该是最高的0位加上符号位1。
   而正数的最少表示位数与负数的最少表示位数是一样的，例如3 = 011，-3 = 101。我们只需要求正数的最高位的1所处的位置再加上符号位即可。用分治法，每次划分一半区间。
   先在高16位找，如果有1，则把x右移动16位；
   接着高8位找，如果有1，则把x右移动8位；
   ...
   即可把表示正数的位数算出来，再加上符号位即可。

- 记忆点

1. 如何用在位操作条件下表示出decimal位数。
2. 使用符号位来区别整合数。
3. 分治法的使用广泛性：在搜索相关的问题中都可以应用二分法，要掌握这样的思维方式学会应用。



### floatScale2
> 参数和返回结果以无符号int类型表示float浮点数，返回2*f。
```c
/* 
 * floatScale2 - Return bit-level equivalent of expression 2*f for
 *   floating point argument f.
 *   Both the argument and result are passed as unsigned int's, but
 *   they are to be interpreted as the bit-level representation of
 *   single-precision floating point values.
 *   When argument is NaN, return argument
 *   Legal ops: Any integer/unsigned operations incl. ||, &&. also if, while
 *   Max ops: 30
 *   Rating: 4
 */
 ```
 
 - solution
 ```c
 unsigned floatScale2(unsigned uf) {
  int exp = (uf & 0x7f800000) >> 23;
  int sign = uf & (1 << 31);
  if (exp == 0) { 
    return uf << 1 | sign;
  }
  if (exp == 255) {
    return uf;
  }
  exp++;
  if (exp == 255) {
    return 0x7f800000 | sign;
  }
  return (exp << 23) | (uf & 0x807fffff);
}
```

- Note

  float32位的表示方法: `Sign - 1 bit`，`exp - 8 bits`，`frac - 23 bits`。

  根据exp的编码分为3种不同的情况：

  1. 规范化：当exp不全为0，且不全为1时，也就是(exp != 0 && exp != 255)，阶码字段被解释为以偏置形式表示的有符号整数，阶码`E = e - Bias`，e为有符号数为e<sub>7</sub>e<sub>6</sub>e<sub>5</sub>e<sub>4</sub>e<sub>3</sub>e<sub>2</sub>e<sub>1</sub>e<sub>0</sub>，Bias为2<sup>7</sup>-1，float单精度的Bias为127。小数字段frac，`0 <= f < 1`，尾数定义为`M = f + 1`，使得`1 <= M < 2`。
  2. 非规范化：exp全为0，表示的数非常接近于0
  3. 特殊值：exp全为1，小数域全为0，得到无穷大或者无穷小；exp全为1，小数域非0，NaN(Not a Number)。

  所以我们需要根据exp的情况来判断返回值：

  1. 如果exp为0，表示的数非常接近于0。将uf保留符号位左移一位即可得到结果2*uf。
  2. 如果exp为255，根据题目要求直接返回uf。
  3. 如果exp不为255且不为0，那么此时的uf为标准化的形式。由于浮点数的表示是V = (-1)<sup>s</sup> * M * 2<sup>E</sup>，将`uf * 2`，即可以把`E -> E + 1`，将`exp++`即可。此时如果exp为255，返回无穷值；否则返回原数加上改变的exp。


### floatFloat2Int
> 将float转为int。
```c
/* 
 * floatFloat2Int - Return bit-level equivalent of expression (int) f
 *   for floating point argument f.
 *   Argument is passed as unsigned int, but
 *   it is to be interpreted as the bit-level representation of a
 *   single-precision floating point value.
 *   Anything out of range (including NaN and infinity) should return
 *   0x80000000u.
 *   Legal ops: Any integer/unsigned operations incl. ||, &&. also if, while
 *   Max ops: 30
 *   Rating: 4
 */
 ```
 
 - solution
```c
int floatFloat2Int(unsigned uf) {
  int s = uf >> 31;
  int exp = (uf & 0x7f800000) >> 23;
  int Bias = 127;
  int E = exp - Bias;
  int frac = uf & 0x007fffff;
  int M = frac | 0x00800000; // M = frac + 1;
  if (!(uf & 0x7fffffff)) {
    return 0;
  }
  if (E > 31) {
    return 0x80000000u;
  }
  if (E < 0) {
    return 0;
  }
  if (E > 23) {
    M = M << (E - 23);
  } else {
    M = M >> (23 - E);
  }
  if (!(M >> 31) ^ s) {
    return M;
  } else if (M >> 31) {
    return 0x80000000u;
  } else {
    return ~M + 1;
  }
}
```

- Note

  将浮点数中的符号、exp、frac、尾数、E计算出来，
  尾数`M = 1 + frac`
  如果E阶码大于31会溢出，小于0得到0。
  如果E大于23，将尾数M左移；
  如果E小于23，将尾数M右移动。
  最后检查M的符号来判断是否溢出。


### floatPower2
> 计算2.0的x次方。
```c
/* 
 * floatPower2 - Return bit-level equivalent of the expression 2.0^x
 *   (2.0 raised to the power x) for any 32-bit integer x.
 *
 *   The unsigned value that is returned should have the identical bit
 *   representation as the single-precision floating-point number 2.0^x.
 *   If the result is too small to be represented as a denorm, return
 *   0. If too large, return +INF.
 * 
 *   Legal ops: Any integer/unsigned operations incl. ||, &&. Also if, while 
 *   Max ops: 30 
 *   Rating: 4
 */
```

- solution
```c
unsigned floatPower2(int x) {
    int INF = 0xff << 23;
    int exp = x + 127;
    if (exp >= 255) {
      return INF;
    } else if (exp <= 0) {
      return 0;
    } else {
      return exp << 23;
    }
}

 unsigned floatPower2(int x) {
  int M, exp;
  if(x < -126) return 0;
  if(x>127) return 0x7f800000;
  if(x>=0 && x <= 127){
    M = (2<<x - 1)>>9;
    exp = (x + 127) << 23;
  }
  else if(x < 0 && x > -1){
    x = ~x+1;
    M = (2>>x - 1) >>9;
    exp = (x + 127)<<23;
  }
  else if(x<-1 && x>=-126){
    x = ~x+1;
    M = (2>>x) >> 9;
    exp = 0xff800000;
  }
  return exp|M;
}
```

- Note

   根据V = (-1)<sup>s</sup> * M * 2<sup>E</sup>次方，我们已有参数x即E,根据`E = exp - Bias`可以得到exp，如果exp>=255或exp<=0皆为非规范化情况。
