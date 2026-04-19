---
title: "&#91;Algorithm&#93; Greedy Algorithms (1)"
layout: profession-post
categories:
  - Lecture Notes
tags:
  - Algorithm
  - CSED331
use_math: true
toc: true
toc_sticky: true
type: profession
---

> 이 글은 포스텍 오은진 교수님의 CSED331 알고리즘 수업의 강의 내용과 자료를 기반으로 하고 있습니다.

알고리즘 5주차 강의 요약이다.

5주차라고는 했지만, 실제로는 6주차 수업에 가깝다. 

# 1. Greedy Algorithms

**Greedy algorithm**, 소위 욕심쟁이 알고리즘은 매 선택 시, **지금 이 순간 당장 최적인 답**을 고르는 알고리즘을 말한다.

굉장히 단순하고 직관적이어서 구현하기가 무척 쉽다는 장점이 있지만, 쉬운만큼 그닥 유용하지는 않다.

항상 매 순간 최적의 해를 고른다고 해서 그게 최종적으로 최적일 가능성은 별로 없기 때문이다.

다만 지금 선택이 다음 선택에는 전혀 무관한 값이고, 매 순간의 최적해가 문제에 대한 최적해가 되는 경우라면 유용하다고 할 수 있겠다.

# 2. Minimum Spanning Tree

Greedy algorithm을 설명하기 위해 Minimum Spanning Tree 문제를 풀어보자.

**Minimum Spanning Tree**란 **가장 적은 비용으로 모든 노드들을 연결하는 트리**를 의미한다.

![MINI][I_1]

예를 들어 좌측 그래프의 minimum spanning tree는 우측과 같다.

![Formula][I_2]

다시말해 Minimum Spanning Tree Problem이란, 주어진 Graph의 모든 노드들을 포함하면서 **edge weight의 합을 최소화시키는 트리**를 찾는 문제이다.

## Kruskal's algorithm

아주 기쁘게도, 이 문제는 greedy algorithm으로 풀 수 있는 몇 안되는 문제 중 하나이다.

다만 무작정 weight가 가장 작은 edge를 고르는 게 아니다!

트리-connected acyclic graph를 찾는 것인 만큼 **cycle을 형성하지 않으면서** weight가 가장 작은 edge를 고르는 것이 핵심이다.

이런 접근법을 **Kruskal's algorithm**이라고 한다.

이해를 위해 가볍게 예제를 하나 풀어보자.

![1][I_3]

이 graph의 minimum spanning tree를 Kruskal's algorithm으로 풀 것이다.

![2][I_4]

우선 가장 작은 edge는 $B-C$ 이므로 이걸 선택했다.

![3][I_5]

그 다음으로 작은 건 $C-D$와 $B-D$인데, 이 중 $C-D$를 선택했다. 

여기서 알 수 있듯이, 만들어진 minimum spanning tree는 유일하지 않다.

여러 개의 선택지 중 어떤 것을 고르냐에 따라 답이 달라질 수 있음에 유념하자.

![4][I_6]

가장 작은 건 $B-D$ 이지만, 이 경우 cycle을 형성하므로 배제한다.

그 대신 그 다음으로 작은 $C-F$를 선택했다.

![5][I_7]

4를 가진 edge가 $A-D$, $D-F$, $E-F$ 가 있는데 $D-F$는 cycle을 형성하므로 배제하고, 둘 중 $A-D$를 선택했다.

![6][I_8]

마지막으로 $E-F$를 선택했고, 최종 solution을 얻었다.

이처럼 아주 간단하기 그지 없지만, correctness에 대한 증명은 조금 까다롭다.

### Cut property

증명을 위해서는 Cut property를 알아야 한다.

![Cut][I_9]

**Cut**이라는 건 Vertices를 2개의 set으로 나누는 partition을 의미한다.

또한 **crossing edge**란 이 나눠진 set을 가로지는 edge를 의미한다.

이때, 임의의 cut에 대해서 minimum weight의 crossing edge는 항상 MST에 속한다는 것이 **cut property**이다.

왜 이게 성립하는지 증명해보자.

우리가 보여야 할 것은 minimum weight crossing edge를 선택해서 만든 tree가 MST라는 것이다.

#### Proof of cut property

![Cut property 증명][I_10]

어떤 MST $T$가 존재한다고 해보자.

이때, $T$의 일부분을 $X$라고 하자.

$X$는 모든 vertice를 연결하진 않았으므로, 전체 $V$를 $X$가 가로지르지 않는 두 영역 $S$와 $V-S$으로 나눌 수 있다.

여기서 minimum weighted crossing edge를 $e$라고 하자.

만약 $e$가 $T$의 일부라면 증명은 끝난다.

반대로 $e$가 $T$의 일부가 아닌 상황을 고려해보자.

우리가 보일 것은 **$e$를 포함해서 만든 새로운 tree $T^{′}$이 MST**라는 것이다.

원래 $T$가 $e$ 대신 $e^{′}$을 포함한다고 해보자.

새로운 Tree $T^{′}$을 만들기 위해, $T$에서 $e^{′}$을 제거하고 $e$를 더해보자. $(T^{′}=T\cup \lbrace e \rbrace-\lbrace e^{′}\rbrace)$

$T^{′}$이 MST임을 증명해보자.

$T^{′}$의 weight는 $weight(T^{′})=weight(T)+w(e)-w(e^{′})$이다.

이때 $w(e)\leq w(e^{′})$이므로, $weight(T^{′})\leq weight(T)$가 된다.

따라서 **$T^{′}$ 또한 MST가 된다.**

아래는 cut property의 증명의 핵심을 담고있는 그림이다.

증명을 이해했으면 그림이 무슨 소리를 하는지 이해하는데 지장이 없을 것이라 생각한다.

![Example][I_11]

### Apply cut property to Kruskal's algorithm

우리가 위에서 실컷 증명한 cut property와 크루스칼 알고리즘이 무슨 관계일까?

잘 생각해보면, cut property가 크루스칼 알고리즘 그 자체라는 걸 알 수 있다.

크루스칼 알고리즘은 매번 cycle을 형성하지 않는 minimum weight edge를 선택하는데, 이 edge는 사실 **crossing edge**다.

그리고 이 crossing edge가 잇는 두 집합이 $S$와 $V-S$인 것이다.

약간 와닿지 않을 수도 있는데, 예시를 보자.

![7][I_12]

우리가 위에서 풀었던 예제를 cut property의 개념을 적용해서 보자.

그러면 이렇게 두 집합 $\lbrace A,E\rbrace$와 $\lbrace B,C,D,F\rbrace$로 나눌 수 있다.

![8][I_13]

여기서 우리가 선택한 A-D는 사실 $\lbrace A,E\rbrace$와 $\lbrace B,C,D,F\rbrace$를 잇는 **crossing edge**라고 볼 수 있는 것이다.

# 3. Union-Find

크루스칼 알고리즘을 제대로 이해했다면, 이 질문이 떠올라야 한다.

> Cycle을 형성하는지 어떻게 판단하지??

다시 말해, 어떤 edge $(u, v)$를 고를 때 **다른 component**에서 $u$와 $v$를 골라야 하며, 고르고 난 후 **두 component는 합쳐져야 한다**는 것이다.

이런 기능을 지원하는 자료구조가 Union-Find 이다.

**Union-Find**의 핵심은 **여러 개의 노드가 존재할 때 두 개의 노드를 선택해서 현재 이 노드가 서로 같은 집합에 속하는지를 판별하는** 것이다.

이를 위해 총 3가지의 기능을 지원한다.

- `makeset(x)`: create a singleton set containing just x
- `find(x)`: to which set does x belong?
- `union(x,y)`: merge the two sets containing x and y

크루스칼 알고리즘은 이 3가지 기능을 적절히 활용하여 구현된다.

![Code][I_14]

`find(u)!=find(v)`를 통해 cycle을 형성하는지 판단하고,

cycle을 형성하지 않는다면 `union(u,v)`를 통해 두 componenet를 합치고 있다.

## Implementation of Union-Find

구현의 핵심은 각 set을 directed tree로 간주하는 것이다.

예를 들어 아래의 set이 있다고 해보자.

![Set][I_15]

이 Set을 이렇게 Tree로써 생각하는 것이다.

![Tree][I_16]

왜 이렇게 해야할까?

다른 set에 속해있는지 판단하는 방법 중 가장 간단한 것은 아마 배열을 순회하는 방식일 것이다.

그런데 이건 너무 느리다.

애초에 같은 set에 속해있는지 판단하는데 전체를 순회할 필요조차 없다. 

잘 생각해보면, 어떤 set의 대표값을 세우고, 이 **대표값만 비교하면 된다**는 걸 알 수 있다.

이 아이디어가 바로 Tree로 생각하는 아이디어이다.

대표값을 root로 두고, 이 set에 속한 다른 값들은 node로 표현하는 것이다.

그러면 $x,y$가 같은 set에 있는지 판단하기 위해서는 각각이 소속된 tree를 거슬러 올라가서 root를 얻어내고 이를 비교해보기만 하면 된다는 결론에 이른다.

이제 아이디어를 캐치했으니, 알고리즘을 자세하게 알아보자.

![Make][I_17]

- $\pi (x)$: pointer to its parent.
- $rank(x)$: height of the subtree haning from $x$.

`makeset(x)`은 단순하다.

$x$를 root로 가지는 tree를 하나 만든 것이다.

이때 기억해야 할 점은 root의 parent를 root로 둔 것이다.

![Find][I_20]

`find(x)`도 단순하다.

root에 도달할 때까지 tree를 거슬러올라가서, 찾으면 root를 반환한다.

이때 root인지 판단하기 위해 parent가 자기 자신인지 체크하고 있다는 것만 기억하면 된다.

![Union][I_18]

`union(x,y)`는 조금 생각해볼 점이 있다.

두 개의 tree를 합칠 때, 어떻게 합쳐야 `find(x)`에 걸리는 시간을 최소화 할 수 있을까?

다시 말해, 어떻게 합쳐야 전체 tree의 height를 최소화 할 수 있을까?

간단하다. **height가 작은 tree을 height가 큰 tree의 root의 자식으로 편입**시키면 된다.

![ex][I_19]

이런 식으로 말이다.

코드는 이 알고리즘을 표현한 것에 불과하다. 한 번 슥 읽어보면 이해가 갈 것이다.

아래는 예시이다. 위 과정을 잘 이해했으면 별 문제없이 이해할 수 있을 것이다.

![Exmplae2][I_21]

## Properties of Union-Find

Union-Find에는 5가지 중요한 성질이 있다.

하나하나 알아보자.

> For any non-root node x, $rank(x)<rank(\pi (x))$

굉장히 자명하다. 당연히 부모 노드의 rank가 더 크다.

> For a root $x$, $rank(x)$ is the height of the tree containing $x$

1번 property에 의한 당연한 귀결이다.

> If $x$ is not a root, $rank(x)$ will never change again

`union(x,y)`는 root에 곧바로 tree를 이어붙이니, 다른 자식 노드들에게는 영향이 없기 때문이다.

> Any root node of rank $k$ has at least $2^k$ nodes in its tree

이건 최소 binary tree가 되어야 성립할 말이지 않나 라는 의문이 들 수 있다.

근데 우리가 만드는 트리는 높이가 더 낮은 트리를 더 높은 트리의 **root에 병합**시키는 조금 독특한 트리임을 잊으면 안 된다.

한 번 곰곰히 생각해보면, 왜 이 property가 성립하는지 감을 잡을 수 있을 것이다.

당연히 직관과는 별개로 증명이 존재한다.

Induction으로 증명해보자

- Basis: $k=0$ 에서 성립함.
- Induction Step:
  
  $k-1$에서 성립한다고 가정.

  Node $x$ of rank $k$는 rank $k-1$짜리 두개를 합쳐야 만들어질 수 있다.

  이때 각각은 $\geq 2^{k-1}$ nodes를 가지므로, $x$는 $\geq 2\cdot 2^{k-2}=2^k$개의 nodes를 가진다.

> If there are $n$ elements overall, there can be at most $n/2^k$ nodes of the rank $k$
> > → the max rank is $log n$, so the tree height $\leq logn$ 

4번 property로 부터 유도되는 property 이다

중요한 것은 두 번째 문장이다.

Height가 $\leq logn$이라는 말은, `find(x)`와 `union(x,y)`의 시간 복잡도가 $O(logn)$이라는 말이 되기 때문이다.

## Time complexity of Kruskal's algorithm

위 사실로부터 전체 시긴 복잡도를 구해보자.

처음에 전제 edge를 weight로 정렬하는데 $O(\left\lvert E \right\rvert log\left\lvert E \right\rvert) \approx O(\left\lvert E \right\rvert log\left\lvert V \right\rvert)$.

이후에 본격적으로 알고리즘을 돌리면, 최악의 경우 총 $\left\lvert E \right\rvert$ 번 실행될 것이며 `find(x)`와 `union(x,y)`는 $O(log\left\lvert V \right\rvert)$ 만큼 걸리므로 총 $O(\left\lvert E \right\rvert log\left\lvert V \right\rvert)$.

따라서 토탈 $O(\left\lvert E \right\rvert log\left\lvert V \right\rvert)$이 된다.

[I_1]: /assets/lecture/algo/5/mini.PNG
[I_2]: /assets/lecture/algo/5/formula.PNG
[I_3]: /assets/lecture/algo/5/1.PNG
[I_4]: /assets/lecture/algo/5/2.PNG
[I_5]: /assets/lecture/algo/5/3.PNG
[I_6]: /assets/lecture/algo/5/4.PNG
[I_7]: /assets/lecture/algo/5/5.PNG
[I_8]: /assets/lecture/algo/5/6.PNG
[I_9]: /assets/lecture/algo/5/cur.PNG
[I_10]: /assets/lecture/algo/5/cut.PNG
[I_11]: /assets/lecture/algo/5/example.PNG
[I_12]: /assets/lecture/algo/5/7.PNG
[I_13]: /assets/lecture/algo/5/8.PNG
[I_14]: /assets/lecture/algo/5/code.PNG
[I_15]: /assets/lecture/algo/5/set.PNG
[I_16]: /assets/lecture/algo/5/tree.PNG
[I_17]: /assets/lecture/algo/5/make.PNG
[I_18]: /assets/lecture/algo/5/union.PNG
[I_19]: /assets/lecture/algo/5/example_2.PNG
[I_20]: /assets/lecture/algo/5/find.PNG
[I_21]: /assets/lecture/algo/5/example_3.PNG