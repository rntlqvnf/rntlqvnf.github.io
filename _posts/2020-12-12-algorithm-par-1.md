---
title: "&#91;Algorithm&#93; Parametrized Algorithm (1)"
categories:
  - Lecture Notes
tags:
  - algorithm
  - csed331
use_math: true
toc: true
---

> 이 글은 포스텍 오은진 교수님의 CSED331 알고리즘 수업의 강의 내용과 자료를 기반으로 하고 있습니다.

개인적으로 제일 이해가 안간 파트이다.

대체 이걸 왜 해야 하는가.. 여기서 뭔 문제를 낼 수 있는가..

까먹고 강의 녹화도 안해서 공부하는데 너무 애를 먹었다.

아.. 알고리즘 너무 싫다..

# 1. Parametrized Algorithm

우리는 이때까지 efficient의 기준을 input size로만 잡아왔다.

근데 사실 생각해보면 분명 output size에 의해서도 영향을 받아야만 한다.

예를 들어, maximum clique 문제를 푼다고 하자.

$n$이 엄청 크더라도 주어진 graph가 complete graph라면 굉장히 빠르게 해를 구할 수 있다.

이 아이디어를 도입한 것이 **Parametrized Algorithm**이다.

Parametrized Algorithm의 목표는 input size 외에 output size 같은 parameter를 도입해서, 시간복잡도를 $O(f(k)\cdot poly(n))$의 형태로 나타내는 것이다.

예를 들어 Vertex cover algorithm을 parametrized algorithm으로 표현하면 다음과 같다.

![Param][I_1]

만약 하나의 parameter로 안된다면 parameter를 늘려서라도 위의 형태를 만들면 된다.

이번 강의에서 배울 것은 Parametrized Algorithm의 예시들이다.

# 2. Parametrized Algorithm for Vertex Cover

우리가 알아볼 것은 Vertex Cover를 푸는 $O(2^{k^2} + n^2)$-time algorithm이다.

알고리즘은 아래와 같다.

![Algo][I_2]

Rule1과 Rule2를 계속 적용해서 만든 $G$에 아래의 if-otherwise를 적용해 해를 뱉는 것이다. 

Rule1과 Rule2는 대충 직관적으로 와닿을 것이다.

- **Rule1**
   
  당연히 isolated vertex는 vertex cover에 속할 수 없다.

- **Rule2**

  vertex cover의 사이즈가 $k$이므로, degree가 $k+1$ 이상인 vertex는 당연히 vertex cover에 포함되어야 한다.

  이때 decrement $k$ by one이란, 찾아야 하는 vertex cover의 size를 1 줄인다는 의미이다 (하나를 찾았으므로)

그럼 If-otherwise는 대체 뭔소릴까?

증명을 통해 느낌을 잡아보자.

> Claim: If $(G,k)$ is a yes-instance and none of the rules is applicable to $G$, then $\lvert V(G) \rvert \leq k^2+k$ and $\lvert E(G) \rvert \leq k^2$

- **Proof**
  
  $S$를 사이즈가 $k$인 vertex cover라고 하자.

  Rule2에 의해서, $G$의 모든 vertex는 최대 $k$의 이웃을 가졌다.

  따라서 $\lvert V - S \rvert \leq k \lvert S \rvert$ 가 된다.

  이때 $\lvert S \rvert = k$이므로, $\lvert V - S \rvert = k^2$이 되고, $S$를 넘기면  $\lvert V \rvert = k^2 + k$를 얻는다.

  곧, $V$는 $S$의 이웃과 $S$를 합한 값이라는 의미이다.

  또 한편 Rule2에 의해, $S$는 최대 $k^2$개의 edge를 가질 수 있다. ($k$는 무시한 듯 보인다. 왜 무시한 걸까?)
  
  따라서 $\lvert E(G) \rvert \leq k^2$ 이다.

이제 알고리즘이 무슨 의미인지 이해했을 것이다.

자 이제 시간 복잡도를 구할 차례이다.

> Claim: The running time of the algorithm is $O(2^{k^2} + n^2)$

- **Proof**
  
  Rule1과 Rule2는 모두 $O(n)$ 시간에 적용할 수 있다.

  또한 각 rule은 최대 $n+k$ 번 적용되므로, 총 $O(n^2)$ 시간이 걸린다.

  If를 판단하는 데에는 역시 선형시간이 걸린다.

  Otherwise를 적용하는 데에는, 현재 $G$가 최대 $k^2$개의 vertcies를 가졌으므로 $O(2^{k^2})$이 걸린다.

  따라서 총 $O(2^{k^2} + n^2)$이다.

# 3. Faster Parametrized Algorithm

이제 조금 더 빠른 $O(1.6181^k n^2)$-time algorithm을 알아보자.

알고리즘은 아래와 같다.

![Algo][I_3]

식이 더럽게 복잡한데, 알고보면 되게 간단한 아이디어로부터 나왔다.

> Every vertex cover must contain either $v$ or $N(v)$

알고리즘은 이를 수식으로 표현했을 뿐이다.

그럼 시간 복잡도를 증명해보자

> Claim: This algorithm takes $O(1.6181^k n^2)$ time

- **Proof**
  
  ![Recurse][I_4]

  그냥 점화식을 풀면 된다.

  이때 maximum degree vertex를 찾는게 $O(n^2)$이 걸리므로, 총 $O(1.6181^k n^2)$이 된다.

[I_1]: /assets/lecture/algo/par/ex.PNG
[I_2]: /assets/lecture/algo/par/algo.PNG
[I_3]: /assets/lecture/algo/par/algo_2.PNG
[I_4]: /assets/lecture/algo/par/recurse.PNG