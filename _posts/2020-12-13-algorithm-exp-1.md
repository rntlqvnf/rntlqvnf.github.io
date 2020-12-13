---
title: "&#91;Algorithm&#93; Exponential Algorithm (1)"
categories:
  - Lecture Notes
tags:
  - algorithm
  - csed331
use_math: true
toc: true
---

> 이 글은 포스텍 오은진 교수님의 CSED331 알고리즘 수업의 강의 내용과 자료를 기반으로 하고 있습니다.

공부하기 싫다!! 내용도 도통 뭔 소린지 모르겠고 ㅠㅠ

개인적으로 최근 ~~ algorithm 파트들이 제일 싫다.

뭔 소린지도 모르겠고, 대체 어떻게 문제를 낼 지도 모르겠다.



# 1. Exponential Algorithm

우리가 지금까지 배운 Approximation / Parametrized algorithm은 이론 수학자가 NP-complete / NP-hard 문제를 다루는 방법들이다.

**Approximation**은 근사를 통해 poly-time algorithm을 만든다.

**Parametrized**는 output param을 도입해서 input size에 대해선 polytime을 만든다.

그런데 위 두가지 방법을 적용해도 polytime을 만들 수 없는 경우가 존재한다.

이 경우, 그냥 efficient(poly-time) 조건 자체를 완화하여 Exponential Algorithm을 찾아야 한다.

오늘 우리가 배울 것은 바로 이 Exponential Algorithm case이다.

# 2. Maximum Clique Problem

![Max][I_1]

Maximum Clique problem이란, clique 중에 가장 큰 놈을 찾는 문제이다.

슬프게도, $P=NP$가 아닌 이상 이 문제를 polynomial time에 풀 수는 없다.

## Algorithms

지난 시간에 배웠던 아이디어를 도입하여 ouput size를 고정하면 어떻게 될까?

![OUT][I_2]

놀랍게도 $O(m^{3/2})$로 줄어든다.

이 일고리즘은 [3번](#2-maximum-clique-problem)에서 알아볼 것이다.

그럼 원래의 maximum clique는 얼마나 걸릴까?

![max cliq][I_3]

가장 빠른건 $O(2^{0.249n})$이지만, 오늘은 가장 느린 것을 알아볼 것이다.

# 3. Finding All Cliques of Size 3

알고리즘의 아이디어를 차근차근 밟아보자.

1. > 모든 vertex가 constant degree를 가진다면 가장 빠른 알고리즘은 무엇일까?

   ![COnst][I_4]

   각 vertex의 이웃들을 조합하면서 triangle을 이룰 수 있는지 검사해보면 된다.

2. > 아이디어를 조금 발전시켜서, 모든 vertex가 $O(\sqrt{m})$의 degree를 가진다면 가장 빠른 알고리즘은 무엇일까? ($m$은 edge의 개수)
   
   똑같이, 모든 조합에 대해서 조사해보면 된다.

   시간 복잡도를 구해보자.

   $n$개의 vertex가 주어졌다고 하자.

   각 vertex 당 조합의 개수가 $O(m)$이다.

   이때, 각각의 조합이 triangle을 이루는지 조사하는데 걸리는 시간은 $O(1)$이므로 총 $O(nm)$ 시간이 걸린다.

3. > 아이디어를 조금 더 발전시켜서, $O(\sqrt{m})$ degree를 가지는 vertex가 있고 $\Omega (\sqrt{m})$ degree를 가지는 vertex가 있으면 어떻게 될까?
   
   이게 우리가 알아야 할 $O(m^{3/2})$ 알고리즘이다.

   이때 $\Omega (\sqrt{m})$ degree를 가지는 vertex를 **heavy vertex**라고 한다.

   반대로 $O(\sqrt{m})$ degree를 가지는 vertex를 **light vertex**라고 한다.

   한 가지 중요한 observation은, heavy vertex는 $O(\sqrt{m})$개 존재한다는 것이다.

   간단히 증명해보자.

   $\sqrt{m} \cdot \sharp h \leq 2m$

   $\sharp h \leq 2 \sqrt{m}$

   $\therefore \sharp h = O(\sqrt{m})$

   자, 이제 본격적으로 증명해보자.

   Triangle을 두 가지로 type으로 나눠서 증명할 것이다.

   - **Type-1 Triangle**
     
     > All vertices are heavy
     
     이 경우, 그냥 가능한 heavy vertex의 triple에 대해 조사하면 된다.

     이때 가능한 triple의 수는 observation에 의해 $O(\sqrt{m} \cdot \sqrt{m} \cdot \sqrt{m})=O(m^{3/2})$이고, 조사하는 시간은 $O(1)$이므로

     총 $O(m^{3/2})$ 시간이 걸린다.

   - **Type-2 Triangle**
   
     > At least one vertex is light

     2번째 알고리즘과 동일하다.

     각 light vertex의 이웃들을 조합하며 triangle을 형성하는지 알아보면 된다.

     시간 복잡도는 어떻게 될까?

     각 light vertex의 degree를 $d(v)$라 하면, 조합의 수는 $O(d(v^2))$이므로, 이를 모든 light vertex에 대해 더하면 된다.

     ![Type2][I_5]

     첫 번째 등식은 light vertex의 정의에 따라 $O(\sqrt{m})$을 빼낸 것이다.

     두 번째 등식은 $\sum_{light\; v}d(v) \leq \sum d(v) = 2m$ 이므로, $O(m\sqrt{m})=O(m^{3/2})$가 된 것이다.

     결국 다 합쳐서 총 $O(m^{3/2})$ 시간에 모든 size 3 clique를 찾을 수 있다.

# 4. Exact Algorithm for Maximum Clique

![Lem][I_6]

위 theorem이 우리가 알아볼 maximum clique algorithm이다.

이때 theorem에서 maxi**mal** clique라는 표현을 썼는데, maximum과 헷갈리면 안된다.

**Maximal** clique란, 아래의 조건을 만족하는 집합 $S\subseteq V$를 의미한다.

1. $S$ is a clique and
2. For any vertex $v\in S$, $S \cup \lbrace v \rbrace$ is not clique

한편, **Maximum** clique란, maximal clique 중 최대를 의미한다.

우리가 알아볼 알고리즘은 모든 maximal clique를 찾아낸 다음, 이 중 최대를 골라내는 것이다.

알고리즘은 아래와 같다.

![Algo][I_7]

핵심은 $v$를 제외한 graph $G$에 $v$를 추가할 때 두 가지 케이스가 발생한다는 것이다.

만약 $v$가 $C$의 vertices 중 not connected vertex가 존재한다면, $v$를 추가하면 안된다.

반대로 $v$가 $C$의 vertex들과 모두 connected 되어있다면 ($C-N(v) = \phi$) $v$를 추가해도 clique는 유지가 된다.

그런데 maximal도 유지가 될까?

> 그렇지 않다.

$C$의 바깥에 있는 vertex가 $C-N(v) \cup \lbrace v \rbrace$와는 fully connected 일 수 있기 때문이다.

그래서 $C-N(v) \cup \lbrace v \rbrace$를 만들고, 이것이 maximal인지 판단해서 맞다면 반환해야 한다.

이 알고리즘을 재귀적으로 돌리면 모든 maximal clique를 구할 수 있다.

시간 복잡도를 구해보자.

![Lem][I_6]

Lemma에 의해, $G$는 $O(3^{n/3})$개의 maximal cliques를 가지고 있다.

재귀를 하면 두 번째 단계에서는 $O(3^{n-1/3})$, 세 번째에서는 $O(3^{n-2/3})$, $i$ 번째에서는 $O(3^{n-i+1/3})$이 될 것이다.

이걸 전부 합하면 $O(3^{n/3})$이 나온다.

한편 각 step에서 $C-N(v) = \phi$ 를 판단하고, $C-N(v) \cup \lbrace v \rbrace$가 maximal인지 판단하는데 $O(m)$이 걸린다. (incident edge를 조사하므로)

따라서 총 $O(m\cdot 3^{n/3})$이 걸린다.

이로부터 간단한 corollary도 얻을 수 있다.

> All maxiaml cliques of a graph can be generated in $O(mn)$ time per clique

[I_1]: /assets/lecture/algo/exp/max.PNG
[I_2]: /assets/lecture/algo/exp/fix.PNG
[I_3]: /assets/lecture/algo/exp/max_cli.PNG
[I_4]: /assets/lecture/algo/exp/const.PNG
[I_5]: /assets/lecture/algo/exp/type2.PNG
[I_6]: /assets/lecture/algo/exp/lem.PNG
[I_7]: /assets/lecture/algo/exp/algo.PNG