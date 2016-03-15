---
layout: post
title: 八皇后问题 Eight Queens Puzzle
---
八皇后问题是要在在8×8格的国际象棋上摆放八个皇后，使其不能互相攻击，即任意两个皇后都不能处于同一行、同一列或同一斜线上，问有多少种方案。

## 算法
八皇后问题采用回溯法，每在一行放置一个皇后，先判断当前布局是否合理，合理就放置下一行，不合理则回溯，判断这一行的下一个位置。

## C++代码实现
```cpp
#include <iostream>
#include <cstdlib>
using namespace std;

class ChessBoard
{
    int n;
    int counter = 0; // 计数器，统计共有几个方案
    int* queens;

public:
    ChessBoard(int N)
    {
        n = N;
        queens = new int[n + 1]; // 该数组放置皇后的位置，下标1代表第一行
    }

    void Print() // 输出方案
    {
        for(int i = 1; i <= n; i++)
        {
            for(int j = 1; j <= n; j++)
            {
                if(j == queens[i]) // 如果该处放皇后, 就输出'#'
                    cout << "#";
                else
                    cout << "*"; // 如果该处不放皇后,就输出'*'
            }
            cout << "\n";
        }
        counter++; // 计数器自加
        cout << "\n";
    }

    void Place(int i) // 在第i行放置皇后
    {
        if(i > n) // 如果行数大于所求皇后数, 则代表该方案构造完毕, 输出
            Print();

        else
        {
            for(int j = 1; j <= n; j++) // 从第i行第1个位置开始试探
            {
                queens[i] = j; // 把皇后放置在第i行第j个位置
                if(currentOK(i)) // 判断当前布局是否合理
                    Place(i + 1); // 合理则放置下一行,不合理则回溯,试探该行的下一个位置
            }
        }
    }

    bool currentOK(int i) // 判断当前布局是否合理
    {
        for(int k = i - 1; k >= 1; k--)
            if(queens[i] == queens[k] || abs(queens[i] - queens[k]) == i - k) // 判断两个皇后是否列数一样,或是否在一条斜线上
                return false;
        return true;
    }

    int getCounter() // 返回计数器的值
    {
        return counter;
    }
};

int main()
{
    ChessBoard chessBoard(6);
    chessBoard.Place(1); // 从第1行开始放置
    cout << "Solutions: " << chessBoard.getCounter() << endl;
    return 0;
}

```
