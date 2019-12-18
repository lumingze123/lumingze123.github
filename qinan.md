---
layout: page
title: 数据结构——qinan
---

```cpp
#include <stdio.h>
#include <stdlib.h>
#include <time.h>
#include <malloc.h>
#define LEN 100

void randnum(int *a, int n){ //输入一组不重复的随机数 
	int i, j;
	srand((unsigned)time(NULL));
	for (i = 0; i < n; ++i){
		a[i] = rand() % 100 + 1;
		for (j = 0; j < i; ++j){
			if (a[i] == a[j]){
				a[i] = rand() % 100 + 1;
				j = -1;
				continue;
			}
		}
	}
}

void shoudong(int *a,int n) {  //手动输入数据 
	int i;
	printf("请输入数组的每一个元素：\n");
	for(i=0;i<n;i++)
		scanf("%d",&a[i]);
}

//顺序start 
int Seq_Search(int key,int a[],int n){
	int loc=0,i=1;
	while(loc<n&&a[loc]!=key){
		loc++;
		i++;
	}	
	if(a[loc]==key)
		printf("查找成功！\n 查找%d次！a[%d]=%d",i,loc,key);
	else
		printf("查找失败！"); 	
	return 0;
}

int shunxu(){
	int i,key,a[100],n,x;
	printf("请输入要查找的数组的长度：\n");
	scanf("%d",&n);
	printf("请选择数据输入方式(1:随机输入,其他键：手动输入):\n");
	scanf("%d",&x);
	if(x==1) 
		randnum(a,n); 
	else{
		shoudong(a,n);
	}
	printf("\n地址：\t\t");
	for(i=0;i<n;i++)
		printf("%4d",i);
	printf("\n待查找数组：\t");
	for(i=0;i<n;i++)
		printf("%4d",a[i]);
	printf("\n请输入想查找的元素：\n");
	scanf("%d",&key);
	Seq_Search(key,a,n),
	printf("\n");
	return 0; 
}
//顺序end 

//快速排序start 
int Partition(int a[],int low,int high){
	int i;
	i=a[low];
	while(low<high){
		while(low<high&&a[high]>=i){
			high--;
		}
		if(low<high){
			a[low]=a[high];
			low++;
		}
		while(low<high&&a[low]<i){
			low++;
		}
		if(low<high){
			a[high]=a[low];
			high--;
		}
	}
	a[low]=i;
	return low;
}

void Quick_Sort(int a[],int s,int t){
	int i;
	if(s<t){
		i=Partition(a,s,t);
		Quick_Sort(a,s,i-1);
		Quick_Sort(a,i+1,t);
	}
} 
//快速排序end 

//二分strat 
int Binary_Search(int key,int a[],int n){
	int low,high,mid,count=0,count1=0;
	low=0;
	high=n-1;
	while(low<=high){
		count++;
		mid=(low+high)/2;
		if(key<a[mid])
			high=mid-1;
		else if(key>a[mid])
			low=mid+1;
		else if(key==a[mid]){
			printf("查找成功！\n 查找%d次！a[%d]=%d",count,mid,key);
			count1++;
			break;
		}
	}
	if(count1==0)
			printf("查找失败！"); 
	return 0;
}

int erfen(){
	int i,key,a[100],n,x;
	printf("请输入要查找的数组的长度：\n");
	scanf("%d",&n);
	printf("请选择数据输入方式(1:随机输入,其他键：手动输入):\n");
	scanf("%d",&x);
	if(x==1) 
		randnum(a,n); 
	else{
		shoudong(a,n);
	}
	printf("\n地址：\t\t");
	for(i=0;i<n;i++)
		printf("%4d",i);
	printf("\n待查找数组：\t");
	for(i=0;i<n;i++)
		printf("%4d",a[i]);
	printf("\n数组排序后：\t");
	Quick_Sort(a,0,n);
	for(i=0;i<n;i++)
		printf("%4d",a[i]);
	printf("\n请输入想查找的元素：\t");
	scanf("%d",&key);
	Binary_Search(key,a,n);
}

//二叉start
typedef int ElemType;
typedef struct BSTNode{
	ElemType data;
	struct BSTNode *lchild, *rchild;
} BSTNode, *BSTree;


void Insert(BSTree *tree, ElemType item){  //插入新节点
	BSTree node = (BSTree)malloc(sizeof(BSTNode));
	node->data = item;
	node->lchild = node->rchild = NULL;
	if (!*tree)
		*tree = node;
	else{
		BSTree cursor = *tree;
		while (1){
			if (item < cursor->data){
				if (NULL == cursor->lchild){
					cursor->lchild = node;
					break;
				}
				cursor = cursor->lchild;
			}
			else{
				if (NULL == cursor->rchild){
					cursor->rchild = node;
					break;
				}
			cursor = cursor->rchild;
			}
		}
	}
	return;
}

BSTree Search(BSTree tree, ElemType item){ // 查找指定值
	BSTree cursor = tree;
	while (cursor){
		if (item == cursor->data)
			return cursor;
		else if ( item < cursor->data)
			cursor = cursor->lchild;
		else
			cursor = cursor->rchild;
	}
	return NULL;
}

void Inorder(BSTree tree){ /* 中缀遍历 */
	BSTree cursor = tree;
	if (cursor){
		Inorder(cursor->lchild);
		printf("%4d", cursor->data);
		Inorder(cursor->rchild);
	}
}

 
int ercha(){
	ElemType item;
	BSTree root = NULL, ret; /* 必须赋予NULL值，否则出错 */
	int a[LEN], i, n,j=0,x; 
	//输入长度 
	printf("\n请输入要查找的数组的长度：\n");
	scanf("%d",&n);
	printf("请选择数据输入方式(1:随机输入,其他键：手动输入):\n");
	scanf("%d",&x);
	if(x==1) 
		randnum(a,n); 
	else{
		shoudong(a,n);
	}
	printf("\n地址：\t\t");
	for(i=0;i<n;i++)
		printf("%4d",i);
	printf("\n待查找数组：\t");
	for(i=0;i<n;i++)
		printf("%4d",a[i]);
	while (n--)
		Insert(&root, a[j++]);
	printf("\n数组排序后：\t");
	Inorder(root);
	/* 二叉排序树的查找测试 */
	printf("\n请输入查找数据：\n");
	scanf("%d", &item);
	ret = Search(root, item);
	if (NULL == ret)
		printf("查找失败！");
	else
		printf("查找成功！");
}
//二叉end 

//哈希start 
typedef int KeyType;
typedef struct{
	KeyType key;
	int hi;
} DataType;
typedef struct{
	DataType *data;
	int tableSize;
	int curSize;
} HashTable;

void CreateHashTable(HashTable *H,int m,int p,int hash[],int n){//构造一个空的哈希表，并处理冲突 	
	int i,sum,addr,di;
	(*H).data=(DataType*)malloc(m*sizeof(DataType));//为哈希表分配存储空间 
	if(!(*H).data)
		exit(-1);
	for(i=0;i<m;i++){            //初始化哈希表 
		(*H).data[i].key=-1;     //初始值为-1，代表无。余数不会有负数 
		(*H).data[i].hi=0;       //初始冲突次数为0 
	}
	for(i=0;i<n;i++){                 //求哈希函数地址并处理冲突 
		sum=0;                        //冲突的次数 
		addr=hash[i]%p;               //利用除留余数法求哈希函数地址，此处%7 
		di=addr;                      //di,余数地址 
		if((*H).data[addr].key==-1){      //不冲突：则将元素存储在表中 
			(*H).data[addr].key=hash[i];
			(*H).data[addr].hi=1;
		}
		else{                              //冲突：用线性探测再散列法处理冲突 
			do{ 
				di=(di+1)%m;
				sum+=1;
			}
			while((*H).data[di].key!=-1);
			(*H).data[di].key=hash[i];
			(*H).data[di].hi=sum+1;
		}
	}
	(*H).curSize=n;                //哈希表中关键字个数为n 
	(*H).tableSize=m;              //哈希表长度 
}

int SearchHash(HashTable H,int k){
	int d,dl,m;
	m=H.tableSize;
	d=dl=k%m;
	while(H.data[d].key!=-1){
		if(H.data[d].key==k)
			return d;
		else
			d=(d+1)%m;
		if(d==dl)
			return 0;
	}
	return 0;
}

void HashASL(HashTable H,int m){
	float average=0;
	int i;
	for(i=0;i<m;i++)
		average=average+H.data[i].hi;
		average=average/H.curSize;
		printf("平均查找长度ASL=%.2f\n",average);
}

void DisplayHash(HashTable H,int m){
	int i;
	printf("\n关键字 key:\t");
	for(i=0;i<m;i++)
		printf("%4d",H.data[i].key);
	printf("\n冲突次数：\t");
	for(i=0;i<m;i++)
		printf("%4d",H.data[i].hi);
	printf("\n");
}

int haxi(){  
	int n,x,pos,i,k;
	int hash[LEN],p=3;
	printf("请输入要查找的数组的长度：\n");
	scanf("%d",&n);
	printf("请选择数据输入方式(1:随机输入,其他键：手动输入):\n");
	scanf("%d",&x);
	if(x==1) 
		randnum(hash,n); 
	else{
		shoudong(hash,n);
	}
	printf("地址：\t\t");
	for(i=0;i<n+1;i++) 
		printf("%4d",i);
	printf("\n待查找数组：\t");
	for(i=0;i<n;i++)
		printf("%4d",hash[i]);
	HashTable H;
	CreateHashTable(&H,n+1,p,hash,n);
	DisplayHash(H,n+1);
	printf("\n请输入想查找的元素：\t");
	scanf("%d",&k);
	pos = SearchHash(H,k);
	printf("关键字%d在哈希表中的位置为：%d\n",k,pos);
	HashASL(H,n+1);
	return 0;
}
//哈希end 

void xinxi(){     //5.学生信息
	printf("学院：信息与通信工程学院\n");
	printf("班级：18物联3班\n");
	printf("学号：1810818125\n");
	printf("姓名：秦倩\n");
}

int xuanze(){     //选择返回目录还是退出 
	int choice;
	printf("\n返回目录按1，退出按其它键：");
	scanf("%d",&choice);
	if(choice==1)
		return 1;
	else
 		return 0;
} 
 
int main(){
	int s,i;
	do{ 
		system("cls");
		printf("-----欢迎使用二叉排序树演示程序-----\n\n"); 
		printf("            ===目录===              \n");
		printf("           1.顺序查找               \n");
		printf("           2.二分查找               \n");
		printf("           3.二叉排序树查找         \n");
		printf("           4.哈希查找               \n");
		printf("           5.学生信息               \n");
		printf("           0.退出程序               \n\n");
		printf("     请输入序号选择查找方式：");
		scanf("%d",&s);
		switch(s){
		case 1:shunxu(); i=xuanze();break;
		case 2:erfen(); i=xuanze();break;
		case 3:ercha(); i=xuanze();break;
		case 4:haxi(); i=xuanze();break;
		case 5:xinxi(); i=xuanze();break;
		case 0:return 0;
		}
	} while(i==1);
}

```