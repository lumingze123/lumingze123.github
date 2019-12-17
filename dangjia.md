---
layout: page
title: 数据结构——当家版
---

![图片alt](public\image\c语言真相表情包.gif "数据结构")
```cpp
#include <stdio.h>
#include <stdlib.h>
#include <malloc.h>
#define LEN 100

void shuru(int x,int *a, int n){
	int i;
	if(x==1) 
		for (i = 0; i < n; ++i){
			a[i] = rand() % 100 + 1;
	}
	else{
		printf("请输入数组的每一个元素：\n");
		for(i=0;i<n;i++)
			scanf("%d",&a[i]);
	}
}

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
	shuru(x,a,n);
	printf("\n待查找数组：\t");
	for(i=0;i<n;i++)
		printf("%4d",a[i]);
	printf("\n请输入想查找的元素：\n");
	scanf("%d",&key);
	Seq_Search(key,a,n),
	printf("\n");
	return 0; 
}

 
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
	shuru(x,a,n);
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

typedef int ElemType;
typedef struct BSTNode{
	ElemType data;
	struct BSTNode *lchild, *rchild;
} BSTNode, *BSTree;


void Insert(BSTree *tree, ElemType item){  
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

BSTree Search(BSTree tree, ElemType item){ 
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

void Inorder(BSTree tree){ 
	BSTree cursor = tree;
	if (cursor){
		Inorder(cursor->lchild);
		printf("%4d", cursor->data);
		Inorder(cursor->rchild);
	}
}


int ercha(){
	ElemType item;
	BSTree root = NULL, ret; 
	int a[LEN], i, n,j=0,x; 
	printf("\n请输入要查找的数组的长度：\n");
	scanf("%d",&n);
	printf("请选择数据输入方式(1:随机输入,其他键：手动输入):\n");
	scanf("%d",&x);
	shuru(x,a,n);
	printf("\n待查找数组：\t");
	for(i=0;i<n;i++)
		printf("%4d",a[i]);
	while (n--)
		Insert(&root, a[j++]);
	printf("\n数组排序后：\t");
	Inorder(root);
	printf("\n请输入查找数据：\n");
	scanf("%d", &item);
	ret = Search(root, item);
	if (NULL == ret)
		printf("查找失败！");
	else
		printf("查找成功！");
}

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

void CreateHashTable(HashTable *H,int m,int p,int hash[],int n){
	int i,sum,addr,di;
	(*H).data=(DataType*)malloc(m*sizeof(DataType));
	if(!(*H).data)
		exit(-1);
	for(i=0;i<m;i++){            
		(*H).data[i].key=-1;     
		(*H).data[i].hi=0;      
	}
	for(i=0;i<n;i++){                 
		sum=0;                       
		addr=hash[i]%p;              
		di=addr;                     
		if((*H).data[addr].key==-1){     
			(*H).data[addr].key=hash[i];
			(*H).data[addr].hi=1;
		}
		else{                             
			do{ 
				di=(di+1)%m;
				sum+=1;
			}
			while((*H).data[di].key!=-1);
			(*H).data[di].key=hash[i];
			(*H).data[di].hi=sum+1;
		}
	}
	(*H).curSize=n;                
	(*H).tableSize=m;              
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
	shuru(x,hash,n);
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
 
int main(){
	int s,i;
	printf("1.顺序查找               \n");
	printf("2.二分查找               \n");
	printf("3.二叉排序树查找         \n");
	printf("4.哈希查找               \n");
	printf("请输入序号选择查找方式：\n");
	scanf("%d",&s);
	switch(s){
		case 1:
			shunxu(); break;
		case 2:
			erfen(); break;
		case 3:
			ercha(); break;
		case 4:
			haxi(); break;
	}
}

```


# 阉割版
```cpp
#include <stdio.h>
#include <stdlib.h>
#include <malloc.h>
#define LEN 100

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
int shunxu(int a[],int n){
	int key;
	printf("\n请输入想查找的元素：\n");
	scanf("%d",&key);
	Seq_Search(key,a,n),
	printf("\n");
	return 0; 
}

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
int erfen(int a[],int n){
	int i,key,x;
	printf("\n请输入想查找的元素：\t");
	scanf("%d",&key);
	Binary_Search(key,a,n);
}

typedef int ElemType;
typedef struct BSTNode{
	ElemType data;
	struct BSTNode *lchild, *rchild;
} BSTNode, *BSTree;
void Insert(BSTree *tree, ElemType item){  
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
BSTree Search(BSTree tree, ElemType item){ 
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
void Inorder(BSTree tree){ 
	BSTree cursor = tree;
	if (cursor){
		Inorder(cursor->lchild);
		printf("%4d", cursor->data);
		Inorder(cursor->rchild);
	}
}
int ercha(int a[],int n){
	ElemType item;
	BSTree root = NULL, ret; 
	int i,j=0,x; 
	while (n--)
		Insert(&root, a[j++]);
	printf("\n数组排序后：\t");
	Inorder(root);
	printf("\n请输入查找数据：\n");
	scanf("%d", &item);
	ret = Search(root, item);
	if (NULL == ret)
		printf("查找失败！");
	else
		printf("查找成功！");
}

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
void CreateHashTable(HashTable *H,int m,int p,int hash[],int n){
	int i,sum,addr,di;
	(*H).data=(DataType*)malloc(m*sizeof(DataType));
	if(!(*H).data)
		exit(-1);
	for(i=0;i<m;i++){            
		(*H).data[i].key=-1;     
		(*H).data[i].hi=0;      
	}
	for(i=0;i<n;i++){                 
		sum=0;                       
		addr=hash[i]%p;              
		di=addr;                     
		if((*H).data[addr].key==-1){     
			(*H).data[addr].key=hash[i];
			(*H).data[addr].hi=1;
		}
		else{                             
			do{ 
				di=(di+1)%m;
				sum+=1;
			}
			while((*H).data[di].key!=-1);
			(*H).data[di].key=hash[i];
			(*H).data[di].hi=sum+1;
		}
	}
	(*H).curSize=n;                
	(*H).tableSize=m;              
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
int haxi(int hash[],int n){  
	int x,pos,i,k;
	int p=3;
	HashTable H;
	CreateHashTable(&H,n+1,p,hash,n);
	DisplayHash(H,n+1);
	printf("\n请输入想查找的元素：\t");
	scanf("%d",&k);
	pos = SearchHash(H,k);
	printf("关键字%d在哈希表中的位置为：%d\n",k,pos);
	return 0;
}
 
int main(){
	int s,i,n,a[LEN];
	printf("请输入要查找的数组的长度：\n");
	scanf("%d",&n);
	printf("请输入数组的每一个元素：\n");
	for(i=0;i<n;i++)
		scanf("%d",&a[i]);
	printf("请选择查找方式           \n");
	printf("1.顺序查找               \n");
	printf("2.二分查找               \n");
	printf("3.二叉排序树查找         \n");
	printf("4.哈希查找               \n");
	printf("请输入序号选择查找方式：\n");
	scanf("%d",&s);
	switch(s){
		case 1:shunxu(a,n); break;
		case 2:erfen(a,n); break;
		case 3:ercha(a,n); break;
		case 4:haxi(a,n); break;
	}
}
```