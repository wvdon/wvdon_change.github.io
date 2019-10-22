---
title: 'JAVA数据结构与算法:数学问题'
date: 2019-08-02 15:50:39
tags: 
  - java
  - 数据结构
  - 数学问题
categories: [JAVA数据结构与算法]
mathjax: true
description: JAVA数据结构与算法中有关于数学的问题
---

## <span id="math">数学问题</span>

写在前面

自然数：非负整数，0,1,2,3,4......

合数：存在a(1<a<n),使得n % a ==0,称n为合数

素数(质数）：对于任意a(1<a<n),都有n % a !=0,称n为素数。

​	质数是指在大于1的自然数中，除了1和它本身以外不再有其他因数的自然数。

约数与倍数：

整数a能够被整数b整除，那么a就是b的倍数，b就是a的约数。

```java
int a,b;
b/a==0约数又称为因数
```



### 5.1 最大公约数和最小公倍数

#### 5.1.1 最大公约数

Q1:什么是最大公约数

> 最大公约数，又称最大公因数，指两个整数相同的最大约数。

```java
int a=12,b=8;
gcd(12,8)
//gcd(12,8)=4
```

Q2:有什么用呢？

> 欧几里德算法可用于RSA加密等领域。

Q3:如何去求最大公约数

```java
int gcd(int a,int b){
 // a>=b
 swap(a，b);
 if(b==0){
     return a;
 }
 return gcd(b,a%b);
}
void swap(a,b){
 int temp;
 if(a<b){
     a=temp;
     a=b;
     b=temp;
 }
}
```



> 证明：
>
> > 既证明 gcd(a,b) = gcd(b,a mod b) 的过程
> >
> > 其计算原理依赖于下面的定理：
> > *定理：两个整数的最大公约数等于其中较小的那个数和两数相除余数的最大公约数。最大公约数（Greatest Common Divisor）缩写为GCD。
> > gcd(a,b) = gcd(b,a mod b) (不妨设a>b 且r=a mod b ,r不为0)*
> >
> > a可以表示成a = kb + r（a，b，k，r皆为正整数，且r<b），则r = a % b
> > 假设d是a,b的一个公约数，
> > 而r = a - kb，两边同时除以d，
> >
> >  $$ \frac{r}{d}=\frac{a}{d}-\frac{kb}{d}=m $$
> >
> > 由等式右边可知m为整数，因此d是r的约数
> > 因此d也是b,a % b的公约数
> > 进而d|a.因此d也是a,b的公约数，由于d的任意性，得a和b的公约数都是a%b的公约数
> > 由a = kb + r,同理可证 b 和 a % b的公约数都是 a和b的公约数。
> > 因此(a,b)和(b,a mod b)的公约数是一样的，其最大公约数也必然相等，
> > 得证。

Q4:还有其他方法吗？

> 目前求 最大公约数有 质因数分解法、短除法、辗转相除法、更相减损法
>
> 其中最常用的为辗转相除法（又名欧几里得算法）
>
> 有精力的话可以了解一下扩展欧几里得算法



#### 5.1.2 最小公倍数

最小公倍数建立在最大公约数的基础上，即除了a,b的最大公约数外a与b的成绩除以最大公约数，
表示为  lcm(a,b)
$$
最小公倍数=\frac{a*b}{最大公约数}
$$
证明：

> 有集合A，集合B，
> 则知最大公约数 d=A∩B
> 所以最小公倍数=(A-D)∪(B-D)
> =AB/D

### 5.2 分数的四则运算

真分数：分子小于分母

假分数：分子大于分母

假分数：fen

#### 5.2.1 分数的表示和化简

分数通常写成假分数模式

Q：如何表示分数

1. 保证分母为正
2. 约去公约数
3. 分子为0，分母为1；

```java
class  Fractional{
		Fractional(int up,int down){
			int up;int down;
            up=this.up;
    		down=this.down;
           
        }
}
static Fractional fractional(int up,int down){
     if(down<0){
         up=-up;
         down=-down;
         }
      if(up==0){
          down=1;
         }
       else{
          int d = gcd(abs(up),abs(down))
          up/=d;
          down/=d;	
            }
   return Fractional
}
public static void main(String args[]){
    int up=5,down=2;
    f = new Fractional(up,down)
        
}
```



#### 5.2.2 分数的运算

分数有加减乘除运算

1. 加法运算

   ```java
   Fractional add(Fraction f1,Fraction f2){
       Fractional result;
       result.up = f1.up*f2.down + f2.up*f1.down;
       result.down = f1.down*f2.down;
       return result;
   }
   ```

   

2. 减法，乘法，除法 同理

#### 5.2.3 分数的标准输出

1. 进行化简
2. 1/1输出为1
3. 输出真分数，整数部分为 $$ \frac{r.up}{r.down}$$,分数的分母为 r.up%r.down;

### 5.3 素数

> 1 既不是素数也不是合数 
>
> 素数范围（1，n)

#### 5.3.1素数判断

方法一：对（1，sqrt(n)) 进行遍历 复杂度为 O(sqrt(n))

```java
boolean isPrame(int a){
  
    if(a<1){
        return false;
    }
    for(int i=2;i<=sqrt(a);i++){
        if(a%i==0)
            return false;
        	break;
    }
    return true;
}
```

方法二：对于要求复杂度的算法题时可以使用打表的方法解决

```java
int[] a = new int[101];
int num = 0;
void Find_Prame(){
    for(int i=1){
        if(isPrame(i)){
            a[num++]=i;
            
        }
}
}
```

方法三：（埃氏筛法）复杂度为O(nlog(log(n))

利用从前到后消除的原理

```java
int[] a = new int[101];
boolean [] b = new boolean [n];//如果b[i]=false，是素数，b[i]=true不是素数
void Find_Prame(){
    for(int i=2;i<=n;i++){
        if(b[i]=false){
            for(int j=i+i;j<n;j+=i){
                b[j]=true;
            }
            b[i]=false;
        }
    }
}
```

例题：求解100内所有的素数

```java
public static void main(String args[]){
	   boolean [] b = new boolean [101];
	    Find_Prame(b,100);
	    for(int i=2;i<=100;i++){
	        if(!b[i]){
	            System.out.print(i+" ");
	        }
	    }
}	
static void Find_Prame(boolean[] b,int n){
   
	    for(int i=2;i<=n;i++){
	        if(!b[i]){
	            for(int j=i+i;j<=n;j+=i){
	                b[j]=true;
	            }
	            b[i]=false;
	        } 
	    }
}
```



### 5.4 质因子分解

Q:什么是质因子分解：

> 将一个正整数写成一个或多个质数乘积的形式
> 6=2*3,12=2^2\*3

分解过程

> 构造类 Factor记录 因子x，与因子的个数 cunt
>
> 因为所取最简化的因子一定为素数，直接采取对n进行取模运算。
>
> 对于正整数n，只存在一个因子大于sqrt(n),会存在一个或多个小于n的因子，
>
> 以此入手，如果在前sqrt(n)循环之前，仍然存在n!=1,必定n为素数
>
> 此时算法复杂度降到O(sqrt(n))

```java
public static void main(String args[]){
	  int n=1997;
	  int m=n;
	  boolean []b=new boolean[10100];  
	  Factor [] fac= new Factor[10];  
	  int num=0;
	   int [] prime = Find_Prame(b);
	   
		for(int i=2;prime[i-2]<Math.sqrt(m);i++){
			if(n%prime[i-2]==0){
				fac[num]=new Factor();
				fac[num].x=prime[i-2];
				fac[num].cunt=0;
				while(n%prime[i-2]==0){
					fac[num].cunt++;
					n/=prime[i-2];
                    //每次都除以因子
				}
				num++;
            }			
		}
		if(n!=1){
			fac[num] = new Factor();
			fac[num].x=n;
			fac[num].cunt=1;
		}
		System.out.print(m+"=");
		
		for(int i=0;i<num;i++){
			//System.out.println(fac[0].x);
			System.out.print(fac[i].x+"^"+fac[i].cunt+"+");
		}
		System.out.print(fac[num].x+"^"+String.valueOf(fac[num].cunt));
}	
static  int[] Find_Prame(boolean[] b){
	int [] prame = new int[1000];
	int num = 0;
	for(int i=2;i<=prame.length;i++){
		if(!b[i]){
			for(int j=i+i;j<=b.length-1;j+=i){
				b[j]=true;
				
			}
			b[i]=false;
			prame[num++]=i;
            //记录素数
		}
	}
	return prame;
}
}
class Factor{
	//素数 x代表因子，cunt代表该因子的次方数
	int x;
	int cunt;	
	
}
```



### 5.5 大数运算

#### 5.5.1 基本数据类型

为什么要用大数计算？

> 当数值大于可以取的范围时就不得不用到大数了。

> 八种基本类型的运算范围如下
>
> | 数据类型 | 字节数 | 二进制位数 | 范围                                       | 规律        |
> | -------- | ------ | ---------- | ------------------------------------------ | ----------- |
> | byte     | 1      | 8          | -128～127                                  | -27～27-1   |
> | short    | 2      | 16         | -32768～32767                              | -215～215-1 |
> | int      | 4      | 32         | -2147483648～2147483647                    | -231～231-1 |
> | long     | 8      | 64         | -9223372036854775808 ~ 9223372036854775807 | -263～263-1 |
> | float    | 4      | 32         | 1.4E-45~3.4028235E38                       |             |
> | double   | 8      | 64         | 4.9E-324~1.7976931348623157E308            |             |
> | char     | 2      | 16         | 0～65535                                   | 0~216-1     |
> | boolean  | 1      | 8          | true或false                                | true或false |

#### 5.2.2 BigInteger

JAVA里面提供了Integer类，专门运用于大数的计算。官方文档这样介绍

> 不可变的任意精度的整数。所有操作中，都以二进制补码形式表示 BigInteger（如 Java 的基本整数类型）。BigInteger 提供所有 Java 的基本整数操作符的对应物，并提供 java.lang.Math 的所有相关方法。另外，BigInteger 还提供以下运算：模算术、GCD 计算、质数测试、素数生成、位操作以及一些其他操作。

也就是说

> BigInteger 任意大的整数，原则上是，只要你的计算机的内存足够大，可以有无限位的.
>
> BigInteger属于java.math.BigInteger;

由于biginteger不是基本类型，所以需要 new 

```
BigInteger abs()  返回大整数的绝对值
BigInteger add(BigInteger val) 返回两个大整数的和
BigInteger and(BigInteger val)  返回两个大整数的按位与的结果
BigInteger andNot(BigInteger val) 返回两个大整数与非的结果
BigInteger divide(BigInteger val)  返回两个大整数的商
double doubleValue()   返回大整数的double类型的值
float floatValue()   返回大整数的float类型的值
BigInteger gcd(BigInteger val)  返回大整数的最大公约数
int intValue() 返回大整数的整型值
long longValue() 返回大整数的long型值
BigInteger max(BigInteger val) 返回两个大整数的最大者
BigInteger min(BigInteger val) 返回两个大整数的最小者
BigInteger mod(BigInteger val) 用当前大整数对val求模
BigInteger multiply(BigInteger val) 返回两个大整数的积
BigInteger negate() 返回当前大整数的相反数
BigInteger not() 返回当前大整数的非
BigInteger or(BigInteger val) 返回两个大整数的按位或
BigInteger pow(int exponent) 返回当前大整数的exponent次方
BigInteger remainder(BigInteger val) 返回当前大整数除以val的余数
BigInteger leftShift(int n) 将当前大整数左移n位后返回
BigInteger rightShift(int n) 将当前大整数右移n位后返回
BigInteger subtract(BigInteger val)返回两个大整数相减的结果
byte[] toByteArray(BigInteger val)将大整数转换成二进制反码保存在byte数组中
String toString() 将当前大整数转换成十进制的字符串形式
BigInteger xor(BigInteger val) 返回两个大整数的异或
BigInteger compareTo(BigInteger val) 根据该数值是小于、等于、或大于 val 返回 -1、0 或 1；
equals：判断两数是否相等，也可以用compareTo来代替；
intValue，longValue，floatValue，doublue：把该数转换为该类型的数的值。
```

#### 5.5.2 Integer

说到BigIntegerJ就不得说说integer了。

> **java.lang.Integer**
>
> `Integer` 类在对象中包装了一个基本类型 `int` 的值。`Integer` 类型的对象包含一个 `int` 类型的字段。
> 此外，该类提供了多个方法，能在 `int` 类型和 `String` 类型之间互相转换，还提供了处理 `int` 类型时非常有用的其他一些常量和方法。

概括就是：int与integer范围上是一样的，不过封装了不少的方法可以调用

```
compareTo(Integer anotherInteger) 在数字上比较两个 Integer 对象
doubleValue(),floatValue() ,intValue(),longValue()以 xx 类型返回该 Integer 的值
parseInt(String s) 将字符串参数作为有符号的十进制整数进行解析
toString() 返回一个表示该 Integer 值的 String 对象。。将该参数转换为有符号的十进制表示形式，并以字符串             的形式返回它。
public static String 
将十进制数转换为二进制数（由0 1组成）
public static String 
将十进制数转换为八进制数（以0开头）
public static String 
将十进制数转换为十六进制数（以0x开头）

```

```
最常用的方法了，进制转换，类型转换。
Integer i = new Integer(10);
int ii = i.intValue();//Integer 转换成 int
Integer kk = Integer.valueOf(ii)//将ii转换为Integer
int iii = Integer.parsetInt("10");//字符串转成int类型
k = Integer.parseInt("110", 2);//radix进制的字符串转换成int
toBinaryString(int i),toOctalString(int i),toHexString(int i),转换成2,8,16进制
```

#### 5.5.3补充：

Q: ‘==’,和equals()的区别

> 众所周知,==比较的是地址,equals比较值。
> 但是在integer中存在一个**IntegerCache**，也就是范围在[-128,127]的数值是直接在IntegerCache里面缓存的，所以当赋值的时候不会重新的new，而是直接的进行调用。
> 那么 有了以下的比较：
>
> > a = new Integer(12), Integer b = 12; 由于a是new进行创建对象的，所以a会存在于堆里面，而b是在IntegerCache里获取的.
> >
> > 所以(a==b)为false
>
> > 当 integer不在该范围的时候，Integer会进行new, 然后自动装箱产生对象;
> > 若 Integer a = 128,Integer b=128;
> > 也会出现 (a==b)为false
> > 但是如果Integer a = 127,Integer b=127;
> > (a==b)为true
>
> > 当int与Integer进行比较的时候回自动的拆箱，转换为int进行比较
>
> 
>
> [参考文章](https://blog.csdn.net/wangyang1354/article/details/52623703)

Q：默认值

> Integer的默认值是null，int的默认值是0

装箱就是  自动将基本数据类型转换为包装器类型；拆箱就是  自动将包装器类型转换为基本数据类型。

关于拆箱和分箱可以参考博客文章 [深入剖析Java中的装箱和拆箱](https://www.cnblogs.com/dolphin0520/p/3780005.html)

### 5.6 组合数 

> 从n个不同元素中，任取m(m≤n)个元素并成一组，叫做从n个不同元素中取出m个元素的一个组合；
>
> 从n个不同元素中取出m(m≤n)个元素的所有组合的个数，叫做从n个不同元素中取出m个元素的组合数。
>
> 公式：
> $$
> C_{n}^{m} = \frac{n!}{m!(n-m)!}
> $$
> C<sub>n</sub><sup>n</sup>=C<sub>n</sub><sup>0</sup>=1;;      C<sub>n</sub><sup>m</sup>=C<sub>n</sub><sup>n-m</sup>;

#### 5.6.1阶乘的质因子

Q: 什么是阶乘呢？

> 便于记录连乘，形如 n! =  1\*2\*3.....*n

Q: n! 中有多少质因子2呢？

> 最直接的方式就是 套两个for循环，复杂度达到O(N^2)
>
> 但是当n很大时，内存占用的会非常多的。

方法一：

通过我们的观察

​                                                   2     n!中2^3的个数

​                          2                       2	n!中2^2的个数

​              2          2           2         2            2  n!中2的个数

10!=1 * 2 * 3* 4 * 5 * 6 * 7 * 8 * 9 * 10

通过上面得到 在10！中 因子 2的个数为5,2^2 为2个，2^3 1个
10！的质因子共 1+2+5=8

同理 求 n!的质因子为p个数

> $$
> \frac{n}{p}+\frac{n}{p^{2}}+\frac{n}{p^{3}}+\frac{n}{p^{4}}+\frac{n}{p^{5}}+·····
> $$

求解的终止条件为当int m=log<sub>2</sub>(n),既log(n)的向下取整

```java
int cal(int n,int p){
    int m=n;
    int sum=0;
    for(int i=0;i<=Math.log(m)/Math.log(2);i++){
        sum+=n/p;
        n/=p;
    }
    return sum;
}
```

其中 在java提供的Math函数求对数

Math.log(m) = log<sub>e</sub>(m) 
$$
log_{2}^{m} = \frac{log_{e}^{m}}{log_{e}{2}}
$$
然后发现这样也是可以的(判断条件简单多了，用for用的太多了，瞬间感觉while挺好的)

```java
int cal(int n,int p){
    int sum=0;
    while(n!=0){
        sum+=n/p;
        n/=p;
    }
    return sum;
}
```



> 通过这个方法也可以求 n!的结果中有多少个0?
> 即是求cal(n,5)
> 这里求的是质因子5的个数，是因为在一个阶乘中每有一个二就会有一个5，2*5会创造一个0,而不是一个10才创造一个0;

方法二：逆向递归
从书上的理解是：

> 10! = 2<sup>5</sup>+5!+1\*3\*5\*7\*9
>
> 5! = 2<sup>2</sup> + 2！+ 1\*3\*5
>
> 和刚刚方法一是一样的原理

```java
int cal(int n,int p){
    if(n<p) return 0;
    return n/p + cal(n/p,p);
}
```

#### 5.6.2 组合数的计算

1，计算 C<sub>n</sub><sup>m</sup>
$$
C_{n}^{m} = \frac{n!}{m!(n-m)!}=\frac{(n-m+1)(n-m+2)····(n-m+m)}{m！}
$$
通过展开式能够看出，分子分母都为m项的，且C<sub>n</sub><sup>m</sup>的结果都是为整数，故此我们可以边乘边除，而不是每一项分开除再乘的；这样的话就可以避免乘法的溢出。

```java
  int C(int n,int m){
        int sum=1;
        for(int i=1;i<=m;i++){
            sum=sum*(n-m+i)/i;
            /**不能写成sum*=(n-m+i)/i;这样是先计算每一项除法的结果，而每一项无法保证为整数，只有先乘后除**/
        }
        return sum;
    }
```

方法二：通过递推公式

当我们从n个数中取出m个的组合也就是  C<sub>n</sub><sup>m</sup> ,如何我们先不取最后一个数，先从n-1个当中取出m个，然后选取最后一个，再从n-1个当中选取m-1个，既可以得到
$$
C_{n}^{m} = C_{n-1}^{m}+c_{n-1}^{m-1}
$$
运用递归可以很快的写出代码，递归的判断条件是当m=0，或者m=n；

> 写递归一定要先判断好递归终止的条件 C<sub>n</sub><sup>n</sup>=C<sub>n</sub><sup>0</sup>=1;

```java
 int C(int n,int m){
     if(n==m||m==0){
         return 1;
     }
        return C(n-1,m)+C(n-1,m-1);
  }
```

​                                                                                                                                             



