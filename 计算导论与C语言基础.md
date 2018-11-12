# C语言中的数据成分

## 第一课

###  1、整数型的类别

1）sizeof运算符

    用于计算某种类型的对象在内存中所占的字节数。

```c++
cout  << sizeof(int) << endl; // 4
```

### 2、浮点型

1）浮点型的精度

```c++
float a = 3.1415926;
cout << a << endl; // 3.14159 cout默认打印6位精度，整数也算在精度之内
```

```c++
#include <iomanip>
float a = 3.1415926535897;
cout << setprecision(100); 
cout << a << endl; // 3.1415927410125732421875 float 精度是7
```

```c++
cout<<fixed<<setprecision(2) << f; // f = 20.00 来保留f后面的两位小数。
```

2）浮点型的存储

![浮点型的存储](https://raw.githubusercontent.com/woshiamiaojiang/image-hosting/master/Snipaste_2018-11-12_10-25-33.png)

float 32位

1位符号位

8位指数位（含1位符号位） 0 ~ 128
$$
log_{10}(2^{127})\approx38.23
$$

23位二进制小数位（默认为1.XXXX）
$$
log_{10}(2^{24})\approx7.225
$$

# C语言中的运算成分

##  第三课

### 1、逻辑运算与混合运算

判断闰年

![](https://raw.githubusercontent.com/woshiamiaojiang/image-hosting/master/leapyear.png)



# C程序中的数组

## 第一课

### 1、再谈一维数组

```c++
cout <<  setw(3) << endl; // 右对齐，设置宽度为3，3的前面填补空格
// setw是iomanip库里定义的格式控制操作符，需要#include <iomanip> 包含这个头文件。
```

```c++
int a[10] = {0}; // a数组中的10个元素都初始化为0
```

## 第二课

### 1、数组的作用之二

开根号

```c++
#include <cmath>
sqrt(100.0) 
```

# C程序中的字符串

## 第一课

### 1、字符数组与字符串

```C++
char c[] = {'C',  'h', 'i', 'n', 'a'};
```

| C[0] | C[1] | C[2] | C[3] | C[4] |
| ---- | ---- | ---- | ---- | ---- |
| C    | h    | i    | n    | a    |

```C++
char c[] = "China";
```

| C[0] | C[1] | C[2] | C[3] | C[4] | C[5] |
| ---- | ---- | ---- | ---- | ---- | ---- |
| C    | h    | i    | n    | a    | \0   |

### 2、一个字符的输入

1）方法一：直接用cin输入字符

```C++
char c;
cout << "enter a sentence:" << endl;
while (cin >> c) // cin 不读空格与回车
	cout << c;
```

输入：abc def g

输出：abcdefg

2）方法二：用cin.get()输入字符

```C++
char c;
cout << "enter a sentence:" << endl;
while ((c = cin.get()) != EOF) 
    cout << c;
```

输入：abc def g

输出：abc def g

3）方法三：用cin.get(char)输入字符

```C++
char c;
cout << "enter a sentence:" << endl;
while (cin.get(c)) 
    cout << c;
```

输入：abc def g

输出：abc def g

4）方法四：用getchar输入字符

```C++
char c;
cout << "enter a sentence:" << endl;
while (cin = getchar()) // 不跳任何字符，特指^Z
    cout << c;
```

输入：abc def g

输出：abc def g

## 第二课

### 1、一串字符的输入

1）方法一：直接用cin输入字符串

```c++
chatr str[10];
cout << "enter a sentence" endl;
while (cin >> str)
	cout  << str << endl;
return 0;
```

输入： abc def g

输出：

abc

def

g

^Z

2）方法二：用cin.get()函数输入

``` c++
char ch[20];
cout<< "enter a sentence:" << endl;
cin.get(ch, 10,'o'); // 读取10个字符，指定终止符为'o'
cout << ch << endl;
```

输入：We are good friends.

输出：We are g

3）方法三：用cin.getline()函数输入

``` c++
char ch[20];
cout<< "enter a sentence:" << endl;
cin.getline(ch, 10,'o'); // 读取10个字符，指定终止符为'o'
cout << ch << endl;
```

输入：We are good friends.

输出：We are g

> getline与get区别
>
> getline遇到终止字符时，缓冲区指针移到终止标志字符之后；
>
> get遇到终止字符时停止读取，指针不移动



### 2、字符串应用例题

```c++
char str[200];
while (cin.getline(str,200))
```

```c++
#include <string>
char str1[20], str2[20];
strcpy(str2, str1); // 字符串拷贝
```

