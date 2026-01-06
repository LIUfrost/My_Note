---
data: 2025-12-03T19:15:00
tags:
  - C
---
# 一.==无向图==
- 定义：一个由边链接的成对出现的点的集合
- 路径：是由边所链接的结点的序列
- 环：头和尾是同一个结点
- 连接：两个点之间存在路径
>question:
>is there a path between a and b
>shortest path?
>is there a cycle?
>is there a cycle that uses each edge exactly once?(Euler tour)
>is there a cycle that uses each vertex exactly once?(Hamilton tour)
>connectivity?
>MST:what is the best way to connect all of the vertices
>Biconnectivity:is there a vertex whose removal disconnects the graph?
>planarity:can you draw the graph in the plane with no crossing edges?
>Graph isomorphism:do two adjacency lists represent the same graph?
### graph API
- 用0～V-1来表示V个顶点
```C
Graph(int V);
Graph(In in);   //create a graph from input stream
void addEdge(int v,int w);
Iterable<Integer> adj(int v);  //遍历vertices adjacent to v
int V();//numbers of vertices
int E();//numbers of edges
String toString()//string representation
```
- 表示方法
  邻接矩阵结构（V\*V的数组）
  邻接链表（每一个点后都连接着所有与他相邻的点）
- 实现addedge 和 遍历相邻点功能
### 深度优先搜索(Depth-first search)
- Tremaux maze exploration(Tremaux迷宫探索算法)
>标记每一个走过的交叉路口
>如果一个走不通，回溯我们的脚步（栈）
```c
Paths(Graph G,int s);
boolean hasPathTo(int v);
Iterable<Integer> pathTo(int v);   //path from s to v:NULL if no path
```
- 深度优先搜索：查看和目标点连通的点的个数，并找到路径
>步骤：
>1.建立邻接数组链表；建立标记数组；建立edge数组
>2.标记起始点（在标记数组中，起始点位置存入ture）
>3.访问起始点的邻接链表，标记第一个（存入true，并在edge数组中存入上一个点）
>4.访问第一个的邻接链表，检查第一个点是否被标记（存入第一个不被标记的点）
>5.如遇到邻接链表已全部标记的点，回退
>6.直到所有起始点的邻接点的邻接...全部标记
>[注意]：利用递归！
```c
void dfs(Graph G,int v){
	marked[v] = true;
	id[v] = count;
	for(int w:G.adj(v)){
		if(!marked[w]){
			dfs(G,w);
		}
	}
}
```

### 广度优先搜索（Breadth_first search）
>步骤：
>1.建立一个队列，再建立一个辅助数组使得能倒退回去寻找路径
>2.对于原点，将其所有的相邻点添加进队列中，并标记原点；
>3.对每一个在队列中的点，当检查它时，将它删去并将其所有未标记的相邻点
>   添加进队列；
>4.直到队列为空为止；
### 互联组件（connected component）
- 想法:利用dfs来将某个点的所有链接点组成一个组件

### 图标挑战
- **bipartite**：能否将一个图分成两个子集，使得每一条边都分别连接两个子集中的点
- problem is:How difficult this question is?
- **cycle**: 图中是否包含循环
- **Euler Cycle**: 是否包含一种循环，使得每条边都恰好用一次
- **Hamilton Cycle**:是否包含一种循环，使得每个顶点都恰好用一次
- **Tarjan 算法**：使一个图变平坦
# 二.==有向图==
- Directed graphs: Set of bertuces connected partwise by directed edges
>question:
>path:is there a path from s to t?
>shortest path:shortest path?
>Topological sort:Can you draw a digraph so that all edges point upwards?
>strong connectivity:is there a directed path between all pairs of vertices?
- API
```c
Digraph(int V);  //create an empty digraph with V vertices
void addEdge(int v,int w);
Iterable<Integer> adj(int v);  //vertices pointing from v;
int V();
int E();
Digraph reverse();  //reverse of the Digraph
```
- 数据结构：类似于无向图
### 数形搜索
- **DFS完全一样，可以直接使用**
- **BFS也完全一样，可以直接使用**
>BFS也可以扩展到多个顶点，只需将一开始的多个顶点装进queue即可
### 拓扑排序（topological sort）
- 使用基础：DAG----Directed acyclic graph(无循环)
- 使用反向DFS postorder
>步骤：
>1.确定一个原点；
>2.对每个递归的点，若完成（其指向的全部已标记），将其放入堆栈中；
>3.若原点也已经全部标记，再另找一个点即可；
- **此时，对所有的edge v->w，拓扑顺序的返回值w总在v前**
>*如何确定是否是DAG?*
### 强连接组件（strongly-connected components）
- def:v & w是强连接的当且仅当v和w之间相互链接
- **Kasaraju-Sharir algorithm demo**
>步骤：
>1.对GRAPH1进行反向，并使用反向DFS postorder;
>2.对所有目标点从后往前存入一个堆栈中；
>3.对堆栈中的点作为原点，从前往后每个点进行DFS；
>4.若该点和其所有连接点周围的都标记了，将它们编号为相同的数。

# 三.==最小生成树（MST）==
### 1.定义
**生成树**
- 对于一个无向图，其连接所有顶点（是一个子图）
- 不含有环结构
**最小生成树
- 对每条边加上权重
- 树的所有边的权重和最小
- 边数为顶点数-1
>做假设：
>1.图中每条边的权重都不相同---否则会出现不止一条MST
>2.图是连接的----否则会出现最小跨度森林
### 2.贪婪算法
- cut:对图中的每个顶点，将其分为两部分（A&B）
- crossing edge：将A中的点连接B中的点的边
- **则，crossing edge中权重最小的边一定在MST中**
>步骤
>1.开始：所有边都是灰色的
>2.找到一组cut使得没有crossing edge是黑色的，将其最小权重的crossing edge涂成黑色
>3.重复直到涂黑了V-1条边
### 3.权重图程序接口（weight edge API）
```java
Edge(int v,int w,double weight)  //create a weighted edge v-w
int either()   //either endpoint
int compareTo(Edge that)  //compare this edge to that edge
```
```Java
EdgeWeightGraph(int v)   //creat an empty graph with V vertices
void addEdge(Edge e)   //add weighted edge e to this graph
Iterable<Edge> adj(int v) //return edges incident to v(include weight)
MST(Graph G)   //return edges
```
- 数据结构形式
- 建立顶点列表，每一个顶点都接一个链表，链表里是边类；
- 边类包含：两个顶点和权重；
### 4.克鲁斯卡尔算法（Kruskal's algorithm）
>步骤
>1.将所有边按照权重排序
>2.从小到大依次链接，若这边构成循环，则跳过
>3.直到连了V-1条为止
- 证明：对每一个第二步，总可以找到一个对应的cut
- 判断circle：检查edge的两个点是否联通，若连通，则加上边构成循环
>并查集（Union find）
### 5.普里姆算法（Prim's algorithm）
>步骤
>1.从零开始
>2.每次要加的边满足：和已知的图相连且权重最小
>3.直到连接了V-1条边为止
- ***lazy solution:***
	- maintain a PQ of **edges** with (at lest) one end point in T
	- key = edge;priorty = weight of edge;
	- Delete-min to determine next edge e = v-w to add to T;
	- Disregard if both end points v and w are in T;
	- otherwise,let w be the vertex not in T;
		- add to PQ any edge incident to w(assuming other endpoint not in T)
		- add w to T
>**时间复杂度**：ElogE
>	一共E个顶点，使用二叉树存储数据，每个顶点进入需要logE
- ***eager solution***
	- maintain a PQ of **vertices** connected by an edge to T where priority of vertex v = weight of shortest edge connecting v to T.(存一个表包含所有不在T中的顶点，每个不在T中的顶点后存储的是他和T中顶点相连的权重最小的边)
	- update PQ by considering all edges e = v-x incident to v
		- ignore if x is already in T
		- add x to PQ if not already on it
		- decrease priority of x if v-x becomes shortest edge connecting x to T
>**时间复杂度**：ElogV(更迅速)
### 6.MST背景
- ***Single-link Problem***
	- 定义团：两个团之间的距离指的是在这两个团中各取一个点，之间的最短距离
	- 给定整数k，要求给出k个团的划分，使得每两个相邻团之间的距离只和最大。
	- **Kruskal's algorithm**：进行MST划分直到只剩K个连接组件即可
- ***Euclidean MST"
	- 求MST，使得两点之间边的权重等于两点之间的欧式距离
	- **Delaunay 三角剖分法**，时间复杂度为n\*logn
# 四.==最短路径==
***Q：对于一个有向连连接图，任意给定两个端点，求之间的最短路径***
### 1.weighted directed edge API
```Java
DirectedEdge(int v,int w,double weight);
int from();//vertex v
int to();//verteex w
double weight()//weight of this edge
```
```Java
public class EdgeWeightedDigraph
EdgeWeightedDigraph(int V) //edge weighted digraph with V vertices
void addEdge(DirectedEdge e)//add weighted directed edge e
Iterable<DirectedEdge> adj(int v)//edges pointing from v
int v()//number of vertices
```
```Java
//Single-source shortest paths API(SP)
public class SP
SP(EdgeWeightedDigraph G,int s) //shortest paths from s in graph G
double distTo(int v) //length of shortest path from s to v
Iterable <DirectedEdge> pathTo(int v)//shortest path from s to v
```

### 2.最短路径属性
***Goal:Find the shortest path from s to every vertex
an important conclusion:shortest-paths tree(SPT) solution exists***
- Consequence:maintain two arrays
- **distTo\[v] is length of shortest path from s to v
- **edgeTo\[v] is the last edge on shortest path from s to v
***Edge relaxation***
- to relax edge e->v->w
	- distTo[v] is length of shortest known path from s to v
	- distTo[w] is length of shortest known path from s to w
	- edgeTo[w] is last edge on shortest known path from s to w
	- if e->v->w gives shorter path to w through v——update both distTo[w] and edgeTo[w]
### 3.戴克特斯拉算法（Dijkstra algorithm）
>步骤
>1.维护双数组:distTo[];edgeTo[],distTo[0] = 0,其余为正无穷；edgeTo[]全部设置为自身；
>2.加入distTo中最小的点；
>3.每加进来一个点，relax所有源于他的边；
### 4.边缘加权DAG
- 若对于一个DAG：**可以先进行拓扑排序，在直接按顺序relax每个点的边**
- 应用：seam carving(图像压缩)
	- 每一个像素对应一个顶点，其有往下，下左，下右三条路径
	- 从最上一行出发到最下一行，计算权重最低的一条路径，把这一条路径的像素删除，实现左右压缩
- 应用2：Parallel job scheduling(并行作业调度)
	- 每一个作业都有一个作业时长，以及必须先完成哪些作业才能做这个
	- 做法：
		- 对每一个作业建立两个顶点(作业开始和作业完成)
		- 在第一项作业(可能不止一个)前建立一个开头，同理建立一个结束
		- 开头/结尾和作业之间的权重为0，作业完成到下一个作业开始的权重也为0
		- 计算从开头到结束的最长路径
### 5.负权重与负权环
- ***Problem:Dijkstra algorithm failed***
- 一个解决方法：所有权重加上负值，是的最低负权重的边变为0
- 糟糕的问题：负权环->使得path的值可以无限减小
- ***Bellman - Ford algorithm***
	- Repeat V times:relax all E edges
	- 若某一次重复时，无改变，则停止。
	- 时间复杂度：最好E+V，最坏EV
- ***扩展API***
	- boolean ifNegativeCycle()
	- 若存在，Bellman - Ford 算法会无限循环
	- 即，如果最后一遍relax循环时，仍有vertex在更新，则存在负权环
- ***应用***
	- ***arbitrage detection(套利检测)***
		- 每个国家建模一个顶点，之间的path权重为汇率的-ln值
		- 只需找到一个负权环，即可找到一个套利路径
# 五.==最大流量与最小流量==
### 1.maxflow简介
