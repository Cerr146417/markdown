# Week 2 C程序中的函数

## 第一课

### 函数的定义与声明

- 已知一个数，求其平方根

- r = sqrt(100.0);

- 已知底数x，幂指数y，求
  $$
  x^y
  $$

- k = pow(x, y);

- 求一个字符串的长度
- i = strlen(str1);
- 比较两个字符串的大小
- v =  strcmp(str1, str2)
- 把字符串转换为相应的整数
- n = atoi(str1)

> 函数是值传递

*数组名做函数参数，传递的是地址

# Week 3 函数的递归

递归，自己调用自己，并且有结束条件。递归都可以改为循环。

编程题＃4：扩号匹配问题

**描述**

在某个字符串（长度不超过100）中有左括号、右括号和大小写字母；规定（与常见的算数式子一样）任何一个左括号都从内到外与在它右边且距离最近的右括号匹配。写一个程序，找到无法匹配的左括号和右括号，输出原来字符串，并在下一行标出不能匹配的括号。不能匹配的左括号用"$"标注,不能匹配的右括号用"?"标注.

**输入**

输入包括多组数据，每组数据一行，包含一个字符串，只包含左右括号和大小写字母，字符串长度不超过100

注意：cin.getline(str,100)最多只能输入99个字符！

**输出**

对每组输出数据，!!!输出两行，第一行包含原始输入字符!!!，第二行由"$","?"和空格组成，"$"和"?"表示与之对应的左括号和右括号不能匹配。

**样例输入**

((ABCD(x)
)(rttyy())sss)(

**样例输出**

((ABCD(x)
\$$
)(rttyy())sss)(
?            ?$

```java

//索引值 递增匹配右括号，匹配到时，索引值递减寻找左括号，如果能找到，将左右括号都改为普通字符 如L，R。如果找不到，则将右括号改为?。依次往复。注意索引值为0的情况讨论。注意要while (cin.getline(str, 101))

#include <iostream>
#include <cstring>
using namespace std;

int left = 0;
int right = 0;
char strOrg[101];

int main() {
	char str[101];
	while (cin.getline(str, 101)) {
		strcpy(strOrg, str);
		int index = 0;
		while(str[index] != '\0'){
			// 当值匹配到右括号时，记录下此时的值，
			// 索引开始递减，看能否匹配到左括号
			if(str[index] == ')' && index != 0) {
				int right = index;
				index--;
				while(str[index] != '(' && index != 0) {
					index--;
				}
				if(str[index] == '(') {
					int left = index;
					str[right] = 'R';
					str[left] =  'L';
					index = right;
				}
				if(index == 0 && str[index] != '('){
					str[right] = '?';
					index = right;
				}
			}
			index++;
		}
		cout <<  strOrg << endl;
		for(int i = 0; i < strlen(str); i++) {
			if('(' ==  str[i]) {
				cout << '$';
			} else if (')'  == str[i] || '?' ==  str[i]) {
				cout  << '?';
			} else {
				cout <<  ' ';
			}
 		}
 		cout << endl;
	}
	return 0;
}
```



# Week 4 指针（一）

## 第一课

### 什么是指针

**变量的三要素**

| 变量的地址 | 变量的值 | 变量的名字 |
| ---------- | -------- | ---------- |
| 0x0012FF78 | 76       | c          |

把某个变量的地址称为指向该变量的指针

**如何取变量的地址？**

&c

**如果通过地址操作变量？**

*&c = 24 

其实就等同于 c =  24

> &取地址，*取地址上存的东西

### 什么是指针变量

定义一个指针变量

| 指针变量的基类型 | 指针类型 | 指针的变量名 |
| ---------------- | -------- | ------------ |
| int              | *        | pointer      |

```c++
int c =76
int *pointer;
pointer = &c;
// pointer存储的是地址，*pointer操作地址上的值
```

## 第三课

### 数组与指针

> 数组名相当于数组第一个元素的地址。数组名a是地址常量，不能给a复制。

### 指向二维数组的指针

```c
int a[3][4];
int (*p)[4];
p = a;
```

# Week 5 指针（二）

## 第一课 

### 字符串与指针

```c
char c[6] = {'h', 'e', 'l', 'l', 'o', '\0'};
char *pc = c;
cout << pc << endl; // hello
// cout 打印指向字符串的指针会自动打印字符串数组，而不是指针指向的内存地址
```

```c
char c[6] = {'h', 'e', 'l', 'l', 'o', '\0'};
char *pc = c;
cout << c << endl; // 0x0013FF54
cout << static_cast<void*>(pc) << endl; // 0x0013FF54
// cout 打印指向字符串的指针会自动打印字符串数组，而不是指针指向的内存地址
```

```c
char *pc = "hello";
pc++;
cout << pc << endl; // ello
cout << *pc << endl; // e 打印*pc，打印的是指针指向的字符
```



### 取地址与指针运算

a与&a的区别。
a 是 数组首元素的地址指针。a + 1 是移一个基类型。
&a 是 整个数组的指针。&a + 1 是移动整个数组。

*(&a) 打印的是 指针指向的整个数组，打印的是a，是a数组第一个元素的地址。



## 第二课

### 二维数组名的含义

![](https://raw.githubusercontent.com/woshiamiaojiang/img/master/image_39808.png)

![](https://raw.githubusercontent.com/woshiamiaojiang/img/master/image_21684.png)

### 二维数组名引用示例

![](https://raw.githubusercontent.com/woshiamiaojiang/img/master/image_29536.png)

# Week 6 指针（三）

## 第一课

### 指针做函数参数

![image.png](https://upload-images.jianshu.io/upload_images/3128142-829cbfcf8807620e.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

![1542880050964](d:\user\80003415\Application Data\Typora\typora-user-images\1542880050964.png)



![image.png](https://upload-images.jianshu.io/upload_images/3128142-69970465c2b18fa3.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

![image.png](https://upload-images.jianshu.io/upload_images/3128142-bb314dc316fab396.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

这样做的问题，a[10] 第二个到最后的值会被修改。

### 限制指针实参的功能

![image.png](https://upload-images.jianshu.io/upload_images/3128142-a6ac620c3ac48354.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

将a的值拷贝给b，并且不希望对a的值做任何的修改。这样定义mystrcpy



![image.png](https://upload-images.jianshu.io/upload_images/3128142-92d68febcfb344a4.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)



## 第二课

### 指针做函数返回值

![image.png](https://upload-images.jianshu.io/upload_images/3128142-fe745a59c86d266f.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

(*arr)[4]    或者  int arr[][4]  可以用来接收二维数组



![image.png](https://upload-images.jianshu.io/upload_images/3128142-9b84485cbedfe1da.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

getInt1()被释放掉。getInt2()占据了getInt1()的位置。所以打印*p的值大部分情况下会打印30。

但getInt2()也会被释放掉。所以*p的值也是不能确定的。



不释放的情况。全局变量或者静态变量。

![image.png](https://upload-images.jianshu.io/upload_images/3128142-b61dad07dc0d39fe.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

![1542880902086](d:\user\80003415\Application Data\Typora\typora-user-images\1542880902086.png)



### 静态局部变量

![image.png](https://upload-images.jianshu.io/upload_images/3128142-9563932379dd1b21.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

![image.png](https://upload-images.jianshu.io/upload_images/3128142-8dda89461f09bb41.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)





