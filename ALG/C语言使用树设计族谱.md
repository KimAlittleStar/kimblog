# C语言使用树设计族谱  

## 问题描述
 **家属关系查询系统**  
 1. 问题描述：用树结构表示你所在家族的家谱关系，实现家族信息的管理与查询。
 2. 具体要求：此系统应实现如下功能。  
     1. 建立(打开)家族关系树。  
     2. 添加(删除)家属成员。  
     3. 家属关系查询，例如查找某人的祖先、双亲、兄弟、孩子、堂兄弟、后代子孙、查询某入居于家族中的第几代。


## 族谱规则
 族谱只会跟踪记录男性以及男性的后代,对于女性我们只会记录她在何时出嫁,并不记录她的后代,或者说族谱中的人员向上追溯的时候默认追溯的是父亲一支的关系;  

## 逻辑分析
 族谱与数据结构中树的概念相结合,每一个节点就是族谱中的个人,于是我们就需要知道每个人最基本的特性,于是就可以抽象化变成树中的属性;   
 - 父母  (parent)       --人
 - 兄弟  (brother)      --人
 - 伴侣  (soulmate)     --人
 - 孩子  (children)     --人
 - 姓名  (name)         --字符串
 - 生辰八字 (birthday)  --日期
 - 性别  (gender)       --性别
  
  那么我们对应的树的结构就出来了
  ```C
  // def Tree node 定义节点
  typedef struct _FamilyNode
  {
      struct _FamilyNode* parent;
      struct _FamilyNode* brother;
      struct _FamilyNode* soulmate;
      struct _FamilyNode* children;
      char name[30];
      char birthday[20];
      char gender[6];
  } FamilyNode;

  // def Family Tree 定义族谱
  typedef struct _FamilyTree
  {
      FamilyNode* root;
      int deep;
  } FamilyTree;
  ```
  

## 其他问题

- **关于孩子的问题**  
很多人的孩子不止一个,那么我们就会将二儿子/三儿子添加到对应的孩子的brother指针,并且将parent指针指向自己哥哥的父母;

- **关于离婚的问题**  
  我们都是好孩子,不考虑离婚再婚.

- **关于变性问题**  
  - ### ***不考虑---不要问---问了就是不考虑***  

- 简单示例   
  这是小明家的族谱  
  ![小明族谱.png](https://s2.ax1x.com/2020/01/02/lYybnO.png)  
  ```
  小明->parent = NULL;
  小明->brother = NULL;
  小明->soulmate = NULL;
  小明->children = 武大郎;

  武大郎->parent = 小明;
  武大郎->brother = 武松;
  武大郎->soulmate = 潘金莲;
  武大郎->children = 小红;

  武松->parent = 小明;
  武松->brother = NULL;
  武松->soulmate = 玉兰;
  武松->children = 龙龙;

  ....

  ```

## **代码实现部分**  

### 头文件定义

 ```C
 // headfile familytree.h

 #ifndef _familytree_h
 #define _familytree_h
 #include <stdio.h>
 
 typedef struct
 {
     char name[30];
     char birthday[20];
     char gender[6];
 } Human;
 
 typedef struct _FamilyNode
 {
     struct _FamilyNode *parent;
     struct _FamilyNode *brother;
     struct _FamilyNode *soulmate;
     struct _FamilyNode *children;
     Human man;
     int deep;
 } FamilyNode;
 
 // def Family Tree 定义族谱
 typedef struct _FamilyTree
 {
     FamilyNode *root;
     char introduce[200]; ///< 对于该族谱的简单介绍
 } FamilyTree;
 
 FamilyTree *newFamilyTree(Human origin);
 void *deleteFamilyTree(FamilyTree *tree);
 
 int insertNewBaby(FamilyNode *parent, Human baby);
 int marryPeople(FamilyNode *wifi, FamilyNode *husband);
 int editFamilyNode(FamilyNode *old, FamilyNode *newone);
 void removeFamilyNode(FamilyTree *tree, FamilyNode *node);
 
 FamilyNode *findFamilyNodee(Human man);
 FamilyNode *findOrigin(FamilyNode *node, int generation);
 FamilyNode *findParent(FamilyNode *node);
 
 void printBrother(FamilyNode *node);
 void printCousin(FamilyNode *node);
 
 extern int
 compareFamilyNode(FamilyNode *a, FamilyNode *b);
 
 #endif // _familytree_h
 ```  

### 增删改查基础操作

#### 1. new 一个家族的崛起
#### 2. delete 这个家族覆灭了  

#### 3. insert 我要当爸爸了
#### 4. edit 我把我儿子改名叫"狗子"
#### 5. remove 我与某某人断绝父子关系
#### 6. find 皇上派我来查查你的底细  

  
### 解决上述题目中的几个问题

 1. 找祖先
 2. 找双亲
 3. 找兄弟
 4. 找孩子
 5. 找堂兄弟
 6. 找后代子孙
 7. 查询某入居于家族中的第几代。

### 运行结果


## 后记  
 本问题并不考虑算法问题,因此效率/空间/时间复杂度在这里不做考虑;

