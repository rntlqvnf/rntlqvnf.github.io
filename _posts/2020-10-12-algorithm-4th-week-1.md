---
title: "&#91;Algorithm&#93; 4주차 강의 요약 (1)"
categories:
  - Lecture Notes
tags:
  - algorithm
  - csed331
use_math: true
toc: true
---

알고리즘 4주차 강의 요약이다.

알고리즘이 제일 글 쓰기 쉬운 것 같다.

전달하고자 하는 주제가 참 명확해서 좋다.

그와 별개로 과제의 난이도는... 이걸 푸는 학생들에게 경외감을 가지게 만드는 수준이다.

알골 랩은 아닌 것 같다... ㅠㅠ

# 1. DFS on Directed Graphs

이제 열심히 배운 DFS를 directed graph에서 해보자.

그런데 그냥 하는게 아니라, 좀 더 그 결과를 분석할 수 있도록 여러가지 용어와 개념을 정의해보자.

## 1. previsit and postvisit

가장 먼저 previsit과 postvisit 이라는 개념을 정의해야한다.

**Previsit**이란 어떤 node를 처음 방문하는 이벤트를 의미한다.

반대로 **postvisit**이란 어떤 node를 최종적으로 떠나는 이벤트를 의미한다.

당연하게도 방문하는 모든 node들에 대해 previsit과 postvisit 이벤트 둘 다 일어날 것이다.

이제 각각의 이벤트에 숫자를 붙여 이벤트 발생 순서를 명시해보자.

![Pre post code][I_2]

숫자를 부여하는 방법은 간단하다.

clock을 1로 초기화 한 다음, 처음 방문하는 경우 `previsit(v)`를, 최종적으로 떠나는 경우 `postvisit(v)`를 호출해주면 된다.

![Previsit postvisit이란?][I_1]

예를 들어 왼쪽 그래프의 A node에서 DFS를 시작한다면 우측과 같이 pre-postvisit number가 부여되게 된다.

## 2.  Terminology for edges

그럼 이 pre-postvisit number가 어떻게 쓰일까?

잘 생각해보면 previsit 값이 더 크다는 말은 나중에 방문한다는 얘기이고, postvisit 값이 더 크다는 말은 나중에 떠난다는 얘기이므로,

두 노드의 pre-postvisit number의 관계를 통해 두 노드를 잇는 edge의 성질을 정의할 수 있음을 알 수 있다.

이걸 종합해서 새로운 개념들을 정의해보자.

![Order of pre post][I_4]

$(u,v)$가 있다고 하자.

만약 $u$보다 $v$를 나중에 방문하며 $(u_{pre}<v_{pre})$, $v$보다 $u$를 나중에 떠난다면 $(v_{post}<u_{post})$ tree/forward edge라고 부른다.

만약 $u$보다 $v$를 먼저 방문하며, $(v_{pre}<u_{pre})$, $v$보다 $u$를 먼저 떠난다면 $(u_{post}<v_{post})$ back edge라고 부른다.

만약 $u$보다 $v$를 먼저 방문하며, $(u_{pre}<v_{pre})$, $v$보다 $u$를 나중에 떠난다면 $(v_{post}<u_{post})$ back edge라고 부른다.

![Tree][I_3]

사실 저렇게 글로 적어놓으면 상당히 이해하기 어렵다.

간단히 DFS의 정방향 edge라면 tree/forward, 역방향 edge라면 back, 횡방향 edge하면 cross라고 이해하면 된다.

## 3. back edge and cycle

이제 이 terminologies와 관련된 property를 알아보자!

> A directed graph has a cycle if and only if its depth-frst search reveals a back edge.

생각해보면 굉장히 당연한데, 한 번 증명해보자. 

한 쪽은 꽤 자명하다.

back edge $(u,v)$가 있다는 말은 **$v$에서 $u$로 가는 path** + $(u,v)$를 하면 cycle이 생기므로 성립한다.

그런데 다른 한 쪽은 살짝 증명법이 생각이 안 날 수도 있다. 근데 알고보면 굉장히 당연하다.

cycle $v_0, v_1, v_2, \cdots, v_k, v_0$가 있을 때, DFS가 이 cycle에서 처음으로 방문하는 node를 $v_i$라고 하자.

$v_i$로부터 모든 cycle의 node들이 방문 가능하므로, DFS는 언젠간 $v_{i-1}$을 방문하게 될 것이고, 곧 back edge $(v_{i-1},v_i)$를 찾게 될 것이다. 따라서 성립한다.

저번 과제에 이거랑 살짝 다르게 acycle iff no back edge를 증명하는게 나왔었는데, 한 번쯤 증명 해보는 걸 추천한다.

# 2. DAG

뜬금없이 cycle은 왜 배웠는가.. 사실 DAG를 설명하기 위한 빌드업이었다.

![DAG란][I_5]

**DAG, directed acyclic graph**란 말 그대로 cycle이 없는 directed graph이다. Tree의 정의와 동일하다.

이 DAG의 아주 중요한 특성은 **linearize** (or **topologically sort**)가 가능하다는 것이다.

**Linearize**란, 한 쪽 방향의 edge만 존재하도록 노드들을 정렬하는 것을 말한다.

![Origin][I_6]

예를 들어, 이 그래프를 linearize 하면

![After][I_7]

이와 같은 형태가 된다.

이런 식으로 graph를 변형해놓으면 edge가 한쪽 방향으로만 쏠려서 여러 제약 문제를 풀 때 굉장히 유용하다.

예를 들어 인공지능 탐색을 할 때 우측에서 좌측으로만 contraint propagation을 하면 된다던가, scheduling을 할 때 우측부터 하나씩 할당하면 된다던가...

여튼 그럼 이 linearize를 어떻게 만들 수 있을까?

총 두 가지 방법이 있다.

## 1. Using DFS

첫 번째로는 DFS를 이용하는 것이다.

DAG를 잘 살펴보면, 아래의 property를 만족한다는 걸 알 수 있다.

> In a dag, every edge leads to a vertex with a lower post number.

자, 여기서 post number는 DFS에서 떠나는 순서를 의미한다.

그러면 DFS를 활용해서 DAG를 구할 수 있지 않을까?

맞다. DFS를 돌려서 먼저 떠나는 것 부터 우측에서 좌측으로 정렬하면 DAG가 만들어진다.

![Code][I_8][^1]

실제 구현은 stack을 이용해서 한다.

먼저 떠나는 순서대로 stack에 push하고, DFS가 끝나면 stack에서 pop 한 순서대로 좌측에서 우측으로 정렬하면 DAG가 완성된다.

간단한 코드니, 읽어보고 제대로 이해해두는 것을 강력히 권장한다. 

그럼 이 linearize의 time compexity는 얼마일까?

그냥 DFS하면 구해지므로, 심플하게 **linear time**이다.

## 2. Using source, sink node

두 번째 방법은 source, sink를 활용하는 것이다.

**Source node**는 들어오는 edge가 없는 node를 말하고

**Sink node**는 나가는 edge가 없는 node를 말한다.

DAG를 잘 살펴보면 가장 post number가 작은 node는 sink이고, 가장 큰 node는 source임을 알 수 있다.

증명은 간단하다.

만약 highest post number node $v$가 source가 아니라고 해보자.

그러면 해당 node로 들어오는 어떤 edge $(u,v)$가 존재할 것이고, 그러면 $u$가 $v$보다 더 큰 post number를 가지므로 모순이므로 $v$는 source이다.

Sink도 동일한 방식으로 증명된다.

여튼, source-sink를 이용해서 DAG를 만드는 방법은 아래와 같다.

1. Find a source, output it, and delete it from the graph.
2. Repeat until the graph is empty.

이 접근법을 잘 기억해두자. 

[3번](#3-connectivity-in-directed-graphs)의 핵심 접근법이다.

# 3. Connectivity in Directed Graphs

지난 시간에 배웠던 [strongly connected component][P_1]를 생각해보자.

Directed graph는 이와 관련된 재밌는 property가 하나 있다.

> Every directed graph is a dag of its strongly connected components

예를 들어 이런 것이다.

![DAG_STR][I_9]

이 Property가 가지는 의미는 DAG의 개념을 일반화 했다는 것이다.

이런 관점에서 우리가 2번에서 배운 건 모든 node가 각각 strongly connected component인 특수 케이스라 할 수 있다.

이제 어떻게 만드는지 배울 시간이다.

그런데 생각보다 꽤 막막할 것이다.

Cycle이 존재하므로 무작정 DFS를 돌릴 수도 없는 데다가, 어떻게 component를 분리해내야 할지 생각도 안 날 것이다.

관련된 properties를 하나씩 천천히 알아보며 빌드업해보자.

>  If the `explore` subroutine is started at node $u$, then it will terminate precisely when all nodes reachable from u have been visited

Property 자체는 심플하고 자명하다.

중요한 점은, 만약 sink strongly connected component의 node에서 DFS를 돌리면, 위 property에 의해 정확히 sink component만 찾고 DFS가 끝난다는 점이다.

이 아이디어를 적용하여 DAG를 찾는 알고리즘을 만들어보면 아래와 같을 것이다.

1. sink component를 찾는다
2. 이를 원래 graph에서 지운다.
3. Graph가 빌 때 까지 1,2 를 반복한다 

그러면 근데 sink strongly connected component를 어떻게 찾아야 할까?

Direct하게 찾는 방법은 생각보다 어렵다.

> The node that receives the highest post number in a depth-first search must lie in a source strongly connected component

여기서 property 2가 등장한다.

이걸 어떻게 이용해야 할까?

Reversed graph $G^R$를 생각해보자

$G^R$에서 **source** strongly connected component를 찾으면, 이건 $G$의 **sink** strongly connected component가 된다는 놀라운 사실을 알 수 있다.

이때 property 2에 의해 $G^R$의 highest post number node는 source strongly connected component에 속하므로,

$G^R$에서 DFS를 돌려 highest post number node를 구하고, 이 노드를 starting point 삼아 $G$에서 DFS를 돌리면 sink strongly connected component를 구할 수 있다!

자 그럼 하나 구하고 나서 어떻게 반복해야 할까?

> If $C$ and $C_1$ are strongly connected components, and there is an edge from a node in $C$ to a node in $C_1$, then the highest post number in C is bigger than the highest post number in $C_1$

여기서 property 3이 등장한다.

Property 3를 이용하면, 위에서 구한 source strongly connected component를 $G^R$에서 지우고 남은 graph의 highest post number node는, 지운 component와 연결된 component에 존재한다는 것을 알 수 있다.

따라서, dag of its strongly connected components를 만드는 알고리즘은 다음과 같다.

1. $G^R$을 만든다
2. $G^R$에 대해 DFS를 돌려 node들을 post number로 정렬한다.
3. Higest post number node를 starting point로 잡아 $G$에서 DFS를 돌려 sink strongly connected component를 찾는다.
4. 3번에서 구한 sink strongly connected component를 $G$에서 지운다.
5. 3-4번을 graph가 빌 때까지 반복한다.

시간 복잡도를 구해보자.

1번과 2번 과정은 당연히 linear하다.

3-4번을 반복하는 건, $\sum{\left\lvert SCC \right\rvert} = O(\left\lvert V \right\rvert+\left\lvert E \right\rvert)$ 이므로 linear하다.

따라서 전체 시간 복잡도는 **linear**이다!

[I_1]: /assets/lecture/algo/4/pre_post.PNG
[I_2]: /assets/lecture/algo/4/pre_post_code.PNG
[I_3]: /assets/lecture/algo/4/tree.PNG
[I_4]: /assets/lecture/algo/4/order.PNG
[I_5]: /assets/lecture/algo/4/dag.PNG
[I_6]: /assets/lecture/algo/4/origin.PNG
[I_7]: /assets/lecture/algo/4/after.PNG
[I_8]: /assets/lecture/algo/4/code.PNG
[I_9]: /assets/lecture/algo/4/dag_str.PNG

[^1]: https://upcount.tistory.com/91

[P_1]: {% post_url 2020-09-30-algorithm-3nd-week-2 %}/#strong--weak-connected