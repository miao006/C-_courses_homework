# Simple Game ---- Escape From the Hell

# Simple Game

## Description

Let's play a game. The description may be very long so I don't want to pretend B.

有一位大熊弟因为秀恩爱而遭到FFF团的制裁——被扔进了下界，而你，正以上帝视角暗中观察这一切。(但你只能观察)
观察到的地图如下

```
*********
*   $   *
*  #    *
*#   #  *
*       *
*A      *
*   @   *
*@    $ *
*********

A --- 大熊弟
* --- 边界
# --- 墙
@ --- 水
$ --- 火

```

大熊弟会**尝试**移动，而你要告诉FFF团他的情况(打印出**尝试**移动后当前的地图)

## 细节

### 遇到地形时的行为
+ `*` 边界
  + 不会阻挡大熊弟(下面简称**A**)的移动，当遇到边界时，会从其对面边界穿出
  + 如上图情况，若A向左走，下一地图应该如下
    ```
      *********
      *   $   *
      *  #    *
      *#   #  *
      *       *
      *      A*
      *   @   *
      *@    $ *
      *********
    ```
+ `#` 墙
  + 会阻挡A的移动，假设`A`上方有`#`，而又尝试向上移动，则A的位置不变，输出原地图
+ `@` 水
  + 当A移动到`@`，其失去一次移动机会(即下一次移动尝试不会移动成功)
  + 输出时，在此处输出`A`；但A离开后，此处还会是`@` (即重叠时输出`A`)
+ `$` 火
  + 当A移动到`$`，A死亡，不输出地图，输出`Game Over!`并结束程序

### 输入

第一行是整数`n`(6 <= n <= 12)，表示地图中活动空间的大小(如上面的示例中`n == 7`)
第2~n+3行接下来是一张地图，表示地图初始状态
之后是一组尝试移动的方向(EOF结束)
+ a:向左
+ d:向右
+ w:向上
+ s:向下

### 输出

每次接收到尝试移动的指令后都输出当前状态的地图 或 `Game Over!`

### 样例输入
```
7
*********
*   $   *
*  #    *
*#   #  *
*       *
*A@     *
* $ @   *
*@    $ *
*********
a
d
w
w
s
d
d
s
```

### 样例输出
```
*********
*   $   *
*  #    *
*#   #  *
*       *
* @    A*
* $ @   *
*@    $ *
*********
*********
*   $   *
*  #    *
*#   #  *
*       *
*A@     *
* $ @   *
*@    $ *
*********
*********
*   $   *
*  #    *
*#   #  *
*A      *
* @     *
* $ @   *
*@    $ *
*********
*********
*   $   *
*  #    *
*#   #  *
*A      *
* @     *
* $ @   *
*@    $ *
*********
*********
*   $   *
*  #    *
*#   #  *
*       *
*A@     *
* $ @   *
*@    $ *
*********
*********
*   $   *
*  #    *
*#   #  *
*       *
* A     *
* $ @   *
*@    $ *
*********
*********
*   $   *
*  #    *
*#   #  *
*       *
* A     *
* $ @   *
*@    $ *
*********
Game Over!


```

## 提示

在`utility.h`中有部分已声明的变量可供参考

使用`scanf(" %c");`可以保证不读到空白字符

>使用全局变量不是一种好方法，虽然只是做这一道题目没有问题，但可以思考
>1. 为什么全局变量不是好方法
>2. 如果不使用全局变量，这道题会不会变得太复杂？有无解决方法？

如何读取含有空格的一行：
1. 使用``getchar``一个一个慢慢读，直到``'\n'``为止。<del style="font-weight: bolder">简单粗暴最实用</del>。
1. 使用函数``gets``。由于``gets``函数极其不安全（有兴趣的同学可以去了解一下为什么），在Matrix上使用此函数的代码将过不了编译检查阶段。为了您和您计算机的安全，我们不得不做出这个艰难的决定，全面禁止使用``gets``函数（c11里面已经禁止了``gets``，Thanks for c11）。在这里，我们向您墙裂退件升级到更为安全的全新版函数``gets_s``（好像GNU并没有实现这个标准的函数，尴尬了，12月9号注）。
2. 使用函数``fgets``来获取一行。这里你可能要问了，这``fgets``会不会跟``gets``一样不安全。<del>如果你硬要问我安不安全，我只能说安全</del>，``fgets``第二个参数是读取字符串的最大长度，大致等价于``gets_s``的第二个参数。<del>文件操作的``fgets``， 不知道比你们``gets``高到。。。</del>。``fgets``的第三个参数是文件指针，这里直接写``stdin``即可，如``fgets(str, 65535, stdin)``，至于为什么，请自学文件操作或等老师教学进度。
3. 使用scanf的<del>玄学</del>格式化读取。 具体为 ``scanf("%65535[^\n]%*c", str)``，其中``65535``是读取的最大长度，大致等价于前两种的第二个参数，但只能写死在格式化字符串中。至于是什么个原理，请自行查百度或者谷歌``%[]``和``%*c``。<del>查不到也就算了，反正是玄学</del>
4. 其他方法自行谷歌