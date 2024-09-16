- 八皇后问题
    - 问题描述　
    给定一个n*n的棋盘，棋盘中有一些位置不能放皇后。现在要向棋盘中放入n个黑皇后和n个白皇后，使任意的两个黑皇后都不在同一行、同一列或同一条对角线上，任意的两个白皇后都不在同一行、同一列或同一条对角线上。问总共有多少种放法？n小于等于8。
    - 输入格式
    输入的第一行为一个整数n，表示棋盘的大小。
    接下来n行，每行n个0或1的整数，如果一个整数为1，表示对应的位置可以放皇后，如果一个整数为0，表示对应的位置不可以放皇后。
    - 输出格式
　　输出一个整数，表示总共有多少种放法。


``` cpp
#include <iostream>
#include <algorithm>
using namespace std;

int queen[2][8]={0};
int s;

bool isok1(int row,int column){
	int leftup=column-1;
	int rightup=column+1;
	for(int i=row-1;i>=0;i--){
		for (int i = row-1; i >=0 ; --i) {
			if (queen[0][i]==column) return false;//看垂直方向是否有皇后
			if (leftup>=0){
				if (queen[0][i]==leftup) return false;//看左上角斜线是否有皇后
			}
			if (rightup<s){
				if (queen[0][i]==rightup) return false;//看右上角斜线是否有皇后
			}
			--leftup;
			++rightup;
		}
		return true;
	}
}


bool isok2(int row,int column){
	int leftup=column-1;
	int rightup=column+1;
	for(int i=row-1;i>=0;i--){
		for (int i = row-1; i >=0 ; --i) {
			if (queen[1][i]==column) return false;//看垂直方向是否有皇后
			if (leftup>=0){
				if (queen[1][i]==leftup) return false;//看左上角斜线是否有皇后
			}
			if (rightup<s){
				if (queen[1][i]==rightup) return false;//看右上角斜线是否有皇后
			}
			--leftup;
			++rightup;
		}
		return true;
	}
}

void eightqueen(int row,int **t,int& count){
	if (row==s){
		count++;
		return;
	}
	for (int column = 0; column < s; column++) {
		if (isok1(row,column)&&t[row][column]){
			
			queen[0][row]=column;
			for(int column2 = 0; column2 < s; column2++){
				if(column2!=column){
					if(isok2(row,column2)&&t[row][column2]){
						queen[1][row]=column2;
						eightqueen(row+1,t,count);
					}
				}
			}
		}
	}
}

int main(){
	cin>>s;
	int **t=new int*[s];
	for(int i=0;i<s;i++){
		t[i]=new int[s]();
		
	}
	for(int i=0;i<s;i++){
		for(int j=0;j<s;j++){
			cin>>t[i][j];
		}
	}
	int c=0;
	eightqueen(0,t,c);
	cout<<c;
}

```
