---
title: "&#91;Algorithm&#93; 3주차 강의 요약 (2)"
categories:
  - Lecture Notes
tags:
  - algorithm
  - csed331
use_math: true
toc: true
---

3주차 두번째 알고리즘 강의 요약이다.

graph 이론에 대해서 배운다!

# 1. Graph란?

> A graph $G=(V,E)$ consists of a nonempty set V of vertices (or nodes) and a set E of edges

예를 들어 아래의 그림은 4개의 vertices와 5개의 edges로 이루어진 graph이다.

![Graph example][I_1]

Graph는 크게 [undirected graph](#2-undirected-graph란)와 [directed graph](#3-directed-graph란)로 나뉜다.

# 2. Undirected Graph란?

> 방향성이 없는 graph

![Undirect graph example][I_2]

Undirected graph에서는 **어떤 노드끼리 연결되어있다는 사실 자체**에 집중하지, 어떤 방향으로 연결되어 있는지는 관심이 없다.

곧, $(1,2)$건 $(2,1)$이건 똑같이 생각한다는 것이다.

# 3. Directed Graph란?

> 방향성이 있는 graph

![Direct graph example][I_3]

반대로 directed graph에서는 **어떤 방향으로 연결되어 있는지**에도 관심이 있다.

곧, $(a,c)$와 $(c,a)$는 다르다는 것이다.

# 4. Graph Representation

Graph를 어떻게 구현해야 할까? 

크게 두가지 방법이 있다.

## 1. Adjacency matrix

첫 번째는 **Adjacency matrix**, $n\times n$ 크기의 행렬로 표현하는 방법이다.

이 Matrix를 $A_G$라고 할 때, $A_G$는 아래와 같은 값을 가진다.

$A_G=[a_{ij}],\, where\; a_{ij}=\left\lbrace\begin{matrix} 1& if\, \lbrace v_i, v_j \rbrace \ is\, an\, edge\, of\, G\\\ 0 &otherwise \end{matrix}\right.$

![Adjacency matrix example][I_4]

위처럼, edge $\lbrace i,j\rbrace$가 있다면 1, 그렇지 않다면 0으로 표시한다. 

이 방법의 장점은 $(u,v) \in E\; or\; not$ 을 상수시간($O(1)$)에 결정할 수 있다는 점이다.

그러나, edge의 유무에 관계없이 $O(n^2)$의 matrix space를 유지하고 있어야 하기 때문에 **공간적 효율성이 떨어진다**는 단점이 있다.

## 2. Adjacency list

두 번째는 **Adjacency list**, 각 vertice에 대해, 각각과 연결되어 있는 vertices를 linked list로 표현하는 방법이다.

![Adjacency list example][I_5]

이 방법의 장점은 필요없는 edge에 대해서는 저장할 필요가 없기 때문에 $O(\left\lvert V \right\rvert + \left\lvert E \right\rvert)$라는 **좋은 공간적 효율성**을 보여준다는 것이다.

그러나, $(u,v) \in E\; or\; not$ 를 결정하기 위해서 list를 탐색해야 하므로, 상수 시간이 아닌  $O(min\lbrace deg(u),deg(v)\rbrace )$ 만큼 걸린다는 단점이 있다.

# 3. Connectivity

## 1. Path

> A **path** of a graph $G=(V,E)$ is a **sequence of nodes**, for example $v_1, v_2, \cdots, v_3$

## 2. Cycle

> A **cycle** is a path which **begins and ends at the same node**

<span style="color:red">$v_1$</span> $, v_2, \cdots,$ <span style="color:red">$v_1$</span> 처럼 처음으로 돌아오는 path를 의미한다.

## 3. Connectedness

만약 어떤 pair of node에 대해서도 path가 존재한다면, 해당 그래프는 **connected** 되었다고 부른다.

반대로 path가 없는 pair가 존재한다면 **disconnected** 되었다고 부른다.

### Connected component

> A connected component of a graph *G* is the maximal connected subgraph of *G*

maximal이라는 말이 헷갈릴 수 있는데, 다른 말로 하자면 **다른 connected subgraph의 subgraph가 아닌 것**을 말한다.

![Connected component example][I_6]

예를 들어, 여기서 connected subgraph는 $b, a, c$ 점 각각도 될 수 있는데, 이 점들은 $H_1$의 subgraph이므로(=maximal이 아니므로) connected component가 아니다.

따라서 위 그림에서 connected components는 $H_1, H_2, H_3$ 가 된다.

### Strong & weak connected

![Strong weak connected][I_7]

Strong이란 **양방향**으로 path가 존재하는 것을 말한다.

반대로 weak란 최소 **한방향**으로 path가 존재하는 것을 말한다. 

![Strong weak connected example][I_8]

예를 들어, *G*는 모든 pair of nodes에 대해 양방향으로 path가 존재하므로 **strongly connected**이다.

또한 *H*는 $a \rightarrow b$ 로 가는 path가 없으므로 **strongly connected**는 아니지만, 모든 pair of nodes에 대해 최소 한방향으로는 path가 존재하므로 **weakly connected**이다.

그리고 *H*에서 <span style="color:red">$node\;a$</span>, <span style="color:skyblue">$node\;b$</span>, <span style="color:green">$subgraph\;of\;the\;vertices\;b,c,d\;and\;edges\;(b,c), (c,d),\;and\;(d,b)$</span>, 이 세개가 **strongly connected components**이다.

## 4. Tree

> A tree is an undirected graph that is **connected** and **acyclic**

Tree는 undirected graph의 특수한 형태이다.

Tree를 connected & acyclic 이라고 기억하기 보다는, 아래의 3개의 조건 중 2가지를 만족하는 undirected graph라고 기억하는게 좋다.

**(a)** *G* is connected

**(b)** *G* does not contain a cycle

**(c)** *G* has n-1 edges

사실 3개 중 2개만 만족해도 나머지 하나를 만족하는데, 이걸 증명하는게 시험으로 나온 적이 있었다.

한번쯤은 증명해보자 ㅎㅎ

이제 Tree에 대한 property 하나만 증명해보고 넘어가자.

> **Property.** An undirected graph is a tree iff there is a **unique simple path** between any pair of nodes

1. $\Leftarrow$
   
   모든 pair of nodes에 대해 path가 있다 $\rightarrow$ connected graph
   
   path가 unique하다 $\rightarrow$ acyclic graph
   
   $\therefore$ graph는 tree이다.

2. $\Rightarrow$
   
   Proof by contradiction:

   Path가 두 개 이상 존재하거나, simple하지 않으면 해당 path들이 cycle을 이루므로 가정에 모순이다. ( $\because$ Tree is an acyclic graph)

# 6. Depth-First Search(DFS)

DFS는 **깊이를 우선적으로 탐색**하는, 즉 한 방향만 우직하게 가보는 탐색법이다.

핵심은 아래와 같다.

- 한번 방문한 node는 다시 방문하지 않는다

- 자식을 찾으면 곧장 explore 한다 (깊이 우선 탐색)

이를 수도코드로 쓰면 아래와 같다.

![procedure reachable][I_9]

![procedure explore][I_10]

예를 들어보자.

아래의 graph의 1번 node에서 DFS를 한다고 하자.

![DFS example][I_11]

그러면 아래와 같이 탐색하게 될 것이다.

![DFS solution][I_12]

## Proving correctness of a DFS

이제 그럼 DFS의 알고리즘이 과연 올바른지 증명을 해보자.

> Q. Does it find all vertices reachable from v?

**Proof by contradiction:**

![Proof 1][I_13]

**가정**: *u*가 *v*로부터 reachable하지만 `explore()`가 *u*를 잡아내지 못한다

Reachable하다는 말은 *v*에서 *u*로 가는 path가 존재한다는 것이고, 이 path를 $\pi$라고 하자.

그리고 이 $\pi$에서 `explore()`가 마지막으로 방문한 node를 *z*라고 하자.

그리고 이 *z*의 자식 중 $\pi$ 위에 있는 node를 *w*라고 하자.

그러면 당연하게도 procedure은 *w*를 언젠간 방문할 것이다.

위 과정을 계속 반복하면 결국에는 *u*에 도달할 수 있을 것이고, 이는 가정에 모순이다.

$\therefore$ **DFS는 모든 reachable한 node들을 찾아낸다.**

> Q. Are all the returned vertices reachable from v?

DFS 과정을 역으로 거슬러올라가보면 결국에는 *v*에 도달하므로 당연히 *v*로부터 reachable하다.

## Running time of reachable

모든 node들을 방문하므로 우선 $O(\left\lvert V \right\rvert)$ 이고,

각각의 edge $(u,v)\in E$ 를 `explore(u)`, `explore(v)` 에서 각각 탐색하므로, $O(2 \left\lvert E \right\rvert)$이다.

따라서 합치면 $O(\left\lvert V \right\rvert + \left\lvert E \right\rvert)$가 된다.

[I_1]: /assets/lecture/algo/3/graph_example.PNG
[I_2]: /assets/lecture/algo/3/undirected_example.PNG
[I_3]: /assets/lecture/algo/3/directed_example.PNG
[I_4]: /assets/lecture/algo/3/adj_matrix.PNG
[I_5]: /assets/lecture/algo/3/adj_list.PNG
[I_6]: /assets/lecture/algo/3/connected_component.PNG
[I_7]: /assets/lecture/algo/3/strong_weak.PNG
[I_8]: /assets/lecture/algo/3/strong_weak_example.PNG
[I_9]: /assets/lecture/algo/3/reachable.PNG
[I_10]: /assets/lecture/algo/3/explore.PNG
[I_11]: /assets/lecture/algo/3/dfs_example.PNG
[I_12]: /assets/lecture/algo/3/dfs_solution.PNG
[I_13]: /assets/lecture/algo/3/proof_1.PNG