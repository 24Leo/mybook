#基础知识
1、数据结构：相互之间存在一种或者多种关系的数据元素的集合。
    <br>  　　　　　　　数据(D)之间的关系我们称之为结构(S)
    <br>  　 定义：Data_Structure = (D,S)    
２、基本结构：
* 
集合
    <br>  　　数据之间仅仅属于一个集合而已，没有其他关联
* 
[线性结构](liner.md)
    <br>  　　一对一的关系
* 
[树形结构](tree.md)
    <br>  　　一对多的关系
* 
[图状/网状](graph.md)
    <br>  　　多对多关系

3、
以上讨论的都是数据间的逻辑关系，我们也可称为逻辑结构，但是我们要在计算机中实现他们。数据结构在计算机中的表示称为数据的物理结构。主要分为以下两种：
* 
顺序存储结构：逻辑上相连的物理上也相连
* 
链式存储结构：逻辑上相连的物理上不一定相连

4、算法：对某一特定问题解题步骤的描述。具有一下特性：
* 
有穷性：　一个算法总是要在(合适的输入)有穷步内结束，其中每一步都可在有穷时间内完成。
* 
确定性：　算法中的每一个语句或指令都有明确的含义，且在任何条件下，只有唯一的执行路径。
* 
可行性：　一个算法是可以运行的，都可以通过已经实现的基本运算在有限次执行来实现。
* 
输入/输出：一个算法有一个或多个输出，零个和多个输入。

5、算法设计的原则：
* 
正确性：　语法无错误、各种输入情况都能给出正确输出(一般情况下只要满足典型和苛刻的输入即合格了)
* 
可读性：　算法首先是人的阅读和交流，其次是计算机的执行。
* 
健壮性：　算法只处理合适的输入，非法时不应继续计算。
* 
高效率及低存储：时间少、空间小。

6、算法效率的度量
* 
时间：　O(f(n))  　　n代表问题规模
* 
空间：　O(f(n))　　如果额外空间相对于输入是常数，此算法是原地工作：O(1)

[返回目录](README.md)