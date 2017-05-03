# Minesweeper

#include <stdio.h>
#include <time.h>
#include <stdlib.h>
#define N 10
int a[N][N],b[N][N];
int m=0,n=0,p,q;
int minenumber()//雷的个数
 {  int minenum=0;
    int i=0,j=0;
    for(i=0;i<N;i++)
        for(j=0;j<N;j++)
   { if(a[i][j]==42)//在这之前哪里有a[i][j]=42?
         minenum++;
   }
   return minenum;
}
void putmine()//放雷
{   int minenumber();
    //int i=0,j=0;
    srand(time(NULL));//随机数种子
    for(; minenumber()<30;)//i=0,i++有什么用？
        {   m=rand()%N , n=rand()%N;//随机放置雷
            a[m][n]=42;
        }
}
int inminearea(int m,int n)//去掉边缘外的点
 {  if(m>=0&&m<N&&n>=0&&n<N)
        return 1;
    return 0;
}
int aroundmine(int m,int n)//计算周围雷的个数
{   int inminearea(int m,int n);
    int around=0;
    int i=0,j=0;
    for(i=m-1;i<=m+1;i++)
        for(j=n-1;j<=n+1;j++)
    {   if(!inminearea(i,j))//去掉边缘外的点
            continue;
        if(a[i][j]==42)
            around++;
    }
    return around;
}
int expand(int m,int n)//拓展
{   int aroundmine(int m,int n);//有around已知
    int inminearea(int m,int n);//去掉边缘外的点
    int i=0,j=0;
    if( a[m][n]=='*')
        return 0;//作用？
    if( aroundmine(m,n)!=0 )
       {   b[m][n]=aroundmine(m,n)+48;//得到雷个数数字，但是b不是int吗？
           return 0;
       }
    b[m][n]='0';
    for(i=m-1;i<=m+1;i++)
        for(j=n-1;j<=n+1;j++)
        {
            if( !inminearea(i,j) )
                continue;
            if(b[i][j]!='#')
                continue;
            b[i][j]=aroundmine(i,j)+48;
            if (aroundmine(i,j)==0)
                expand(i,j);//一直输出，直到周围均非零
         }
}
int Remain()//得到#的个数odd,单局确定
{  int odd=0,i=0,j=0;
   for(i=0;i<N;i++)
    for(j=0;j<N;j++)
       if(b[i][j]=='#')
          odd++;
   return odd;
}
/*void myprintf1()//输出a[i][j]
{   int i=0,j=0;
    printf("\n");
    for(i=0;i<N;i++)
        {   for(j=0;j<N;j++)
               printf("%c\t",a[i][j]);
            printf("\n\n");
        }
}*/
void myprintf2()//输出b[i][j]
{   int i=0,j=0;
    printf("\n 下面是游戏雷阵>>  \n");
    for(i=0;i<N;i++)
        {    for(j=0;j<N;j++)
                printf("%4c",b[i][j]);
             printf("\n\n");
        }
}
void main()
{   /*void putmine(int n1);
    int expand(int m,int n);
    int Remain();
    void myprintf1();
    void myprintf2();
    int aroundmine(int m,int n);*/
    //printf("-------------------------------------------------------------------\n*         您接下来要玩一个%d*%d的扫雷游戏。               *\n*请按提示操作（否则会出意外的），                         *\n*         在这个游戏中“*”代表地雷，“#”代表未打开的盒子。       *\n*         当您将全部非雷盒子打开后，您就赢了！                     *\n*         谢谢参与游戏！                                                    *\n-------------------------------------------------------------------",N,N);
    int i=0,j=0;
    for(i=0;i<N;i++)
    {  for(j=0;j<N;j++)
          {a[i][j]=46, b[i][j]='#';
          printf("%4c",b[i][j]);}
          printf("\n");}  //初始设置a为0，b为#
    //printf("\n\n此游戏为%d*%d雷阵,请输入需布雷的数量(%d以内)>> ",N,N,N*N);
    //scanf("%d",&n1);
    //printf("\n下面是所布雷阵>> \n");
    putmine();//先定义雷的位置
    //myprintf1();//输出定义后的数组a
    for(i=0;i<N;i++)
            for(j=0;j<N;j++)
    { if(a[i][j]!='*')
          a[i][j]=aroundmine(i,j)+48;     }
    //printf("\n计数雷阵>>     \n");
    //myprintf1();
    while(Remain()!=30)
    {    printf("\n请输入扫雷坐标,以“，”分开>>   ");
    scanf("%d,%d",&m,&n);
    for(i=0;i<N;i++)
        for(j=0;j<N;j++)
        {  if(i==m-1&&j==n-1)
           p=i,q=j;      }
        if(a[p][q]==42)
        {
            printf("\n杯具，你踩到雷了，所以你lose了!\n");
            break;
        }
           //goto end;
        else expand(p,q);
        myprintf2();
    }
  printf("\n小样，你赢了！ 有本事再来一次！  \n");
  //end:    if(Remain()!=30)
            // printf("\n杯具，你踩到雷了，所以你lose了!\n");
}
