
//散列表查找算法（Hash）
#include <stdio.h>
#include <stdlib.h>
#define OK 1
#define ERROR 0
#define TRUE 1
#define FALSE 0
#define SUCCESS 1
#define UNSUCCESS 0
#define HASHSIZE 7
#define NULLKEY -32768
typedef int Status;
typedef struct
{
  int *elem;           //基址
  int count;           //当前数据元素个数
}HashTable;

int m=0; // 散列表表长

/*初始化*/
Status Init(HashTable *hashTable)
{
  int i;
  m=HASHSIZE;
  hashTable->elem = (int *)malloc(m * sizeof(int)); //申请内存
  hashTable->count=m;
  for (i=0;i<m;i++)
  {
    hashTable->elem[i]=NULLKEY;
  }
  return OK;
}

/*哈希函数(除留余数法)*/
int Hash(int data)
{
  return data % m;
}

/*插入*/
void Insert(HashTable *hashTable,int data)
{
  int hashAddress=Hash(data); //求哈希地址

  //发生冲突
  while(hashTable->elem[hashAddress]!=NULLKEY)
  {
    //利用开放定址的线性探测法解决冲突
    hashAddress=(++hashAddress)%m;
  }

  //插入值
  hashTable->elem[hashAddress]=data;
}

/*查找*/
int Search(HashTable *hashTable,int data)
{
  int hashAddress=Hash(data); //求哈希地址

  //发生冲突
  while(hashTable->elem[hashAddress]!=data)
  {
    //利用开放定址的线性探测法解决冲突
    hashAddress=(++hashAddress)%m;

    if (hashTable->elem[hashAddress]==NULLKEY||hashAddress==Hash(data)) return -1;
  }

  //查找成功
  return hashAddress;
}

/*打印结果*/
void Display(HashTable *hashTable)
{
  int i;
  printf("\n//==============================//\n");

  for (i=0;i<hashTable->count;i++)
  {
    printf("%d ",hashTable->elem[i]);
  }

  printf("\n//==============================//\n");
}
//斐波那契数列
int Fbi(int i)
{
    if(i<2)
        return i==0?0:1;
    return Fbi(i-1)+Fbi(i-2);
}
//顺序查找
int sequential_search(int * S,int n,int key )
{
    int i=n-1;
    while(S[i]!=key)
        i--;
    return i;
}
//有序查找：折半(必须基本有序)
int zheban_search(int *S,int n,int key)
{
    int low,high,mid;
    low=0,high=n-1;
    while(low<=high)
    {
        mid=(high+low)/2;
        if(S[mid]<key)
            low=mid+1;
        else if(S[mid]>key)
            high=mid-1;
        else
            return mid;

    }
}
//有序查找：插值(必须基本有序)
int chazhi_search(int *S,int n,int key)
{
    int low,high,mid;
    low=0,high=n-1;
    while(low<=high)
    {
        mid=low+(high-low)*(key-S[low])/(S[high]-S[low]);
        if(S[mid]<key)
            low=mid+1;
        else if(S[mid]>key)
            high=mid-1;
        else
            return mid;

    }

}
//有序查找：斐波那契查找
int Fib_search(int *S,int n,int key)
{
    int k=0;
    int j=n-1;
    int low=0;
    int high=n-1;
    while(j>Fbi(k)-1)
        k++;
    for(j=n-1;j<Fbi(k)-1;j++)
        S[j]=S[n-1];
    /*int i=0;
    for(i=0;i<Fbi(k)-1;i++)
        printf("%d  ",S[i]);*/
    while(low<=high)
    {
        int mid = low+Fbi(k-1)-1;
        if(S[mid]<key)
        {
            low=mid+1;
            k=k-2;
        }
        else if(S[mid]>key)
        {
            high=mid-1;
            k=k-1;

        }
        else
        {
            if(mid<=n-1)
                return mid;
            else
                return n-1;
        }

    }

}
//二叉树排序
typedef struct BitNode{
    int data;
    struct BitNode *left,*right;

}BitNode,*BitTree;

int SearchBST(BitTree T,int key,BitTree f,BitTree p)
{
    if (!T)
    {
        p=f;
        return FALSE;
    }
    else if(key==T->data)
    {
        p=T;
        return TRUE;
    }
    else if(key<T->data)
        return SearchBST(T->left,key,T,p);
    else
        return SearchBST(T->right,key,T,p);
}
//二叉排序树插入操作
int InsertBST(BitTree T,int key)
{
    BitTree p,s;
    if(!SearchBST(T,key,NULL,p))
    {
        s=(BitTree)malloc(sizeof(BitNode));
        s->data=key;
        s->left=s->right=NULL;
        if(!p)
            T=s;
        else if(key<p->data)
            p->left=s;
        else
            p->right=s;
        return TRUE;
    }
    else
        return FALSE;
}
//创建二叉排序树
void CreateBST(BitTree bst)
{
    int key;
    bst = NULL;
     scanf("%d", &key);
    while(key != -1)
    {
        InsertBST(bst, key);
        scanf("%d", &key);
    }
}
/*
 *打印二叉树：
 *中序遍历
 * */
void InOrder(BitTree root)
{
    if(root != NULL)
    {
        InOrder(root->left);
        printf(" %d ", root->data);
        InOrder(root->right);
    }
}
/*
*****************百度二叉排序树********
typedef struct node
{
    int key;
    struct node *lchild, *rchild;
}BSTNode, *BSTree;

//插入
int InsertBST(BSTree *bst, int k)
{
    BSTree r, s, pre;
    r = (BSTree)malloc(sizeof(BSTNode));
    r->key = k;
    r->lchild = NULL;
    r->rchild = NULL;
    if(*bst == NULL)
    {
        *bst = r;
        return 1;
    }
    pre = NULL;
    s = *bst;
    while(s)
    {
        if(k == s->key)
                return 0;
        else if(k < s->key)
        {
            pre = s;
            s = s->lchild;
        }
        else
        {
            pre = s;
            s = s->rchild;
        }
    }
    if(k < pre->key)
            pre->lchild = r;
    else
            pre->rchild = r;
    return 1;
}


void CreateBST(BSTree *bst)
{
    int key;
    *bst = NULL;
    scanf("%d", &key);
    while(key != -1)
    {
        InsertBST(bst, key);
        scanf("%d", &key);
    }
}

/*
 *打印二叉树：
 *中序遍历
 * */
 /*
void InOrder(BSTree root)
{
    if(root != NULL)
    {
        InOrder(root->lchild);
        printf(" %d ", root->key);
        InOrder(root->rchild);
    }
}

/*
 *搜索
 * */
 /*
BSTree SearchBST(BSTree bst, int key)
{
    BSTree q;
    q = bst;
    //递归
    while(q)
    {
            if(q->key == key)
                    return q;
            if(q->key > key)
                    q=q->lchild;
            else
                    q=q->rchild;
    }
    return NULL;                        //查找失败
}
*/
int main()
{
  /*
    int data[10] = {2,4,6,7,23,43,65,65,79,98};
    //int tmp[10]; BSTree T;

    int i;
*/
   /* BSTree T;
    int tag = 1;
    int m, n;
    printf("建立二叉排序树，请输入序列以-1结束：\n");
    CreateBST(T);
    printf("中序遍历二叉树，序列为：\n");
    InOrder(T);
    printf("\n");
    while(tag != 0)
    {
        printf("请输入你要查找的元素:\n");
        scanf("%d", &n);
        if(SearchBST(T, n) == NULL)
                printf("抱歉查找失败!\n");
        else
                printf("查找成功！查找的数字为%d\n", SearchBST(T,n)->key);
        printf("是否继续查找 是 ：请按 1 否则按 0:\n");
        scanf("%d", &tag);
    }*/
   int i,j,result;
  HashTable hashTable;
  int arr[HASHSIZE]={13,29,27,28,26,30,38};

  printf("***************Hash哈希算法***************\n");

  //初始化哈希表
  Init(&hashTable);

  //插入数据
  for (i=0;i<HASHSIZE;i++)
  {
    Insert(&hashTable,arr[i]);
  }
  Display(&hashTable);

  //查找数据
  result= Search(&hashTable,29);
  if (result==-1)
      printf("对不起，没有找到！\n");
  else
      printf("29在哈希表中的位置是:%d\n",result);
  return 0;


}
