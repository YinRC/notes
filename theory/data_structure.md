# 数据结构复习	



## 1. 线性表



### 	1.1 性质

> + 线性表 $\rightarrow$ 顺序表 & 链表
>
> + 单链表 $\rightarrow$ head、tail、next
> + 双链表 $\rightarrow$ head、tail、next、prev



### 	1.2 顺序表

```c++
//顺序表的插入 右移 变长
for(int i=Length; i>position; i--)		a[i]=a[i-1];
```

```c++
//顺序表的删除 左移 缩短
for(int i=position; i<Length-1; i++)	a[i]=a[i+1];
```



### 	1.3 单链表

```c++
//单链表的插入
Link<T> *p,*q;
if (p = setPos(i - 1) == NULL){		//p为前驱，q为待插结点
	cout << "false";
    return false
}
q = new Link<T>(value, p->next);	//q的指针域设为p->next
p->next = q;
if (p == tail)	tail = q;
return true;
```

```c++
//单链表的删除
Link<T> *p,*q;
if (p = setPos(i - 1) == NULL || p == tail){
    cout << "false";
    return false;
}
q = p->next;
if (q == tail){
    p->next = NULL;
    delete q;
    tail = p;
}
else/*else if (q != NULL)*/{
    p->next = q->next;
    delete q;
}
return true;
```



### 	1.4 双链表

```c++
//双链表删除
p->prev->next = p->next;	//p的前后
p->next->prev = p->prev;
p->prev = NULL;				//p的删除
p->next = NULL;
delete p;
```

```c++
//双链表的插入
Link<T> *q; 
q = new Link<T>(value);
q->prev = p;				//新结点在链表中的关系
q->next = p->next;
p->next = q;				//修改q的前驱和后继
q->next->prev = q;
```



## 2. 栈与队列



### 		2.1 顺序栈

```c++
bool push(const T item){
    if (top == size - 1){
        /*cout << "false";
        return false;*/
        T *newSt = new T [size*2];
        for(int i = 0; i < size; i++){
            newSt[i] = st[i];
        }
        delete[] st;	//清理
        st = newSt;		//重指
        size = size*2;	//改参
    }
    else {
        st[++top] = item;
        return true;
    }
}
bool pop(T &item){
    if (top == -1){				//空栈top = -1
        cout << "false";
        return false;
    }
    else {
        item = st[top--];
        return true;
    }
}
bool top(T &item){
    if (top == -1){
        cout << "false";
        return false;
    }
    else {
        item = st[top];
        return true;
    }
}
```



### 		2.2 链式栈

```c++
bool push(const T item){
    Link<T> *tmp = new Link<T>(item, top);
    top = item;
    size++;		//可变的空间
    return true;
}
bool pop(T &item){
    Link<T> *tmp;
    if (size == 0){
        cout << "false";
        return false;
    }
    else { 
        tmp = top;
        item = tmp->value;
        top = top->next;
        delete item;
        size--;		//可变的空间
        return true;
    }
}
bool top(T &item){
    if (size == 0){
        cout << "false";
        return false;
    }
    item = top->value;
    return true;
}
```



###			2.3 顺序队列

> 循环队列：节省空间，防止数组越界 、防止假溢出现象

```c++
mSize = size + 1;						//区别FULL和EMPTY
qu = new T [mSize];
front = rear = 0;
bool enQueue(const T item){
    if ((rear + 1) % mSize == front){	//FULL
        cout << "FULL";
        return false;
    }
    else {
        qu[rear] = item;
        rear = (rear + 1) % mSize;
        return true;
    }
}
bool deQueue(T &item){
    if (rear == front){					//EMPTY
        cout << "EMPTY";
        return false;
    }
    else {
        item = qu[front];
        front = (front + 1) % mSize;
        return true;
    }
}
```



### 	2.4 链式队列

```c++
int size;
Link<T> *front, *rear;
size = 0;
front = rear = NULL;
bool enQueue(const T item){
    if (rear == NULL){
        front = rear = new Link<T>(item, NULL);
    }
    else {
        rear->next = new Link<T>(item, NULL);
        rear = rear->next;
    }
    size++;
    return true;
}
bool deQueue(T &item){
    Link<T> *tmp;
    if(/*front == NULL*/size == 0){
        cout << "EMPTY";
        return flase;
    }
    else {
        item = front->value;
        tmp = front;
        front = front->next;
        delete tmp;
        if (front == NULL)	
            rear = NULL;
        size--;
        return true;
    }
}
```



### 2.5 栈的应用



#### 2.5.1 括号匹配

> ch=非括号：不做任何处理
>
> ch=左括号：入栈
>
> ch=右括号：
>
> if (栈空) return false
>  else {
>        出栈，检查匹配情况，
>         if (不匹配)  return false
>  }
> 如果结束后，栈非空，返回false



```c++
bool matching(char exp[ ])
	{ //验证括号匹配情况，匹配返回1。‘#’为表达式结束符。
  		Stack s;
             i=0;  ch=exp[i++];
  		while (ch!=‘#’) {
        	    switch (ch) {
           	    	case ‘(‘, ‘[’ :   s.push(ch);   break;
          	    	case ‘)’:  s.top(e); 
                                      if (!s.isEmpty() && e ==‘(‘) 
                                          { s.pop(e); break; }
                                	  else 
                                          return false;
                 	case ‘]’:  s.top(e); 
                                        if (!s.isEmpty()&&e ==‘[‘) 
                                           { s.pop(e); break; }
                                         else 
                                           return false;
            	   }
                   ch=exp[i++];
        }
    	if (!s.isEmpty()) 
               return false;
    	else return true;
    }
```



## 3. 二叉树



### 	3.1 性质

> + 二叉树：可以为空集，可以是独根
> + 度：一个结点的子树数目
> + 内部结点：非终端（叶子）结点
> + 外部结点：树叶
> + 满二叉树：任何结点或者是树叶，或者是左右子树非空
> + 完全二叉树：从左至右顺序排列
> + 扩充二叉树：在空子树的位置添加空树叶 $E = I +2n$ 
> + **深度**：路径边的个数、最大层数（从0开始）
> + **高度**：路径点的个数、最大层数 + 1（从1开始）
> + 性质
>   1. 第 $i$ 层上最多有 $2^i$ 个结点
>2. 深度为$k$ 的二叉树至多有$2^{k+1}-1$ 个结点
>   3. 深度为$k$ 的二叉树至少有$2^{k}$ 个结点
>4. $n_0 = n_2+1$**即度数为0的结点=度数为2的结点+1**
>   5. **满二叉树定理**：非空满二叉树 **树叶 = 内部结点 + 1** 
>6. **满二叉树定理推论**：非空二叉树 **空子树数目 = 结点数 + 1**
>   7. 有$n$ 个结点的**完全二叉树**   
>
>      + **深度**为$\lceil\log_2^{n+1}\rceil-1$ 
>   + **高度**$\lceil\log_2^{n+1}\rceil$
>   8. 有$n$ 个结点的**完全二叉树**对任意一结点$i(0\leq i\leq n-1)$ 有：
>
>      + 父结点 $\lfloor (i-1)/2 \rfloor$
> 
>   + 左子 $2i+1$ 右子 $2i+2$



### 	3.2 深度优先周游

> 前序周游 (t L R次序)
>
> 中序周游 (L t R次序)
>
> 后序周游 (L R t次序)



#### 				3.2.1 递归

```c++
//前序周游
template<class T>
void BinaryTree<T>::PreOrder( BinaryTreeNode<T>* root){
    if (root != NULL){
        visit(root->value);
        PreOrder(root->left);
        PreOrder(root->right);
    }
}
//中序周游
template<class T>
void BinaryTree<T>::InOrder(BinaryTreeNode<T>* root){
    if (root != NULL){        
        InOrder(root->left);
        visit(root->value);
        InOrder(root->right);
    }
}
//后序周游
template<class T>
void BinaryTree<T>::PostOrder(BinaryTreeNode<T>* root){
    if (root != NULL){        
        PostOrder(root->left);
        PostOrder(root->right);
        visit(root->value);
    }
}
```



#### 					3.2.2 非递归

```c++
//前序周游
template <class T>
void BinaryTree<T>::PreOrder(BinaryTreeNode<T>* root){
    using std::stack;
    //用stack保留待访问的右子结点
    stack<BinaryTreeNode<T>*> aStack;
    BinaryTreeNode<T> *pointer = root;
    //栈底监视哨 栈空时也要visit
    aStack.push(NULL);
    while (pointer != NULL) {
        visit(pointer->value());
        //stack-> 右 左
        if (pointer->right() != NULL)     
            aStack.push(pointer->right());
        if (pointer->left() != NULL)      
            aStack.push(pointer->left());
        //pointer指向下一个根
        pointer = aStack.top();            
        aStack.pop();
    }
}
//中序周游
template <class T>
void BinaryTree<T>::InOrder(BinaryTreeNode<T>* root){
    using std::stack;
    stack<BinaryTreeNode<T>*> aStack;
    BinaryTreeNode<T> *pointer = root;
    while ((pointer != NULL) || (!aStack.empty()){
        if (pointer != NULL){
            aStack.push(pointer);
            pointer = pointer->left;
        }
        else {
            pointer = aStack.top();
            aStack.pop();
            visit(pointer->value);
            pointer = pointer->right;
        }
    }
}
//后序周游
	//栈元素定义           
enum Tags{left, right};
template <class T>
class StackElement {
public:
   BinaryTreeNode<T> *pointer;
   Tags tag;
};
	//周游算法
template <class T>
void BinaryTree<T>::PostOrder(BinaryTreeNode<T>* root){
    using std::stack;
    StackElement<T> element;
    stack<StackElement<T>> aStack;
    BinaryTreeNode<T> *pointer = root;
    while ((pointer != NULL) || (!aStack.empty()){
        while (pointer != NULL){
            element.tag ==left;
            elemrnt.pointer = pointer;
            aStack.push(element);
            pointer = pointer->left;
        }
        //取顶弹栈
        element = aStack.top();
        aStack.pop();
        pointer = element.pointer;
        //从左子树返回，进入右子树
        if (element.tag == left){
            element.tag = right;
            aStack.push(element);
            pointer = pointer->right;
        }
        //从右子树返回
        else {
            visit(element.pointer->value);
            //继续弹栈
            pointer = NULL;
        }
    }
}
           
```



### 3.3 广度优先周游

```c++
//广度优先周游
template <class T>
void BinaryTree<T>::LevelOrder(BinaryTreeNode<T> *root){
    using std::queue;
    queue<BinaryTreeNode<T> *> aQueue;
    BinaryTreeNode<T> *pointer = root;
    aQueue.push(pointer);
    while (!aQueue.empty()){
        pointer = aQueue.front();
        aQueue.pop();
        visit(pointer->value);
        if (pointer->left != NULL)
            aQueue.push(pointer->left);
        if (pointer->right != NULL)
            aQueue.push(pointer->right);
    }
}
```



### 3.4 存储结构



#### 3.4.1 链式结构

```c++
//查找双亲结点（中序周游）
template <class T>
void BinaryTree<T>::Parent(BinaryTreeNode<T>* current){
    using std::stack;
    stack<BinaryTreeNode<T>*> aStack;
    BinaryTreeNode<T> *pointer = root;
    if (current != NULL && root != NULL)
    while ((pointer != NULL) || (!aStack.empty()){
        if (pointer != NULL){
            if (current == pointer->left() || current == pointer->right())
                 return pointer;
            aStack.push(pointer);
            pointer = pointer->left;
        }
        else {
            pointer = aStack.top();
            aStack.pop();
            visit(pointer->value);
            pointer = pointer->right;
        }
    }
}           
```



#### 3.4.2 顺序结构

> + 完全二叉树大多采用顺序存储
> + 结点$i$ 的左子节点为$2i+1$ 右子结点为$2i+2$ 父结点为$\lfloor(i-1)/2\rfloor$ 



### 3.4 二叉搜索树（BST）

```c++
//插入
template <class T>
void BinarySearchTree<T>::InsertNode(BinaryTreeNode<T> *root, BinaryTreeNode<T> *newpointer) {
    BinaryTreeNode<T> *pointer = NULL;
    if (root == NULL) {
        Initialize(newpointer);
        return;
    } 
    else
        pointer = root;
    
    while (pointer != NULL) {
        if (newpointer->value == pointer->value)
            return;
        else if (newpointer->value < pointer->value) {
            if (pointer->left == NULL) {
                pointer->left = newpointer;
                return;
            } 
            else
                pointer = pointer->left;
        } 
        else {
            if (pointer->right == NULL) {
                pointer->right = newpointer;
                return;
            } 
            else
                pointer = pointer->right;
        }
    }
}
```



### 3.5 堆（heap）

```c++
//初始化堆
template<T>
MinHeap<T>::MinHeap(const int n){
    if (n <= 0)
        return;
    currSize = 0;	//当前堆中元素数目
    maxSize = n;	//堆容量
    heapArray = new T [maxSize];
    /*赋值*/
    BuildHeap();
}
//叶子结点判断
template<class T>
bool MinHeap<T>::isLeaf(int pos){
    return (pos >= currSize/2) && (pos < currSize);
}
//左子结点
template<class T>
int MinHeap<T>::leftchild(int pos){
    return pos * 2 + 1;
}
//右子结点
template<class T>
int MinHeap<T>::rightchild(int pos){
    return pos * 2 + 2;
}
//双亲结点
template<class T>
int MinHeap<T>::parent(int pos){
    return (pos - 1) / 2;
}
//向下筛选（筛选法）
template<class T>
void MinHeap<T>::SiftDown(int pos){
    int i = pos;
    int j = leftchild(i);
    T tmp = heapArray[i];
    while (j < currSize){
        //右子结点存在且比左子更小
        if ((j < currSize - 1)) && (heapArray[j + 1] < heapArray[j]))
            j++;
        else if (heapArray[j] < tmp){
            heapArray[i] = heapArray[j];
            i = j;
            j = leftchild(i);
        }
        else break;
    }
    heapArray[i] = tmp;
}
//自底向上调整
template<class T>
void MinHeap<T>::BuildHeap(){
    //最后一个内部结点
    for (int i = currSize / 2 - 1; i >= 0; i--){
        SiftDown(i);
    }
}
//插入新元素
template <class T>
bool MinHeap<T>::Insert(const T &newNode){
    if (currSize == maxSize)
        return false;
    else{
        heapArray[currSize] = newnode;
        SiftUp(currSize);
        currSize++;
        return true;
    }
}
//向上筛选
template<class T>
void MinHeap<T>::SiftUp(int pos){
    int x = pos;
    T tmp = heapArray[x];
    while ((x > 0) && (heapArray[parent(x)] > tmp)){
        heapArray[x] = heapArray[parent(x)];
        x = parent(x);
    }
    heapArray[x] = tmp;
}
//移出最小值
T & MinHeap<T>::RemoveMin(){
    if (currSize == 0){
        cout << "EMPTY";
        return false;
    }
    else{
        //维护curr、last->[currSize - 1]
        swap(0,--currSize);
        if (currSize >= 2){
            SiftDown(0);
        }
        return heapArray[currSize];
    }
}
//删除元素
template<class T>
bool MinHeap<T>::Remove(int pos, T &node){
    if ((pos < 0)) || (pos >= currSize)){
        return false;
    }
    else {
        node = heapArray[pos];
        heapArray[pos] = heapArray[--currSize];
        if (heapArray[parrent(pos)] > heapArray[pos]){
            SiftUp(pos);
        }
        else SiftDown(pos);
        return true;
    }
}
```



## 4. 树



### 	4.1 树与森林

> + 树的度：树中各结点度的最大值
>
> + 二叉树：度为2并且严格区分左右两个子结点的有序树



### 4.2 森林与二叉树的转换

> + 左子右兄



### 	4.3 树的周游



#### 4.3.1 深度优先周游

```c++
//先根次序->先序遍历
template <class T>
void Tree<T>::RootFirstTraverse(TreeNode<T>* root){
    while (root != NULL){
        visit(root->value); 
		RootFirstTraverse(root->LeftMostChild( )); 		
        root = root->RightSibling( );
    }
}
//后根次序->中序遍历
template <class T>
void Tree<T>::RootLastTraverse ( TreeNode<T>* root){
	while (root !=NULL) {
		RootLastTraverse (root->LeftMostChild( ));        
        Visit (root->value( ));
		root=root->RightSibling( );
	}
}
```



#### 4.3.2 广度优先周游

```c++
//森林的广度周游
template <class T>
void Tree<T>::WidthTraverse2(TreeNode<T>* root){
    using std::queue;
    queue<TreeNode<T>*> aQueue;
	TreeNode<T>* pointer=root;
    //将森林中的根节点存入队列
    while (pointer != NULL){
        aQueue.push(pointer); 
        pointer = pointer->RightSibling();
    }
    //周游剩余结点
	while (!aQueue.empty()) {
        //访问根结点
    	pointer = aQueue.front();
    	aQueue.pop();
    	Visit(pointer->Value());
        //指向第一个孩子
    	pointer = pointer->LeftMostChild();
        //所有孩子结点入队
    	while (pointer != NULL) {
        	aQueue.push(pointer);
        	pointer = pointer->RightSibling();
    	}
	}
}
```



### 4.4 树的链式结构

> + “子结点表”表示法
>
> + 静态左子/右兄表示法 -> 易于添加子树
>
> + 动态结点表示法 -> 指针链表（长度与度数有关）
>
> + 动态“左子右兄”二叉链表表示法
>
>   ```c++
>   TreeNode<T> *pchild;
>   TreeNode<T> *pSibling;
>   //判断叶子结点
>   template<class T> 
>   bool TreeNode<T>::isLeaf(TreeNode<T>* node){
>   	if  (node->pChild == NULL)
>   		return true;
>   	return false;
>   }
>   //返回前一个兄弟结点
>   template <class T>
>   TreeNode<T>* Tree<T>::PrevSibling(TreeNode<T>* current){
>       using std::queue;
>       queue<TreeNode<T> *> aQueue;
>       TreeNode<T> *pointer = root;
>       TreeNode<T> *prev = NULL;
>       //当前结点为空、空树、当前为根结点
>       if ((current == NULL) || (pointer == NULL) || (current == pointer))
>   		return NULL;
>       aQueue.push(pointer);
>       //向左子后遍历所有兄弟再左子
>       while(!aQueue.empty()){
>           prev = NULL;
>           pointer = aQueue.front();
>           aQueue.pop();
>           pointer = pointer->pChild;
>           while (pointer != NULL){
>           	if (pointer == current)
>               	return prev;
>           	aQueue.push(pointer);
>           	prev = pointer;
>           	pointer = pointer->pSibling;
>           }
>       }
>   	return NULL;
>   }
>   ```



### 4.5 并查集

> **结点中仅需保存父指针信息**，树本身可以存储为一个**以其结点为元素的数组** 

```C++
private:
	T value;			       //结点的值
	ParTreeNode<T>* parent;    //父结点指针
	int	nCount;                //以此结点为根的子树结点个数

```



### 4.6 树的顺序结构

> + 带右链的先根次序表示法
>
>   ltag 、info 、rlink



### 4.7 哈夫曼树

> 带权路径长度最小的二叉树  



## 5. 图



### 5.1 概念

> + 假设图中有 n 个顶点，e 条边，则含有 e=n(n-1)/2 条边的无向图称作完全图
> + 含有 e=n(n-1) 条弧的有向图称作有向完全图
> + 在无向图中，如果顶点v 和顶点w 之间存在一条边，则称顶点v 和w 互为邻接点，边(v,w) 和顶点v 和w 相关联。
> + 弧尾 -> 弧头
> + 从顶点u 到顶点w 之间存在一条路径，路径上边的数目称作路径长度
> + 简单路径:序列中顶点不重复出现的路径
> + 简单回路:序列中第一个顶点和最后一个顶点相同的简单路径（圈）
> + 若图G中任意两个顶点之间都有路径相通，则称此图为连通图
> + 若无向图为非连通图，则图中各个极大连通子图称作此图的连通分量
> + 对于有向图， 若任意两个顶点之间都存在一条有向路径，则称此有向图为强连通图



### 5.2 邻接矩阵

> + 表示顶点间相邻关系的矩阵
> + 无向图的相邻矩阵为对称矩阵
> + 适用于稠密图的表示

```c++
//构造函数
Graphm::Graphm(int numVert){
    int i, j;
    int **matrix = new int*[numVert];
    for (i = 0; i < numVert; i++){
        matrix[i] = new int[numVert];
    }
    for (i = 0; i < numVert; i++){
        for (j = 0; j < numVert; j++){
            matrix[i][j] = 0;
        }
    }
}
//析构函数
Graphm::~Graphm()
{
    for (int i = 0; i < numVertex; i++)
      delete[] matrix[i];
    delete[] matrix;
}
//第一条边
Edge Graphm::FirstEdge(int oneVertex){
    Edge e;
    e.from = oneVertex;
    for (int i = 0; i < numVertex; i++){
        if (matrix[oneVertex][i] != 0){
            e.to = i;
            e.weight = matrix[oneVertex][i];
            break;
        }
    }
    return e;
}
//下一条边
Edge Graphm::NextEdge(Edge preEdge){
    Edge e;
    e.from = preEdge.from;
    if (preEdege.to < numVertex){
        for (int i = preEdge.to + 1; i < numVertex; i++){
            if (matrix[preEdge][i] != 0){
             	e.to = i;
                e.weight = matrix[preEdge][i];
                break;
            }                
        }
    }
    return e;
}
```



### 5.3 邻接表（出边表）

> + 假设有n个顶点m条边，需用(n+2m)个存储单元
> + 假设有n个顶点m条弧，需用(n+m)个存储单元
> + 在有向图的邻接表中不易找到指向该顶点的弧，即难以确定某个顶点的入度

```c++
//链表类
template<class T>
class LList{
    Link<T> *head;
    LList(){
        head = new Link<T>();
    }
}
//结构体
struct listUnit{
    int vretex;
    int weight;
}
//第一条边
Edge Graphl::FirstEdge(int oneVertex) {
     Edge myEdge;
     myEdge.from = oneVertex;
     Link<listUnit> *temp=graList[oneVertex].head;
     if (temp->next != NULL) {
         myEdge.to=temp->next->element.vertex;
         myEdge.weight=temp->next->element.weight;
     }
     return myEdge;
}

//下一条边
Edge Graphl::NextEdge(Edge preEdge) {
    Edge myEdge;
    myEdge.from = preEdge.from;
    Link<listUnit> *temp=graList[preEdge.from].head;
    while (temp->next != NULL) && temp->next->element.vertex<=preEdge.to)
         temp=temp->next;
    if (temp->next != NULL ) {
         myEdge.to=temp->next->element.vertex;
         myEdge.weight=temp->next->elemnt.weight;
    }
    return myEdge;
}
//添加边
void Graphl::setEdge(int from,int to, int weight){
    Link<listUnit> *temp = graList[from].head;
    while (temp->next != NULL && temp->next->element.vertex < to) 
         temp = temp->next;
    if (temp->next == NULL) {
 		/* 边不存在，最后一条边
        创建结点，设置element.vertex为to
        element.weight为weight
        更新numEdge和to的入度 */
        temp->next = new Link<ListUnit>;
        temp->next->element.vertex = to;
        temp->next->element.weight = weight;
        numEdge++;
        Indegree[to]++;
        return;
    }
    if (temp->next->element.vertex == to) {
 		// 边存在，更新weight
        temp->next->weight =weight;
        return;
    }
    if (temp->next->element.vertex > to) {
 		// 边不存在，但后边还有边
        // 指 >to 为 *other
        // 创建之后再 new->next = other
        Link<listUnit> *other = temp->next;
        temp->next = new Link<listUnit>;
        temp->next->element.vertex = to;
        temp->next->element.weight = weight;
        temp->next->next = other;
        numEdge++;
        Indegree[to]++;
        return;
    }
}
//删除边
void Graphl::delEdge(int from, int to){
    Link<listUnit> *temp = graList[from].head;
    while ((temp->next != NULL) 
           && (temp->next->element.vertex < to)){
        temp = temp->next;
    }
    // 边已经被删除
    if ((temp->next = NULL)) 
        || (temp->next->element.vertex > to))
        return;
    if (tem->next->element.vertex == to){
        Link<lisUnit> *other = temp->next->next;
        delete temp->next;
        temp->next = other;
        numEdge--;
        Indegree[to]--;
    }
}
```



### 5.4 十字链表

> + 每个表目代表一个弧
> + 表目：五个域@头顶点序号、尾顶点序号、弧头指针、弧尾指针、弧权值
> + 表头：三个域@data域、出度指针、入度指针

### 5.5 图的周游

> + 给出一个图G和其中任意一个顶点V0，从V0出发系统地访问G中所有的顶点，每个顶点访问一次且仅一次，叫做图的周游
> + 从一顶点出发，有可能不能到达所有的顶点，例如，非连通图
> + 有可能会陷入死循环，例如，存在回路

```c++
void graph_traverse(Graph& G) {
       //对图所有顶点的标志位进行初始化	
      for ( int i = 0; i < G.VerticesNum(); i++) 
		  G.Mark[i] = UNVISITED;

      //检查图的所有顶点是否被标记过，如果未被标记，
      //则从该未被标记的顶点开始继续周游
      //do_traverse函数用深度优先或者广度优先
      for ( int i = 0; i < G.VerticesNum() ;i++) 	
             if (G.Mark[i] == UNVISITED)
                    do_traverse(G, i);
} 
```



#### 5.5.1 深度优先周游（DFS）

> + 从图中某个顶点V0 出发，访问此顶点，然后依次从V0的各个未被访问的邻接点出发深度优先搜索遍历图中的其余顶点，直至图中所有与V0有路径相通的顶点都被访问到为止
> + 邻接表$O(|V|+|E|)$ 
> + 邻接矩阵$O(|V^2|)$ 

```c++
//以v为始点开始深搜
void DFS(Graph &G, int v){
    visit(G, v);
    G.Mark[v] = VISITED;
    for (Edge e = G.FirstEdge(v); G.IsEdge(e); e = G.NextEdge(e)){
        if (G.Mark[G.toVertex(e)] == UNVISITED)
            DFS(G, G.ToVertex(e));
    }
}
//邻接矩阵DFS
void DFS(Graphm &G, int v) {
    G.Mark[v] = 1;
    Visit(G, v);
    for (int i = 0; i < N; i++)
        if (!G.Mark[i] && G.matrix[v][i] == 1)
            DFS(G, i);
}
```



#### 5.5.2 广度优先搜索（BFS）

> 从图中的某个顶点V0出发，并在访问此顶点之后依次访问V0的所有未被访问过的邻接点，随后按这些顶点被访问的先后次序依次访问它们的邻接点，直至图中所有与V0有路径相通的顶点都被访问到为止

```c++
void BFS(Graph &G, int v){
    using std::queue;
    queue<int> Q;
    visit(G, V);
    G.Mark[v] = VISITED;
    Q.push(v);
    while (!Q.empty()){
        // 以 v 为始点(from)，搜索所有邻接点(to)
        int v = Q.front;
        Q.pop();
        for (Edge e = G.FirstEdge(v); G.IsEdge(e); e = G.NextEdge(e)){
            // 遇到没访问的邻接点，访问之
            if (G.Mark[G.ToVertex(e)] == UNVISITED){
                visit(G, G.ToVertex(e));
                G.Mark[G.ToVertex(e)] = VISITED;     
                // 入队，队头为下一轮的始点(from)
                Q.push(G.ToVertex(e));
            }            
        }
    }
}
```



### 5.6 拓扑排序

> 若在**有向无环图**G中从顶点$V_i$ 到$V_j$ 有一条路径，则在序列中顶点$V_i$ 必在顶点$V_j$ 之前
>
> 时间复杂度 $O(n+e)$ 
>
> 1. 选择一个入度为0的顶点且输出之
> 2. 从图中删掉此顶点及所有的出边
> 3. 回到第 1 步继续执行。直至图空或者图不 空但找不到无前驱（入度为0 ）的顶点为止

```c++
void Topology(Graph &G){
    for (int i = 0; i < numVert; i++)
        G.Mark[i] = UNVISITED;
    using std::queue;
    queue<int> Q;
    // 入度为 0 的结点入队
    for (i = 0; i < numVert; i++){
        if (G.Indegree[i] == 0)
            Q.push(i);
    }
    while (!Q.empty()){
        int v = Q.front();
        Q,pop();
        visit(G, v);
        G.Mark[v] = VISITED;
        // 删掉 v 的所有出边
        for (Edge e = G.FirstEdge(e); G.IsEdge(e); G.NextEdge(e)){
            G.Indegree[e.to]--;
            if (G.Indegree[e.to] == 0)
                Q.push(e);
        }
    }
    // 图有环
    for (i = 0; i < numVert; i++){
        if (G.Mark[i] == UNVISITED){
            cout << "circle";
            return;
        }
    }
}
```



### 5.7 最短路径



#### 5.7.1 单源最短（Dijkstra）

> 设置辅助数组D，其中每个分量D[k] 表示当前所求得的从源点到顶点 k 的最短路径

```c++
//Dijkstra
class Dist{
    int length;	// 与源的距离
    int pre;	// 先前顶点
    int index;	// 顶点的索引值
}
void Dijkstra(Graph &G, int s, Dist *&D){
    D = new Dist[G.numVert];
    // 初始化Dist
    for (int i = 0; i < G.numVert; i++){
        G.Mark[i] = UNVISITED;
        D[i].length = INFINITY;
        D[i].pre = s;
    }
    D[s].length = 0;
    for (i = 0; i < G.numVert; i++){
        // 每次取剩余结点中 length 最小的开启下一循环
        int v = minVert(G, D);
        // 最小为无穷即无解
        if (D[v].length == INFINITY)
            break;
        visit(G, v);
        G.Mark[v] = VISITED;
        // 以 v 为 from 遍历 各个邻接点
        for (Edge e = G.FirstEdge(v); G.IsEdge(e); G.NextEdge(e)){
            // 判断是否需要更新 D[to].length
            if (D[G.toVertex(e)].length > (D[v].length + e.weight)){
                D[G.ToVertex(e)].length = D[v].length + e.weight;
                // 成立则更新 pre
                D[G.ToVertex(e)].pre = v;
            }
        }
    }
}
```



#### 5.7.2 两点最短（Floyd）

> + $adj^{(k)}[i, j]$  的值等于从顶点$v_i$ 到$v_j$ 路径上所经过的顶点序号不大于$k$ 的最短路径长度
> + $path[i, j]$ 的值等于$adj^{(k)}[i, j]$ 达到最小值时$k$ 的值，如若没有最短路径就设为 -1 （不可能值）
> + 时间复杂度$O(n^3)$ 

```c++
//Floyd
void Floyd(Graph &G, Dist **&D){
    // 声明计数器
    int i, j, v;
    // 声明[n][n]的二维数组
    D  = new Dist*[G.numVert];
    for (int i = 0; i < G.numVert; i++){
        D[i] = new Dist[G.numVert];
    }
    // 初始化adj(D[][].length)和path(D[][].pre)
    for (i = 0; i < G.numVert; i++){
        for (j = 0; j < G.numVert; j++){
            if (i == j){
                D[i][j].length = 0;
                D[i][j].pre = i;
            }
            else{
                D[i][j].length = INFINITY;
                D[i][j].pre = -1;
            }
        }
    }
    // adj^(0) path^(0)
    for (v = 0; v < G.numVert; v++){
        for (Edge e = G.FirstEdge(e); G.IsEdge(e); G.NextEdge(e)){
            D[v][G.toVertex(e)] = G.weight(e);
            D[v][G.toVertex(e)].pre = v;
        }
    }
    // 启用的 v 是否起到捷径作用
    for (v = 0; v < G.numVert; v++)
    	for (i = 0; i < G.numVert; i++)
    		for(j = 0; j < G.numVert; j++){
            	if (D[i][j].length > (D[i][v].length + D[v][j].length)){
                    D[i][j].length = D[i][v].length + D[v][j].length;
                    // D[i][j].pre = v;
                    D[i][j].pre = D[v][j].pre;
                }  
            }    
}
```



### 5.8 最小生成树（MST）

> 对于带权的连通无向图G，其最小生成树是一个包括G的所有顶点和部分边的图，这部分的边满足下列条件：
>
> 1. 这部分的边能够保证图是连通的
>
> 2. 这部分的边，其权的总和最小

> + 生成树的顶点个数与图的顶点个数相同
> + 生成树是图的极小连通子图
> + 一个有n个顶点的连通图的生成树有n-1条边
> + 生成树中任意两个顶点间的路径是唯一的



#### 5.8.1 稠密图MST（Prim）

```c++
void Prim(Graph &G, int s, Edge *&MST){
    int MSTtag=0;  	//最小生成树边的计数器
      MST=new Edge[G.VerticesNum()-1];  //MST保存最小生成树的边
     Dist* D= new Dist[G.VerticesNum()];     
     for (int i = 0; i<G.VerticesNum(); i++)  {
           G.Mark[i]=UNVISITED; 
           D[i].index=i;
           D[i].length=INFINITY;
           D[i].pre=s;
     }
     int v=s;
     G.Mark[v] =VISITED;          //标记开始顶点
     D[s].length=0;
     for (i=0;i<G.VerticesNum()-1;i++) {
           if (D[v].length==INFINITY) return;   //非连通
          for (Edge e= G. FirstEdge(v);G.IsEdge(e);e=G. NextEdge(e)) 
             if (G. Mark[G. ToVertex(e)] == UNVISITED&&(D[ToVertex(e)].length>e.weight)) {
                D[G.ToVertex(e)].length=e.weight;
                D[G.ToVertex(e)].pre=v;
             }
	      v=minVertex(G,D);
            G.Mark[v]=VISITED;
             Edge edge(D[v].pre, D[v].index, D[v].length);
            AddEdgetoMST(edge, MST, MSTtag++);
        } 
}
int minVertex(Graph& G,Dist* &D) {
    int i,v;
    for (i = 0; i < VerticesNum();  i++) {
         if (G.Mark[i] == UNVISITED) {
              v = i;                     // v为随意的一个未访问的顶点
              break;
         }
    }
    for (i = 0; i < VerticesNum();  i++)
         if ( (G.Mark[i] == UNVISITED)&&(D[i].length<D[v].length)) 
                            v = i;       //  具有最小距离的顶点
      return v;
}
```



#### 5.8.2 稀疏图MST（Kruskal）

> 先构造一个只含 n 个顶点的子图 SG，然后从权值最小的边开始，若它的添加不使SG 中产生回路，则在 SG 上加上这条边，如此重复，直至加上 n-1 条边为止

```c++
void Kruskal(Graph& G, Edge* &MST ) {
      Partree A(G.VerticesNum());  //等价类
      MinHeap<Edge> H(G.EdgesNum());  
	  MST=new Edge[G.VerticesNum()-1];       
	  int MSTtag=0;                 //最小生成树边的标号
      for(int i = 0; i<G.VerticesNum(); i++) {
	    for(Edge e = G. FirstEdge(i); G.IsEdge(e); e = G. NextEdge(e)) {
	          if (G.FromVertex(e)< G.ToVertex(e))  
                  H.Insert(e);  
	    }
      } 
	  int EquNum=G.VerticesNum();    //开始时有|V|个等价类
      while(EquNum>1)  {                      //合并等价类
	      Edge e=H.RemoveMin();      //获得下一条权最小的边
	      if (e.weight==INFINITY) {
				Print(“不存在最小生成树.“);
                delete [] MST; 
                MST=NULL; return;
	      } 
          int from = G.FromVertex(e);             //记录该条边的信息
          int to= G.ToVertex(e);
          if (A.differ(from,to)){   //如果边e的两个顶点不在一个等价类
		   		A.UNION(from,to); 
 	            AddEdgetoMST(e,MST,MSTtag++); 
	            EquNum--;             
          }
      } 
}
```



## 6. 排序

> + 记录(Record)：就是结点，排序的基本单位
> + 排序码：需求（某一科目的成绩）
> + 关键码：唯一的特征值（学号）
> + 正序：正好符合排序要求（逆序相反）
> + 影响相对运行时间：记录的数量、记录的大小、记录的原始有序程度、排序码
> + “稳定的” 排序算法 ：如果存在多个具有相同排序码的记录，经过排序后这些记录的相对次序仍然保持不变



### 6.1 插入排序

> 不断地将无序子序列中的记录“插入”到有序序列中，从而逐渐地增加有序子序列的长度，直到所有记录都进入有序序列中为止



#### 6.1.1 直接插入排序

> + 逐个处理待排序的记录，每个新记录都要与前面已排好序的记录进行比较，然后插入到适当的位置
> + 时间$O(n^2)$ 空间$O(1)$
> + 适用原始数据**基本有序**或**数据量不大**的情况
> + 属于**稳定**性排序
> + 在最好情况（序列本身已是有序的）下时间代价为$O(n)$
> + 对于短序列，直接插入排序比较有效

```c++
template<class Record>
void InsertSort(Record Array[], int n){
	Record tmp;
    int i, j;
    // n - 1 趟
    for(i = 1; i < n; i++){
        tmp = Array[i];
        // j 为 i 的前一项
        j = i - 1;
        // 依次向前比较，直到遇到比tmp小的
        while(j >= 0 && tmp > Array[j]){
            // 后移
            Array[j + 1] = Array[j];
            j--;
        }
        Array[j + 1] = tmp;
    }
}
```



#### 6.1.2 希尔排序（Shell）

> + 先将序列转化为若干小序列，小序列内进行插入排序
> + 逐渐扩大小序列的规模，减少小序列个数
> + 将 n 个记录分成 d 个子序列，其中，d 称为增量，它的值在排序过程中从大到小逐渐缩小，直至最后一趟排序减为 1
> + 时间$O(n^2)$ 空间$O(1)$
> + 属于**不稳定**性排序
> + 选择适当的增量序列，可以使得时间代价接近Θ(n)



### 6.2 交换排序

> 不断地考查待排序列中关键字之间的有序性，一旦发现非有序关系，将其“交换”，直到所有记录均按照有序性就位为止



#### 6.2.1 冒泡排序

> + 不断地比较相邻的记录，如果不满足排序要求，就交换相邻记录，直到所有的记录都已经排好序
> + 时间$O(n^2)$ 空间$O(1)$
> + 属于**稳定**性排序

```c++
template <class Record>
void BubbleSort(Record Array[], int n){
   // n - 1
	for (int i=1; i<n; i++)	
        // n - i - 1
		for (int j=n-1; j>=i; j--)
		      if (Array[j] < Array[j-1]))	
                      swap(Array, j, j-1);
}
//改进：检查每次冒泡过程中是否发生过交换，如果没有，则表明整个数组已经排好序了，排序结束。避免不必要的比较
template <class Record>
void ImprovedBubbleSort(Record Array[], int n){
    // 是否发生交换的标志
	bool NoSwap;	        
	for (int i=1; i<n; i++)  {
        // 标志初始为真
		NoSwap = true;	
		for (int j=n-1; j>=i; j--)
			if (Array[j] < Array[j-1]) {	
             	swap(Array, j, j-1);	
		     	NoSwap = false;
			}
        // 有一轮没有进行交换
		if (NoSwap) 
            return;
	} 
}
```



#### 6.2.2 快速排序

> + 首先对无序的记录序列进行“一次划分”，之后分别对划分后的两个子序列“递归”进行快速排序
> + 找一个记录，以它的关键字作为“枢轴”，凡其关键字小于枢轴的记录移至该记录之前，反之，移至该记录之后
> + 尽可能使L，R长度相等
> + 时间$O(nlogn)$ 空间$O(logn)$
> + 属于**不稳定**性排序
> + 若待排记录的初始状态为按关键字**有序**时，快速排序将**退化为冒泡**排序

```c++
template <class Record>
void QuickSort(Record Array[], int left,int right) {	
     //Array[]为待排序数组，i,j 分别为数组两端
	// 如果子序列中只有0或1个记录，就不需排序
	if (right <= left)  
        return;
	//选择轴值
	int pivot = SelectPivot(left, right); 	
	//分割前先将轴值放到数组末端
	swap(Array, pivot, right);   
	// 对剩余的记录进行分割
	pivot = Partition(Array, left, right); 	   
	QuickSort(Array, left, pivot-1);
    QuickSort(Array, pivot +1, right); 
}
//分隔函数
template <class Record>
int Partition(Record Array[], int left, int right) {
    //分割函数，分割后轴值已到达正确位置
	Record TempRecord;	       //存放轴值的临时变量
	int i = left， j = right;       // i为左指针，j为右指针
	TempRecord = Array[j];
	while (i != j)  {
	    //左指针右移,越过小于等于轴值的记录
        while ((Array[i] <= TempRecord) && (i < j))  
            i++;
	    if (i < j) {
	      	Array[j] = Array[i];  
            j--;   //将逆置元素换到右边的空位, 右指针向左移动
	    }
	  //右指针左移,越过大于等于轴值的记录 
        while (Array[j] >= TempRecord) && (i < j))  
            j--;
	    if (i < j)  {
	        Array[i] = Array[j];   
            i++; //将逆置元素换到左边的空位, 左指针向右移动
	    }
	}
	Array[i] = TempRecord;  //轴值回填
	return i;    //返回分界位置			
}
```



### 6.3 选择排序

> 不断地从无序子序列中“选择”关键字最小(或最大)者，并将其加入到有序子序列中，以此方法增加有序子序列的长度，直到所有的记录都进入有序序列中为止



#### 6.3.1 简单选择排序

> + 找出未排序记录中的最小记录，然后直接与数组中第i个记录交换（比冒泡排序减少了移动次数）
> + 时间$O(n^2)$ 空间$O(1)$
> + 属于**不稳定**性排序 

```c++
template <class Record>
void SelectSort(Record Array[], int n) {	
	for (int i=0; i<n-1; i++)	{
		// 首先假设记录i就是最小的
		int Smallest = i;
		// 开始向后扫描所有剩余记录
		for (int j = i + 1; j < n; j++)
		// 如果发现更小的记录，记录它的位置
              if (Array[j] < Array[Smallest])
                      Smallest = j;	
		//将第i小的记录放在数组中第i个位置
        swap(Array, i, Smallest);
	}
}
```



#### 6.3.2 堆排序

> + 基于最大值堆来实现（从小到大）
> + 时间$O(nlogn)$ 空间$O(1)$
> + **节省空间**且**对原始数据的排序不敏感**

```c++
template <class Record>
void sort(Record Array[], int n) {	
	Record record;
	MaxHeap< Record >  max_heap = new 
                               MaxHeap< Record >(Array,n,n);	
	 // 依次找出剩余记录中的最大记录，即堆顶
	 for (int i=0; i<n-1; i++)
		max_heap.RemoveMax();	
}
T& MinHeap<T>::RemoveMin( )	
{
       if(CurrentSize==0) {
		cout<<"Can't Delete";
		exit(1);
	}
	else	{
		  swap(0,--CurrentSize);	//交换堆顶和堆尾的元素	  
                if(CurrentSize>1) 	
                      SiftDown(0);					  return heapArray[CurrentSize];
	}
}
```



### 6.4 归并排序

> 通过“归并”两个或两个以上的有序子序列，逐步增加有序序列的长度，直到所有的记录都进入一个有序序列中为止
>
> 1. 将原始序列划分为两个子序列
> 2. 分别对每个子序列递归排序
> 3. 最后将排好序的子序列合并为一个有序序列，即归并过程 
>
> + 时间$O(nlogn)$ 空间$O(n)$
> + **不依赖于原始数组的输入情况**

```c++
template <class Record>
void MergeSort(Record Array[], Record TempArray[],
                                                                        int left, int right) {	
	// 如果序列中只有0或1个记录，不必排序
	if (left < right) {
		 //从中间划分为两个子序列
		int middle=(left+right)/2;
		 //对左边一半进行递归
		MergeSort (Array, TempArray,left,middle);		
		 //对右边一半进行递归
		MergeSort (Array, TempArray,middle+1,right);
		 // 进行归并	
		Merge(Array, TempArray,left,right,middle);	
	}
}
//归并函数
template <class Record>
void Merge(Record Array[], Record TempArray[],int left,int right,int middle)
{	//归并过程，将数组暂存入临时数组
	for (int j=left; j <= right; j++)  
        TempArray[j] = Array[j];
	int index1=left，index2=middle+1;   //左右边子序列的起始位置
	int i = left;			//从左开始归并
	while ((index1 <= middle)&&(index2 <= right)) {
        if (TempArray[index1] < TempArray[index2])
			Array[i++] = TempArray[index1++];
	    else
	        Array[i++] = TempArray[index2++];
	}
	//处理剩余记录
	while (index1 <= middle)  
        Array[i++] = TempArray[index1++];
	while (index2 <= right)  
        Array[i++] = TempArray[index2++];
}
```



### 6.5 分配排序

> 不需要进行比较,但需要**事先知道**记录序列的**分布情况**
>
> **稳定**



#### 6.5.1 桶式排序

> + 事先知道序列中的记录都位于某个小区间段[0，m)内
> + 将具有相同值的记录都分配到同一个桶中，然后依次按照编号从桶中取出记录，组成一个有序序列
> + 时间$O(m+n)$空间$O(m+n)$

```c++
template <class Record>
void BucketSorter<Record>::Sort(Record Array[], int n,int max) { 
     int* TempArray=new Record[n];	//临时数组
     int* count=new int[max];//小于等于i的元素个数
     int i;
     for (i=0;i<n;i++)  TempArray[i]=Array[i];
     //所有计数器初始都为0
     for (i=0;i<max;i++)  	count[i]=0;
     //统计每个取值出现的次数
     for (i=0;i<n;i++)   count[Array[i]]++;
     //统计小于等于i的元素个数
     for (i=1;i<max;i++)   count[i]=count[i-1]+count [i];  
     //按顺序输出有序序列
     for (i=n-1;i>=0;i--)
         Array[--count[TempArray[i]]] = TempArray[i];
}
```



#### 6.5.2 基数排序

> + 高位优先、低位优先
> + 顺序、链式



### 6.6 排序算法比较

> + **平方级**：直接插入、冒泡、优化冒泡、选择
>   + 有序状态时，**直接插入**和**优化冒泡**线性
>   + **选择排序**不稳定（跨数据移动）
>   + 空间代价都是$O(1)$
> + **线性&平方之间**：希尔排序
>   + **不稳定**
>   + 空间代价$O(1)$
> + **对数级**：快排、归并、堆排
>   + 归并、堆排**与数组初始状态无关**
>   + 快排在**数组有序**时退化为冒泡
>   + 归并应用了**辅助数组**即空间$O(n)$
>   + 快排递归应用了**栈**即空间$O(logn)$
>   + 堆排和快排**不稳定**
>   + 归并**稳定**
> + **线性级**：桶式排序、基数排序



## 7. 检索



### 7.1 顺序检索

> + 插入直接插在表尾$O(1)$
>
> + 检索时间长$O(n)$
>
> + ASL分析
>
>   + 检索成功$\sum_{i =1}^n P_i*(n-i+1)=(n+1)/2$
>
>   + 检索失败$n+1$

```c++
template  <class T>  
class Item {
	public: 
	    Item(T value):key(value) { }
        Type getKey() {  return key; }//获取关键码值
        void setKey(T  k) { key=k; }  //设置关键码                                        
	private:
		T  key;                     	//关键码域
		...... ;                    	//其他域
}; 
vector<Item<T>*> dataList;
//“监视哨”
template<class T>
int SeqSearch(vector<Item<T>*>& dataList, int length, T k){
    int i=length; 
    //将第0个元素设为待检索值，保证while循环一定终止
    dataList[0]->setKey(k); 
    //从后往前逐个比较
    while(dataList[i]->getKey()!=k)     
        i--; 
    //返回元素位置
    return i;                        
}
```



### 7.2 二叉检索

> + 必须是有序表：
>   + 有序排列
>   + 顺序存储
> + 用给定值K与中间元素 dataList [i] 比较
> + 一般情况下，表长为 n 的二分查找的判定树的深度和含有 n 个结点的完全二叉树的深度相同
> + 不易更新
> + ASL = $\log_2^{n+1}-1$

```c++
template  <class Type> 
int BinSearch (vector<Item<Type>*>& dataList, int length, Type k){
     //low, high分别记录数组首尾位置
     int low=1, high=length, mid; 
     while (low<=high){
         //下取整
         mid=(low+high)/2;
         if (k < dataList[mid]->getKey()) 
               //右缩检索区间
               high = mid-1;              
         else if (k > dataList[mid]->getKey()) 
               //左缩检索区间
               low = mid+1;                
         else 
               //检索成功
               return mid;
      }
      //检索失败，返回0
	  return 0;   
}
```



### 7.3 分块检索

> + 线性表中共有n个数据元素，将表分成b块
> + 每块中的关键码不一定有序
> + 但前块中的最大关键码必须小于后块中的最小关键码
> + 索引表
>   + 放置各块中的**最大关键码**及**起始位置**
>   + 有时还需块中元素个数
> + ASL 
>   + 二分 + 顺序
>   + 顺序 + 顺序



### 7.4 散列表（Hash）

> + 不允许关键码值重复
> + 负载因子：$n/m$



#### 7.4.1 散列函数

> + 除余法
>   + 用关键码x除以M(取散列表长度)，并取余数作为散列地址
>   + 通常选择一个质数作为M值
> + 乘余取整法
>   + hash ( key ) =  n * ( A * key % 1 ) 
> + 数字分析法

> **聚集**
>
> 散列地址不同的结点，争夺同一后继散列地址，小的聚集可能汇合成大的聚集，导致很长的探查序列
>
> **二级聚集**
>
> 如果两个关键码散列到同一个基地址，得到相同的探查序列所产生的聚集

#### 7.4.2 开散列法（拉链法）

> 当冲突发生时就建立一条同义词链表，动态申请同义词的空间，适合于内存操作(**伪随机探查和二次探查可以消除**)

#### 7.4.3 闭散列法

> + 当插入K时，若基地址上的结点已被别的数据元素占用，则按上述地址序列依次探查，将找到的第一个开放的空闲位置di作为K的存储位置
> + 若所有后继散列地址都不空闲，说明该闭散列表已满，报告溢出

线性探查

> + 如果记录的基位置存储位置被占用，就在表中下移，直到找到一个空存储位置$P(K,i)=i$

改进的线性探查

> + 每次跳过常数c个而不是1个位置$P(K,i)=i*c$

二次探查

> + 探查序列依次为：
>       $1^2$，-$1^2$，$2^2$ ，-$2^2$，...，

墓碑

> + 删除产生
> + 不能插入
> + 重排清除墓碑



## 8. 索引

> + 主码：唯一标识
> + 辅码：可以重复



### 8.1 B树

> + 子树 $[  \lceil m/2 \rceil ,m ]$
> + 关键字 $[ \lceil m/2 \rceil - 1,m-1]$
> + 即 $j$ 个关键码就有 $j+1$ 个指针
> + 插入分裂：$\lceil m/2 \rceil$向上
> + 删除借调：~



### 8.2 B+树

> + 所有的关键码都出现在叶结点上
> + 各层复写下一层最大（最小）
> + 有$n$棵子树的结点含有$n$个关键字
> + 整体上是有序链表
> + 必须在叶节点才能查找停止
> + 两种查找方式



## 9. 高级数据结构



### 9.1 多维数组

> + 元素个数、关系不发生变化
> + 存储空间是一维的
> + 行序为主序、列序为主序
> + 三角矩阵：$i(i-1)/2+j-1$ 
> + 对称矩阵：$i(i+1)/2+j$ 
> + 稀疏矩阵：$t/m*n<=0.05$ 
>   + 三元组
>   + 稀疏矩阵的十字链表



### 9.2 广义表

> + 如果一个线性表中还包括一个或者多个子表，就称之为广义表
> + L 是广义表的名称、n为长度（原子、子表）
> + 可嵌套定义、可递归定义
> + 有相对次序
> + 广义表的**长度**定义为最外层包含元素个数
> + 深度为最大括号重数，嵌套相加，递归无穷大
> + 原子的深度为 0、空表深度为1
> + 表头：第一个数据元素（可能是子表）
> + 表尾：剩余数据元素构成的表（非空广义表表尾一定是子表）

```c++
//表头、表尾分析法
typedef enum{ATOM,LIST} TAG;
typedef struct GLNode{
    //表示该结点是ATOM或者LIST
       TAG type;   
       union{
          Elemtype data;
    //如果是LIST ,则指向它的元素的首结点
          GLNode *sublist; 
       };
    //指向下一个结点
       GLNode  *next;
};
```



### 9.3 平衡的BST

> + 平衡因子：
>   + 右子树高度减左子树高度
>   + 可能取值 0 1 -1
> + LL
> + RR
> + LR
> + RL