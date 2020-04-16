#include <stdio.h>
#include <string.h>
#include <stdlib.h>

//大话朴素
void Nor_Index(char *s,char *p,int sLength,int pLength)
{
    int i=0;
    int j=0;
    while(i<sLength && j<pLength){
        if(s[i] == p[j]){
            i++;
            j++;
        }
        else{
            i=i-j+1;
            j=0;
        }
    }
    if(j>pLength-1){
        int e=i-pLength;
        printf("匹配:%d \n",e);

    }
    else
        printf("失败 \n");

}
//大话数据结构

void getNext(char * p,int pLength,int * next) {

    next[0] = -1;

    int j = 0;
    int i;

    int k = -1;

    while (j < pLength - 1) {

       if (k == -1 || p[j] == p[k]) {


            j++;
            k++;
           if (p[j] == p[k]) { // 当两个字符相等时要跳过
              next[j] = next[k];

           } else {

              next[j] = k;

           }

       } else {

           k = next[k];

       }

    }
    for(i=0;i<pLength;i++){
        printf("改进next：%d\n",*(next+i));
    }

    //return next;

}
void getNext1(char * p,int pLength,int * next) {

    next[0] = -1;

    int j = 0;
    int i;

    int k = -1;

    while (j < pLength - 1) {

       if (k == -1 || p[j] == p[k]) {


            j++;
            k++;


              next[j] = k;



       } else

           k = next[k];



    }
    for(i=0;i<pLength;i++){
        printf("未改进next：%d\n",next[i]);
    }

    //return next;

}
 void KMP(char *s,char *p,int sLength,int pLength) {


    int i = 0; // 主串的位置

    int j = 0; // 模式串的位置
    int next[255];

    getNext1(p,pLength,next);

    while (i < sLength && j < pLength) {

       if (j == -1 || s[i] == p[j]) { // 当j为-1时，要移动的是i，当然j也要归0

           i++;

           j++;

       } else {

           // i不需要回溯了

           // i = i - j + 1;

           j = next[j]; // j回到指定位置

       }

    }

    if(j>pLength-1){
        int e=i-pLength;
        printf("匹配:%d \n",e);

    }
    else
        printf("失败 \n");

}




int main()
{
    char *s = "abbacbcdhia";
    char *p = "ababb";
    int pLength = strlen(p);
    int sLength = strlen(s);
    //int *prefix = (int *)malloc(pLength*sizeof(int));
    //kmpPrefixFunction(p,pLength,prefix);
    //printf("字符串的最长前缀长度分别是:");
    /*int i;
    for(i=0; i<pLength; i++)
    {
        printf("%d\t",prefix[i]);
    }
    */
    printf("\n使用KMP匹配\n");
   // kmpMatch(s,strlen(s),p,pLength,prefix);
   KMP(s,p,sLength,pLength);
    printf("使用朴素算法:\n");
   // normal_match(s,strlen(s),p,pLength);
   Nor_Index(s,p,sLength,pLength);

    return 0;
}
https://blog.csdn.net/starstar1992/article/details/54913261/
https://blog.csdn.net/v_july_v/article/details/7041827
https://www.cnblogs.com/yjiyjige/p/3263858.html

