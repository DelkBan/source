---
title: PTA-Reversing Linked List
date: 2018-10-16 20:07:22
tags:
- C++
- PTA
---
**Tips:**
<algorithm>中的reverse(first,last)可以将区间内的顺序倒置,包含first不包含last。
这是数据结构链表部分的习题，最贴切的做法是使用链表，难点在于翻转，在本文最后给出了陈越老师的做法。还有一种使用大数组的牺牲空间换取时间的做法，值得玩味。
<!--more-->

##### 描述
Given a constant K and a singly linked list L, you are supposed to reverse the links of every K elements on L. For example, given L being 1→2→3→4→5→6, if K=3, then you must output 3→2→1→6→5→4; if K=4, you must output 4→3→2→1→5→6.

**Input Specification:**

Each input file contains one test case. For each case, the first line contains the address of the first node, a positive N (≤10<sup>5</sup> ) which is the total number of nodes, and a positive K (≤N) which is the length of the sublist to be reversed. The address of a node is a 5-digit nonnegative integer, and NULL is represented by -1.

Then N lines follow, each describes a node in the format:

	Address Data Next

where `Address` is the position of the node,` Data` is an integer, and` Next` is the position of the next node.

**Output Specification:**

For each case, output the resulting ordered linked list. Each node occupies a line, and is printed in the same format as in the input.

**Sample Input:**

00100 6 4
00000 4 99999
00100 1 12309
68237 6 -1
33218 3 00000
99999 5 68237
12309 2 33218

**Sample Output:**

00000 4 33218
33218 3 12309
12309 2 00100
00100 1 99999
99999 5 68237
68237 6 -1

##### 我的尝试

这个题目有一些不太容易理解的地方，观察数据可以发现数据是按照地址顺序递增的，也就是说只要把地址按照顺序拍好，数字就是有序的。

我使用的是最笨的一维数组的方法，在逆转部分有O(n*n)的复杂度，导致倒数第二个过不了。而最后一个提示是可能有不在链表中的节点，这个没能解决。

代码如下：

```C++

//Reversing Linked List
#include<iostream>
#include<cstring>

using namespace std;

int main()
{
	freopen("c:\\Users\\DelkBan\\Desktop\\test.txt","r",stdin);
	int n,k;
	string start;
	cin>>start>>n>>k;
	string curPos[n],nextPos[n];
	int data[n];
	for(int i=0;i<n;i++)
	{
		cin>>curPos[i]>>data[i]>>nextPos[i];
	}
	string orderCur[n],orderNext[n];
	int orderData[n];
	orderCur[0]=start;
	for(int i=0;i<n;i++)
	{
		if(curPos[i]==orderCur[0])
		{
			orderData[0]=data[i];
			orderNext[0]=nextPos[i];
			break;
		}
	}

	for(int i=1;i<n;i++)
	{
		orderCur[i]=orderNext[i-1];
		if(orderCur[i]=="-1")
			break;
		else
		{
			for(int j=0;j<n;j++)
			{
				if(curPos[j]==orderCur[i])
				{
					orderData[i]=data[j];
					orderNext[i]=nextPos[j];
					break;
				}
			}
		}
	}
	int revers[n];
	int temp[k];
	string tempp[k];//地址
	string revPos[n],revNext[n];
	int count; //翻转组数 
	k==1 ? count=0:count=n/k;
	for(int i= 0;i<count;i++)
	{
		//revers里面存最终排列好的，temp里面存k个刚刚翻转的 
		for(int j=1;j<=k;j++)
		{
			temp[k-j]=orderData[j+3*i-1];
			tempp[k-j]=orderCur[j+3*i-1];
		}
		for(int j=1;j<=k;j++)
		{
			revers[j+3*i-1]=temp[j-1];
			revPos[j+3*i-1]=tempp[j-1];
		}
	}
	for(int i=count*k;i<n;i++){
		revers[i]=orderData[i];
		revPos[i]=orderCur[i];
		}
	for(int i=1;i<n;i++)
	{
		revNext[i-1]=revPos[i];
	}
	revNext[n-1]="-1";
	for(int i=0;i<n;i++)
	{
		cout<<revPos[i]<<" "<<revers[i]<<" "<<revNext[i]<<endl;
	}
	
	 return 0;
}

```

##### 用二维数组做

翻看网上资料，发现了一个很好的思路，https://www.cnblogs.com/ljwTiey/p/4294484.html

设计的非常巧妙，收获颇多

```c++

//Reversing Linked List
//大佬二维数组做法
#include<iostream>
#include<algorithm>
using namespace std;
int list[100010];//存放原始的地址序列
int node[100010][2]; //0存放数据，1存放next
int main()
{
	freopen("c:\\Users\\DelkBan\\Desktop\\test.txt","r",stdin);
	int st,num,r;
	cin>>st>>num>>r;
	int address,data,next,i;
	for(i=0;i<num;i++)
	{
		cin>>address>>data>>next;
		node[address][0]=data;
		node[address][1]=next;
	}
	int m=0,n=st;
	while(n!=-1)
	{
		list[m++]=n;
		n=node[n][1];
	}
	i=0;
	while(i+r<=m)
	{
		reverse(list+i,list+i+r);
		i=i+r;
	}
	for(i=0;i<m-1;i++)
	{
		printf("%05d %d %05d\n",list[i],node[list[i]][0],list[i+1]);
	}
	printf("%05d %d -1\n",list[i],node[list[i]][0]);
 } 
 
 ```