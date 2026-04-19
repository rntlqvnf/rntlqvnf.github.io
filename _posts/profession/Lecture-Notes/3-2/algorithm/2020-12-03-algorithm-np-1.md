---
title: "&#91;Algorithm&#93; NP-complete problmes"
layout: profession-post
categories:
  - Lecture Notes
tags:
  - Algorithm
  - CSED331
use_math: true
toc: true
toc_sticky: false
type: profession
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

> Any proposed solution *S* to an instance *I* can be *quickly checked* for correctness

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

핵심은 모든 점을 방문하고 다시 돌아오는 경로를 구한다는 점이다.

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

균형있는 cut을 찾는 문제이다.

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

어떤 edge $(u,v)\in E$에 대해 $u$ 또는 $v$가 $(u,v)$에 닿을 수 있는 vertex라고 생각하면 된다.

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

이렇게 polynomial time에 given solution을 verity 할 수 있는 문제를 **class NP**로 분류한다.

한편, 위에서 알아보았던 오일러 경로 같이 **문제 자체를 polynomial time에 풀 수 있는** class NP 문제를 **class P**로 분류한다.

여기서 질문.

> polynomial time에 풀 수 없는 search problem이 존재할까?

![PNP][I_18]

다른 말로, $P \neq NP$ 일까? 

이게 바로 한 번쯤은 들어보았을 수학 난제 $P-NP$ 문제이다. 

질문은 간단하지만 참 사악하기 그지없는 문제다. 

난제는 잠시 냅두고, 우리가 알아볼 것은 class NP 안에 존재하는 **NP-hard**와 **NP-complete**라는 소분류이다.

NP-hard의 정의는 NP-complete 이면서 NP에 속하는 문제를 의미하니, NP-complete의 정의를 알아보자.

NP-complete의 정의는 다소 이해하기 어렵다.

> A search problem is NP-complete if all other search problems **reduce** to it

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

Reduce 라는게 formal하게 무슨 뜻인지 알아보자.

![Reud][I_19]

Search problem *A* 에서 search problem *B*로의 **reduction**은 (denote by $A\rightarrow B$) 

*A*의 instance *I*를 **polynomial-time** algorithm *f*를 통해 *B*의 instance *f(I)*로 변형하여 *B*에 대한 solution *S*를 구하고, 

이를 **polynomial-time** algorithm *h*를 통해 원래의 문제 *A*의 solution으로 바꾸거나, 해가 없음을 판별하는 걸 말한다.

정의가 무식하게 길지만 그림을 보면 곧바로 이해가 갈 것이다.

그럼 질문.

> $A\rightarrow B$ 에서 $A$와 $B$ 중 뭐가 더 어려운 문제일까?

당연히 $B$를 풀 수 있어야 $A$가 풀리므로 $B$가 더 어렵다고 할 수 있다. (정확하게는 어렵거나 같다)

또한 reduction에는 **composition**이라는 중요한 성질이 있다.

> If $A\rightarrow B$ and $B\rightarrow C$, then $A\rightarrow C$

NP-complete의 정의를 상기해보자.

> A search problem is NP-complete if all other search problems reduce to it

![Reduce][I_20]

곧, 만약 우리가 $A$가 NP-complete 임을 안다면, $A\rightarrow B$ 임을 증명하기만 하면 $B$ 또한 NP-complete 임을 알 수 있게 된다.

이때 주의할 점은 $B$가 search problem이라는 것이다.

만약 이 가정이 없다면 $B$는 NP-hard 가 된다.

$B$가 search problem, 즉 class NP라는 가정이 있기 때문에 NP-hard + NP = NP-complete 가 된 것이다.

이번 챕터에서는 이를 이용하여 NP-completeness을 증명함을 목적으로 한다.

![Tree][I_21]

이는 우리가 증명할 문제들의 관계도이다. 

위에서부터 차근차근 reduction 해나간다면, 위에 표시된 모든 문제들이 NP-complete 임을 알 수 있다.

PPT 상에서는 순서가 좀 중구난방이어서, 본 포스트에서는 순서대로 서술하도록 하겠다.

본격적으로 증명하기 전에, 간단한 웜업을 해보자.

## Hamiltonian (s,t) path → Hamiltonian Cycle

![Reduction Hamiltonian (s,t) path to Hamiltonian Cycle][I_22]

핵심 아이디어는 $(s, t)$-path를 연장해서 cycle로 만드는 것이다.

$s,t$를 임의의 vertex $x$로 연장하면 문제는 $x$ to $x$ Hamiltonian Cycle 문제가 된다는 것이다.

이때, path to cycle, cycle to path의 전환은 poly-time 안에 이뤄지므로 reduction이 가능하다.

## Any NP problem → SAT

![Tree][I_23]

SAT의 generalization인 Circuit SAT로 reduce 됨을 보일 것이다.

### Circuit SAT, generalization of SAT

우선 Circuit SAT가 왜 generalization of SAT 인지 보이자.

엄밀한 증명은 아니고, 핵심 아이디어를 소개하고 넘어가려고 한다.

![Circuit][I_24]

Circuit SAT는 위와 같이 combinatorial circuit의 sink node가 true가 되도록 하는 assignment를 찾거나, 없음을 보이는 문제이다.

![SAT][I_1]

한편 SAT 은 **CNF** boolean formula가 true가 되도록 하는 해를 찾는 문제이다.

잘 생각해보면 괄호는 OR circuit으로 표현하고, 그 circuit끼리 AND로 연결하여 어떤 CNF라도 combinatorial circuit으로 표현할 수 있음을 알 수 있다.

### Any NP problem → Circuit SAT

![NP to Circuit SAT reduction][I_25]

사실 원래 증명은 이것보다 훨씬 더 길고, 이건 정말 간단히 아이디어만 적어둔 것이다.

그럼에도 불구하고 모든 reduction 중에 가장 어려운 게 아닐까 싶다.

핵심은 NP 문제를 bit로 표현하는 것이다.

찬찬히 읽어보면 이해가 갈 것이다. 근데 내 생각에 딱히 중요하진 않은 것 같다.

## Circuit SAT → 3SAT

![Circuit SAT to 3SAT reduction][I_26]

3SAT는 3개 이하의 literal로 이루어진 괄호만 존재하는 SAT를 말한다.

Circuit SAT를 3SAT로 reduction 하는 것은 어렵지 않다.

![Cir][I_28]

Circuit의 모든 노드를 variable로 생각하자.

![Circuit][I_27]

그리고 각각의 합성 회로를 위와 같이 3SAT의 clause로 변형하면 된다.

각각이 왜 이렇게 변형되는지는 디시설에서 배운 table을 그려보면 증명할 수 있다.

사실 직관적으로도 금방 알 수 있을 것이다.

![Sol][I_29]

그러면 위와 같이 3SAT로 reduce 할 수 있다.

이때, 3SAT의 정의를 exactly 3 literal로 생각한다면, $length<3$인 clauses의 literal을 복제하여 $length=3$ 으로 만들어주면 된다.

근데 상당히 헷갈리는 것이, 책에서는 3SAT를 at most 3 literal로 생각하기 때문에 위 과정이 없다.

조사해본 바에 따르면 보통 exactly 3로 간주하고, at most 3가 약간 3SAT의 아종? 요런 느낌으로 간주되는 것 같다.
## 3SAT → Independent Set

![3SAT to Independent Set Reduction][I_30]

3SAT를 푸는 방법은 각 괄호에서 true로 만들 literal을 하나씩 선택하는 것이다.

이를 그대로 Graph에 적용하면 된다.

![11][I_31]

모든 literal을 vertex로 생각하고, 각 괄호의 literal 끼리 연결시켜주자.

이러면 Independent Set은 각 삼각형에서 하나의 vertex만 선택할 수 있으므로, 3SAT의 조건과 일치하게 된다.

![12][I_32]

또, 각 괄호에서 literal을 선택함에 따라 그와 연동되는 literal들이 자동으로 확정되게 된다.

예를 들어 $x_1$을 선택해 $x_1=true$를 할당했다면, $\overline{x_1}$은 선택할 수 없다.

이 조건을 graph로 표현하기 위해, $x_i$와 $\overline{x_i}$를 연결시켜주자.

이러면 Independent Set은 $x_i$와 $\overline{x_i}$ 중 하나만 선택할 수 있으므로, 3SAT의 조건과 일치하게 된다.

PPT의 설명이 triangle이라고 되어있는 것을 보아 교수님은 3SAT의 정의를 exactly 3 literal로 생각하시고 풀이한 것 같다.

하지만 exactly 3 literal가 아니더라도 동일한 알고리즘이다.

![13][I_33]

보다시피, 3 literal이 아니면 그냥 선 혹은 점으로 표현된다.

## 3SAT → 3D Matching

![3][I_34]

### 3SAT → Constrained 3SAT

3SAT → 3D Matching를 위해서는 우선 3SAT → Constrained 3SAT를 증명해야 한다.

PPT 상에서는 살짝 순서가 꼬여있는데, Constrained 3SAT를 먼저 증명해야 3SAT → 3D Matching에서 예외 케이스가 안생긴다.

Constrained 3SAT란 각 literal이 최대 2번 등장하는 3SAT를 말한다.

이때 literal과 variable은 다르다.

Literal은 $x_i$와 $\overline{x_i}$를 구분하고, variable은 그렇지 않다.

곧, Constrained 3SAT는 $x_i, x_i, x_i$는 안 되지만 $x_i, \overline{x_i},\overline{x_i}$ 는 된다는 것이다.

아래는 간단한 예시이다.

![Allow][I_35]

근데 PPT를 보면 죄다 at most three times라고 되어있다.

교수님이 수업시간에 계속 수정하신 부분인데, twice times가 되어야 한다.

사실 이건 교과서의 정의를 교수님이 살짝 변형하는 과정에서 수정하지 않은 부분이다.

교과서의 정의는 **variable**이 at most three times 등장하는 3SAT를 Constrained 3SAT라고 하기 때문이다.

이걸 읽을 당시에 너무 헷갈려서 메일을 통해 교수님께 질문해본 결과, 둘 다 맞다고는 한다.

근데 그래도 좀 의아한 부분이 많아서, 내 나름대로 옳다고 생각하는 방향으로 적고자 한다. 에러가 있으면 댓글로 알려주길 바란다.

![Cons][I_36]

증명은 간단하다.

3번 **이상** 등장하는 literal $x$를 각각 $x_1, x_2, \cdots, x_k$로 치환환다.

이때 각 literal $\bar{x}$는 $\bar{x_1}, \bar{x_2}, \cdots, \bar{x_j}$ 로 치환한다.

![EQ][I_37]

그리고 위 수식을 추가하여, $x_1, x_2, \cdots, x_k$가 모두 같다는 조건을 넣어준다.

왜 위 수식이 $x_1=x_2=\cdots=x_k$를 강제하는지는 생각해보면 꽤 당연하다. 하나라도 같지 않으면 무조건 false인 괄호가 등장하기 때문이다.

곧, 위와 같은 변형을 통해 3SAT를 Constrained 3SAT로 바꿀 수 있다.

### Constrained 3SAT → 3D Matching

아마 증명 중 가장 희안한 증명이 아닐까 싶다.

이런 아이디어를 대체 어떻게 생각해냈을까 싶을 정도다.

Constrained 3SAT가 3D Matching로 reduce됨을 보이려면, Constrained 3SAT를 변형해서 만든 3D Matching 문제가 perfect matching 일 때 Constrained 3SAT의 해가 존재함을 보이면 된다.

![Gadget][I_38]

우선 각 **variable**에 대해 위와 같은 gadget(unit of graph)를 그려주자 

이 Gadget의 boy-girl 을 matching 할 수 있는 방법은 뭐가 있을까?

![Boolean of gadget][I_39]

이렇게 두 가지 경우가 존재하므로 이를 boolean variable의 동작과 mapping 시킬 수 있다.

이제 한 clause 에 대해 gadget들을 그려보자.

각 **variable**마다 하나씩 gadget을 그려주고, boy-girl pair를 하나 추가로 그려줄 것이다.

![Clause gadget][I_40]

3SAT를 푸는 방법은 각 괄호에서 true로 만들 literal을 하나씩 선택하는 것이다.

이를 gadget으로 표현하려는 것이다.

모든 gadget을 clause의 boy-girl pair와 엮어주자.

이때, 어떤 pet과 연결하는지가 꽤 중요하다.

![Boolean of gadget][I_39]

$x_i$의 형태인 경우 (상 하) 두개의 pet 중 하나와 연결하고, $\overline{x_i}$의 형태인 경우 (좌 우) 두개의 pet 중 하나의 연결한다.

이유인 즉슨, $x_i$를 고르게 되면 그림처럼 (좌 우) pet이 matching이 될 것이고, 남은 pet이 (상 하) 두개의 pet이기 때문이다.

곧, 어떤 clause $C_j$에서 $x_i$를 선택하는 걸

1. $x_i$를 골라 (좌 우) pet을 matching 시키고
2. $C_j$ pair을 $x_i$의 (상 하) pet 중 하나와 matching 시키는 것

으로 표현한다는 것이다.

$\overline{x_i}$를 선택할 경우는 (좌 우)와 (상 하)를 바꿔서 생각하면 된다.

이때 pet이 (좌 우) 2개, (상 하) 2개만 존재해도 되는 이유가 뭘까?

Constrained 3SAT이므로 **각 literal이 최대 2번 등장**하기 때문이다. 

이 때문에 그냥 3SAT에서 3D Matching으로 reduction 하지 않고, Constrained 3SAT로 변형하는 중간 과정을 거친 것이다.

![Cla2][I_41]

여하튼, 위 과정을 모든 clauses에 대해 수행하면 위와 같은 graph가 나온다.

이 graph가 가지는 의미를 이해하겠는가?

$x_1$을 간단히 예로 들어보면, $x_1$을 선택했다고 하자.

그러면 (좌 우) pet이 gadget의 boy-girl pair와 matching 되고, $C_2$의 boy-girl pair와 상단 pet이 matching 된다.

이로 인해 $x_1$ gadget은 $C_1$와 $C_3$의 boy-girl pair와 matching할 수 없게 되고, 곧 $\overline{x_1}$을 선택할 수 없게 된다.

곧, 위 graph는 **각 괄호에서 true로 만들 literal을 하나씩 선택**해야 하며, **충돌하는 선택은 할 수 없다**는 걸 표현하고 있다.

한편, 위에서 주장하길 3D matching이 **perfect matching**이 되어야 3SAT의 solution이 존재한다고 했다.

그러나 위의 그래프 만으로는 perfect matching을 만들 수 없다. 

아무 곳과도 연결되지 않은 pet들이 존재하기 때문이다.

![CLA 3][I_42]

따라서 이 남은 pet들을 matching 시켜주는 **generic animal lover couple**을 만들어서, 모든 pet과 연결해준다.

Generic animal lover couple이라는 말이 참 웃긴데, 어떤 'pet'과도 match 될 수 있는 boy-girl pair를 의미한다.

그럼 이 Generic animal lover couple이 몇개나 필요할까?

다시 말해, 남은 pet들이 몇개일까?

Variable의 수를 $n$이라고 하고, equation의 수를 $m$이라 하자.

각 Variable에 값을 할당하고 (true, false 선택) 남는 pet의 수는 $2n$개이고, 이 중 $m$개는 equation의 pair과 연결되므로 총 $2n-m$개의 pet이 남는다.

곧, 이 $2n-m$개 Generic animal lover couple들을 추가하여, 이들과 남은 pet들을 하나씩 수거하게 함으로써 perfect matching을 만드는 것이다.

위 그래프에서는 하나의 Generic animal lover couple에 대해서만 표시해두었다.

Full graph는 $2n-m$개의 Generic animal lover couple에 대해 표시해주어야 한다.

## Independent Set → Vertex Cover & Clique

![5][I_43]

1번에서 3개의 문제는 사실 동일하다고 했던 게 기억나는가?

이제 그걸 증명할 차례이다.

혹시 각 문제가 기억이 안난다면 [바로가기](#independent-set-vertex-cover-and-clique)에서 다시 공부하고 오자.

### Independent Set → Vertex Cover

![Indepndent set to vertex cover reduction][I_44]

두 문제의 정의를 잘 떠올려보면 매우 당연하다.

Indp set은 edge가 없는 set이므로, Indp set의 여집합은 edge를 모두 포함한 set일 것이다.

반대로 vertex cover은 모든 edge를 커버하는 set이므로, 여집합은 edge가 없는 set일 것이다.

위의 증명은 이걸 좀 더 formal 하게 썼을 뿐이다.

### Independent Set → Clique

![Indepndent set to clique reduction][I_45]

이것도 매우 당연하다.

Indp set은 두 vertex를 연결하는 그 어떤 edge도 존재하지 않는 set이므로, 이를 뒤집으면 그 두 vertex를 연결하는 그 어떤 edge도 **존재하는** set, 곧 clique를 구할 수 있다.

이의 역도 마찬가지이다.

위의 증명은 이걸 좀 더 formal 하게 썼을 뿐이다.

## 3D Matching → ZOE

![3D matching to ZOE reduction zero one equation][I_46]

우선 ZOE의 정의부터 알아보자.

![ZOE 란][I_47]

ZOE, zero one equation이란 위와 같이 zero one matrix(entry가 0 또는 1인 matrix) $A$에 대해, $Ax=1$ 형태의 방정식을 말한다.

곧, ZOE의 해 $x$ vector는 1 또는 0으로 이루어진 vector가 된다.

3D matching to ZOE는 그닥 어렵지 않다.

![3d to ZOE][I_48]

모든 가능한 $n$개의 triple에 대해 각각 $x_1,\cdots,x_n$이라는 variable을 부여하자.

이때, $x_i=0$이라는 말은 해당 triple을 선택하지 않았다는 의미이고, $x_i=1$이라는 말은 해당 triple을 선택했다는 의미가 된다.

그러면, 각각의 boy, girl, pet은 한 번만 선택 될 수 있으므로 각각이 포함되는 triple variable을 모두 더하면 1이 나와야 한다.

이 조건을 잘 조작하면 zero one matrix의 형태로 만들 수 있다.

곧, 3D Matching 문제는 zero one matrix $A$에 대해 $Ax=1$을 만족하는 zero one vector $x$를 찾는 ZOE 문제가 된다.

## ZOE → Hamiltonian Cycle

![ZOE to Hamiltonian Cycle reduction][I_49]

이를 증명하기 위해서는 두 단계를 거쳐야 한다.

참고로 Rudrata cycle은 Hamiltonian Cycle랑 똑같은 말이다.

### ZOE → Hamiltonian Cycle with Paired Edges

![Hamiltonian Cycle with Paired Edges][I_50]

Hamiltonian Cycle with Paired Edges란, 주어진 pair 중 하나의 edge만 지나는 Hamiltonian Cycle이다.

이때, pair끼리 disjoint하지 않고 얼마든지 중복되게 edge를 선택 할 수 있음에 유념하자.

간단한 예제를 통해 이해도를 높여보자.

![Pair ex][I_51]

ZOE → Hamiltonian Cycle with Paired Edges는 좀 아이디어가 신박하다.

ZOE의 해는 어떻게 구할 수 있을까?

우선 ZOE는 궁극적으로 각 varaible $x_i$에 0을 할당할지, 1을 할당할지 고르는 것이다.

다만 이 선택이 각 equation $x_i+x_{i+1}+\cdots=1$를 만족하도록, 각 equation에서 단 하나의 variable만 1이 되도록 해야 한다는 것이다.

이를 Hamiltonian Cycle with Paired Edges의 문제로 번역해보자.

![ZOE HAM][I_52]

각 variable에 값을 할당하는 것을 두 개의 edge 중 하나의 경로를 고르는 것으로 생각한다.

곧, 하나의 경로는 $x_i=1$을 의미하고, 다른 하나의 경로는 $x_i=0$을 의미하는 것이다.

또한, 각 equation $x_1+x_{2}+\cdots+x_t=1$을 만족하게 하는 단 하나의 variable을 선택하는 것을 $t$개의 경로 중 하나를 고르는 것으로 생각한다.

곧, 각 경로는 equation을 이루는 variable을 의미한다.

이때, variable part에서 $x_i=1$의 경로를 탔다고 해보자. 

그러면 $x_i$를 포함하는 equation은 반드시 $x_i$와 대응하는 edge를 지나야 한다. 

이걸 paring으로 표현하는 것이 핵심이다.

잘 생각해보면, $x_i=0$ edge와 equation $\gamma$에서 $x_i$와 대응하는 edge를 pair 함으로써 위 조건을 표현할 수 있다.

만약 $x_i=0$ edge를 고른다면, 이와 pair인 $x_i$ edge in equation은 타지 않게 된다.

반대로 $x_i=1$ edge를 고른다면, Hamiltonian Cycle은 같은 vertex를 두 번 방문하지 않으므로 자연스레 $x_i=0$ edge를 타지 않게 되고, 이에 따라 pair인 $x_i$ edge in equation을 타게 된다.

### Hamiltonian Cycle with Paired Edges → Hamiltonian Cycle

수업 시간에 꽤 질문이 많이 나왔을 만큼 신박하고 어려운 증명이다.

근데 알고보면 별로 안어렵다.

![Hamiltonian Cycle with Paired Edges to Hamiltonian Cycle reduction][I_53]

Hamiltonian Cycle with Paired Edges의 instance $(G=(V,E),C)$를 Hamiltonian Cycle의 instance $G^{\backprime}=(V^{\backprime},E^{\backprime})$으로 바꿔보자.

다시 말해, Hamiltonian Cycle with Paired Edges을 기반으로 Hamiltonian Cycle의 그래프를 만들어낸다는 것이다.

각 pair에 대해, 우측과 같은 gadget을 만들어주자.

(abcd) 사이에 있는 vertex들은 gadget의 vertex 이외에 어떤 것과도 연결되지 않는다.

이런 식으로 gadget을 잡으면 놀랍게도 a to b, c to d (or reverse)를 강제할 수 있다.

왜인지 알아보자.

![A gadget][I_54]

a를 탐색했다고 하자.

그러면 일단 1로 이동할 것이다.

그럼 여기서 4로 움직일까 2로 움직일까?

잊지 말자. Hamiltonian Cycle은 각 vertex를 딱 한 번 방문하는 cycle을 찾는 것이다.

만약 4로 가게 되면, 절대로 2를 방문할 수 없게 된다.

왜냐면 2의 degree가 2이기 때문에, 이후에 c → 3 → 2를 방문해서 빠져나올 경로가 없기 때문이다.

따라서 a → 1 → 2 → 3의 경로로 움직이게 된다.

그럼 이제 c로 움직일까 6으로 움직일까?

c로 움직이게 되면, 5 또는 8 또는 11을 방문할 수 없게 된다.

c로 움직였다고 가정해보자.

![Exception][I_55]

그러면 위와 같이 5, 8, 11를 모두 지나서 들어올 수 없게 된다.

![Why 6][I_56]

이와 같은 이유로, 이같은 gadget을 만들 수 없다.

수업 시간에 이 질문이 들어왔었는데, 교수님도 순간 당황해서 대답을 못했었다. PPT가 잘못됐나 보다라고 말하시다가! 나중에 아니라는 걸 깨달으셨다.

나도 처음엔 PPT가 잘못된 줄 알았었다. 설명을 들어도 사실 잘 이해가 안갔었는데, 공부하다보니까 당연한 소리였다 ㅋㅋ..

여하튼, 따라서 a → 1 → 2 → 3 → 6의 경로로 움직이게 된다.

이후에는 쭉 동알힌 이유에 의해 a → 1 → 2 → 3 → 6 → 5 → 4 → 7 → 8 → 9 → 12 → 11 → 10 → b 로 움직이게 된다.

c to d 의 경로도 마찬가지이다.

곧, 이런 방식으로 Hamiltonian Cycle에 paring을 구현할 수 있는 것이다.

여기서 질문.

> $ad$가 다른 pair에도 소속되어 있으면 어떻게 표현할까?

다시 말해, $(ab,cd)$ pair 이외에 $(ab, gh)$ pair가 존재하면 어떻게 gadget을 만들어야 할까?

![F][I_57]

이미 만들어진 gadget에서, a와 가장 인접한 vertex $f$가 이루는 edge $(a,f)$를 기준으로 gadget을 만들어준다.

곧, $(af, gh)$로 gadget을 만들어주면 된다.

왤까? 생각해보면 당연한데, $f$를 타는 순간 a to b 경로를 무조건 타야 하기 때문에 자연스럽게 $(ab,cd)$ pair 조건을 충족시킬 수 있기 때문이다.

## ZOE → ILP

![ZOE to ILP reduction][I_58]

생각해보면 너무 당연하다.

ILP가 ZOE의 generalization이기 때문이다.

![ZOE ILP][I_59]

그냥 각 variable들에 대한 조건 $x_i \leq 0$, $-x_i\leq 0$만 추가해주면 ILP 가 된다.

이때 굳이 음수를 씌운 이유는 부등호 방향을 일치시키기 위해서이다.

그러면 해당 ILP가 해를 가질 때에만 ZOE도 해를 가지게 된다.

여기서 알아가야 하는 한 가지 중요한 property가 있다.

> If Problem A is a generalization of a NP-complete problem, then A is also NP-complete

여기서 ILP가 ZOE의 generalization이고, ZOE가 NP-complete problem이기 때문에 자연스럽게 ILP도 NP-Complete가 된다는 것이다.

## Hamiltonian Cycle → TSP

![HAM][I_60]

드디어 PPT의 마지막이다.

최종보스지만 매우 간단하다. 토이카식 전개 같다 ㅋㅋ

![TSP SOL][I_61]

TSP는 가장 저비용으로 모든 vertex를 방문하는 cycle을 찾는 문제다.

Hamiltonian Cycle에 저비용이라는 조건만 추가하면 된다.

원래 존재하던 edge에는 1의 비용을 주고, 존재하지 않던 edge에는 2의 비용을 준 다음 TSP를 돌려보자.

TSP의 solution의 cost가 $n$이면 원래 있던 edge만 사용해서 방문한 것이므로 Hamiltonian Cycle을 찾은 것이 된다.

## ZOE → Subset Sum

![Subset Sum ZOE reduction][I_62]

PPT에는 없지만 얘 혼자 증명 안하기에는 뭐해서 내가 따로 공부해서 넣었다.

근데 되게 간단하다.

응선대를 조금이라도 공부해봤다면, $Ax=b$ 형태의 수식은 사실 column의 합을 표현한다는 것을 알 것이다.

![A][I_63]

곧, 위의 zero one matrix가 주어졌을 때, ZOE $Ax=1$은 사실 column을 더해서 binary integer $11111_2 = 31$을 만드는 문제라는 것이다.

이때 $x_i=1$은 $i$번째 column을 더한다는 의미고 $x_i=0$은 $i$번째 column을 더하지 않는다는 의미가 된다.

그러면 이 문제를 subset sum 문제로 바꿔보면

> $10010_2 = 18, 00101_2 = 5, 00100_2 = 4, 01000_2 = 8$ 이 4개의 integer를 가지고 $11111_2 = 31$를 만들어라!

가 된다.

참 쉽죠?

응선대 얘기를 하니 2학년 때의 기억이 떠오른다.

내 주변에 있던 후배 두 명이 시험이 90점대였고(아마 1, 2등이 아니었나 싶다), 나는 80점이어서 아.. A+은 못받겠구나 했는데 받아서 상당히 황당했던 기억이 있다.

아직도 의문이다. 그렇게 1등이랑 점수 차가 많이 나는데 A+을 어떻게 받을 수 있었을까? 흠.. 

교수님이 천사이신가 보다 ㅎㅎ

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
[I_19]: /assets/lecture/algo/np/red.PNG
[I_20]: /assets/lecture/algo/np/reduce.PNG
[I_21]: /assets/lecture/algo/np/tree.PNG
[I_22]: /assets/lecture/algo/np/ham_2.PNG
[I_23]: /assets/lecture/algo/np/1.PNG
[I_24]: /assets/lecture/algo/np/circut.PNG
[I_25]: /assets/lecture/algo/np/circuit_np.PNG
[I_26]: /assets/lecture/algo/np/2.PNG
[I_27]: /assets/lecture/algo/np/circuit_to_sat.PNG
[I_28]: /assets/lecture/algo/np/cir.PNG
[I_29]: /assets/lecture/algo/np/cir_sol.PNG
[I_30]: /assets/lecture/algo/np/3.PNG
[I_31]: /assets/lecture/algo/np/11.PNG
[I_32]: /assets/lecture/algo/np/12.PNG
[I_33]: /assets/lecture/algo/np/13.PNG
[I_34]: /assets/lecture/algo/np/4.PNG
[I_35]: /assets/lecture/algo/np/allow.PNG
[I_36]: /assets/lecture/algo/np/cons.PNG
[I_37]: /assets/lecture/algo/np/eq.PNG
[I_38]: /assets/lecture/algo/np/gadget.PNG
[I_39]: /assets/lecture/algo/np/gad.PNG
[I_40]: /assets/lecture/algo/np/cla.PNG
[I_41]: /assets/lecture/algo/np/cla_2.PNG
[I_42]: /assets/lecture/algo/np/cla_3.PNG
[I_43]: /assets/lecture/algo/np/5.PNG
[I_44]: /assets/lecture/algo/np/vc.PNG
[I_45]: /assets/lecture/algo/np/cli.PNG
[I_46]: /assets/lecture/algo/np/6.PNG
[I_47]: /assets/lecture/algo/np/zoe.PNG
[I_48]: /assets/lecture/algo/np/zoe_3d.PNG
[I_49]: /assets/lecture/algo/np/rud.PNG
[I_50]: /assets/lecture/algo/np/pair.PNG
[I_51]: /assets/lecture/algo/np/pair_ex.PNG
[I_52]: /assets/lecture/algo/np/zoe_ham.PNG
[I_53]: /assets/lecture/algo/np/pair_gadget.PNG
[I_54]: /assets/lecture/algo/np/a_gadget.PNG
[I_55]: /assets/lecture/algo/np/exception.PNG
[I_56]: /assets/lecture/algo/np/why6.PNG
[I_57]: /assets/lecture/algo/np/f.PNG
[I_58]: /assets/lecture/algo/np/ilp_zeo.PNG
[I_59]: /assets/lecture/algo/np/ilp_zoe.PNG
[I_60]: /assets/lecture/algo/np/ham_tsp.PNG
[I_61]: /assets/lecture/algo/np/tsp_sol.PNG
[I_62]: /assets/lecture/algo/np/subset_Sum.PNG
[I_63]: /assets/lecture/algo/np/A.PNG