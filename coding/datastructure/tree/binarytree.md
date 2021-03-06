* [二叉树](#二叉树)
* [概述](#概述)
* [特性](#特性)
* [典型应用](#典型应用)
	* [表达式树 ](#表达式树)
	* [二叉查找树](#二叉查找树)
		* [操作](#操作)
	* [AVL树](#avl树)
		* [操作](#操作-1)
	* [伸展树](#伸展树)
		* [操作](#操作-2)
	* [红黑树](#红黑树)
		* [操作](#操作-3)
		* [应用](#应用)
* [Reference](#reference)
* [Back](/methods/datastructure/tree)

# 二叉树

# 概述
二叉树，表示树中每个节点不能多于两个子节点。 以下列举一些具有特殊性质（除正文描述的二叉树）的二叉树：
- 完全二叉树， 除最后一层外，其他层的节点都是满的，最后一层如果不满，缺少的节点都在右边。
- 满二叉树，每一层的节点都是满的，最后一层的节点无任何儿子节点。满二叉树层数L，节点数为N，则N=pow(2, L) -1，L = log2(N + 1)。

# 特性
1. N个节点，N-1条边；
2. 从节点n1到nk的路径，定义为节点n1、n2...nk的一个序列，路径长为k-1；
3. 从根到ni的路径长i -1，表示节点ni的深度；
4. ni到叶子节点的最长路径长，表示节点ni的高度；
5. N个节点的二叉树，有N + 1个null链。

# 典型应用
## 表达式树 
树叶是操作数，其他节点是操作符。以下是通过遍历的方式，序列化表达式的形式：
- 中序 -> 中缀
- 前序 -> 前缀
- 后序 -> 后缀（逆波兰式），通过后缀表达式构造表达式树，通过栈，遇到操作数入栈，遇到操作符则进行两次弹栈操作，并将弹出的值作为该操作符的左右子树，并将操作符入栈。

## 二叉查找树
二叉查找树（二叉搜索树）的特性就是在树中任意节点X的左子树的值都小于该X的值，而右子树的值都大于该X的值。通常二叉查找树应用于查找操作，注意，二叉查找树的中序遍历得到的序列，是有序的。

### 操作
- 插入，可以通过递归的方式，或者非递归循环的方式插入（比较插入值与节点值大小，小于则插入左子树，大于则插入右子树，如果相等，可以通过计数值或辅助存储，表或另一颗查找树）。
- 查找，与插入类似，比较查找值和节点值，如果相同，则找到，如果不同，则按照大小，递归查找左子树或右子树。
- 删除，最困难的操作，分为两步，首先找到删除的值，利用查找的思路；第二步，找到被删除节点，如果被删除节点左右子树都不为空，则将右子树的最小值与当前值交换，并递归删除右子树，如果被删除节点右子树或左子树为空，则将单个子树连上父节点。（如果删除次数不多，常通过懒惰删除机制，将其标记为删除）
- 最大值/最小值，左子树的左孩子节点（一直往左移动），右子树的右孩子节点（一直往右移动）。

> 注：
按照如上操作，插入、删除多次，可能造成左右子树的高度差过大的情况，从树的结构图中直观可看到左右不平衡。这种情况的后果就是，查找、插入和删除操作的耗时不稳定，一次操作可能需要O(N)的时间复杂度（有序数组依次插入）。所以，需要加上一些操作去放置树的过度失衡，也就接下来介绍的带平衡条件的二叉树。

## AVL树
AVL树是带有平衡条件的二叉查找树（严格平衡），性质是对于树中任意节点左右子树的高度差不超过1。（节点的信息一般会记录节点高度）

### 操作
AVL树的操作的增删改查的操作与普通的二叉查找树类似，主要多出平衡的调整操作。下面主要描述平衡操作，出现失衡情况归类：
    1. 对A节点左儿子的左子树插入；
    2. 对A节点右儿子的右子树插入；
    3. 对A节点左儿子的右子树插入；
    4. 对A节点右儿子的左子树插入；
将上述四种情况归类成两种，一种是在目标失衡节点的“外边”插入，采用单旋转的操作；另外一种是在目标失衡节点的“里边”插入，采用双旋转的操作。具体旋转图如下所示：

- 单旋转

![](/images/methods/datastructure/tree/tree_singlerotation_left.PNG)

```java
/**
* 上述情况的代码实现如下。
**/
private AvlNode<AnyType> rotateWithLeftChild( AvlNode<AnyType> k2)
{
    AvlNode<AnyType> k1 = k2.left;
    k2.left = k1.right;
    k1.left = k2;
    k2.height = Math.max( height(k2.left), height(k2.right) ) + 1;
    k1.height = Math.max( height(k1.left), height(k1.right) ) + 1;
    return k1;
}
```


![](/images/methods/datastructure/tree/tree_singlerotation_right.PNG)

- 双旋转

![](/images/methods/datastructure/tree/tree_doublerotation_left.PNG)

```java
// 双旋转可以拆分成两次单旋转。
private AvlNode<AnyType> doubleWithLeftChild( AvlNode<AnyType> k3)
{
    k3.left = rotateWithRightChild(k3.left);
    return rotateWithLeftChild(k3);
}
```


![](/images/methods/datastructure/tree/tree_doublerotation_right.PNG)



> 注：
> AVL树严格控制的左右子树的高度差，也就可以使每一次操作时间复杂度控制在O(logN)，但是，由于频繁的平衡操作，也就导致AVL的效率不高，应用也不是很广泛。因此，产生一种不严格的带平衡调节的二叉树，他只是确保连续M次操作时间复杂度控制在O(MlogN)范围，后面的伸展树和红黑树都是类似的实现。


## 伸展树
保证空树在开始连续M次操作消耗的时间最多花费O(MlogN)。允许一次访问出现最坏的情况O(N)，但是要求多次操作有一个摊还时间界(O(logN))。

### 操作
基本想法，当一个节点被访问后，它就要经过一系列AVL树的旋转被推到根上。

> 注，由于伸展树的应用较少，此处只是描述一下其的简单想法。



## 红黑树
红黑树是AVL树的变种，具备性质如下：
1. 每个节点或者着成红色，或者着成黑色；
2. 根节点是黑色；
3. 如果一个节点是红色，那么它的子节点必须是黑色；
4. 从一个节点到一个null应用的每一条路径必须包含在相同数目的黑色节点。

> 结论，红黑树的高度最多2log(N + 1)。


### 操作
红黑树的添加、删除操作基于二叉查找树操作后，进行调整（平衡），主要满足第3和4的条件。此处主要讨论插入的情况，具体实现可以看Java的TreeMap源码（底层是用红黑树实现）。插入操作首先查找到可插入的父节点，根据父节点颜色分情况讨论：
1. 当父节点为黑色，直接插入红色子节点。
2. 当父节点为红色，有两种情况：
    - 父节点的兄弟节点是黑色，那么插入节点后（默认插入节点为红色），需要进行调整，具体操作类似于AVL树，分两种情况：
        - “一”字形，外侧，进行单旋转，并交换父节点和祖父节点的颜色（主要为了保证根节点为黑色，才能对其上的节点无影响）。
        - “之”字形，内侧，进行双旋转，交换插入节点和祖父节点颜色。
        
        ![](/images/methods/datastructure/tree/tree_redblack_one.PNG)
    
    - 父节点的兄弟节点是红色，也就意味着不能将根节点设置为黑色，会破坏平衡，那么需要将根节点设置为红色。当根节点的父节点为黑色，则无影响，如果根节点的父节点是红色，则造成新的不平衡的状况，也就需要进行上滤操作进行调整。具体颜色变化分以下两种情况：
    - “一”字形，外侧，进行单旋转，将插入节点设置成黑色。
    - “之”字形，内侧，进行双旋转，插入节点变成根节点，将父节点变成黑色。
    
    由于没有指向父节点的指针（如果存在父节点，则可从根节点进行上滤操作），所以需要自顶向下进行上滤操作，具体操作细节如下：
    自顶向下，进行颜色翻转，如果根节点的父节点是红色，则进行调整（与上面相同），此外，由于颜色翻转的操作，将保证父节点的兄弟节点不是红色（越往上的翻转操作，越早进行），通过上滤的操作保证插入节点的父节点的兄弟节点是黑色。

### 应用
红黑树能够保证最坏情况下花费O(logN)的时间，而其需要的平衡操作（旋转）比AVL树少，实践证明红黑树执行插入时间开销低（旋转次数少），而查找性能好。此外，红黑树的典型实现就是java容器类中TreeMap的实现。




# Reference
- 《数据结构与算法描述 Java语言描述》



















