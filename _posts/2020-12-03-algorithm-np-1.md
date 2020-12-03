---
title: "&#91;Algorithm&#93; NP-complete problmes (1)"
categories:
  - Lecture Notes
tags:
  - algorithm
  - csed331
use_math: true
toc: true
---

> 이 글은 포스텍 오은진 교수님의 CSED331 알고리즘 수업의 강의 내용과 자료를 기반으로 하고 있습니다.

기나긴 NP complete 파트가 드디어 끝이 났다.

개인적으로 개념적으로 참 받아들이기 힘들었던 챕터였다.

한편으로는 대체 이거가지고 어떻게 문제를 낼까 싶었다. 그냥 NP-complete problem은 뭐가 있다~~ 정도만 가르쳐주셨는데.

이번 과제를 풀어보면 알겠지 ㅎㅎ..

저번 과제는 내가 예상했던 대로 꽤 쉬웠었다. 시험까지 쉬우면 얼마나 좋을까 ㅎ..

# 1. Search Problems

어떤 문제의 해를 찾는 가장 기초적인 방법은 가능한 모든 해들에 대해 하나하나 조사해보는 것이다.

하지만 이는 너무너무 많은 시간이 걸려 실제로 사용하기에는 저어된다.

그래서 우리는 이 무식한 탐색 방법을 피하기 위한 다양한 기법들-Greedy, LP, DP-을 무려 두 달 동안이나 배워왔다.

슬프게도 이런 기법들이 전혀 먹히지 않는 문제들이 존재한다.

다시말해, 무식하게 탐색하는 것 보다 좋은 알고리즘을 찾는데 실패한 문제가 존재한다는 것이다.

가장 대표적인 예시가 Satisfiability problem이다.

## Satisfiability

Satisfiability problem은 CNF boolean formula가 true가 되도록 하는 해를 찾는 문제이다.

해가 있으면 해를 반환하고, 없으면 "none exist"를 반환한다.

![SAT][I_1]

이때 CNF는 conjunctive normal form으로, 위와 같이 괄호 안에는 $\wedge$, 괄호끼리는 $\vee$로 연결된 form을 말한다. (디시설 때 배운걸 떠올려보자)

간단한 예제를 통해 이해도를 높여보자.

![SAT_EX][I_2]

## Definition

위 SAT 문제처럼, 어떤 *instance I*를 주고, solution *S*를 찾거나, 존재하지 않음을 보이는 문제가 **search problem**이다.

instance는 문제라고 생각하면 된다.

한편, search problem은 한 가지 중요한 특성을 가진다.

> Any proposed solution *S* to an instance *I* aan be *quickly checked* for correctness

Quickly checked란 polynomial time 안에 확인할 수 있다는 의미이다.

예를 들어, SAT는 문제를 한 번 탐색해서 곧바로 확인할 수 있으므로 quickly checked라 할 수 있다.

중요한 점은 property를 이용해서 search problem을 재정의 할 수 있다는 것이다.

![DEF][I_3]

핵심은 polynomial time안에 verify 할 수 있는 문제라는 것이다.

이후 이 정의가 P-NP 문제를 정의하는 데에 사용되니, 제대로 이해하고 넘어가는 걸 추천한다.

이후 이 소단원에서는 search problem의 예시에 대해서 알아볼 것이다.

이에 대해 잘 이해해야 [2번](#2-np-complete-problems)의 증명들을 쉽게 이해할 수 있으므로, 시간을 들여서 제대로 이해하고 넘어가는 걸 추천한다.

## Traveling Salesman Problem (TSP)

![TSP][I_4]

DP 단원에서 배웠던 문제이다.

![TSP 2][I_5]

DP 에서 배웠던 optimization 문제와 다른 점은 search problem으로 전환하기 위해 **budget**이라는 변수가 추가되었다는 점이다.

Search problem ver은 $cost \leq b$인 경로를 찾는다면 optimization ver은 $min(cost)$인 경로를 찾는다.

아마 이런 의문이 들 것이다.

> 그냥 optimization problem을 search problem으로 쓰면 안되나?

실제로 두 문제는 서로 맞물려 있기도 하다.

Buget b에 대해 binay search를 하면서 search problem을 풀면 optimization problem을 풀 수 있고, optimization problem의 solution이 $\leq b$인지 판단해서 search problem을 풀 수 있다.

그러나, optimization problem은 polynomial-time checkable이 아니라는 점에서 search problem이 될 수 없다.

"is a tour"이나 "has total length $\leq b$"는 polynomial-time check가 가능하지만, "is optimal"을 어떻게 polynomial-time check할 수 있겠는가?

## Euler Path

![Euler][I_6]

상당히 유명한 문제다.

아마 전산수학 시간에 배웠겠지만, 간단한 알고리즘으로 polynomial time에 해의 존재 여부를 알 수 있다.

아이디어는 이렇다.

1. The graph is connected
   
2. Evety vertex, without the possible exception of two vertices, has even degree

구체적인 알고리즘이 중요한 건 아니니, 궁금하면 찾아보길 바란다.

## Hamiltonian Cycle

![Ham][I_7]

쌍둥이처럼 오일러 경로와 항상 같이 등장하는 문제다.

차이점은 오일러 경로는 모든 **edges**를 방문하는 **path**를, 해밀턴 사이클은 모든 **vertices**를 방문한다는 **cycle**을 의미한다는 점이다.

## Balanced Cut

![Bal][I_8]

Balance 있는 cut을 찾는 문제이다.

이때 edges in cut이란, 잘려진 두 subset을 지나는 edge들을 의미한다.

## Integer Linear Programming

![ILP][I_9]

LP인데, 해가 음수가 아닌 정수인 LP이다.

조건이 하나 달라졌을 뿐이지만, 놀랍게도 아직까지도 exponential time보다 효율적인 알고리즘이 만들어지지 않았다고 한다.

## Three-dimensional Matching

![Three][I_10]

이후에 아주 자주 출현하는 친구니, 확실히 이해하고 가는 걸 추천한다.

문제 자체는 간단하다. Boy-Girl-Pet triple을 충돌 없이 만들면 된다.

간단한 예제를 통해 이해도를 높여보자.

![Three Ex][I_11]

위 같이 3 boy, 3 girl, 3 pet이 주어졌다고 해보자.

충돌이 나지 않게 모든 boy-girl-pet을 매칭하려면 어떻게 해야할까?

![Sol][I_12]

여러 답이 있겠지만, 위의 빨간색 동그라미 같이 매칭하면 될 것이다.

## Independent Set, Vertex Cover and Clique

![IVC][I_13]

이 세개를 묶어서 설명하는 이유는 나중에 증명하겠지만 서로 같은 문제이기 때문이다.

Indp Set prob은 이전에 봤을 것이다.

**Vertex Cover problem**은 모든 edge에 닿을 수 있는 vertex set을 찾는 것이다.

닿는다는 정의가 애매할 수 있다.

어떤 edge $(u,v)\inE$에 대해 $u$ 또는 $v$가 $(u,v)$에 닿을 수 있는 vertex라고 생각하면 된다.

곧, Vertex Cover는 모든 edge에 대해 그 end point 중 최소 하나는 들어있는 set이라는 의미이다.

**Clique problem**은 모든 $u\in S, v\in S$에 대해 $(u,v)$가 존재하는 set $S$를 찾는 것이다.

## Longest Path

![LP][I_14]

TSP에서 봤던 것처럼 search problem으로 바꾸기 위해 longest라는 단어가 사라지고 *at least g*가 추가되었다는 것만 기억하면 된다.

간단한 예제를 통해 이해도를 높여보자.

![LP_Ex][I_15]

## Subset Sum

![Subset][I_16]

DP에서 배웠던 knapsack 문제의 special case다.

별 것 없지만, 이것도 상당히 풀기 어렵다고 한다.

# 2. NP-complete problems

중요한 개념을 알아볼 차례이다.

Search problem은 아래와 같이 정의된다.

![DEF][I_3]

이렇게 polynomial time에 solution을 verity 할 수 있는 문제를 **class NP**로 분류한다.

한편, 위에서 알아보았던 오일러 경로 같이 문제 자체를 polynomial time에 풀 수 있는 class NP 문제를 **class P**로 분류한다.

여기서 질문.

> polynomial time에 풀 수 없는 search problem이 존재할까?

![PNP][I_18]

다른 말로, $P \neq NP$ 일까? 

이게 바로 한 번쯤은 들어보았을 수학 난제 $P-NP$ 문제이다. 

질문은 간단하지만 참 사악하기 그지없는 문제다. 

난제는 잠시 냅두고, 우리가 알아볼 것은 class NP 안에 존재하는 **NP-hard**와 **NP-complete**라는 소분류이다.

NP-hard의 정의는 NP-complete 이면서 NP에 속하는 문제를 의미하니, NP-complete의 정의를 알아보자.

NP-complete의 정의는 다소 이해하기 어렵다.

> A search problem is NP-complete if all other search problems reduce to it

Reduce라는게 뭘까? 

이를 제대로 이해하기 위해서는 [3번](#3-reductions)에서 설명할 reduction에 대해서 알아야 한다.

지금은 간단히 문제를 치환할 수 있다는 의미로 받아들이면 된다.

곧, 모든 NP 문제가 해당 문제로 치환된다면 그 문제는 NP-complete인 것이다.

이 정의를 이용해서 [1번](#1-search-problems)에서 알아본 문제들을 NP-complete와 P 문제로 분류하면 다음과 같다. 

![PNP][I_17]

[3번](#3-reductions)에서는 왜 이 위의 문제들이 NP-complete인지 증명할 것이다.

또, 이를 통해 사실 NP-complete의 문제들은 근본적으론 모두 같은 문제임을 알아볼 것이다.

이는 만약 이 문제 중 하나만이라도 polynomial time algorithm을 찾아낸다면 $P=NP$임이 증명됨을 의미한다.

찾아내면 졸업 가능 ㅎㅎ

# 3. Reductions

[I_1]: /assets/lecture/algo/np/sat.PNG
[I_2]: /assets/lecture/algo/np/sat_ex.PNG
[I_3]: /assets/lecture/algo/np/sat_def.PNG
[I_4]: /assets/lecture/algo/np/tsp.PNG
[I_5]: /assets/lecture/algo/np/tsp_2.PNG
[I_6]: /assets/lecture/algo/np/euler.PNG
[I_7]: /assets/lecture/algo/np/ham.PNG
[I_8]: /assets/lecture/algo/np/bal.PNG
[I_9]: /assets/lecture/algo/np/ilp.PNG
[I_10]: /assets/lecture/algo/np/three.PNG
[I_11]: /assets/lecture/algo/np/three_ex.PNG
[I_12]: /assets/lecture/algo/np/three_sol.PNG
[I_13]: /assets/lecture/algo/np/ivc.PNG
[I_14]: /assets/lecture/algo/np/lp.PNG
[I_15]: /assets/lecture/algo/np/lp_ex.PNG
[I_16]: /assets/lecture/algo/np/subset.PNG
[I_17]: /assets/lecture/algo/np/pnp.PNG
[I_18]: /assets/lecture/algo/np/pnp_2.PNG