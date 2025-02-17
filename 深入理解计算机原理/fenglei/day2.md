#### 第二章 信息的表示和处理

现代计算机储存和处理的信息是以二进制信号表示的，其中有三种重要的数字表示

* 无符号(unsigned)	基于传统的二进制表示法，表示大于或者等于零的数字
* 补码(two's-complement)   表示有符号的整数的最常见的方式，有符号数就是可以为正或为负的数字
* 浮点数(floating-point)  表示实数的科学记数法的以 2 为基数的版本

计算机用这些不同的表示方法实现算数运算

计算机的表示法是用有限的位来对一个数字编码，因此，当结果太大以至不能表示时，某些运算就会溢出(overflow)

整数的运算满足人们所熟知的真正整数运算的许多性质，例如 乘法结合律和乘法交换律

浮点数运算是不可结合的，例如在 C 语言中 (3.14 + 1e20) - 1e20 的值为 0 而 3.14 + (1e20 - 1e20) 的值为 3.14

整数运算和浮点运算有不同的数学属性是因为 整数的表示时精确的而浮点表示是近似的

* 信息的储存
  * 机器级程序将内存视为一个非常大的字节数组称为虚拟内存。
  * 内存的每个字节都由一个唯一的数字标识，称为地址
  * 所有的地址的集合称为虚拟地址空间
  * 十六进制表示法
    * 一个字节由 8 位组成，二进制表示法太冗长，替代方式是十六进制数（简写 "hex"）
    * 十六进制使用数字 0~9 以及字符 A~F 来表示 16 个可能的值，在 C语言中 0X 或 0x 开头的数字常量被认为是十六进制数，不区分大小写
    * 十六进制转二进制可以通过记住每个数字对应的值来进行转换 比如 0x173A4C 即 173A4C 每个数字展开成二进制在拼接即使二进制数
    * 二进制转十六进制可以把一个二进制数分为每4位一组，如果总数不是4的整数倍，最左边的一组可以少于4位，前面补0，然后将4位转换成十六进制在拼接

```
十六进制	  十进制		二进制
	 0				 0			0000
	 1				 1			0001
	 2				 2			0010
   3				 3			0011
...
   D			   13			1101
   E				 14		  1110
   F				 15			1111
```

* 字数据大小

  * 每台计算机都有一个字长(word size)，通常是32位或64位，代表指针数据的标称大小(nominal size)，字长决定了虚拟地址空间的最大值，例如一个字长为 w 位的机器，虚拟地址范围是 0 ~ 2^w - 1
  * 大多数 64 位机器向后兼容 32位机器，反之不行
  * C 语言中只有 char* 和 long 在 32位 和 64位的机器中不同
    *  char*  	32占 4 个字节	64占8个字节
    *  long        32占 4 个字节	64占8个字节

* 寻址和字节顺序

  * 对于跨越多字节程序的对象，必须建立两个规则：这个对象的地址是什么以及在内存中是如何排序的
  * 几乎在所有的机器上，多字节对象都被储存为连续的字节序列，对象的地址为所使字节中最小地址
  * 排列一个对象的字节有两个通用的规则
    * 大端法	最高有效字节在最前面
    * 小端法.   最低有效字节在最前面
    * android 和 IOS 都是小端模式
    * 网络传输中为了避免不同类型之间字节顺序不一样，必须遵守网络应用的字节顺序的规则，发送方内部转换成网络标准，接收方按照网络标准转换为需要的内部表示


* 表示字符串: C 语言中字符串被编码为一个以null(其值为0)字符结尾的字符数组，每个字符都由某个标准编码来表示，比如 ASCII 码
* 表示代码：不同机器之间二进制代码不兼容，因此二进制代码很少能在不同机器和操作系统之间移植
* 布尔代数：^ 异或  任何数和 0 异或  则结果为任何数本身，任何数和 1 异或，则结果为任何数取反
* 位级运算:  确定一个位级表达式最好的方法是将十六进制的参数扩展成二进制表示并执行二进制运算，然后在转回十六进制 | & ~ ^
* 逻辑运算符: || && ! ，和位级运算不同
* 移位运算: x 向左移 k 位，丢弃最高的 k 位，在右端补 0，x 向右移 k 为，丢弃最低的 k 位，在左端补 0，对于无符号数，右移必须是逻辑位移，C 语言中进行位移操作最好加上括号
* 整数表示: unsigned 表示非负整数， signed 表示负数，整数，零，两者取值范围不同
* 无符号数的编码：无符号数的二进制 0 ~ 2^w - 1之间的数都有唯一一个 w 位的值编码，例如 十进制值 11 只有一个 4 位 的表示 [1011]
* 补码编码:不太理解，应该是计算机按照补码编码的方式计算 int 型变量 b 的值， 补码主要用于表示有符号数，按位取反与补码编码无关，但是结果一样，TMax 有符号数的最大值，TMin 有符号数的最小值，UMax 无符号数的最大值
* 有符号数和无符号数的转换:如果取值在 0 <= x <= TMax，在位级的表示上没有变化(都一样)，如果在取值范围之外取值范围需要加上或者减去 2^w
* C 语言中的有符号数和无符号数，无符号数后缀有 U 或 u 比如 0x1A2Bu，如果没有后缀默认为有符号数，**当有符号数和无符号数运算时，会把有符号数强转无符号数，并假设两个数都是非负的,这样做对标准运算没有什么差异，但是对位移运算影响较大**
* 扩展一个数字的位表示
  * 要将一个无符号数转换为一个更大的数据类型，只需要在表示的开头添加 0 这种称为**零扩展**
  * 将一个补码数转换为更大的数据类型要执行**符合扩展** 规则是在表示位中添加最高有效值的值的副本
  * 一个数据大小到另一个数据大小的转换，以及一个有符号和无符号数字的转换顺序不一样，得到的结果也不一样

* 截断数字
  * 截断无符号数: 当一个 w 位的数截断为一个 k 位的数时，丢弃 w - k 个高位
  * 截断补码数值:  不太理解公式含义



####  小结

​		储存二进制表有三种方式(无符号，补码，浮点数)，储存的表示法为十六进制表示法，储存大小由字长(通常是 32 位 和 64 位)决定，多字节储存的方式通常是连续的内存地址，分为大端法和小端法两种，可以表示字符串，代码，布尔代数和整数，整数分为有符号数(非负数)和无符号数(负数，正数，零)，两者取值范围不一样，运算规则分为位级运算，逻辑运算，移位运算，有符号数和无符号数之间运算时，会把有符号数转成无符号数，并假设两个数都是非负数再进行运算，对位移运算影响较大，有符号数和无符号数可以进行相互的转换，如果要将数据类型不同的数据进行转换可以进行扩展和截取操作

##### 思考

​		补码编码，无符号数编码，无符号数和有符号数转换，具体运算方式不太理解(主要是公式的运算符看不懂)



