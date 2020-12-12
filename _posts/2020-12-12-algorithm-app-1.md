---
title: "&#91;Algorithm&#93; Approximation Algorithm (1)"
categories:
  - Lecture Notes
tags:
  - algorithm
  - csed331
use_math: true
toc: true
---

> 이 글은 포스텍 오은진 교수님의 CSED331 알고리즘 수업의 강의 내용과 자료를 기반으로 하고 있습니다.

요즘 알고리즘 무슨 내용인지 알아듣기가 힘들다.

NP-complete 증명까지는 어떻게든 이해했는데, 요즘 뭐.. approximation이니 parametrized니.. 이게 뭔지, 왜 하는지도 모르겠다.

심지어 블로그는 고사하고 교과서 찾아봐도 관련된 내용이 없어서 독학도 어렵다!

아.. 인생 재밌다.

# 1. Approximation Algorithm

Optimization problem이란 어떤 instance *I*가 주어졌을 때, 그에 따른 optimum solution을 찾는 문제를 말한다.

이때 이 optimum solution을 $OPT(I)$라고 적는다.

이후 여러 논의들을 간단하게 하기 위해 $OPT(I)$가 항상 positive integer 라고 가정한다.

Optimization problem은 흔히 minimization problem과 maximization problem으로 나뉜다.

이 중, minimization problem을 생각해보자. (maximization problem에도 동일하게 적용할 수 있다!)

Minimization problem을 풀기 위해 우리가 만들어낸 알고리즘을 $A$라고 하고, solution을 $A(I)$라고 하자.

이때 **approximation ratio of algorithm A**는 아래와 같이 정의된다.

![Approximation ratio][I_1]

곧, $\alpha_A$는 우리가 만든 알고리즘의 solution이 optimal solution보다 얼마나 큰지를 말해주는 값이라 할 수 있다.

우리가 왜 approximation ratio를 배웠을까?

이는 approximation ratio가 NP-complete optimization problem을 푸는 알고리즘을 평가하는 매우 중요한 지표이기 때문이다.

근데 잘 생각해보면 좀 의아한 점이 있다.

> Optimal solution은 어떻게 구하는데? 애초에 뭐가 optimum인지 어떻게 아는데?

바로 이 문제가 오늘 강의의 핵심이다.

우리는 오늘 몇 가지 예시를 통해 어떻게 optimal solution을 추측하고, $\alpha$를 구하는지를 배울 것이다.

# 2. Vertex Cover

Vertex cover 문제는 너무 자주 봐서 아마 다시 설명할 필요는 없을 것이다.

우리가 할 것은 아래의 알고리즘이 factor-2 approximation algorithm 이라는 것을 증명하는 것이다.

![VC][I_2]

우선 $C$가 vertex cover임을 증명하자.

> Claim: $C$ is a vertex cover

- **Proof**
  
  만약 $C$가 vertex cover가 아니라고 하자.

  그러면 $C$가 cover하지 못하는 edge가 존재한다는 의미이다.

  그러나 그런 edge가 존재한다면 알고리즘에 의해 발견될 것이므로, 이는 모순이다.

  따라서 $C$는 vertex cover이다.

factor-2를 증명하기 위한 보조 정리로 $M$의 edge들은 vertex를 공유하지 않음을 증명하자.

> Claim: No two edges of $M$ share a common vertex

- **Proof**
  
  어떤 edge $(a,b)$가 선택되었다고 해보자.

  그러면 $a$와 $b$를 endpoint로 가지는 모든 edge들은 cover 가능한 edge로 분류되므로, 알고리즘에 의해 선택되지 않는다.

  따라서 $M$의 어떤 edge라도 vertex를 공유하지 않는다.

이제 factor-2 algorithm 임을 증명하자.

> Claim: $\lvert C \rvert \leq 2\cdot OPT$ 

- **Proof**

  Vertex cover는 $M$의 edge들을 cover하기 위해, $M$의 각 edge의 endpoint 중 최소 하나는 가져야만 한다. 

  두 번째 claim에 의해 $M$은 vertex를 공유하지 않으므로, optimal solution은 아래의 부등식을 만족한다.

  $OPT \geq \lvert M \rvert$

  이때, $\lvert C \rvert = 2\lvert M \rvert$ 이므로

  $\lvert C \rvert = 2\lvert M \rvert \leq 2 \cdot OPT$이다.

  곧, factor-2 algorithm이다.

아마 이제 어떻게 $OPT$를 계산하는지 알았을 것이다.

이 기세를 이어 나머지 것도 알아보자.

# 3. Weighted Vertex Cover

![WVC][I_3]

Weighted vertex cover는 vertex cover 중에서 경로 비용의 합을 최소인 놈을 찾는 문제이다.

이 문제를 좀 더 formal 하게 LP로 적어보자.

![LP][I_4]

굉장히 직관적이어서 바로 알아들을 수 있을 것이다.

이때, LP-relaxation이란 원래의 LP는 $x_u$와 $x_v$ 중 하나만 포함되도록 강제했지만, 이를 완화하여 둘 중 하나 이상이 포함되도록 바꿨다는 말이다.

우리가 할 것은 아래의 알고리즘이 factor-2 approximation algorithm 이라는 것을 증명하는 것이다.

![WVC2][I_5]

우선 $S$가 vertex cover임을 증명하자

> Claim: $S$ is a vertex cover

- **Proof**
  
  $E$의 edge를 $(u,v)$라 하자.

  $x_u^{\ast}+x_v^{\ast} \geq 1$이므로, $x_u^{\ast} \geq 1/2$ 또는 $x_v^{\ast} \geq 1/2$ 이어야만 한다.

  곧, $u$나 $v$는 $S$ 안에 포함되어 있다.

  따라서 $S$는 vertex cover이다.

이제 factor-2 algorithm 임을 증명하자.

> Claim: $c(S)\leq 2\cdot OPT$

- **Proof**
  
  LP-relaxation의 해를 $OPT_{LP}=\sum_v c(v)x_v$라고 하자.

  LP-relaxation은 원래의 Integer LP를 완화한 것이다.

  곧, Integer LP는 0, 1 밖에 가지지 못하지만 LP-relaxation은 0~1의 실수를 택할 수 있으므로, $\sum_{v\in V}c(v)x_v$를 더 작게 만들 수 있다.

  따라서 $OPT \leq OPT_{LP}$이다.

  이때 우리의 알고리즘은 LP-relaxation의 해에서 $x\geq 1/2$인 것들만 골라낸 것이므로 아래의 관계가 성립한다.

  $OPT_{LP} \geq \sum_{v\in S} 1/2 c(v)$

  이때 $\sum_{v\in S} c(v)=c(S)$이므로, $c(S)\leq 2\cdot OPT$이 된다.

# 4. TSP

TSP 문제에서는 역으로 polynomial time $t$-approximation algorithm이 존재하지 않는다는 것을 보일 것이다.

![PNP][I_6]

아이디어는 hamiltonian cycle to TSP reduction이다.

- **Proof**
  
  Hamiltonian cycle 문제에 graph $G=(V,E)$가 주어졌다고 하자.

  우리는 이걸 complete graph $H=(V, E^{\backprime}$으로 변형하여, TSP 문제로 reduction을 할 것이다.

  아래는 $H=(V, E^{\backprime}$의 정의이다.

  ![Ham][I_7]

  TSP의 polynomial time $t$-approximation algorithm $A$가 존재한다고 가정하자.

  우리가 보일 것은 $A$가 tour of length $\lvert V \rvert$를 반환하는 것은 $G$가 hamiltonian cycle을 갖는 것과 동치라는 것이다.

  > Claim: $A$ returns a tour of length $\lvert V \rvert$ for $H$ iff $G$ has a Hamiltonian cycle

  - **If**
    
    ![If][I_8]

    Hamiltonian cycle을 TSP도 solution으로 채택할 것이므로 동일하다는 의미이다.

  - **Only If**
  
    Length가 $\lvert V \rvert$라는 말은 $e\in E$인 edge들만 방문했다는 얘기이다.

    이는 곧 $G$에서의 Hamiltonian cycle을 의미한다.

이로부터, TSP가 polynomial time **approximation algorithm**을 가진다는 말은 NP-complete **hamiltonian cycle** problem을 푸는 polynomial time algorithm이 존재한다는 말이 된다.

곧, $P=NP$가 아니라면 TSP를 푸는 polynomial time approximation algorithm이 존재하지 않는다.

# 5. Metric TSP

![Metric TSP][I_9]

Metrix TSP는 triangle inequaility를 만족하는 TSP를 의미한다.

Triangle inequaility란, 삼각형 $(a,b,c)$가 있을 때 $a$에서 $c$로 바로 가는 것이 $b$를 경유해서 가는 것 보다 빨라야 한다는 제약이다.

놀랍게도 이걸 추가하면 factor-2 approximation algorithm이 존재한다!

알고리즘의 아이디어는 MST를 이용하는 것이다.

정확하게는, MST의 시작점과 끝점을 잇고, 경유 경로를 삭제하는 것이다.

이게 무슨 소리인지 예제를 통해 알아보자.

다음과 같은 점들이 주어졌다고 해보자. (원래는 그래프지만 대충 알아들으면 된다)

![MST][I_10]

이 점들의의 mst는 다음과 같다.

![MST_2][I_11]

A를 TSP의 시작점, B를 끝점이라고 하자. (정확하게는 A로 돌아가기 전 마지막에 방문하는 점이다)

MST를 따라 A에서 B로 이동하면서 모든 vertex를 방문하려면 어떻게 해야할까?

![MST_3][I_12]

이렇게 갔던 점을 갔다가 다시 되돌아와야 할 것이다.

그런데 이렇게 되면 점을 두 번 방문하는 셈이다.

![2MST][I_14]

아마 최악의 경우에는 이렇게 죄다 왔다갔다 해야할 것이다.

곧, 모든 edge를 두 번씩 방문해야 한다. ($2\cdot cost(MST)$)

이를 개선하기 위해, 경유해서 가야하는 경로를 없애주자. 

![MST_4][I_13]

triangle inequaility에 의해, 이 tour는 두 번씩 방문하는 것보다 훨씬 적은 비용이 든다.

따라서 $cost(tour) \leq 2\cdot cost(MST)$이다.

이때 한편 $cost(MST) \leq cost(TSP)$이므로, 이를 종합하면 아래의 부등식을 얻는다.

> $cost(tour) \leq 2\cdot cost(MST) \leq 2\cdot cost(TSP)$

따라서 factor-2 approximation algorithm 이다.

[I_1]: /assets/lecture/algo/app/ratio.PNG
[I_2]: /assets/lecture/algo/app/vc.PNG
[I_3]: /assets/lecture/algo/app/wvc.PNG
[I_4]: /assets/lecture/algo/app/lp.PNG
[I_5]: /assets/lecture/algo/app/wvc_2.PNG
[I_6]: /assets/lecture/algo/app/pnp.PNG
[I_7]: /assets/lecture/algo/app/ham.PNG
[I_8]: /assets/lecture/algo/app/if.PNG
[I_9]: /assets/lecture/algo/app/metric.PNG
[I_10]: /assets/lecture/algo/app/mst.PNG
[I_11]: /assets/lecture/algo/app/mst_2.PNG
[I_12]: /assets/lecture/algo/app/mst_3.PNG
[I_13]: /assets/lecture/algo/app/mst_4.PNG
[I_14]: /assets/lecture/algo/app/2mst.PNG