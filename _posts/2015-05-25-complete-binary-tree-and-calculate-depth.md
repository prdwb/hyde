---
title: 判断完全二叉树及计算繁茂度
layout: post
---
本文介绍二叉树的相关算法，包括建立二叉树、二叉树的中序遍历、计算二叉树繁茂度以及判断完全二叉树。

相关代码如下:

```cpp
#include<stdio.h>
#include<stdlib.h>

#define A 20
#define MAX 100

typedef struct TNode
{
    char data;
    struct TNode *lchild, *rchild;
} TNode, *BiTree;

typedef struct
{
    BiTree node;
    int layer;
} BTNrecord;

typedef struct
{
    BiTree data[MAX];
    int top;
} SeqStack;

void inorderTraverse(BiTree &T)
{
    SeqStack s;
    s.top = -1;
    BiTree p = T;
    printf("该二叉树的中序遍历为：");
    while( p || ( s.top != -1 ))
    {
        while(p)
        {
            if( s.top == MAX - 1 )
                exit(0);
            s.data[ ++ s.top ] = p;
            p = p -> lchild;
        }
        if(s.top != -1)
        {
            p = s.data[s.top --];
            printf("%c", p -> data);
            p = p -> rchild;
        }
    }
}

int fanmaodu(BiTree &T)
{
    int Count[A];
    BTNrecord Q[MAX], p;
    int f = 0, r = 0, maxl, k, i;
    if(T == NULL) return 0;
    Q[r].node = T;
    Q[r ++].layer = 1;
    for(i = 0; i < A; i ++)
        Count[i] = 0;
    while(r != f)
    {
        p = Q[f];
        f = (f + 1) % MAX;
        Count[p.layer] ++;
        if(p.node -> lchild)
        {
            if((r + 1) % MAX == f)
            {
                printf("OVERFLOW");
                exit(0);
            }
            Q[r].node = p.node -> lchild;
            Q[r].layer = p.layer + 1;
            r = (r + 1) % MAX;
        }
        if(p.node -> rchild)
        {
            if((r + 1) % MAX == f)
            {
                printf("OVERFLOW");
                exit(0);
            }
            Q[r].node = p.node -> rchild;
            Q[r].layer = p.layer + 1;
            r = (r + 1) % MAX;
        }
    }
    k = p.layer;
    for(maxl = Count[1], i = 2; i <= k; i ++)
        if(maxl < Count[i]) maxl = Count[i];
    printf("该二叉树的繁茂度为%d\n", maxl * k );
}

bool checkCpBiTree(BiTree &T)
{
    bool last = false, isCpBiTree = true;
    BiTree s[MAX], p;
    int f = 0, r = 0;
    if(!T) return true;
    s[r] = T;
    r = (r + 1) % MAX;
    while(r != f)
    {
        p = s[f];
        f = (f + 1) % MAX;
        if(!last)
        {
            if(p -> lchild && p -> rchild)
            {
                if((r + 1) % MAX == f) exit(0);
                s[r] = p ->lchild;
                r = (r + 1) % MAX;
                s[r] = p ->rchild;
                r = (r + 1) % MAX;
            }
            else if(p -> lchild && p -> rchild == NULL)
            {
                last = true;
                if((r + 1) % MAX == f) exit(0);
                s[r] = p -> lchild;
                r = (r + 1) % MAX;
            }
            else if(p -> lchild == NULL && p -> rchild)
            {
                last = true;
                isCpBiTree = false;
                break;
            }
            else last = true;
        }
        else
        {
            if(p -> lchild || p -> rchild)
            {
                isCpBiTree = false;
                break;
            }
        }
    }
    if(isCpBiTree)
        printf("该二叉树是完全二叉树\n");
    else
        printf("该二叉树不是完全二叉树\n");
    return isCpBiTree;
}

void createBiTree(BiTree &T)
{
    char ch;
    scanf("%c", &ch);
    if(ch == '#')
        T = NULL;
    else
    {
        if(!(T = (TNode *)malloc(sizeof(TNode))))
        {
            printf("内存分配失败！");
            exit(0);
        }
        T -> data = ch;
        createBiTree( T -> lchild );
        createBiTree( T -> rchild );
    }
}

int main()
{
    BiTree T;
    printf("请按先序序列输入一棵二叉树（空树以“#”代替）：\n");
    createBiTree(T);
    inorderTraverse(T);
    printf("\n");
    fanmaodu(T);
    checkCpBiTree(T);
    return 1;
}
```