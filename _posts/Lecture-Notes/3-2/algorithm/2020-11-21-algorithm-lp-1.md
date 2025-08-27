---
title: "&#91;Algorithm&#93; Linear Programming (1)"
layout: kb-post
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

![Overview of LP][I_1]

이번 시간에는 Linear programming에 대해서 배운다.

# 1. Linear Programming 이란?

**Linear Programming**이란 linear constraints에 대해, linear objective function을 최대화, 혹은 최소화 하는 해를 찾는 문제를 말한다.

이떄 linear constraint란 linear equation 또는 linear inequalities를 의미한다.

또한 linear objective fuction이란 찾아낸 해를 평가하는 함수라고 생각하면 된다.

예를 들어 LOF가 $100x+y$라면 $x=1, y=2$는 $102$으로 평가되고, $x=3, y=1$은 $301$으로 평가되는 식이다.

어떤 문제를 linear programming으로 풀려면 일련의 과정을 거쳐야 한다.

1. Variable을 정의한다.
2. Constraints(linear equations, linear inequalities)를 정의한다.
3. Linear object function을 정의한다.
4. Linear object function을 최대화 또는 최소화 하는 variable-value pair를 찾는다.

딱딱한 정의만 놓고 보면 뭔가 어려워보이는데 사실 중학교 때 해봤던거다.

## Example (중학교 ver)

![Middle school ver example][I_2]

위에서 설명한 과정대로 풀어보자.

1. Variable을 정의한다.
   
   - $x_1$: POSPEN 개수
   - $x_2$: POSPEN Gold 개수

2. Constraints(linear equations, linear inequalities)를 정의한다.
   
   - $x_1,x_2\geq 0$
   - $x_1\leq 200$, $x_2\leq 300$
   - $x_1+x_2\leq 400$

3. Linear object function을 정의한다.
   
   - $x_1+6x_2$

4. Linear object function을 최대화 또는 최소화 하는 variable-value pair를 찾는다.
   
   중학교 때 했던 방식대로, 그래프를 그려보자.

   ![Feasible regin graph][I_3]

   제약 조건에 대해서 그래프를 전부 그려보면 위와 같은 feasible region이 나온다.

   이제 해를 찾기 위해, $x_1+6_x2=c$ 그래프를 움직이며 해를 최대화하는 $c$를 찾아보자.

   ![Example graph 2][I_4]

   이로부터 우리는 해 $x_1=100, x_2=300$를 얻는다.

뭔가 중학교 때 기억이 새록새록나지 않는가?

모든 LP 문제가 이렇게 쉬우면 좋을텐데, 과제를 보면 한숨만 나온다.. 하..

## Simplex method 소개

위의 예제와 같이 그래프를 그려서 푸는 방식을 **Graphical Solution Method** 라고 한다.

그런데 아무래도 이 방법은 잘 쓰이지 않는다.

대신 **Simplex Method**를 많이 사용한다.

![Simplex method][I_5]

**Simplex method**는 근본적으로는 graphcial method와 다르지 않다. 다만 대수적으로 접근할 뿐이다.

Simplex method는 LP 문제의 해는 Feasible region의 꼭짓점에 존재한다는 사실을 이용한다.

그래서 그냥 반복적으로 꼭짓점을 탐색할 뿐이다. 구체적인 과정은 이렇다.

1. 특정 꼭짓점에서 시작한다
2. 더 좋은 꼭짓점으로 이동한다
3. 더 좋은 꼭짓점이 없을 때 종료한다 (Local optimal vertex = global optimal vertex)

이걸 하는 방법에 대해서는 [4. Simplex Method](#4-simplex-method)에서 배운다.

지금은 그냥 이런게 있구나~ 정도만 알고 있으면 된다.

# 2. Flows in Network

이번에 풀어볼 문제는 아주 유명한 최대 흐름 문제(maximum flow problem)이다.

문제는 아주 간단하다.

![Digraph][I_6]

위와 같은 directed graph가 주어졌다고 해보자.

각각의 edge는 수용할 수 있는 최대 용량이 존재한다고 하자.

이때 source *s*에서 sink *t*로 보낼 수 있는 최대 흐름 용량을 구하여라.

간단히 비유하자면, 그래프는 상수도 네트워크고, 흐름은 물이라 생각할 수 있다. 곧 문제는 한 지점에서 다른 지점으로 어떻게 해야 최대한의 물을 보낼 수 있는가를 묻는 것이 된다.

![Result][I_7]

결과적으로는 이렇게 흐름을 할당하면 된다.

이제 이 문제를 LP로 풀어보자.

1. Variable을 정의한다.
   
   문제 정의
   - Given directed graph $G=(V<E)$.
   - Two special nodes $s,t\in V$, a **source** and a **sink** of G, respectively; and
   - capacities $c_e >0$

   Variable 정의
   - $f_{uv}$ (flow from *u* to *v*)

2. Constraints(linear equations, linear inequalities)를 정의한다.
   
   - 용량의 제한
      - $0\leq f_e \leq c_e \forall e\in E$
   - 유량의 보존
      - $\sum_{(w,u)\in E}f_{wu}=\sum_{(u,z)\in E}f_{uz}$ 

3. Linear object function을 정의한다.
   
   - $size(f)=\sum_{(s,u)\in E}f_{su}=\sum_{(u,t)\in E}f_{vt}$
   - maximize $size(f)$

4. Linear object function을 최대화 또는 최소화 하는 variable-value pair를 찾는다.
   
   Simplex method의 아이디어를 이용해서 푼다.

   Simplex의 핵심은 더 이동할 수 없을 때까지 더 좋은 vertex로 계속 이동하는 것이다.

   이를 문제에 적용해서, max flow를 더 증가시킬 수 없을 때까지 증가시켜보자. 구체적으로는 다음과 같다.

   1. Start with zero flow
   
   2. Repeat: choose an appropriate path from *s* to *t*, and increase flow along the edges of this path as much as possible

   아래는 위 알고리즘의 간단한 예시이다.

   ![Simple example][I_8]

   (a)의 graph가 주어졌을 때, 두 번의 반복 (b), (c)를 거치면 더 이상 max flow를 증가시킬 수 없다.

   따라서 최종적으로 (d) 그래프를 얻고, max flow는 2가 된다.

   그런데 이 알고리즘은 완벽하지 않다.

   ![Imperfect][I_9]

   만약 첫 번째 탐색에서 (e) 경로를 찾았다고 하자.

   그러면 더 이상 추가 경로를 탐색할 수 없으므로, 알고리즘은 종료되어버리고 잘못된 답을 얻는다.

   이를 방지하기 위해 **cancel existing flow path** 개념을 도입해야 한다.

   Cancel exisiting flow path란, 이전에 할당한 flow를 일부 되돌리는 path를 의미한다.

   위의 그림에서 (f) 경로의 (b,a) edge는 기존 그래프 (a)에서 존재하지 않던 edge이지만, 기존에 (e)가 할당한 (a,b) flow를 되돌리는 역할로써 존재한다.

   이 개념을 도입하면 (f) 경로까지 무사히 탐색할 수 있고, 올바른 답을 얻는다.

   정리하자면, simplex method는 두 가지 타입의 edge를 모두 고려해서 경로를 탐색한다.

   1. $(u,v)$ is in the original network, and is not yet at full capacity
   
   2. $(v,u)$ is the reverse edge of $(v,u)$

   이때, reverse edge의 capacity는 어떻게 정해야 할까?

   Reverse edge가 유효해지는 건 original edge에 flow가 할당되었을 때이다.
   
   또한, reverse edge가 되돌릴 수 있는 최대 flow는 **original edge에 할당된 flow**만큼이다.

   따라서, reverse edge의 capacity는 original edge의 flow가 된다.

   ![Reverse flow][I_10]

   이런 식으로 edge capacity를 변경해가며 만드는 graph를 **residual graph(잔여 그래프)**라고 한다.

   아래는 간단한 예제이다.

   ![1][I_11]

   첫 번째 탐색에서 *scbdet* 경로를 찾았다.

   이때 이 경로에서 흐를 수 있는 최대 flow는 (b,d)에 의해 1이다.

   따라서 *scbdet* 경로의 original edge들은 1을 감소시키고, reverse edge는 1을 증가시키자.

   ![2][I_12]

   residual graph가 왜 이렇게 바뀌었는지 이해가 가야한다.

   두 번째 탐색에서 *sabt* 경로를 찾았다. 

   이때 이 경로에서 흐를 수 있는 최대 flow는 (a,b)에 의해 1이다.

   따라서 *sabt* 경로의 original edge들은 1을 감소시키고, reverse edge는 1을 증가시키자.

   ![3][I_13]

   residual graph가 왜 이렇게 바뀌었는지 이해가 가야한다.

   세 번째 탐색에서 *sdet* 경로를 찾았다. 

   이때 이 경로에서 흐를 수 있는 최대 flow는 (s,d)에 의해 1이다.

   따라서 *sdet* 경로의 original edge들은 4를 감소시키고, reverse edge는 4를 증가시키자.

   ![4][I_14]

   residual graph가 왜 이렇게 바뀌었는지 이해가 가야한다.

   네 번째 탐색에서 *sacbt* 경로를 찾았다. 

   이때 이 경로에서 흐를 수 있는 최대 flow는 (c,b) 또는 (b,t)에 의해 1이다.

   따라서 *sacbt* 경로의 original edge들은 1을 감소시키고, reverse edge는 1을 증가시키자.

   ![5][I_15]

   residual graph가 왜 이렇게 바뀌었는지 이해가 가야한다.

   이제 더 이상 경로가 없다.

   따라서 좌측의 current flow가 최종 flow graph가 되고, max flow(*size(f)*)는 7이 된다.

## Max-flow Min-cut Theorem

이제 optimality를 증명할 차례이다.

사실 이 theorem이 이 문제에서 배우고자 하는 핵심이다.

Maximum flow problem을 다른 방식의 LP로 풀 수 있을까?

![Anthoer][I_16]

생각해보면 우리가 구한 해는 위와 같이 두 그룹으로 나눴을 때, 두 그룹 사이에 흐르는 flow와 같다.

여기서 아이디어를 얻어서 새로운 LP를 정의해보자.

variable과 contraints는 동일하고, objective function을 다시 정의해보자.

- $(s,t)\- cut$: A cut which partitions the vertices into two disjoint groups $L$ and $R$ such that $s\in L$ and $t\in R$.

- $capacity(L,R)$: flow across two group

새로운 LP의 objective function $capacity(L,R)$과 원래 LP의 objective function $size(f)$는 어떤 관계를 가질까?

잘 생각해보면 보존 법칙에 의해 아래의 관계가 성립함을 알 수 있다.

- Pick any flow $f$ and any $(s,t)\- cut\;(L,R)$. Then $size(f)\leq capacity(L,R)$

그러면, 새로운 LP의 목표를 **minimize $capacity(L,R)$**로 두면, 원래 LP의 해를 근사할 수 있지 않을까?

놀랍게도 실제로는 **두 LP의 해는 같다!**

이렇게 min cappacity of $(s,t)\- cut$와 max flow of network가 같다는 theorem이 바로 **Max-flow min-cut theroem**이다.

눈치채야 할 것은 이런 식으로 두 LP가 상호적인 관계를 이루는(max solution = min solution) 경우가 존재한다는 것이다.

나중에 배우겠지만, 이런 LP의 쌍은 항상 존재하며 이 성질을 duality라고 한다. (위 경우는 weak duality)

아래는 Max-flow min-cut theroem의 증명이다. 별로 어렵진 않으니 그냥 쓱 보고 가면 된다.

### Proof of Max-flow Min-cut Theorem

증명의 핵심은 $size(f)=capacity(L,R)$인 $(s,t)\- cut$이 존재함을 보이는 것이다.

이 cut의 존재성을 보이면, 이보다 작은 capacity를 가지는 cut은 존재할 수 없으므로($\because size(f)\leq capacity(L,R)$) 자연스레 min cut 임이 증명되고, 두 LP의 해가 같음도 증명된다.

cut $(L,R)$을 아래와 같이 잡아보자.

- $L$ = residual graph $G^f$에서 $s$에서 도달가능한 노드들의 집합
- $R$ = 나머지 ($R=V-L$)

예를 들어, 우리가 위에서 본 예제에서 $(L,R)$은 다음과 같이 구할 수 있다.

![FInal res][I_18]

- $L$: $s$에서 도달가능한 노드 집합 $s,a,c$

- $R$: 나머지 $b,d,e,t$

따라서 cut은 아래와 같이 된다.

![Cut][I_20]

이때 이 cut은 원래의 그래프 $G$에 대한 것이다.

Residual network는 계산하는데 쓰인 거고, cut은 원래의 그래프 상에서의 cut이라고 생각해야 헷갈리지 않는다.

이제 $size(f)=capacity(L,R)$ 임을 보이자.

![Proof][I_19]

*L*에서 *R*로 가는 edge $e\in E$의 flow는 얼마일까?

Residual network에서 *L*에서 *R*로 가는 edge가 없다는 의미는 $e$가 포화상태($f_e = c_e$)라는 의미이다.

다시 말해, 모든 capacity를 채웠기 때문에 남은 capacity가 0이 되어서 original edge $e$ (L to R)는 사라지고, reverse edge (R to L)만 존재한다는 의미이다.

따라서 *L*에서 *R*로 가는 edge $e\in E$의 flow는 그 용량과 동일하다. ($f_e = c_e$)

그럼 *R*에서 *L*로 가는 edge $e^{\backprime} \in E$의 flow는 얼마일까?

Residual network에서 *L*에서 *R*로 가는 edge가 없다는 의미는 $e^{\backprime}$에 flow가 없다는 의미이다.

만약 flow가 있다고 해보자. 그러면 그에 따른 reverse edge가 생긴다. 이 말은 residual network에서 *L*에서 *R*로 가는 edge가 생긴다는 의미이고, 이는 모순이다.

따라서 $f_{e^{\backprime}}=0$이다.

보존 법칙에 의해서 $\sum f_e=size(f)$이므로, $size(f)=capacity(L,R)$이다.

이때, $size(f)\leq capacity(L,R)$이므로 이보다 작은 capacity를 가지는 cut은 존재할 수 없다.

따라서 우리가 구한 cut은 min cut이 된다.

곧, maximum flow in a network는 capacity of smallest $(s,t)\- cut$이 된다.

# 3. Duality

위에서 살펴봤듯이, duality란 어떤 LP에 대해 그에 상응하는 또다른 LP가 존재하는 성질을 의미한다.

1번에서 알아봤던 POSPEN LP의 dual을 구해보자.

![POPSNE][I_21]

아이디어는 inequality들을 적절히 상수배를 해서 더해 objective function의 꼴을 만드는 것이다.

![Formula 1][I_22]

각각의 상수배를 $y_1, y_2, y_3$라 하자.

모든 inequality를 더하면 아래와 같이 나온다.

![For 2][I_23]

우리가 원하는 것은 objective function의 꼴이다.

단순히 $y_1+y_3=1,y_2+y_3=6$로 놓기 보다는, LP를 만드는 것이므로 제약 조건을 만들기 위해 upper bound로 만들어보자.

$x_1+6x_2 \leq (y_1+y_3)x_1+(y_2+y_3)x_2 \leq 200y_1+300y_2+400y_3$

따라서 아래와 같은 새로운 LP를 얻을 수 있다.

![NEW][I_25]

참고로 각 $y$가 모두 0 이상이라는 조건은 부등식의 방향을 바꾸지 않기 위해 나온 조건이다.

여하튼 이렇게 새롭게 만들어진 LP를 **dual LP**라 하고, 원래의 LP를 **primal LP**라고 한다.

곧, dual LP의 any feasible value는 primal LP의 upper bound가 된다.

그러면, 만약 primal LP의 feasible value와 dual LP의 feasible value가 일치하는 어떤 pair을 찾을 수 있다면 그 feasible value는 optimal value가 된다!

아래는 그 pair이다.

![Pair][I_26]

두 LP모두 1900의 값을 가지므로, 우리는 1900이 optimal value라 생각할 수 있다.

곧, 서로가 서로의 optimality를 보장하는 것이다.

![DUal][I_27]

놀라운 점은 특정 LP가 아닌, **모든 LP에 대해서 그에 대응하는 dual을 찾을 수 있다**는 점이다.

Dual LP의 일반식은 다음과 같다.

![General form of DUal LP][I_28]

선형대수를 배우지 않았더라도 이정도는 아마 이해할 수 있을 것이다.

그래도 이해를 위해 풀어쓴 버전의 수식도 같이 첨부한다.

![Tow general form][I_29]

우리가 이제까지 해온 과정을 한 문장으로 정리하자면 아래와 같다.

> **Duality theorem**
>> If a linear program has a bounded optimum, then so does its dual, and the
two optimum values coincide.

## Max Flow and Min Cut

우리가 2번에서 했던 max flow에 대해서도 dual form을 구할 수 있다.

![Max dual][I_30]

근데 왜 이렇게 되는지 직관적으로 이해는 가는데, 세세하게 뜯어보면 의문점이 많다.

교과서에도 없고 하니.. 그냥 느낌만 알고 가도 되지 않을까?

## Proof of Duality Theorem

이해 불가.. 책에도 없다 ㅠㅠ

# 4. Simplex Method

이제 simplex method에 대해서 배울 차례이다.

개인적으로 참 골때리는 파트라 생각한다. 이해는 가지만 너무 귀찮달까.. 시험에 나오면 분명 계산 삑사리 나서 틀릴 것 같다.

여하튼, simplex method는 간단히 두 단계로 이루어진다.

1. **Check whether the current vertex is optimal (and if so, halt)**
   
   문제가 되는건 어떻게 optimal인지 알 수 있을까? 이다.

   재미있게도, current vertex를 origin으로 잡아버리면 간단히 판별할 수 있다.

   왜 그런지 알아보자.

   ![Generic lp][I_31]

   위와 같은 generic LP 가 주어졌다고 해보자.

   Origin vertex($x_1 = 0, x_2 = 0 \cdots$)가 optimal solution이 되기 위해서는 딱 한가지 조건만 만족하면 된다.

   > The origin is opitmal if and only if all $c_i\leq 0$

   왤까? 생각해보면 당연하다. 

   $c_i\leq 0$이므로, origin이 아닌 경우 objective function value가 음수가 되기 때문이다.

   반대로 $c_i\geq 0$라면 $x_i$를 올려서 더 좋은 objective function value를 얻어낼 수 있으므로 origin은 optimal 하지 않으므로 2번 과정으로 넘어가게 된다.

   곧, 1번 과정은 current vertex가 origin인 상태에서, $c_i\leq 0$인지 확인하는 과정과 동일하다.

2. **Determine where to move next (and move to next)**

   Vertex란, 더 이상 값을 증가시킬 수 없는 boundary에서의 값을 의미한다.

   곧, 어떤 constraint를 tight하게 만족시킬 때까지 $x_i$를 증가시키면 그게 또다른 vertex가 된다.

   예를 들어 아래의 LP가 주어졌다고 하자.

   ![Increase][I_32]

   Origin에서 simplex method를 시작했다고 해보자.

   $c_i\geq 0$이므로 origin은 optimal 하지 않고, 다음 vertex로 이동해야 한다.

   적당히 3번 constraint를 잡아 tight해질 때까지 $x_2$를 증가시켜보자.

   그러면 $x_1=0, x_2=3$이라는 새로운 vertex를 얻게 된다.

   다시말해 새로운 vertex는 3, 4번 constaint에 의해 주어지게 된 것이다.

   근데 문제가 있다. 새로 얻은 vertex는 origin이 아니므로 1번 과정으로 갈 수가 없다. 어떻게 해야할까?

   바로 **좌표 변환을 통해 vertex를 origin으로 옮기면** 된다!

   좌표 변환 아이디어는 간단하다.

   새로운 좌표 $y_i$를 $y_i = b_i - a_i \cdot x$ 형태로 잡으면 된다.

   그냥 부등식을 한 쪽으로 전부 이항시킨 것이다. 어렵지 않다.

   이해를 위해 방금 전 예제를 이어서 풀어보자.

   ![Increase][I_32]

   3, 4번 constraint를 tight하게 만족시켜 새로운 vertex $(x_1=0, x_2=3)$를 얻었다.

   이걸 좌표변환을 하기 위해 3, 4번 constraint를 변형시켜 새로운 vertex $(y_1, y_2)$를 얻어보자

   $(y_1=x_1,\; y_2=3+x_1-x_2)$

   새로 얻은 좌표계에 대해서 LP를 다시 작성하면 아래와 같이 된다.

   ![New][I_33]

   이제 다시 1번으로 돌아가면 된다!

아래는 예제로 풀어보았던 LP에 대한 전체 과정이다.

예제가 이해가 간다면 simplex method를 전부 이해한 것이다!

![Example][I_34]

[I_1]: /assets/lecture/algo/lp/overview.PNG
[I_2]: /assets/lecture/algo/lp/lp_middle_ex.PNG
[I_3]: /assets/lecture/algo/lp/ex_graph_1.PNG
[I_4]: /assets/lecture/algo/lp/ex_graph_2.PNG
[I_5]: /assets/lecture/algo/lp/simplex_sample.PNG
[I_6]: /assets/lecture/algo/lp/digraph.PNG
[I_7]: /assets/lecture/algo/lp/result_digraph.PNG
[I_8]: /assets/lecture/algo/lp/simple_example.PNG
[I_9]: /assets/lecture/algo/lp/imperfect.PNG
[I_10]: /assets/lecture/algo/lp/reverse_flow.PNG
[I_11]: /assets/lecture/algo/lp/1.PNG
[I_12]: /assets/lecture/algo/lp/2.PNG
[I_13]: /assets/lecture/algo/lp/3.PNG
[I_14]: /assets/lecture/algo/lp/4.PNG
[I_15]: /assets/lecture/algo/lp/5.PNG
[I_16]: /assets/lecture/algo/lp/another.PNG
[I_18]: /assets/lecture/algo/lp/final_res.PNG
[I_19]: /assets/lecture/algo/lp/proof.PNG
[I_20]: /assets/lecture/algo/lp/cut.PNG
[I_21]: /assets/lecture/algo/lp/pospen.PNG
[I_22]: /assets/lecture/algo/lp/for_1.PNG
[I_23]: /assets/lecture/algo/lp/for_2.PNG
[I_25]: /assets/lecture/algo/lp/new_lp.PNG
[I_26]: /assets/lecture/algo/lp/pair.PNG
[I_27]: /assets/lecture/algo/lp/dual.PNG
[I_28]: /assets/lecture/algo/lp/general.PNG
[I_29]: /assets/lecture/algo/lp/general_2.PNG
[I_30]: /assets/lecture/algo/lp/max_dual.PNG
[I_31]: /assets/lecture/algo/lp/generic_lp.PNG
[I_32]: /assets/lecture/algo/lp/increase.PNG
[I_33]: /assets/lecture/algo/lp/newnew.PNG
[I_34]: /assets/lecture/algo/lp/example.PNG