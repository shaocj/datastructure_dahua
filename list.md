#include <stdio.h>
#include <stdlib.h>

#define LIST_INIT_SIZE    5    //链表初始长度
#define ERROR   0
#define OK      1
typedef int Elemtype;
//链表结构体
typedef struct LNode
{
    Elemtype data;
    struct LNode *next;
}LNode,*LinkList;
linkList InitListNode(LinkList R)
{
    R = (LinkList) malloc(sizeof(ListNode));
    R->next = NULL;
    return R;
}
//获取链表中指定位置的数据
int GetElem_L(LinkList L, int i, Elemtype *e)
{
    //L为带头结点的单链表的头指针。当第i个元素存在时，其赋值给e并返回OK，否则返回ERROR
    LinkList p;
    p= L->next;
    int j = 1;
    while (p&&j<i)
    {
        p = p->next;
        ++j;
    }
    if (!p || j>i) return ERROR;
    *e = p->data;
    return OK;
}

//向链表中指定位置插入数据
int ListInsert_L(LinkList L, int i, Elemtype *e)
{
   LinkList p,s;
    p = L;//p = L->next;
    int j = 0;//int j = 1;
    while (p&&j<i-1)
    {
        p = p->next;
        ++j;
    }
    if (!p || j>i-1) return ERROR;

    s = (LinkList) malloc(sizeof(LNode));
    s->data = *e;
    s->next = p->next;
    p->next = s;
    return OK;
}

//删除链表中指定位置的数据
int ListDelete_L(LinkList L, int i, Elemtype *e)
{
    LinkList p, q;
   p = L;//p = L->next;
    int j = 0;//int j = 1;
    while (p->next && j<i - 1)
    {
        p = p->next;
        ++j;
    }
    if (!(p->next) || j>i - 1) return ERROR;
    q = p->next;
    p->next = q->next;
    *e = q->data;
    free(q);
    return OK;
}
//整表删除
int AllListDelete_L(LinkList L)
{
    LinkList p,q;
    p=L->next;
    while(p)
    {
        q=p->next;
        free(p);
        p=q;
    }
    L->next=NULL;
    return OK;
}
//创建链表,录入n个原始数据
void CreateList_L(LinkList L, int n)
{
    int i;
    int v;
    LinkList p;
    L->next = NULL;

    //循环录入数据
    for (i = 0; i<n; i++)
    {
        p = (LinkList) malloc(sizeof(LNode));
        scanf("%d", &v);
        p->data = v;
        p->next = L->next;  //头插法
        L->next = p;
    }
}

//尾插法
void CreateList_L_W(LinkList L, int n)
{
    int i;
    int v;
    LinkList p,r;

    r=L;
    //循环录入数据
    for (i = n; i>0; --i)
    {
        p = (LinkList) malloc(sizeof(LNode));
        scanf("%d", &v);
        p->data = v;
        r->next=p;
        r=p;
    }
    r->next=NULL;
}

//输出链表中的数据
int PrintList_L(LinkList L, char* s){
   LinkList p;
    printf("%s", s);
    p = L->next;
    if (p == NULL)
    {
        printf("链表为空\n");
        return ERROR;
    }
    while (p != NULL){
        printf("%d   ", p->data);
        p = p->next;
    }
    printf("\n");
    return OK;
}

int main()
{
    //定义链表
    LinkList L,R;
    L=InitListNode(R);
    //初始化链表
    printf("请输入链表中初始的%d个数据：", LIST_INIT_SIZE);
    CreateList_L_W(L, LIST_INIT_SIZE);
    PrintList_L(L,"初始化的链表：");

    //定义插入/删除操作时位置和数值参数
    int s, v;

    //插入功能演示
    printf("请输入数据插入的位置s 和数值v ：");
    scanf("%d%d", &s, &v);
    printf("%s", ListInsert_L(L, s, &v) ? "插入成功.\n" : "插入失败.\n");
    PrintList_L(L,"插入后的链表：");

    //删除功能演示
    printf("请输入数据删除的位置s ：");
    scanf("%d", &s);
    if (ListDelete_L(L, s, &v))
        printf("删除成功.删除的数据是：%d\n", v);
    else
        printf("删除失败.位置有误.");
    PrintList_L(L,"删除后的链表：");

    //查询功能演示
    printf("请输入要查询的数据位置s ：");
    scanf("%d", &s);
    GetElem_L(L,s,&v);
    printf("第%d个数是：%d\n\n",s,v);

    system("pause");
    return 0;
}

/****循环列表****/
/*
#include <stdio.h>
#include <stdlib.h>
typedef struct Node
{
    int data;
    struct Node *pNext;
}NODE, *pNODE;
//创建单向循环链表
pNODE CreateSgCcLinkList(void)
{
    int i, length = 0, data = 0;
    pNODE pTail = NULL, p_new = NULL;
    pNODE pHead = (pNODE)malloc(sizeof(NODE));

    if (NULL == pHead)
    {
        printf("内存分配失败！\n");
        exit(EXIT_FAILURE);
    }

    pHead->data = 0;
    pHead->pNext = pHead;
    pTail = pHead;

    printf("请输入要创建链表的长度：");
    scanf("%d", &length);

    for (i=1; i<length+1; i++)
    {
        p_new = (pNODE)malloc(sizeof(NODE));

        if (NULL == p_new)
        {
            printf("内存分配失败！\n");
            exit(EXIT_FAILURE);
        }

        printf("请输入第%d个节点的元素值：", i);
        scanf("%d", &data);

        p_new->data = data;
        p_new->pNext = pHead;    //这里一定是pHead，因为最后一个节点总是指着头节点
        pTail->pNext = p_new;
        pTail = p_new;
    }

    return pHead;
}
//打印链表
void TraverseSgCcLinkList(pNODE pHead)
{
    pNODE pt = pHead->pNext;

    printf("链表打印如：");
    while (pt != pHead)
    {
        printf("%d ", pt->data);
        pt = pt->pNext;
    }
    putchar('\n');
}

//判断链表是否为空
int IsEmptySgCcLinkList(pNODE pHead)
{
    if (pHead->pNext == pHead)
        return 1;
    else
        return 0;
}

//计算链表长度
int GetLengthSgCcLinkList(pNODE pHead)
{
    int length = 0;
    pNODE pt = pHead->pNext;

    while (pt != pHead)
    {
        length++;
        pt = pt->pNext;
    }
    return length;
}
//向链表中插入节点
int InsertEleSgCcLinkList(pNODE pHead, int pos, int data)
{
    pNODE p_new = NULL;

    if (pos > 0 && pos < GetLengthSgCcLinkList(pHead) + 2)
    {
        p_new = (pNODE)malloc(sizeof(NODE));

        if (NULL == p_new )
        {
            printf("内存分配失败！\n");
            exit(EXIT_FAILURE);
        }

        while (1)
        {
            pos--;
            if (0 == pos)
                break;
            pHead = pHead->pNext;
        }

        p_new->data = data;
        p_new->pNext = pHead->pNext;
        pHead->pNext = p_new;

        return 1;
    }
    else
        return 0;
}
int DeleteEleSgCcLinkList(pNODE pHead, int pos)
{
    pNODE pt = NULL;

    if (pos > 0 && pos < GetLengthSgCcLinkList(pHead) + 1)
    {
        while (1)
        {
            pos--;
            if (0 == pos)
                break;
            pHead = pHead->pNext;
        }

        pt = pHead->pNext->pNext;
        free(pHead->pNext);
        pHead->pNext = pt;

        return 1;
    }
    else
        return 0;
}
//删除整个链表，释放内存
void FreeMemory(pNODE *ppHead)
{
    pNODE pt = NULL;

    while (*ppHead != NULL)
    {
        if (*ppHead == (*ppHead)->pNext) //如果只有头节点一个
        {
            free(*ppHead);
            *ppHead = NULL;
        }
        else                    //如果不止头节点一个
        {
            pt = (*ppHead)->pNext->pNext;
            free((*ppHead)->pNext);
            (*ppHead)->pNext = pt;
        }
    }
}
int main(void)
{
    int flag = 0, length = 0;
    int position = 0, value = 0;
    pNODE head = NULL;

    head = CreateSgCcLinkList();

    flag = IsEmptySgCcLinkList(head);
    if (flag)
        printf("单向循环链表为空！\n");
    else
    {
        length = GetLengthSgCcLinkList(head);
        printf("单向循环链表的长度为：%d\n", length);
        TraverseSgCcLinkList(head);
    }

    printf("请输入要插入节点的位置和元素值(两个数用空格隔开)：");
    scanf("%d %d", &position, &value);
    flag = InsertEleSgCcLinkList(head, position, value);
    if (flag)
    {
        printf("插入节点成功！\n");
        TraverseSgCcLinkList(head);
    }
    else
        printf("插入节点失败！\n");

    flag = IsEmptySgCcLinkList(head);
    if (flag)
        printf("单向循环链表为空，不能进行删除操作！\n");
    else
    {
        printf("请输入要删除节点的位置：");
        scanf("%d", &position);
        flag = DeleteEleSgCcLinkList(head, position);
        if (flag)
        {
            printf("删除节点成功！\n");
            TraverseSgCcLinkList(head);
        }
        else
            printf("删除节点失败！\n");
    }

    FreeMemory(&head);
    if (NULL == head)
        printf("已成功删除单向循环链表，释放内存完成！\n");
    else
        printf("删除单向循环链表失败，释放内存未完成！\n");

    return 0;
}
*/

/***双向链表***/
/*
#include <stdio.h>
#include <stdlib.h>
typedef struct Node
{
    int data;
    struct Node *pNext;
    struct Node *pPre;
}NODE, *pNODE;
//创建双向循环链表
pNODE CreateDbCcLinkList(void)
{
    int i, length = 0, data = 0;
    pNODE p_new = NULL, pTail = NULL;
    pNODE pHead = (pNODE)malloc(sizeof(NODE));

    if (NULL == pHead)
    {
        printf("内存分配失败！\n");
        exit(EXIT_FAILURE);
    }
    pHead->data = 0;
    pHead->pNext = pHead;
    pHead->pPre = pHead;
    pTail = pHead;

    printf("请输入想要创建链表的长度：");
    scanf("%d", &length);

    for (i=1; i<length+1; i++)
    {
        p_new = (pNODE)malloc(sizeof(NODE));

        if (NULL == p_new)
        {
            printf("内存分配失败！\n");
            exit(EXIT_FAILURE);
        }

        printf("请输入第%d个节点元素值：", i);
        scanf("%d", &data);

        p_new->data = data;
        p_new->pPre = pTail;
        p_new->pNext = pHead;
        pTail->pNext = p_new;
        pHead->pPre = p_new;
        pTail = p_new;
    }
    return pHead;
}
//打印链表
void TraverseDbCcLinkList(pNODE pHead)
{
    pNODE pt = pHead->pNext;

    printf("链表打印如：");
    while (pt != pHead)
    {
        printf("%d ", pt->data);
        pt = pt->pNext;
    }
    putchar('\n');
}

//判断链表是否为空
int IsEmptyDbCcLinkList(pNODE pHead)
{
    pNODE pt = pHead->pNext;

    if (pt == pHead)
        return 1;
    else
        return 0;
}

//计算链表的长度
int GetLengthDbCcLinkList(pNODE pHead)
{
    int length = 0;
    pNODE pt = pHead->pNext;

    while (pt != pHead)
    {
        length++;
        pt = pt->pNext;
    }
    return length;
}
//向链表中插入节点
int InsertEleDbCcLinkList(pNODE pHead, int pos, int data)
{
    pNODE p_new = NULL, pt = NULL;
    if (pos > 0 && pos < GetLengthDbCcLinkList(pHead) + 2)
    {
        p_new = (pNODE)malloc(sizeof(NODE));

        if (NULL == p_new)
        {
            printf("内存分配失败！\n");
            exit(EXIT_FAILURE);
        }

        while (1)
        {
            pos--;
            if (0 == pos)
                break;
            pHead = pHead->pNext;
        }

        p_new->data = data;
        pt = pHead->pNext;
        p_new->pNext = pt;
        p_new->pPre = pHead;
        pHead->pNext = p_new;
        pt->pPre = p_new;

        return 1;
    }
    else
        return 0;
}

//从链表中删除节点
int DeleteEleDbCcLinkList(pNODE pHead, int pos)
{
    pNODE pt = NULL;
    if (pos > 0 && pos < GetLengthDbCcLinkList(pHead) + 1)
    {
        while (1)
        {
            pos--;
            if (0 == pos)
                break;
            pHead = pHead->pNext;
        }
        pt = pHead->pNext->pNext;
        free(pHead->pNext);
        pHead->pNext = pt;
        pt->pPre = pHead;

        return 1;
    }
    else
        return 0;
}
//删除整个链表，释放内存空间
void FreeMemory(pNODE *ppHead)
{
    pNODE pt = NULL;
    while (*ppHead != NULL)
    {
        pt = (*ppHead)->pNext->pNext;


        if ((*ppHead)->pNext == *ppHead)
        {
            free(*ppHead);
            *ppHead = NULL;
        }
        else
        {
            free((*ppHead)->pNext);
            (*ppHead)->pNext = pt;
            pt->pPre = *ppHead;
        }
    }
}
int main(void)
{
    int flag = 0, length = 0;
    int position = 0, value = 0;
    pNODE head = NULL;

    head = CreateDbCcLinkList();

    flag = IsEmptyDbCcLinkList(head);
    if (flag)
        printf("双向循环链表为空！\n");
    else
    {
        length = GetLengthDbCcLinkList(head);
        printf("双向循环链表的长度为：%d\n", length);
        TraverseDbCcLinkList(head);
    }

    printf("请输入要插入节点的位置和元素值(两个数用空格隔开)：");
    scanf("%d %d", &position, &value);
    flag = InsertEleDbCcLinkList(head, position, value);
    if (flag)
    {
        printf("插入节点成功！\n");
        TraverseDbCcLinkList(head);
    }
    else
        printf("插入节点失败！\n");

    flag = IsEmptyDbCcLinkList(head);
    if (flag)
        printf("双向循环链表为空，不能进行删除操作！\n");
    else
    {
        printf("请输入要删除节点的位置：");
        scanf("%d", &position);
        flag = DeleteEleDbCcLinkList(head, position);
        if (flag)
        {
            printf("删除节点成功！\n");
            TraverseDbCcLinkList(head);
        }
        else
            printf("删除节点失败！\n");
    }

    FreeMemory(&head);
    if (NULL == head)
        printf("已成功删除双向循环链表，释放内存完成！\n");
    else
        printf("删除双向循环链表失败，释放内存未完成！\n");

    return 0;
}

*/
