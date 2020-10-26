---
title: "&#91;Algorithm&#93; Graph (3)"
categories:
  - Lecture Notes
tags:
  - algorithm
  - csed331
use_math: true
toc: true
---

> 이 글은 포스텍 오은진 교수님의 CSED331 알고리즘 수업의 강의 내용과 자료를 기반으로 하고 있습니다.

알고리즘 4주차 두 번째 강의 요약이다.

핀토스가 끝나니까 정말 인생 살 맛 난다.

근데 문제는 다음주까지 디자인 레포트 제출이다..

재밌기는 한데, 시간을 너무 많이 잡아먹는 것 같다.

나중에 방학 때 핀토스 공략집도 써야겠다.

# 1. Breath-first Search

지난 시간까지 배운 DFS는 한 길만 우직하게, 수직으로 파는 탐색법이다.

오늘 소개할 **BFS**는 DFS와는 달리 횡으로 탐색하는 기법이다.

![Layer][I_1]

BFS가 탐색하는 방식을 그림으로 그려보면 위처럼 **layer by layer**로 탐색하는 모양이 나온다.

개념은 사실 별로 어려운 게 없으니.. 곧바로 구현으로 들어가보자.

![BFS CODE][I_2]

BFS는 알고리즘은 간단히 다음과 같다.

1. Pop node $v$ from queue
2. Iterate neighbors of $v$, if not visited then put it in the queue
3. Repeat 1-2 until queue empty

하나 기억해둘 점은 DFS가 stack을 사용한다면, **BFS**는 **queue**를 사용한다는 점이다.

각 자료구조를 왜 사용하는지 생각해보는걸 추천한다.

아래는 예시이다.

![QUEUE][I_3]

유심히 보면, B를 먼저 탐색하지 않고 우선 현재 layer(A,C,D,E)를 모두 방문하는 것을 목표로 움직이고 있음을 알 수 있다.

자 그럼 시간 복잡도는 어떻게 될까?

간단히 모든 edge과 node를 방문하므로 $O(\left\lvert V \right\rvert+\left\lvert E \right\rvert)$이다.

# 2. Dikstra's algorithm

BFS를 설명하는데 절대로 빠지지 않는 알고리즘이 다익스트라 알고리즘이다.

**다익스트라 알고리즘**이란 하나의 정점에서 다른 모든 정점까지의 경로를 구하는 알고리즘이다.

![Code][I_5]

뭔가 길어보이지만, 한 번 쓱 읽어보면 어떤 알고리즘인지 이해가 곧바로 이해가 갈 것이다.

Priority queue와 BFS를 이용해서 지속적으로 최단 경로를 갱신시켜나가는 것만 알면 된다.

자세한 알고리즘의 설명은 다른 사이트를 참고하길 바란다.

[추천](https://chanhuiseok.github.io/posts/algo-47/)

## Time complexity

다익스트라 알고리즘은 BFS와 구조적으로 동일하긴 하나, 최소 값을 가지는 노드를 선택해야 하므로 BFS보다 좀 더 많은 시간이 든다.

근데 정확한 시간 복잡도는 구현 방법에 따라 꽤 차이가 난다. 

구하는 과정을 밟아보자.

코드를 떠올려보면, 우선 처음에 queue를 만드는 `makequeue`는 `insert`를 $\left\lvert V \right\rvert$번 실행할 것이다.

또한 결과적으로 모든 노드가 큐에서 제거되어야 다익스트라 알고리즘이 종료되니 `deletemin`은 총 $\left\lvert V \right\rvert$번 실행될 것이며,

최악의 경우 모든 edge에 대해 갱신 작업을 거쳐야 하므로 `decreasekey`은 총 $\left\lvert E \right\rvert$번 실행된다고 할 수 있다.

따라서 `deletemin`은 총 $\left\lvert V \right\rvert$번, `insert`/`decreasekey`는 총 $\left\lvert V \right\rvert+\left\lvert E \right\rvert$번 실행된다.

이때 `insert`와 `decreasekey`를 묶은 이유는 `decreasekey`가 값 수정 후 재배치할 위치를 찾기 위해 탐색하는 방법이 `insert`가 삽입할 위치를 탐색하는 방법이랑 동일하기 때문이다.

여하튼, 이 `deletemin`과 `insert`/`decreasekey`의 시간복잡도는 구현에 따라 판이하게 차이가 난다.

예를 들어 배열의 경우 `deletemin`이 $O(\left\lvert V \right\rvert)$이고, `insert`/`decreasekey`가 $O(1)$이다.

아래의 표가 구현별로 차이점을 잘 정리하고 있다.

![다익스트라 알고리즘 시간복잡도][I_6]

구체적인 복잡도를 외울 필요는 없지만, 왜 저런 복잡도가 나오는지 한 번쯤 증명해보는 것을 추천한다.

아래는 d-ary heap에 대한 증명이다.

![Proof][I_7]

[I_1]: /assets/lecture/algo/4/layer.PNG
[I_2]: /assets/lecture/algo/4/bfs_code.PNG
[I_3]: /assets/lecture/algo/4/queue.PNG
[I_5]: /assets/lecture/algo/4/code_bfs.PNG
[I_6]: /assets/lecture/algo/4/impl.PNG
[I_7]: /assets/lecture/algo/4/proov.PNG