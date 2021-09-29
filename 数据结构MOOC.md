# **01-复杂度1 最大子列和问题**

## 题目要求

给定*K*个整数组成的序列{ *N*<sub>1</sub>, *N*<sub>2</sub>, ..., *N*<sub>*K* </sub>}，“连续子列”被定义为{ *N*<sub>*i*</sub>, *N*<sub>*i*+1</sub>, ..., *N*<sub>*j* </sub>}，其中 1≤*i*≤*j*≤*K*。“最大子列和”则被定义为所有连续子列元素的和中最大者。例如给定序列{ -2, 11, -4, 13, -5, -2 }，其连续子列{ 11, -4, 13 }有最大的和20。现要求你编写程序，计算给定整数序列的最大子列和。

本题旨在测试各种不同的算法在各种数据情况下的表现。各组测试数据特点如下：

- 数据1：与样例等价，测试基本正确性；
- 数据2：10<sup>2</sup>个随机整数；
- 数据3：10<sup>3</sup>个随机整数；
- 数据4：10<sup>4</sup>个随机整数；
- 数据5：10<sup>5</sup>个随机整数；

### 输入格式:

输入第1行给出正整数*K* (≤100000)；第2行给出*K*个整数，其间以空格分隔。

### 输出格式:

在一行中输出最大子列和。如果序列中所有整数皆为负数，则输出0。

### 输入样例:

```in
6
-2 11 -4 13 -5 -2
结尾无空行
```

### 输出样例:

```out
20
结尾无空行
```

## 代码

```c++
#include <bits/stdc++.h>
using namespace std;

int main() {
    int k;
    cin >> k;
    int maxsum = 0;
    int thissum = 0;
    int n;
    for(int i=1; i<=k; i++)
    {
        cin >> n;
        thissum += n;
        if(thissum < 0)
        {
            thissum = 0;
            continue;
        }
        else
        {
            if(thissum > maxsum)
            {
                maxsum = thissum;
            }
        }
    }
    cout << maxsum;
    
    return 0 ;
}
```

## 解题思路

使用“在线处理”方法，设置两个变量，maxsum（最大子列和），thissum（当前自列和）。循环输入数列数字，加入thissum（当前子列和），一旦thissum的大小小于0就说明之后再加任何的数字都只能变小，此时将thissum置0，进入下一次循环。每次循环更新maxsum大小，最后循环结束，输出maxsum。

---

# **01-复杂度2 Maximum Subsequence Sum**

## 题目要求

Given a sequence of *K* integers { *N*<sub>1</sub>, *N*<sub>2</sub>, ..., *N*<sub>*K* </sub>}. A continuous subsequence is defined to be { *N*<sub>*i*</sub>, *N*<sub>*i*+1</sub>, ..., *N*<sub>*j* </sub> } where 1≤*i*≤*j*≤*K*. The Maximum Subsequence is the continuous subsequence which has the largest sum of its elements. For example, given sequence { -2, 11, -4, 13, -5, -2 }, its maximum subsequence is { 11, -4, 13 } with the largest sum being 20.

Now you are supposed to find the largest sum, together with the first and the last numbers of the maximum subsequence.

### Input Specification:

Each input file contains one test case. Each case occupies two lines. The first line contains a positive integer *K* (≤10000). The second line contains *K* numbers, separated by a space.

### Output Specification:

For each test case, output in one line the largest sum, together with the first and the last numbers of the maximum subsequence. The numbers must be separated by one space, but there must be no extra space at the end of a line. In case that the maximum subsequence is not unique, output the one with the smallest indices *i* and *j* (as shown by the sample case). If all the *K* numbers are negative, then its maximum sum is defined to be 0, and you are supposed to output the first and the last numbers of the whole sequence.

### Sample Input:

```in
10
-10 1 2 3 4 -5 -23 3 7 -21
```

### Sample Output:

```out
10 1 4
```

### Code：

```c++
#include<bits/stdc++.h>
using namespace std;

int a[10005];

int main(){
    int n;
    cin >> n;
    for(int i=1;i<=n;i++) cin>>a[i];
    int MaxSum = -1;
    int ThisSum = 0;
    int left = 1, right = n, templeft = 1, tempright = 1;
    for(int i=1;i<=n;i++){
    	tempright = i;
    	ThisSum += a[i];
    	if(ThisSum > MaxSum){
    		left = templeft;
            right = tempright;
    		MaxSum = ThisSum;
		}
    	if(ThisSum<0){
    		ThisSum = 0;
    		templeft = i+1;
    		tempright=i+1;
		} 
	}
	if(MaxSum<0) cout << 0 << " " << a[1] << " " << a[n] << endl;
    else cout << MaxSum << " " << a[left] << " " <<a [right] << endl;
    
	return 0;
}
```

### 解题思路：

这道题是上一道题的难度加大版，需要记录最大子列和首尾的数字，因此需要构建一个数组来存放数字序列以方便计数，注意这里有两个坑，测试时多次未AC。

① 如果序列全为负数时，要输出 ”0 序列首位数字 序列末尾数字“，如果序列中有0存在，其他数字均为负数时，则输出” 0 0 0 “。

② 如果存在多个并列的最大子列和，要输出下标更小的那一组。

---

# **01-复杂度3 二分查找** （函数题）

## 题目要求

本题要求实现二分查找算法。

### 函数接口定义：

```c++
Position BinarySearch( List L, ElementType X );
```

其中`List`结构定义如下：

```c++
typedef int Position;
typedef struct LNode *List;
struct LNode {
    ElementType Data[MAXSIZE];
    Position Last; /* 保存线性表中最后一个元素的位置 */
};
```

`L`是用户传入的一个线性表，其中`ElementType`元素可以通过>、==、<进行比较，并且题目保证传入的数据是递增有序的。函数`BinarySearch`要查找`X`在`Data`中的位置，即数组下标（注意：元素从下标1开始存储）。找到则返回下标，否则返回一个特殊的失败标记`NotFound`。

### 裁判测试程序样例：

```c++
#include <stdio.h>
#include <stdlib.h>

#define MAXSIZE 10
#define NotFound 0
typedef int ElementType;

typedef int Position;
typedef struct LNode *List;
struct LNode {
    ElementType Data[MAXSIZE];
    Position Last; /* 保存线性表中最后一个元素的位置 */
};

List ReadInput(); /* 裁判实现，细节不表。元素从下标1开始存储 */
Position BinarySearch( List L, ElementType X );

int main()
{
    List L;
    ElementType X;
    Position P;

    L = ReadInput();
    scanf("%d", &X);
    P = BinarySearch( L, X );
    printf("%d\n", P);

    return 0;
}

/* 你的代码将被嵌在这里 */
```

### 输入样例1：

```in
5
12 31 55 89 101
31
```

### 输出样例1：

```out
2
```

### 输入样例2：

```
3
26 78 233
31
```

### 输出样例2：

```out
0
```

### 代码：

```c
Position BinarySearch( List L, ElementType X )
{
    Position t = 0, l = 1, r = L->Last;
    while(l <= r)
    {
        t = (l+r) / 2;
        if(X == L->Data[t])
        {
            return t;
        }
        else if(X > L->Data[t])
        {
            l = t + 1;
        }
        else if(X < L->Data[t])
        {
            r = t - 1;
        }
    }
    return NotFound;
}
```

### 解题思路：

简单的二分思想，设置一个temp不断寻找待查区间下标的中间值，与输入的数据比较，如果temp较大，则更新待查区间右端点为temp-1；若temp较小，则更新待查区间左端点为temp+1；若与temp相等，则返回temp，即该元素在线性表中的位置下标；若出现了待查区间左端点下标大于右端点下标的情况，则说明未在线性表中查找到该元素，返回NotFound。

---

# **02-线性结构1 两个有序链表序列的合并**（函数题）

## 题目要求

本题要求实现一个函数，将两个链表表示的递增整数序列合并为一个非递减的整数序列。

### 函数接口定义：

```c++
List Merge( List L1, List L2 );
```

其中`List`结构定义如下：

```c++
typedef struct Node *PtrToNode;
struct Node {
    ElementType Data; /* 存储结点数据 */
    PtrToNode   Next; /* 指向下一个结点的指针 */
};
typedef PtrToNode List; /* 定义单链表类型 */
```

`L1`和`L2`是给定的带头结点的单链表，其结点存储的数据是递增有序的；函数`Merge`要将`L1`和`L2`合并为一个非递减的整数序列。应直接使用原序列中的结点，返回归并后的带头结点的链表头指针。

### 裁判测试程序样例：

```c++
#include <stdio.h>
#include <stdlib.h>

typedef int ElementType;
typedef struct Node *PtrToNode;
struct Node {
    ElementType Data;
    PtrToNode   Next;
};
typedef PtrToNode List;

List Read(); /* 细节在此不表 */
void Print( List L ); /* 细节在此不表；空链表将输出NULL */

List Merge( List L1, List L2 );

int main()
{
    List L1, L2, L;
    L1 = Read();
    L2 = Read();
    L = Merge(L1, L2);
    Print(L);
    Print(L1);
    Print(L2);
    return 0;
}

/* 你的代码将被嵌在这里 */
```

### 输入样例：

```in
3
1 3 5
5
2 4 6 8 10
```

### 输出样例：

```out
1 2 3 4 5 6 8 10 
NULL
NULL
```

### 代码：

```cpp
List Merge( List L1, List L2 )
{
    List L = (List)malloc(sizeof(struct Node));
    List tempL1 = L1->Next, tempL2 = L2->Next;
    List head = L;  // 合并后链表L的头结点
    while(tempL1 && tempL2)
    {
        if(tempL1->Data <= tempL2->Data)
        {
            L->Next = tempL1;
            tempL1 = tempL1->Next;
            L = L->Next;
        }
        else
        {
            L->Next = tempL2;
            tempL2 = tempL2->Next;
            L = L->Next;
        }
    }
    L->Next = tempL1 ? tempL1 : tempL2; // 若一个链表遍历完成，则将另一个链表剩余部分直接接到合并后链表
    L1->Next = NULL;
    L2->Next = NULL;
    return head;
}
```

### 解题思路

合并链表的操作重点就是将较小链表节点（此处记为tempL）插入到合并链表之后：

```cpp
1. L->Next = tempL;
2. tempL = tempL->Next;
3. L = L->Next;
```

直到一个链表遍历完，此时将另一链表剩余部分全部接到合并链表尾：

```cpp
L->Next = tempL1 ? tempL1 : tempL2;
```

在题目的输出样例中，L1 和 L2打印均为 NULL，因此，在函数最后将 L1->Next 和 L2->Next 全部置为NULL。

---

# **02-线性结构2 一元多项式的乘法与加法运算**

## 题目要求

设计函数分别求两个一元多项式的乘积与和。

### 输入格式:

输入分2行，每行分别先给出多项式非零项的个数，再以指数递降方式输入一个多项式非零项系数和指数（绝对值均为不超过1000的整数）。数字间以空格分隔。

### 输出格式:

输出分2行，分别以指数递降方式输出乘积多项式以及和多项式非零项的系数和指数。数字间以空格分隔，但结尾不能有多余空格。零多项式应输出`0 0`。

### 输入样例:

```in
4 3 4 -5 2  6 1  -2 0
3 5 20  -7 4  3 1
```

### 输出样例:

```out
15 24 -25 22 30 21 -10 20 -21 8 35 6 -33 5 14 4 -15 3 18 2 -6 1
5 20 -4 4 -5 2 9 1 -2 0
```

### 解题思路：

根据输入样例，多项式均是以指数递减的方式输入，可用动态数组和单链表来表示多项式，本题采用单链表来表示。链表结构如下

```cpp
typedef struct PolyNode *Polynomial;   // 多项式
struct PolyNode {
    int coef;   // 系数
    int expon;  // 指数
    Polynomial link;
}
```

按照题目要求搭建程序框架：

```
int main()
{
	读取多项式1
	读取多项式2
	乘法运算并输出
	加法运算并输出
	
	return 0;
}
```

因此，需要设计读取，加法和乘法运算，输出四个功能的函数。

① 读取函数

根据题目的输入样例，设计读取函数，由于链表中结点链接到链表中从操作频繁，增加设计一个链接函数Attach()，方便操作。

```cpp
// 链接函数
void Attach(int c, int e, Polynomial *pRear)
{
    Polynomial t = new Polynomial();
    t->coef = c;    // 给新节点赋值
    t->expon = e;
    t->link = NULL;
    (*pRear)->link = t;
    *pReal = t; // 修改*pRear的值
}
// 读取多项式函数
Polynomial ReadPoly()
{
    Polynomial P = new Polynomial();  // 临时生成链表头空节点
    Polynomial Rear, t;
    P->link = NULL;
    Rear = P;
    int n, c, e;
    cin >> n;
    while(n--)
    {
        cin >> c >> e;
        Attach(c,e,&Rear);
    }
    t = P;  // 删除临时生成的头节点
    P = P->link;
    free(t);
    return P;
}
```

② 多项式相加函数

两个多项式相加，就是逐项比较两个链表每一项，将指数相同的项系数相加得到新的项，指数较大的项链接到结果链表后并将指针向后移，直到其中一个链表遍历完，将另一链表剩余部分全部连接到结果链表后。

```cpp
// 多项式相加函数
Polynomial AddPoly(Polynomial P1, Polynomial P2)
{

    Polynomial Rear, t1, t2, t;
    t1 = P1;
    t2 = P2;
    Polynomial P = new PolyNode;
    P->link = NULL;
    Rear = P;
    while(t1 && t2)
    {
        if(t1->expon == t2->expon)
        {
            int sum = t1->coef + t2->coef;
            if(sum != 0)
            {
                Attach(sum,t1->expon,&Rear);
                t1 = t1->link;
                t2 = t2->link;
            }
            else
            {
                t1 = t1->link;
                t2 = t2->link;
            }

        }
        else if(t1->expon > t2->expon)
        {
            Attach(t1->coef,t1->expon,&Rear);
            t1 = t1->link;
        }
        else if(t1->expon < t2->expon)
        {
            Attach(t2->coef,t2->expon,&Rear);
            t2 = t2->link;
        }
    }
    for(;t1;t1=t1->link)    Attach(t1->coef,t1->expon,&Rear);
    for(;t2;t2=t2->link)    Attach(t2->coef,t2->expon,&Rear);
    Rear->link = NULL;
    t = P;
    P = P->link;
    free(t);

    return P;
}
```

③ 多项式相乘函数

两个多项式相乘较为麻烦，可以分解为两个主要步骤：

​	<1> 将乘法运算转变为加法运算

​			将P1当前项（c<sub>i</sub> ,e<sub>i</sub> ) 乘 P2多项式，再加到结果多项式里

```cpp
t1 = P1; t2 = P2;
Polynomial P = new Polynomial();
Rear = P;
while(t2)
{
	Attach(t1->coef*t2->coef,t1->expon+t2->expon,&Rear);
	t2 = t2->link;
}
```

​	<2> 逐项插入

​			将P1当前项（c1<sub>i</sub> ,e1<sub>i</sub> ) 乘P2当前项（c2<sub>i</sub> ,e2<sub>i</sub> )，并插入到结果多项式中。关键是要找到插入位置。其中结果多项式的初始位置可由P1的第一项乘以P2获得。 

```cpp
t1 = t1->link;
    while(t1)
    {
        t2 = P2;
        Rear = P;   // 每一次的t1指向的多项式乘以t2的每一项后，Rear都重新指回P的头结点，以便下一次得到的t1和t2的乘积插入P
        while(t2)
        {
            c = t1->coef * t2->coef;
            e = t1->expon + t2->expon;
            while (Rear->link && Rear->link->expon > e) // 当Rear的下一项不为空且下一项的指数比当前的乘积e大时，Rear后移
            {
                Rear = Rear->link;
            }
            if(Rear->link && Rear->link->expon == e)
            {
                if(Rear->link->coef+c)  // 指数相同时，结果系数不为零
                {
                    Rear->link->coef+=c;
                }
                else    // 指数相同时，结果系数为0
                {
                    t = Rear->link; // 将零项删除
                    Rear->link = t->link;
                    free(t);
                }
            }
            else    // 当前多项式的指数大于Rear的下一项
            {
                t = new PolyNode;
                t->coef = c;
                t->expon = e;
                t->link = Rear->link;
                Rear->link = t;
                Rear = Rear->link;
            }
            t2 = t2->link;
        }
        t1 = t1->link;
    }
```

④ 输出函数

​	要按照输出样例的格式输出，注意到输出的多项式每组系数和指数间都有空格，每个项之间也有空格，但是首尾没有空格。

​	可以把每组数据看作，“空格+系数+指数”，但是第一项没有空格，因此可以设置一个flag来控制输出，flag 初始值为0，在执行一步输出操作后置为1，只有flag为1时才输出空格。

```cpp
// 打印结果多项式函数
void PrintPoly(Polynomial P)
{
    bool flag = 0;
    if(!P)
    {
        cout << "0 0" << endl;
    }
    else
    {
        while(P)
        {
            if(!flag)
                flag = 1;
            else
                cout << " ";
            cout << P->coef << " " << P->expon;
            P = P->link;
        }
    }
    cout << endl;
}
```



### 代码：

```cpp
#include<bits/stdc++.h>
using namespace std;

typedef struct PolyNode *Polynomial;   // 多项式
struct PolyNode {
    int coef;   // 系数
    int expon;  // 指数
    Polynomial link;
};
void Attach(int c, int e, Polynomial *pRear);  // 链接结点到链表的函数
Polynomial ReadPoly(); // 读取多项式函数
Polynomial AddPoly(Polynomial P1, Polynomial P2);    // 多项式相加
Polynomial MultPoly(Polynomial P1, Polynomial P2);  // 多项式相乘
void PrintPoly(Polynomial P);   // 打印结果多项式函数

int main()
{
    Polynomial P1, P2, Pa, Pm;
    P1 = ReadPoly();
    P2 = ReadPoly();
    Pa = AddPoly(P1,P2);
    Pm = MultPoly(P1,P2);
    PrintPoly(Pm);
    PrintPoly(Pa);

    return 0;
}

// 链接函数
void Attach(int c, int e, Polynomial *pRear)
{
    Polynomial t = new PolyNode;
    t->coef = c;    // 给新节点赋值
    t->expon = e;
    t->link = NULL;
    (*pRear)->link = t;
    *pRear = t; // 修改*pRear的值
}
// 读取多项式函数
Polynomial ReadPoly()
{
    Polynomial P = new PolyNode;  // 临时生成链表头空节点
    Polynomial Rear, t;
    P->link = NULL;
    Rear = P;
    int n, c, e;
    cin >> n;
    while(n--)
    {
        cin >> c >> e;
        Attach(c,e,&Rear);
    }
    t = P;  // 删除临时生成的头节点
    P = P->link;
    free(t);

    return P;
}
// 多项式相加函数
Polynomial AddPoly(Polynomial P1, Polynomial P2)
{

    Polynomial Rear, t1, t2, t;
    t1 = P1;
    t2 = P2;
    Polynomial P = new PolyNode;
    P->link = NULL;
    Rear = P;
    while(t1 && t2)
    {
        if(t1->expon == t2->expon)
        {
            int sum = t1->coef + t2->coef;
            if(sum != 0)
            {
                Attach(sum,t1->expon,&Rear);
                t1 = t1->link;
                t2 = t2->link;
            }
            else
            {
                t1 = t1->link;
                t2 = t2->link;
            }

        }
        else if(t1->expon > t2->expon)
        {
            Attach(t1->coef,t1->expon,&Rear);
            t1 = t1->link;
        }
        else if(t1->expon < t2->expon)
        {
            Attach(t2->coef,t2->expon,&Rear);
            t2 = t2->link;
        }
    }
    for(;t1;t1=t1->link)    Attach(t1->coef,t1->expon,&Rear);
    for(;t2;t2=t2->link)    Attach(t2->coef,t2->expon,&Rear);
    Rear->link = NULL;
    t = P;
    P = P->link;
    free(t);

    return P;
}
// 多项式相乘函数
Polynomial MultPoly(Polynomial P1, Polynomial P2)
{
    int c, e;
    if(!P1 || !P2)  return NULL;    // 判断两个多项式都不为零多项式
    Polynomial t, t1, t2, Rear;
    t1 = P1;    t2 = P2;
    Polynomial P = new PolyNode;
    P->link = NULL;
    Rear = P;
    while(t2)   // 先用P1的第一项乘以P2作为结果多项式的初始值
    {
        Attach(t1->coef*t2->coef,t1->expon+t2->expon,&Rear);
        t2 = t2->link;
    }
    t1 = t1->link;
    while(t1)
    {
        t2 = P2;
        Rear = P;   // 每一次的t1指向的多项式乘以t2的每一项后，Rear都重新指回P的头结点，以便下一次得到的t1和t2的乘积插入P
        while(t2)
        {
            c = t1->coef * t2->coef;
            e = t1->expon + t2->expon;
            while (Rear->link && Rear->link->expon > e) // 当Rear的下一项不为空且下一项的指数比当前的乘积e大时，Rear后移
            {
                Rear = Rear->link;
            }
            if(Rear->link && Rear->link->expon == e)
            {
                if(Rear->link->coef+c)  // 指数相同时，结果系数不为零
                {
                    Rear->link->coef+=c;
                }
                else    // 指数相同时，结果系数为0
                {
                    t = Rear->link; // 将零项删除
                    Rear->link = t->link;
                    free(t);
                }
            }
            else    // 当前多项式的指数大于Rear的下一项
            {
                t = new PolyNode;
                t->coef = c;
                t->expon = e;
                t->link = Rear->link;
                Rear->link = t;
                Rear = Rear->link;
            }
            t2 = t2->link;
        }
        t1 = t1->link;
    }
    t2 = P;
    P = P->link;
    free(t2);

    return P;
}
// 打印结果多项式函数
void PrintPoly(Polynomial P)
{
    bool flag = 0;
    if(!P)  // 注意判断P是否为0多项式
    {
        cout << "0 0";
    }
    else
    {
        while(P)
        {
            if(!flag)
                flag = 1;
            else
                cout << " ";
            cout << P->coef << " " << P->expon;
            P = P->link;
        }
    }
    cout << endl;
}
```

---

# **02-线性结构3 Reversing Linked List**

## 题目要求

Given a constant *K* and a singly linked list *L*, you are supposed to reverse the links of every *K* elements on *L*. For example, given *L* being 1→2→3→4→5→6, if *K*=3, then you must output 3→2→1→6→5→4; if *K*=4, you must output 4→3→2→1→5→6.

### Input Specification:

Each input file contains one test case. For each case, the first line contains the address of the first node, a positive *N* (≤10<sup>5</sup>) which is the total number of nodes, and a positive *K* (≤*N*) which is the length of the sublist to be reversed. The address of a node is a 5-digit nonnegative integer, and NULL is represented by -1.

Then *N* lines follow, each describes a node in the format:

```
Address Data Next
```

where `Address` is the position of the node, `Data` is an integer, and `Next` is the position of the next node.

### Output Specification:

For each case, output the resulting ordered linked list. Each node occupies a line, and is printed in the same format as in the input.

### Sample Input:

```in
00100 6 4
00000 4 99999
00100 1 12309
68237 6 -1
33218 3 00000
99999 5 68237
12309 2 33218
```

### Sample Output:

```out
00000 4 33218
33218 3 12309
12309 2 00100
00100 1 99999
99999 5 68237
68237 6 -1
```

### Solution:

题目大致要求是做一个链表的部分反转，要求按照`Address Data Next`的格式进行输入，其中Address就是当前节点的位置，Data是节点存储的数据，Next是下一节点的位置。第一行输入的是链表头结点的位置以及节点的总数还有反转子链的长度K。最后也按照`Address Data Next`的格式输出，但是是按照部分反转后的顺序输出。

根据题目要求搭建程序框架：

```
int main()
{
	输入链表
	链表部分反转操作
	输出结果链表
}
```

因此，大体上需要设计输入，反转，输出三个功能的函数。

① 链表的输入函数

首先观察题目的输入样例，发现数据是用`Address` 和 `Next` 来实现链表功能，因此可以用结构体数组来存储数据，实现链表功能。

```cpp
#define MAXLEN 100005

struct Node {
    int Data;   // 数据
    int Next;   // 下一节点地址下标
};
typedef struct Node Link;
Link LinkList[MAXLEN];
```

按照题目要求，要按照`Address Data Next` 的格式输入节点数据，而第一行输入略有不同，第一行输入的是链表头结点的位置以及节点的总数还有部分翻转的计数K。根据以上要求，设计链表的输入函数。

```cpp
// 输入函数，返回链表头结点位置下标
int ReadList(Link &L)
{
    int Head, n, k; // 起始节点的位置下标，节点总数，反转子链的长度
    int address, num, next;
    cin >> Head >> n >> k;
    while(n--)
    {
        cin >> address >> num >> next;
        L[address].Data = num;
        L[address].Next = next;
    }

    return Head;
}
```

② 链表的反转操作

该部分是解题的关键，也是难点所在。

注意这里有一个坑，测试点中有多余节点不在链表上的情况，因此添加一个计数函数，统计链表有效节点，排除不在链表上的离散干扰节点的影响。

```cpp
// 计数函数，统计链表有效节点，排除不在链表上的离散干扰节点的影响
int count(Link L[], int head)
{
    int cnt = 1;
    for(int i = head; L[i].Next != -1; i = L[i].Next)
        cnt++;

    return  cnt;

}
```

执行反转的操作关键在于将下一个结点的 Next 变为上一个节点，但是这样又会导致丢失下一个节点中原来 Next 的位置，因此需要三个指针来完成反转操作。前两个指针用来进行反转，第三个指针临时存储下一节点位置。同时由于反转子链后需要连接下一段子链，因此需要记录上一段子链最后一个节点的位置以及下一段子链第一个节点的位置。

这里也有小坑，注意考虑 K=1的情况，此时不需要反转。

```cpp
// 链表反转函数
void ReverseList(Link L[], int &head, int k)  // 执行反转操作的链表，链表的头结点位置下标，反转子链的长度
{
    if(k == 1)
        return;
    int cnt = count(L,head);    // 链表有效长度
    int p1, p2, p3; // p1和p2用来执行反转操作，p3临时存储下一节点位置
    int nextHead = head;   // 标记下一段反转子链的第一个结点
    int lastEnd = -2;   // 标记上一段反转子链的最后一个节点，因为-1是链表结束标志，因此赋初值wei-2
    bool first = 1; // 标记第一条反转子链，需要将反转后的第一个子链的头结点赋值为整个链表的头结点
    while(cnt >= k)
    {
        p1 = nextHead;
        p2 = L[p1].Next;
        for(int i=1; i<k; i++)
        {
            p3 = L[p2].Next;    // 将p2的下一个结点位置存储
            L[p2].Next = p1;    // 反转p1与p2的前后顺序，将p2的下一个结点指向p1
            p1 = p2;    // 向后遍历，指针后移
            p2 = p3;
        }
        L[nextHead].Next = p3;  // 链接反转子链与下一段子链
        if(first)    // 如果是第一条反转子链
        {
            head = p1;  // 将反转后的第一个子链的头结点赋值为整个链表的头结点
            lastEnd = nextHead; // 反转子链之前的第一个结点变成最后一个节点
        }

        else
        {
            L[lastEnd].Next = p1;   // 开始新的反转子链
            lastEnd = nextHead;
        }
        nextHead = p2;
        first = 0;
        cnt -= k;
    }
}
```

③ 打印链表的函数

因为输出限制了格式需要将地址按照5位输出，并且空位补零，因此使用C语言的prinf()函数控制格式，“%05d"控制5位补零。但是由于最后一个结点的Next为-1，因此需要另外单独控制输出。

```cpp
// 打印链表函数
void PrintList(Link L[], int head)
{
    int i = head;
    while(L[i].Next != -1)
    {
        printf("%05d %d %05d\n", i, L[i].Data, L[i].Next);
        i = L[i].Next;
    }
    printf("%05d %d %d\n",i, L[i].Data, L[i].Next);
}
```



### Code

```cpp
#include<bits/stdc++.h>
using namespace std;

#define MAXLEN 100005

struct Node {
    int Data;   // 数据
    int Next;   // 下一节点地址下标
};
typedef struct Node Link;
Link LinkList[MAXLEN];  // 结构体数组表示链表
int k, head;    // 反转子链长度，链表头

int ReadList(Link[]);    // 输入函数
void ReverseList(Link L[], int &head, int k); // 链表反转函数
void PrintList(Link L[], int head);   // 打印链表

int main()
{
    head = ReadList(LinkList);
    ReverseList(LinkList, head, k);
    PrintList(LinkList,head);

    return 0;
}

// 输入函数，返回链表头结点位置下标
int ReadList(Link L[])
{
    int Head, n; // 起始节点的位置下标，节点总数，反转子链的长度
    int address, num, next;
    cin >> Head >> n >> k;
    while(n--)
    {
        cin >> address >> num >> next;
        L[address].Data = num;
        L[address].Next = next;
    }

    return Head;
}
// 计数函数，统计链表有效节点，排除不在链表上的离散干扰节点的影响
int count(Link L[], int head)
{
    int cnt = 1;
    for(int i = head; L[i].Next != -1; i = L[i].Next)
        cnt++;

    return  cnt;

}
// 链表反转函数
void ReverseList(Link L[], int &head, int k)  // 执行反转操作的链表，链表的头结点位置下标，反转子链的长度
{
    if(k == 1)
        return;
    int cnt = count(L,head);    // 链表有效长度
    int p1, p2, p3; // p1和p2用来执行反转操作，p3临时存储下一节点位置
    int nextHead = head;   // 标记下一段反转子链的第一个结点
    int lastEnd = -2;   // 标记上一段反转子链的最后一个节点，因为-1是链表结束标志，因此赋初值wei-2
    bool first = 1; // 标记第一条反转子链，需要将反转后的第一个子链的头结点赋值为整个链表的头结点
    while(cnt >= k)
    {
        p1 = nextHead;
        p2 = L[p1].Next;
        for(int i=1; i<k; i++)
        {
            p3 = L[p2].Next;    // 将p2的下一个结点位置存储
            L[p2].Next = p1;    // 反转p1与p2的前后顺序，将p2的下一个结点指向p1
            p1 = p2;    // 向后遍历，指针后移
            p2 = p3;
        }
        L[nextHead].Next = p3;  // 链接反转子链与下一段子链
        if(first)    // 如果是第一条反转子链
        {
            head = p1;  // 将反转后的第一个子链的头结点赋值为整个链表的头结点
            lastEnd = nextHead; // 反转子链之前的第一个结点变成最后一个节点
        }

        else
        {
            L[lastEnd].Next = p1;   // 开始新的反转子链
            lastEnd = nextHead;
        }
        nextHead = p2;
        first = 0;
        cnt -= k;
    }
}
// 打印链表函数
void PrintList(Link L[], int head)
{
    int i = head;
    while(L[i].Next != -1)
    {
        printf("%05d %d %05d\n", i, L[i].Data, L[i].Next);
        i = L[i].Next;
    }
    printf("%05d %d %d\n",i, L[i].Data, L[i].Next);
}
```

---

# **02-线性结构4 Pop Sequence**

## 题目要求

Given a stack which can keep *M* numbers at most. Push *N* numbers in the order of 1, 2, 3, ..., *N* and pop randomly. You are supposed to tell if a given sequence of numbers is a possible pop sequence of the stack. For example, if *M* is 5 and *N* is 7, we can obtain 1, 2, 3, 4, 5, 6, 7 from the stack, but not 3, 2, 1, 7, 5, 6, 4.

### Input Specification:

Each input file contains one test case. For each case, the first line contains 3 numbers (all no more than 1000): *M* (the maximum capacity of the stack), *N* (the length of push sequence), and *K* (the number of pop sequences to be checked). Then *K* lines follow, each contains a pop sequence of *N* numbers. All the numbers in a line are separated by a space.

### Output Specification:

For each pop sequence, print in one line "YES" if it is indeed a possible pop sequence of the stack, or "NO" if not.

### Sample Input:

```in
5 7 5
1 2 3 4 5 6 7
3 2 1 7 5 6 4
7 6 5 4 3 2 1
5 6 4 3 7 2 1
1 7 6 5 4 3 2
```

### Sample Output:

```out
YES
NO
NO
YES
NO
```

### Solution：

题目要求是按照1, 2, 3, ..., *N* 的顺序将数字压入栈中，但是会随机在某一时刻弹出，要求我们可以判断给出的出栈序列是否符合栈的入栈出栈规范，是否可能实现，是的话输出 “YES”，否输出“NO”。

解决方法主要就是模拟入栈和出栈的过程，在此过程中一旦发现违反栈 “先进后出” 的规则（栈顶元素大于当前弹出元素），或者栈溢出，就证明该种情况不成立；若将给出的测试序列全部走完未发现问题，则证明该种情况可能实现。

程序第一行要求输入三个数据：M（栈的最大容量）、N（每个输入序列的数据个数）、K（待测试的序列个数），接下来输入K行待检测序列，开始模拟入栈出栈操作。

### Code：

```cpp
#include <bits/stdc++.h>
using namespace std;

int main()
{
    int m, n, k;    // 堆栈最大容量，序列元素个数，待检测序列个数
    cin >> m >> n >> k;
    int stack[1005] = {0};    // 数组做堆栈，但是这里的数组大小不是堆栈的最大容量
    int top = -1;   // 栈顶指针
    int cnt = 1;    // 入栈元素
    int num;    // 序列输入元素
    bool flag = 1;  // 序列实现可能性的标记
    for(int i=0; i<k; i++)
    {
        flag = 1;   // 重置标记
        top = 0;   // 清空堆栈后重新将1压入堆栈
        stack[top] = 1;
        cnt = 1;    // 入栈元素重置
        for(int j=0; j<n; j++)
        {
            cin >> num;
            if(stack[top] > num)    // 比栈顶元素小，必然不可能
            {
                flag = 0;
                continue;
            }
            while(stack[top] < num) // 比栈顶元素大
            {
                top++;  // 要继续压入数据
                cnt++;
                stack[top] = cnt;
            }
            if(top > m-1)   // 栈溢出
            {
                flag = 0;
                continue;
            }
            if(stack[top] == num)   // 等于栈顶元素，出栈
            {
                top--;
            }
            if(top < 0) // 栈内元素全部出栈
            {
                top++;
                cnt++;
                stack[top] = cnt;
            }
        }
        if(flag)
            cout << "YES" << endl;
        else
            cout << "NO" << endl;
    }

    return 0;
}
```

---

# **03-树1 树的同构**

## 题目要求

给定两棵树T1和T2。如果T1可以通过若干次左右孩子互换就变成T2，则我们称两棵树是“同构”的。例如图1给出的两棵树就是同构的，因为我们把其中一棵树的结点A、B、G的左右孩子互换后，就得到另外一棵树。而图2就不是同构的。

![fig1.jpg](https://images.ptausercontent.com/0c8bbacf-d64e-4c6d-8d4e-1249e33fb0b1.jpg)

<center>图1</center>

![img](https://images.ptausercontent.com/29)

<center>图2</center>

现给定两棵树，请你判断它们是否是同构的。

### 输入格式:

输入给出2棵二叉树树的信息。对于每棵树，首先在一行中给出一个非负整数*N* (≤10)，即该树的结点数（此时假设结点从0到*N*−1编号）；随后*N*行，第*i*行对应编号第*i*个结点，给出该结点中存储的1个英文大写字母、其左孩子结点的编号、右孩子结点的编号。如果孩子结点为空，则在相应位置上给出“-”。给出的数据间用一个空格分隔。注意：题目保证每个结点中存储的字母是不同的。

### 输出格式:

如果两棵树是同构的，输出“Yes”，否则输出“No”。

### 输入样例1（对应图1）：

```in
8
A 1 2
B 3 4
C 5 -
D - -
E 6 -
G 7 -
F - -
H - -
8
G - 4
B 7 6
F - -
A 5 1
H - -
C 0 -
D - -
E 2 -
```

### 输出样例1:

```out
Yes
```

### 输入样例2（对应图2）：

```
8
B 5 7
F - -
A 0 3
C 6 -
H - -
D - -
G 4 -
E 1 -
8
D 6 -
B 5 -
E - -
H - -
C 0 2
G - 3
F - -
A 1 4
```

### 输出样例2:

```out
No
```

### 解题思路：

题目要求判断按格式输入的两个二叉树是否同构，其实就是判断每个结点的左右子树是否相同（左右相同或者左左和右右分别相同），根据题目要求搭建程序框架：

```cpp
int main() {
	建二叉树1
	建二叉树2
	判断是否同构并输出
	
	return 0；
}
```

本题的程序框架较为简单，根据框架需要设计 `读数据建二叉树`、`二叉树同构判断`这两个函数。

① 读数据建二叉树的函数

根据题目的输入要求，我们可以使用静态链表即结构体数组的方式来存储二叉树。

```cpp
struct treeNode {
    char data;
    int left;
    int right;
}T1[10],T2[10];
```

输入样例中可以看到，二叉树根结点的位置不一定位于输入的第一行，因此在建二叉树的过程中，找出所建二叉树的根结点很关键。以简单的四个结点的二叉树为例：

![image-20210820222827421](C:\Users\79485\AppData\Roaming\Typora\typora-user-images\image-20210820222827421.png)

可以观察到，在结构体数组中四个结点分别占用了 0、1、2、3四个下标位置，而结构体数组中左右子树指向的下标位置有 1、2、3，没有出现的 0 就是根结点所在的位置下标。因此，我们在建树的过程中寻找根结点的位置就可以通过遍历结构体数组中左右子树指向的下标，缺少的那个下标即是根结点所在下标。遍历的方法可以是创建一个 `check[]` 数组，初始化所有位置数值都为 0，然后在所有输入的左右子树的下标对应的 `check[]` 数值记为 1，最后遍历 `check[]` 数组找到数值为 0 的下标即是根结点所在位置。

```cpp
// 读取数据建二叉树的函数，返回根结点的下标
int buildTree(Tree t[])
{
    int N, root = -1;    // 结点数量，根结点位置下标
    cin >> N;
    char l, r;
    int check[10] = {0};
    for(int i=0; i<N; i++)
    {
        cin >> t[i].data >> l >> r;
        if (l != '-')
        {
            t[i].left = l - '0';    // 将字符转化为整型
            check[t[i].left] = 1;
        }
        else
            t[i].left = -1;
        if (r != '-')
        {
            t[i].right = r - '0';    // 将字符转化为整型
            check[t[i].right] = 1;
        }
        else
            t[i].right = -1;
    }
    for(int i=0; i<N; i++)
    {
        if(!check[i])
        {
            root = i;
            break;
        }
    }

    return root;
}
```

② 判断两二叉树是否同构的函数

判断二叉树是否同构就是判断两个二叉树的左右子树是否相同（可以是左左相同且右右相同，也可以是左右相同），使用递归判断就可以得出结论，但是要注意判断时要将各种情况考虑全面，包括子树为空时的各种情况。

```cpp
// 判断两二叉树是否同构的函数
int Isomorphic(int t1, int t2)
{
    if (t1 == -1 && t2 == -1)  // 均为空树
        return 1;
    if ((t1 == -1 && t2 != -1) || (t1 != -1 && t2 == -1))    // 两树中有一棵空树
        return 0;
    if (T1[t1].data != T2[t2].data)  // 根结点数据不同
        return 0;
    if (T1[t1].left == -1 && T2[t2].left == -1)    // 左子树均为空
        return Isomorphic(T1[t1].right,T2[t2].right);
    if ((T1[t1].left != -1 && T2[t2].left != -1) && (T1[T1[t1].left].data == T2[T2[t2].left].data) ) // 无需交换左右子树
        return Isomorphic(T1[t1].left,T2[t2].left) && Isomorphic(T1[t1].right,T2[t2].right);
    else    // 需要交换左右子树
        return Isomorphic(T1[t1].left,T2[t2].right) && Isomorphic(T1[t1].right,T2[t2].left);
}
```

### 代码：

```cpp
#include<bits/stdc++.h>
using namespace std;

typedef struct treeNode Tree;

struct treeNode {
    char data;
    int left;
    int right;
}T1[10],T2[10];

int buildTree(Tree t[]);  // 读取数据建二叉树
int Isomorphic(int t1, int t2);   // 判断两个二叉树是否同构的函数

int main()
{
    int t1, t2;
    t1 = buildTree(T1);
    t2 = buildTree(T2);
    if(Isomorphic(t1,t2))
        cout << "Yes";
    else
        cout << "No";

    return 0;
}

// 读取数据建二叉树的函数，返回根结点的下标
int buildTree(Tree t[])
{
    int N, root = -1;    // 结点数量，根结点位置下标
    cin >> N;
    char l, r;
    int check[10] = {0};
    for(int i=0; i<N; i++)
    {
        cin >> t[i].data >> l >> r;
        if (l != '-')
        {
            t[i].left = l - '0';    // 将字符转化为整型
            check[t[i].left] = 1;
        }
        else
            t[i].left = -1;
        if (r != '-')
        {
            t[i].right = r - '0';    // 将字符转化为整型
            check[t[i].right] = 1;
        }
        else
            t[i].right = -1;
    }
    for(int i=0; i<N; i++)
    {
        if(!check[i])
        {
            root = i;
            break;
        }
    }

    return root;
}

// 判断两二叉树是否同构的函数
int Isomorphic(int t1, int t2)
{
    if (t1 == -1 && t2 == -1)  // 均为空树
        return 1;
    if ((t1 == -1 && t2 != -1) || (t1 != -1 && t2 == -1))    // 两树中有一棵空树
        return 0;
    if (T1[t1].data != T2[t2].data)  // 根结点数据不同
        return 0;
    if (T1[t1].left == -1 && T2[t2].left == -1)    // 左子树均为空
        return Isomorphic(T1[t1].right,T2[t2].right);
    if ((T1[t1].left != -1 && T2[t2].left != -1) && (T1[T1[t1].left].data == T2[T2[t2].left].data) ) // 无需交换左右子树
        return Isomorphic(T1[t1].left,T2[t2].left) && Isomorphic(T1[t1].right,T2[t2].right);
    else    // 需要交换左右子树
        return Isomorphic(T1[t1].left,T2[t2].right) && Isomorphic(T1[t1].right,T2[t2].left);
}
```

---

# **03-树2 List Leaves**

## 题目要求

Given a tree, you are supposed to list all the leaves in the order of top down, and left to right.

### Input Specification:

Each input file contains one test case. For each case, the first line gives a positive integer *N* (≤10) which is the total number of nodes in the tree -- and hence the nodes are numbered from 0 to *N*−1. Then *N* lines follow, each corresponds to a node, and gives the indices of the left and right children of the node. If the child does not exist, a "-" will be put at the position. Any pair of children are separated by a space.

### Output Specification:

For each test case, print in one line all the leaves' indices in the order of top down, and left to right. There must be exactly one space between any adjacent numbers, and no extra space at the end of the line.

### Sample Input:

```in
8
1 -
- -
0 -
2 7
- -
- -
5 -
4 6
```

### Sample Output:

```out
4 1 5
```

### Solution：

题目要求通过输入节点数以及每个节点的左儿子和右儿子，从上到下打印出叶节点。解题的关键是通过层序遍历找到叶子结点去输出。

根据题意搭建程序框架：

```cpp
int main() {
	输入建立二叉树
	寻找叶子结点并输出
}
```

程序的框架较为简单，只需要两个函数就可以完成题目要求，接下来分析两个函数的实现方法。

① 输入建立二叉树函数

根据题目的输入要求，我们可以使用静态链表即结构体数组的方式来存储二叉树。

```cpp
struct treeNode {
    char data;
    int left;
    int right;
}T[10];
```

输入的第几行就是代表该节点的值为几。例如样例输入中第0行的1 -代表值为0的节点左孩子的值为1，即指向第1行，右孩子为空（-1），建树应该是这样：![img](https://images0.cnblogs.com/blog2015/788978/201508/211847574724679.png)

二叉树根结点的位置不一定位于输入的第一行，因此在建二叉树的过程中，找出所建二叉树的根结点很关键。我们在建树的过程中寻找根结点的位置就可以通过遍历结构体数组中左右子树指向的下标，缺少的那个下标即是根结点所在下标。遍历的方法可以是创建一个 `check[]` 数组，初始化所有位置数值都为 0，然后在所有输入的左右子树的下标对应的 `check[]` 数值记为 1，最后遍历 `check[]` 数组找到数值为 0 的下标即是根结点所在位置。

```cpp
// 读取数据建二叉树的函数，返回根结点的下标
int buildTree(Tree t[])
{
    int root = -1;    // s根结点位置下标
    cin >> N;
    char l, r;
    int check[10] = {0};
    for(int i=0; i<N; i++)
    {
        t[i].data = i;
        cin >> l >> r;
        if (l != '-')
        {
            t[i].left = l - '0';    // 将字符转化为整型
            check[t[i].left] = 1;
        }
        else
            t[i].left = -1;
        if (r != '-')
        {
            t[i].right = r - '0';    // 将字符转化为整型
            check[t[i].right] = 1;
        }
        else
            t[i].right = -1;
    }
    for(int i=0; i<N; i++)
    {
        if(!check[i])
        {
            root = i;
            break;
        }
    }

    return root;
}
```

② 层序遍历打印叶子节点的函数

要按照从上到下，从左到右的顺序找到并输出叶子结点就要应用到层序遍历二叉树的方法，层序遍历就要用到队列，我这里选择直接调用 STL 的 队列（queue），而没有自己手写队列操作。

首先建立一个保存叶子结点的数组，初始化每个叶子结点为 -1。首先建立队列，将二叉树根结点入队。接着在队列非空时，执行层序遍历操作，若队首元素的左右孩子全部为空，则说明其是叶子结点，将他存入 leaves 数组中；若其子树非空，按照先左后右的顺序入队，然后队首元素出队，直至队列为空，则遍历完毕。

最后控制输出，因为输出的叶子结点之间都有空格，可以看做除了第一个元素外，都是输出“空格 + 叶子结点”，只需要设置标记将第一个输出的元素找到不输出空格就好。然后遍历输出 leaves 数组，即可得到结果。

```cpp
// 层序遍历打印叶子节点的函数
void PrintLeaves(int r, int n)
{
    int leaves[10];  // 存储叶结点的数组
    for (int i = 0; i < 10; ++i) {
        leaves[i] = -1;
    }
    int k = 0;
    int i;
    bool isFirst = 1;   // 控制输出
    queue<int> Q;  // 创建队列
    Q.push(r);   // 将根结点压入队列
    while(!Q.empty())   // 当队列非空时
    {
        i = Q.front();  // 队首元素
        Q.pop();
        if (T[i].left == -1 && T[i].right == -1)
            leaves[k++] = T[i].data;
        if (T[i].left != -1)
            Q.push(T[i].left);
        if (T[i].right != -1)
            Q.push(T[i].right);
    }
    int j = 0;
    while (leaves[j] != -1)
    {
        if (!isFirst)
            cout << " ";
        cout << leaves[j++];
        isFirst = 0;
    }
    cout << endl;
}

```

### Code：

```cpp
#include<bits/stdc++.h>
using namespace std;

typedef struct treeNode Tree;

struct treeNode {
    char data;
    int left;
    int right;
}T[10];
int N;  // 结点数量

int buildTree(Tree t[]);  // 读取数据建二叉树
void PrintLeaves(int r, int n);   // 判断两个二叉树是否同构的函数

int main()
{
    int r;
    r = buildTree(T);
    PrintLeaves(r,N);

    return 0;
}

// 读取数据建二叉树的函数，返回根结点的下标
int buildTree(Tree t[])
{
    int root = -1;    // s根结点位置下标
    cin >> N;
    char l, r;
    int check[10] = {0};
    for(int i=0; i<N; i++)
    {
        t[i].data = i;
        cin >> l >> r;
        if (l != '-')
        {
            t[i].left = l - '0';    // 将字符转化为整型
            check[t[i].left] = 1;
        }
        else
            t[i].left = -1;
        if (r != '-')
        {
            t[i].right = r - '0';    // 将字符转化为整型
            check[t[i].right] = 1;
        }
        else
            t[i].right = -1;
    }
    for(int i=0; i<N; i++)
    {
        if(!check[i])
        {
            root = i;
            break;
        }
    }

    return root;
}

// 层序遍历打印叶子节点的函数
void PrintLeaves(int r, int n)
{
    int leaves[10];  // 存储叶结点的数组
    for (int i = 0; i < 10; ++i) {
        leaves[i] = -1;
    }
    int k = 0;
    int i;
    bool isFirst = 1;   // 控制输出
    queue<int> Q;  // 创建队列
    Q.push(r);   // 将根结点压入队列
    while(!Q.empty())   // 当队列非空时
    {
        i = Q.front();  // 队首元素
        Q.pop();
        if (T[i].left == -1 && T[i].right == -1)
            leaves[k++] = T[i].data;
        if (T[i].left != -1)
            Q.push(T[i].left);
        if (T[i].right != -1)
            Q.push(T[i].right);
    }
    int j = 0;
    while (leaves[j] != -1)
    {
        if (!isFirst)
            cout << " ";
        cout << leaves[j++];
        isFirst = 0;
    }
    cout << endl;
}
```

---

# 03-树3 Tree Traversals Again

## 题目要求

An inorder binary tree traversal can be implemented in a non-recursive way with a stack. For example, suppose that when a 6-node binary tree (with the keys numbered from 1 to 6) is traversed, the stack operations are: push(1); push(2); push(3); pop(); pop(); push(4); pop(); pop(); push(5); push(6); pop(); pop(). Then a unique binary tree (shown in Figure 1) can be generated from this sequence of operations. Your task is to give the postorder traversal sequence of this tree.

![img](https://images.ptausercontent.com/30)

<center>Figure 1</center>

### Input Specification:

Each input file contains one test case. For each case, the first line contains a positive integer *N* (≤30) which is the total number of nodes in a tree (and hence the nodes are numbered from 1 to *N*). Then 2*N* lines follow, each describes a stack operation in the format: "Push X" where X is the index of the node being pushed onto the stack; or "Pop" meaning to pop one node from the stack.

### Output Specification:

For each test case, print the postorder traversal sequence of the corresponding tree in one line. A solution is guaranteed to exist. All the numbers must be separated by exactly one space, and there must be no extra space at the end of the line.

### Sample Input:

```in
6
Push 1
Push 2
Push 3
Pop
Pop
Push 4
Pop
Pop
Push 5
Push 6
Pop
Pop
```

### Sample Output:

```out
3 4 2 6 5 1
```

### Solution：

题目的大意是利用堆栈可以实现一个二叉树的非递归遍历操作，仔细观察题目所给样例
