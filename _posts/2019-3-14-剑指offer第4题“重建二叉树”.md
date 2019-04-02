---
layout:     post
title:      "剑指offer第4题“重建二叉树”"
subtitle:   ""
date:       2019-03-14 14:00:00
author:     "Cfeng"
header-img: "img/post-bg-os-metro.jpg"
catalog: true
tags:
    - 笔试
    - 剑指Offer
    - 算法
---
# 替换空格
## 题目描述
输入某二叉树的前序遍历和中序遍历的结果，请重建出该二叉树。假设输入的前序遍历和中序遍历的结果中都不含重复的数字。例如输入前序遍历序列{1,2,4,7,3,5,6,8}和中序遍历序列{4,7,2,1,5,3,8,6}，则重建二叉树并返回。
***
## 思路
看到题目就知道要用递归
首先确定前序遍历和中序遍历的概念：
前序遍历：根结点 ---> 左子树 ---> 右子树
中序遍历：左子树---> 根结点 ---> 右子树
然后会发现前序序列的首位一定是根结点，找出根结点后，然后再在中序序列寻找根结点的下标index，所以中序序列中的[0:index-1]就是左子树的中序序列，[index+1,in.length-1]就是右子树的中序序列。
那前序序列呢？
观察一下pre中index下标，会发现[1:index]是左子树的前序序列，而[index+1:pre.length-1]是右子树的前序序列。
那左右子树的前中序列已经找到，就可以使用递归算法了，只要再确认传递下去的序列是否为空，如果为空就说明到达叶子节点，只要返回null就行了。
## Java
```
/**
 * Definition for binary tree
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode(int x) { val = x; }
 * }
 */
public class Solution {
    public TreeNode reConstructBinaryTree(int [] pre,int [] in) {
        TreeNode root=reConstructTree(pre,0,pre.length-1,in,0,in.length-1);
        return root;
    }
    private TreeNode reConstructTree(int [] pre,int startPre,int endPre,int [] in,int startIn,int endIn) {
        //递归结束条件
        if(startPre>endPre||startIn>endIn)
            return null;
        TreeNode root=new TreeNode(pre[startPre]);
        for(int i=startIn;i<=endIn;i++)
            if(in[i]==pre[startPre]){
                //左子树
                root.left=reConstructTree(pre,startPre+1,startPre+i-startIn,in,startIn,i-1);
                //右子树
                root.right=reConstructTree(pre,i-startIn+startPre+1,endPre,in,i+1,endIn);
            }
        return root;
    }
}
```
嗯。。然后想到Python分割数组是超级简便的，然后去试着写了下。。再次真香
## Python
```
# -*- coding:utf-8 -*-
# class TreeNode:
#     def __init__(self, x):
#         self.val = x
#         self.left = None
#         self.right = None
class Solution:
    # 返回构造的TreeNode根节点
    def reConstructBinaryTree(self, pre, tin):
        # write code here
        if len(pre) == 0:
            return None
        if len(pre) == 1:
            return TreeNode(pre[0])
        else:
            flag = TreeNode(pre[0])
            flag.left = self.reConstructBinaryTree(pre[1:tin.index(pre[0])+1],tin[:tin.index(pre[0])])
            flag.right = self.reConstructBinaryTree(pre[tin.index(pre[0])+1:],tin[tin.index(pre[0])+1:] )
        return flag

```
