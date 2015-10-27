# Sorted Array to Binary Search Tree

出处

## Solution

将数据结构分成左右两部分，分别构造两个二叉树，再用中间数与这两个局部解合并得到当前的全局解。因为数组已经排序好，因此这个二叉树一定满足BST的条件(左边的任何数一定小于中间数小于右边的任何数)，在选择分界点的时候选择中点，那么也就能满足平衡条件。

## Complexity

算法遍历整个数组，故时间复杂度O(n)，需要额外O(n)空间存储树的节点。

## Code
