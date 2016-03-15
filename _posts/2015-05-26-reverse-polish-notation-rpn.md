---
title: 求逆波兰式
layout: post
---
逆波兰式，又叫后缀表达式，因其在计算机中使表达式求值变得更为容易，所以有着广泛的应用。本文介绍的是将将普通的中序表达式，转换为逆波兰式的算法。

```cpp
#include <stdio.h>;
 
typedef struct
    {
        char data[100];
        int top;
    } Stack;
 
int main()
{
    Stack s;
    char ch;
    s.top = -1;
    scanf("%c", &ch);
    while(ch != '#')
    {
        switch(ch)
        {
        case '(':
            s.data[++ s.top] = ch;
            break;
        case ')':
            while(s.data[s.top] != '(')
            {
                printf("%c", s.data[s.top]);
                s.top --;
            }
            s.top --;
            break;
        case '+':
        case '-':
            if(s.data[s.top] == '(')
                s.data[++ s.top] = ch;
            else
            {
                while(s.data[s.top] != '(' && s.top!=-1)
                {
                    printf("%c",s.data[s.top]);
                    s.top --;
                }
                s.data[++ s.top] = ch;
            }
            break;
        case '*':
        case '/':
            if(s.data[s.top] == '(')
                s.data[++ s.top]=ch;
            else
                while(s.data[s.top] != '(')
                {
                    if(s.data[s.top] == '+' || s.data[s.top] == '-' || s.top==-1)
                    {
                        s.data[++ s.top] = ch;
                        break;
                    }
                    else
                    {
                        while(s.data[s.top]=='*' || s.data[s.top]=='/')
                            printf("%c",s.data[s.top--]);
                        s.data[++s.top]=ch;
                        break;
                    }
                }
            break;
        default:
            printf("%c", ch);
        }
        scanf("%c", &ch);
    }
    while(s.top != -1)
    {
        printf("%c", s.data[s.top]);
        s.top --;
    }
    return 0;
}
```