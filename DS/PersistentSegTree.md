### 一、模板类别

​	数据结构：可持久化线段树

### 二、模板功能

#### 1.建立线段树

1. 数据类型

   模板参数 `typename _Tp` ，表示元素类型。

   模板参数 `typename _Operation`  ，表示区间操作函数的类，默认为 `std::plus<_Tp>` ，也就是加法。

   构造参数 `uint64_t __length` ，表示线段树的覆盖范围为 `[0, __length-1]`。

   构造参数 `_Operation __op` ，表示具体的区间操作函数。默认为 `_Operation` 类的默认实例。本参数接收类型有：普通函数，函数指针，仿函数，匿名函数，泛型函数。

   构造参数 `_Tp __defaultValue` ，表示元素默认值，默认值为 `_Tp` 类的默认实例。

2. 时间复杂度

   $O(1)$ 。

3. 备注

   可持久化线段树可以用于对某个时间某个坐标的单点修改，单点/区间查询。
   
   时间维度，常被称为“版本”，初始化后的版本号为 `0` ，以后每次有修改时自动向后生成一个版本号。
   
   坐标维度的范围为 `[0, __length)​` 。
   
   建树采用动态开点的方法，而非堆式建树，占用空间很大。
   
   **注意：** `__defaultValue` 需要满足：`__op(__defaultValue, __defaultValue)==__defaultValue` 。由于 `_Tp` 类型不一定支持相等运算符，所以 `_check` 函数默认处于注释掉的状态。如果取消注释，则会在建树时进行检查。
   
   **注意：**以下大部分方法传参时都可以用 `-1` 来表示**上一个版本**的版本号。（对版本区间进行查询的方法不能使用 `-1` 来表示版本号，因为需要两个版本号来确定版本区间，`-1`的引入会导致混乱。）

#### 2.建立线段树

1. 数据类型

   构造参数 `_Iterator __first` ，表示区间维护的区间头。

   构造参数 `_Iterator __last` ，表示区间维护的区间尾。（开区间）

   其它同上。

2. 时间复杂度

   $O(n)$。

3. 备注

   同上。

   使用迭代器进行初始化，可以将区间初状态直接赋到线段树里。

   如果通过本方法构造线段树，请确认区间范围不是特别大（$10^6$ 以内）。本方法将一次性开出树中所有结点。

#### 3.设置内存池大小

1. 数据类型

   输入参数 `uint32_t __nodeCount`，表示程序运行最多用到的结点数量。

2. 时间复杂度

   $O(1)$

#### 4.重置

1. 数据类型

   输入参数 `uint64_t __length` ，表示线段树要处理的区间大小。

2. 时间复杂度

   $O(1)$ 。

3. 备注

   调用本函数会将线段树大小改变，并将之前的合并信息重置。版本号重新从 `0` 开始计。

#### 5.重置

1. 数据类型

   输入参数 `_Iterator __first` ，表示区间维护的区间头。

   输入参数 `_Iterator __last` ，表示区间维护的区间尾。（开区间）

2. 时间复杂度

   $O(n)$。

3. 备注

   同上。

   使用迭代器进行重置，可以将区间初状态直接赋到线段树里。版本号重新从 `0` 开始计。

   如果通过本方法重置线段树，请确认区间范围不是特别大（$10^6$ 以内）。本方法将一次性开出树中所有结点。

#### 6.复制版本

1. 数据类型

   输入参数 `uint32_t __prevVersion` ，表示要复制哪个版本。

2. 时间复杂度

   $O(1)$ 。

3. 备注

   本方法生成某个版本的副本，保存为下一个版本号。
   
   本函数没有进行参数检查，所以请自己确保版本号合法。（位于 `[-1, versionCount())` ）

#### 7.单点赋值

1. 数据类型

   输入参数 `uint32_t __prevVersion` ，表示以哪个版本为基础进行修改。

   输入参数 `uint64_t __i​` ，表示单点赋值的下标。

   输入参数 `_Tp __val​` ，表示赋的值。

2. 时间复杂度

   $O(\log n)$ 。
   
   本函数没有进行参数检查，所以请自己确保版本号合法。（位于 `[-1, versionCount())` ）
   
   本函数没有进行参数检查，所以请自己确保下标合法。（位于 `[0, n)` ）

#### 8.单点增值

1. 数据类型

   输入参数 `uint32_t __prevVersion` ，表示以哪个版本为基础进行修改。

   输入参数 `uint64_t __i` ，表示单点增值的下标。

   输入参数 `_Tp __inc​` ，表示增量大小。

2. 时间复杂度

   $O(\log n)$ 。
   
3. 备注

   本函数没有进行参数检查，所以请自己确保版本号合法。（位于 `[-1, versionCount())` ）

   本函数没有进行参数检查，所以请自己确保下标合法。（位于 `[0, n)` ）

#### 9.单点查询

1. 数据类型

   输入参数 `uint32_t __version` ，表示查询哪个版本。

   输入参数 `uint64_t __i` ，表示查询的下标。

2. 时间复杂度

   $O(\log n)$ 。
   
3. 备注

   本函数没有进行参数检查，所以请自己确保版本号合法。（位于 `[-1, versionCount())` ）

   本函数没有进行参数检查，所以请自己确保下标合法。（位于 `[0, n)` ）


#### 10.区间查询

1. 数据类型

   输入参数 `uint32_t __version` ，表示查询哪个版本。

   输入参数 `uint64_t __left` ，表示区间查询的开头下标。

   输入参数 `uint64_t __right`，表示区间查询的结尾下标。(闭区间)

2. 时间复杂度

   $O(\log n)$ 。
   
3. 备注

   本函数没有进行参数检查，所以请自己确保版本号合法。（位于 `[-1, versionCount())` ）

   本函数没有进行参数检查，所以请自己确保下标合法。（位于 `[0, n)` ）

#### 11.查询全部

1. 数据类型

   输入参数 `uint32_t __version` ，表示查询哪个版本。

2. 时间复杂度

   $O(1)$ 。
   
3. 备注

   本函数没有进行参数检查，所以请自己确保版本号合法。（位于 `[-1, versionCount())` ）

#### 12.树上二分查询右边界

1. 数据类型

   输入参数 `uint32_t __version` ，表示查询哪个版本。

   输入参数 `uint64_t __left` ，表示左边界。

   输入参数 `_Judge __judge` ，表示需要满足的判断条件。

   返回类型 `uint64_t` ，表示在满足条件情况下的最大右边界。

2. 时间复杂度

   $O(\log n)$ 。

3. 备注

   假设本函数返回 `r` ，则表示，对于 `i∈[__left, r]`  ，均有 `__judge(__query(__left, i))` 为真。而当 `i>r` 时，有 `__judge(__query(__left, i))` 为假。显然，`r` 的最大值为 `m_length-1` 。

   如果从 `__left` 开始，即使长度为一的区间也不能满足判断条件，那么返回 `__left-1`  。所以 `r` 的最小值为 `__left-1` 。

   本函数没有进行参数检查，所以请自己确保下标合法。（位于`[0，n)`）

#### 13.树上二分查询左边界

1. 数据类型

   输入参数 `uint32_t __version` ，表示查询哪个版本。

   输入参数 `uint64_t __right` ，表示右边界。

   输入参数 `_Judge __judge` ，表示需要满足的判断条件。

   返回类型 `uint64_t` ，表示在满足条件情况下的最小左边界。

2. 时间复杂度

   $O(\log n)$ 。

3. 备注

   假设本函数返回 `l` ，则表示，对于 `i∈[l, __right]`  ，均有 `__judge(__query(i, __right))` 为真。而当 `i<l` 时，有 `__judge(__query(i, __right))` 为假。显然，`l` 的最小值为 `0` 。

   如果从 `__right` 开始往左走，即使长度为一的区间也不能满足判断条件，那么返回 `__right+1`  。所以 `l` 的最大值为 `__right+1` 。

   本函数没有进行参数检查，所以请自己确保下标合法。（位于`[0，n)`）

   **注意：**在树上二分的时候，需确保没有未初始化的结点。

#### 14.查询某个版本第k个元素

1. 数据类型

   输入参数 `uint32_t __version` ，表示查询哪个版本。

   输入参数 `_Tp __k` ，表示要查询的元素从小到大的顺次。

2. 时间复杂度

   $O(\log n)$ 。

3. 备注

   顺次 $k$ 表示第 $k+1$ 小，顺次 $0$ 表示查询最小的元素。

   只有在线段树的元素类型 $T$ 为数字，且累积函数为加法的时候，本函数才有意义。
   
   本函数没有进行参数检查，所以请自己确保版本号合法。（位于 `[-1, versionCount())` ）
   
   本函数没有进行参数检查，所以请自己确保 `__k` 合法。（位于 `[0, queryAll(__version))` ）

#### 15.查询某个版本区间的单点值

1. 数据类型

   输入参数 `uint32_t __leftVersion` ，表示查询版本区间的起点。

   输入参数 `uint32_t __rightVersion` ，表示查询版本区间的末尾。（闭区间）

   输入参数 `uint64_t __i` ，表示查询的下标。

2. 时间复杂度

   $O(\log n)$ 。

3. 备注

   只有在线段树的元素类型 $T$ 为数字，且累积函数为加法的时候，本函数才有意义。

   函数采用差分，用两个版本的信息相减来获取之间的区间信息。
   
   本函数没有进行参数检查，所以请自己确保版本号合法。（位于 `[-1, versionCount())` ）
   
   本函数没有进行参数检查，所以请自己确保下标合法。（位于 `[0, n)` ）

#### 16.查询某个版本区间的区间和值

1. 数据类型

   输入参数 `uint32_t __leftVersion` ，表示查询版本区间的起点。

   输入参数 `uint32_t __rightVersion` ，表示查询版本区间的末尾。（闭区间）

   输入参数 `uint64_t __left` ，表示区间查询的开头下标。

   输入参数 `uint64_t __right`，表示区间查询的结尾下标。(闭区间)

2. 时间复杂度

   $O(\log n)$ 。

3. 备注

   只有在线段树的元素类型 $T$ 为数字，且累积函数为加法的时候，本函数才有意义。

   函数采用差分，用两个版本的信息相减来获取之间的区间信息。
   
   本函数没有进行参数检查，所以请自己确保版本号合法。（位于 `[-1, versionCount())` ）
   
   本函数没有进行参数检查，所以请自己确保下标合法。（位于 `[0, n)` ）

#### 17.在某个版本区间树上二分查询右边界

1. 数据类型

   输入参数 `uint32_t __leftVersion` ，表示查询版本区间的起点。

   输入参数 `uint32_t __rightVersion` ，表示查询版本区间的末尾。（闭区间）

   输入参数 `uint64_t __left` ，表示左边界。

   输入参数 `_Judge __judge` ，表示需要满足的判断条件。

   返回类型 `uint64_t` ，表示在满足条件情况下的最大右边界。

2. 时间复杂度

   $O(\log n)$ 。

3. 备注

   只有在线段树的元素类型 $T$ 为数字，且累积函数为加法的时候，本函数才有意义。

   函数采用差分，用两个版本的信息相减来获取之间的区间信息。

   本函数没有进行参数检查，所以请自己确保版本号合法。（位于 `[-1, versionCount())` ）

   本函数没有进行参数检查，所以请自己确保下标合法。（位于 `[0, n)` ）

   **注意：**在树上二分的时候，需确保没有未初始化的结点。

#### 18.在某个版本区间树上二分查询左边界

1. 数据类型

   输入参数 `uint32_t __leftVersion` ，表示查询版本区间的起点。

   输入参数 `uint32_t __rightVersion` ，表示查询版本区间的末尾。（闭区间）

   输入参数 `uint64_t __right` ，表示右边界。

   输入参数 `_Judge __judge` ，表示需要满足的判断条件。

   返回类型 `uint64_t` ，表示在满足条件情况下的最小左边界。

2. 时间复杂度

   $O(\log n)$ 。

3. 备注

   只有在线段树的元素类型 $T$ 为数字，且累积函数为加法的时候，本函数才有意义。

   函数采用差分，用两个版本的信息相减来获取之间的区间信息。

   本函数没有进行参数检查，所以请自己确保版本号合法。（位于 `[-1, versionCount())` ）

   本函数没有进行参数检查，所以请自己确保下标合法。（位于 `[0, n)` ）

   **注意：**在树上二分的时候，需确保没有未初始化的结点。

#### 19.查询某个版本区间第k个元素

1. 数据类型

   输入参数 `uint32_t __leftVersion` ，表示查询版本区间的起点。

   输入参数 `uint32_t __rightVersion` ，表示查询版本区间的末尾。（闭区间）

   输入参数 `_Tp __k` ，表示要查询的元素从小到大的顺次。

2. 时间复杂度

   $O(\log n)$ 。

3. 备注

   顺次 $k$ 表示第 $k+1$ 小，顺次 $0$ 表示查询最小的元素。

   只有在线段树的元素类型 $T$ 为数字，且累积函数为加法的时候，本函数才有意义。

   函数采用差分，用两个版本的信息相减来获取之间的区间信息。
   
   本函数没有进行参数检查，所以请自己确保版本号合法。（位于 `[-1, versionCount())` ）
   
   本函数没有进行参数检查，所以请自己确保 `__k` 合法。

#### 20.查询版本数量

1. 数据类型

2. 时间复杂度

   $O(1)$ 。

3. 备注

   可以用来查询版本的数量，包括最初的版本 `0` 。

### 三、模板示例

```c++
#include "DS/PersistentSegTree.h"
#include "IO/FastIO.h"

int main() {
    //这是一个长度为5的空数组
    int A[5] = {0, 0, 0, 0, 0};
    //写一个默认的求和树
    OY::PersistentSegTree sum_tree(A, A + 5);

    cout << "version num=" << sum_tree.versionCount() << endl;

    sum_tree.add(-1, 2, 100);
    sum_tree.add(-1, 0, 10);
    sum_tree.add(-1, 3, 11);
    sum_tree.add(-1, 1, 1000);
    sum_tree.add(-1, 4, 5);

    cout << "version num=" << sum_tree.versionCount() << endl;

    cout << "version 3, x=1, y=" << sum_tree.query(3, 1) << endl;
    cout << "version 4, x=1, y=" << sum_tree.query(4, 1) << endl;
    cout << "version 5, sum(A)=" << sum_tree.query(5, 0, 4) << endl;

    sum_tree.add(3, 1, 999);
    cout << "ver.6 based on ver.3, A[1]=" << sum_tree.query(-1, 1) << endl;
    sum_tree.add(4, 1, 999);
    cout << "ver.7 based on ver.4, A[1]=" << sum_tree.query(-1, 1) << endl;

    //查询第三个到第五个操作中加入的第 k 大的数
    cout << sum_tree.periodKth(3, 5, 999) << endl;
    cout << sum_tree.periodKth(3, 5, 1000) << endl;
    cout << sum_tree.periodKth(3, 5, 1010) << endl;
    cout << sum_tree.periodKth(3, 5, 1011) << endl;

    //查询第三个到第五个操作中，1 的值增长了多少
    cout << sum_tree.periodQuery(3, 5, 1) << endl;

    //**************************************************************
    //由于本数据结构动态开点，所以可以维护很大的区间
    //可以预先设置空间量，避免空间浪费
    OY::PersistentSegTree T;
    //一般设为 (操作数+1) * 树高
    T.setBufferSize(100001 * 20);
    T.resize(1000000000);
}
```

```
#输出如下
version num=1
version num=6
version 3, x=1, y=0
version 4, x=1, y=1000
version 5, sum(A)=1126
ver.6 based on ver.3, A[1]=999
ver.7 based on ver.4, A[1]=1999
1
3
3
4
1000

```

