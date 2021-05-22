---
title: "&#91;Review&#93; In-Datacenter Performance Analysis of a Tensor Processing Unit"
categories:
  - Research
tags:
  - tpu
  - computer architecture
  - paper reviews
toc: true
toc_sticky: true
gallery:
  - url: /assets/review/tpu/g1.PNG
    image_path: /assets/review/tpu/g1.PNG
    alt: "TPU"
    title: "TPU Performance"
  - url: /assets/review/tpu/g2.PNG
    image_path: /assets/review/tpu/g2.PNG
    alt: "Haswell"
    title: "Haswell Performance"
  - url: /assets/review/tpu/g3.PNG
    image_path: /assets/review/tpu/g3.PNG
    alt: "NVIDIA"
    title: "NVIDIA Performance"
---

굉장히 오랜만에 글을 쓴다.

방학이 되고 나서 맨날 뒹굴뒹굴 하다가 연참을 시작하는 바람에 다시 공부하고 있다.

김광선 교수님 랩에서 연참을 하고 있는데, 아직 공부를 덜해서 잘은 모르겠지만 나름 재밌다.

사실 랩을 정하는데 정말 많은 고민이 있었다.

대부분의 과목을 잘하는 편이지만, 특출나게 잘하는 과목이 없고, 좋아하는 건 코딩인지라 나에게 맞는 랩을 찾기가 무척 어려웠다.

그냥 제일 잘나가는 비전 랩에 들어가기에는 인공지능이 과연 내가 박사졸을 할 때까지 잘나갈까 싶어서 많이 꺼려졌었다.

그 이외에 흥미가 가는 랩들은 신생이거나, 소문이 좀 안좋거나, 타대(카이스트)여서 쉬이 선택하지 못하고 대략 6달을 끙끙 고민만 했었다.

김광선 교수님 랩을 결국 선택하게 된 것은 굉장히 복합적이고 나름 합리적인 판단 하게 이뤄졌다.

내가 시스템 쪽에 나름 강했고, 아키텍쳐는 유행에 크게 영향받지 않는 분야이며, 랩이 비록 신생이지만 2년이 지난 지라 기반이 거의 갖춰졌고, 또 교수님도 굉장히 열정적이셔서 지도를 잘 받을 수 있을 것 같았기 때문이었다.

그렇게 연참을 시작한지 일주일이 지난 지금 -아직은 내가 방학 모드라서 뒹굴뒹굴하는 시간이 더 길긴 하지만- 꽤 재미를 느끼고 있다.

이번 연참을 통해서 좀 진로를 정할 수 있으면 좋겠다.

이 글에서 소개하고자 하는 논문의 제목은 **In-Datacenter Performance Analysis of a Tensor Processing Unit** 이다.

구글에서 2017년 4월 초에 개발한 TPU와 관련된 논문이다.

내가 연참에서 맡은 주제가 systolic array인지라, 이를 사용한 대표격인 논문을 주신 것 같다.

논문을 제대로 읽는 것은 처음이라, 조금은 장황하게 서술해보려고 한다.

나중에 짬이 좀 차면 핵심만 콕콕 집어서 서술할 수 있을 것이라 생각한다.

# Introduction

![PP][I_1]

약 40년 동안은 CPU 성능이 무어의 법칙 등을 따라 굉장히 빠르게 발전했었다.

그러나 무어의 법칙은 종말을 맞았다.

곧, 더 이상 general performance를 끌어올리긴 힘드므로, 특정한 일의 성능만 끌어올릴 수 있는 domain specific architecture를 개발하는 방향으로 선회하게 되었다.

구글은 이 전환의 필요성을 2013년에 깨달았다고 한다.

구글이 분석하기로, 만약 사람들이 음성 인식 DNNs(deep neural networks)을 하루에 3분 이상을 사용한다면 당시 구글 데이터 센터의 2배에 달하는 계산량이 필요했다고 한다.

이걸 무작정 CPU를 많이 박아서 해결하기에는 돈이 너무 많이 들기 때문에, DNNs만을 굉장히 빠르게 처리할 수 있는 ASIC(application-specific integrated circuit)를 개발하고자 했다.

그렇게 개발에 착수한 구글은 공밀레를 시전하여 무려 15개월 만에 **TPU(Tensor Processing Unit)**을 만들어냈다.

# TPU 

## Target Task

구체적으로 TPU의 target task는 아래의 세가지 NN이다

- **Multi-Layer Perceptrons (MLP)**
  
  ![MLP][I_3]

  >  Each new layer is a set of nonlinear functions of a weighted sum of all outputs (fully connected) from the prior one.

- **Convolutional Neural Networks (CNN)**
  
  ![CNN][I_4]

  >  Each layer is a set of nonlinear functions of weighted sums at different coordinates of spatially nearby subsets of outputs from the prior layer, which allows the weights to be reused.

  [추천 블로그](https://gruuuuu.github.io/machine-learning/cnn-doc/#)

- **Recurrent Neural Network (RNN)**

  ![RNN][I_5]

  > Each subsequent layer is a collection of nonlinear functions of weighted sums of outputs and the previous state. The most popular RNN is Long Short-Term Memory (LSTM). The art of the LSTM is in deciding what to forget and what to pass on as state to the next layer. The weights are reused across time steps.

  [추천 블로그](https://ratsgo.github.io/natural%20language%20processing/2017/03/09/rnnlstm/)

<br>

아래의 표는 구글 데이터 센터에서 위 세 NN이 쓰이는 비율을 나타낸 것이다. 

전체의 95%를 차지함을 알 수 있다.

![Target][I_2]

한가지 재밌는 점은 나름 핫한 CNN이 5% 밖에 차지하지 않는다는 점이다. (최근 통계는 다르려나?)

나중에 결론 부분에서 나오는데, 이 때문에 TPU의 단점인 low memory bandwith가 별로 문제되지 않는다.

암달의 법칙; **low utilization of a huge, cheap resource can still deliver high, cost-effective performance** 을 기억하자!

## TPU Architucture and Implementation

TPU는 빠른 시간 내에 개발해야 했기에 아래의 특징을 가진다.

- PCIe I/O bus를 통해 연결되어 coprocessor로 동작

  - 기존에 있던 서버에 바로 꽂아서 사용할 수 있어야 했기 때문에, CPU와 연동하는 방식으로 만들기엔 부적합함 

  - [PCIe란?](https://www.youtube.com/watch?v=4p9rF193PkU)

- Host로부터 instruction을 전달받음
  
  - TPU가 직접 instruction을 가져와서 실행하는 형태보다 훨씬 더 간단함
  
  - 직접 instruction을 가져오는 경우 프로그램 카운터를 유지하는 등 추가 구현이 필요하기 때문 (디버깅도 어려워짐)

이 때문에 GPU보다는 FPU (floating-point unit)에 가깝다고 할 수 있다.

TPU의 구조는 아래의 그림에 나와있다.

![Arc][I_6]

처음 봤을 땐 이게 뭔소린가 싶었는데 하나하나 공부하고 나니 생각보다 별 것 없었다.

좌측의 **PCIe Gen3**는 host와 소통하기 위한 I/O라고 생각하면 된다.

이 I/O를 통해 instruction과 data가 들어오면 **host interface**를 거쳐 각각 **inst buffer** / **unified buffer** 에 쌓이게 된다.

NN은 근본적으로 weight와 input data 간의 연산으로 이뤄진다.

Input data는 앞서 말했듯 PCIe를 통해 unified buffer에 쌓인다.

Weight는 위쪽의 DDR3 DRAM Chips로 부터 전달받아, weight queue에 들어간다.

이 두 값을 가지고 연산을 진행하는 장치가 우측의 **Matrix Multiply Unit (MMU)**이다.

Matrix multiply unit의 연산 결과는 accumulator에서 누적되고, 누적된 값은 activation unit과 normalize / pool unit를 거쳐 unified buffer에 저장된다.

저장된 값은 matrix multiply unit의 입력으로 재사용되거나 host 인터페이스와 PCIe 인터페이스를 통해 메인메모리에 출력된다

전체적인 흐름은 알아보았고, 조금 더 구체적으로 유닛을 알아보자.

### MMU

TPU의 핵심은 **MMU**이다.

**MMU**는 65,546(256 x 256)개의 8-bit MAC(Multiply and Accumulate)가 systolic array 구조를 띄는 형태이다.

Systolic array가 내 연구 참여 주제이다 ㅎ..

Systolic array를 사용한 이유는 행렬 계산을 효율적으로 하기 위해서이다.

행렬 계산의 특성상 특정 값이 계산에 여러번 참여하는데, 여타 계산기처럼 한번 쓰이고 버리기에는 너무 비효율적이다.

![Sys][I_8]

따라서 MAC의 출력 값이 인접한 다음 줄의 MAC의 입력으로 전달되기 때문에 매번 출력 값을 메모리에 저장해야 하는 일반적인 연산방법보다 메모리 액세스 빈도를 낮춰 전력 소모를 줄이는 데 유리한 systolic array를 채택한 것이다.

구체적인 원리는 [링크](http://web.cecs.pdx.edu/~mperkows/temp/May22/0020.Matrix-multiplication-systolic.pdf)를 참조하면 된다.

그런데 공부하면서 꽤 헷갈리는 점이 있었다.

위 링크의 systolic array는 weight와 input이 둘 다 들어오며, weight와 input이 각각 아래쪽과 오른쪽으로 그대로 나가는 형태이다.

![Systolic][I_7]

그런데 내가 교수님한테는 위와 같이 아래쪽에는 계산 값이, 오른쪽에는 input이 그대로 나가는 형태라고 배웠다.

아마 논문 상에서는 [링크](http://web.cecs.pdx.edu/~mperkows/temp/May22/0020.Matrix-multiplication-systolic.pdf)의 systolic array 구조가 아닌가 싶다.

그런데 두 구현이 그렇게 차이가 나는건 아니고, 그냥 세부적인 구현법만 다른 것 같다.

한 가지 착각하면 안되는 점은 이 MMU가 multiplication만, 그러니까 convolution을 구현하지 못하는 것은 아니다.

Weight를 넣어주는 순서만 살짝 바꿔주면 convolutoin도 구현할 수 있다.

### Accumulator

MMU 아래 쪽에는 32bit reg로 구성된 4MiB accumulator가 있다.

32bit reg는 MMU의 세로방향을 따라 내려온 MAC의 16bit 결과 값을 저장하는데 사용된다.

여기서 굳이 32bit인 이유는 16bit 결과값을 누적해서 늘어날 수 있는 최대 bit를 32bit로 제한하기 위해서이다.

한편 4MiB accumulator란 하나의 entry당 **256**개의 32-bit 레지스터를 가진 **4096**-entry Accumulator를 의미하는데, 여기서 256과 4096는 어디서 나온 숫자일까?

**256**은 MMU에서 한 사이클 당 256개의 결과값이 내려오기 때문이다.

**4096**은 최대 성능에 도달하기 위해선 operation/byte를 1350 정도로 맞춰야 하고, 이걸 2의 거듭제곱으로 round up하고(2048) buffering을 생각해서 두 배를 해주면 4096이 되기 때문이다.

이런 숫자 하나하나가 생각보다 많은 의미를 담고 있으니 꽤 주의해서 봐야 한다.

# Performance

![Rep][I_9]

성능 비교를 위한 비교군은 E5-2699 v3과 K80이다.

위 두 칩을 선택한 이유는 자기네들 데이터 센터에서 주로 사용되고 있기 때문이라고 한다.

![Roof][I_10]

- X-axis: Floating-point operations per DRAM byte accessed (operational intensity)
- Y-axis: Floating-point operations per second (performance)

이 논문에서는 성능 비교를 위해 **roofline model**을 사용하고 있다.
 
그렇게 정확한 모델은 아닌데, performance bottleneck 원인에 대한 직관을 제공해줘서 나름 자주 쓰인다.

그래프의 기울기가 변하는 시점을 기준으로, 좌측에 위치하면 memory BW bounded, 우측에 위치하면 compute bounded로 간주한다.

이유는 생각해보면 당연하다. 

좌측에 위치한다면 메모리에서 읽어들이는 속도 때문에 성능 향상에 제한이 걸렸다는 의미이다.

우측에 위치한다면 메모리에선 빨리 읽어들이지만 계산이 느려서 성능 향상이 안되고 있다는 의미이다.

자 이제 본격적으로 성능을 비교해보자.

{% include gallery %}

맨 좌측의 TPU 그래프를 보면 CNN1을 제외한 다른 모든 DNN은 한계치에 도달했다.

또한 CNN은 computation bound, MLP와 LSTM은 memory bound임을 알 수 있다.

우측의 Haswell과 NVIDIA를 보면 선에서도 꽤 떨어져있고, 그래프의 높이도 TPU 대비 상당히 낮음을 확인할 수 있다.

![Bound][I_14]

Haswell과 NVIDIA이 그래프에서 떨어져있는 이유는 response time 제한 때문이다.

DNN은 7ms 안에 서비스 되어야 하는데, CPU와 GPU의 경우 batch size가 64가 되면 7ms를 넘기 때문에 이보다 작은 batch size를 써야 한다.

이 때문에 큰 성능 저하가 발생하는 것이다.

반면 TPU는 250 대신 200을 쓰면 되므로, 별 차이가 없다.

![Sum][I_12]

위 그래프는 방금 전 본 세 그래프를 모은 것이다.

확연한 성능차를 확인할 수 있다.

GPU와 이렇게 차이나는 이유는 GPU가 32bit 연산을 하며 행렬 곱에 특화되어 있지 않은 등 DNN에 최적화 되어있지 않기 때문이다. 이정도나 차이나는게 놀랍긴 하지만.

![Table][I_13]

위는 CPU 대비 성능을 기록한 표이다.

TPU의 경우 LSTM1을 제외하고 압도적인 성능차를 보인다.

한가지 재밌는 점은 GPU가 CPU 대비 그닥 차이 나지 않는다는 점이다. 

![Watt][I_15]

위는 Performance / Watt를 기록한 표이다.

Performance / Watt는 서버 관리 비용과 밀접한 연관이 있기 때문에 꽤 중요한 수치이다.

보다시피 TPU가 CPU / GPU를 압도함을 알 수 있다.

# Thoughts

하드웨어에 대한 배경지식 하나 없이 논문을 쌩으로 읽으려니까 참 힘들었다.

그럼에도 불구하고 다 읽고나니 꽤 흥미로웠다.

세상에 이런 아이디어도 있구나.. 하는 느낌?

항상 High level programming적인 사고만 하다보니 이런 low level 생각 자체가 굉장히 신선했다.

하지만 아직은 배경지식이 부족해서 왜 이게 DNN에 효과적인지 제대로 설명할 수는 없는게 좀 아쉬웠다.

그래서 그런지 이 논문에서 개선해야 될 점이나, 새로운 아이디어가 떠오르지 못했다. 내 말로 설명할 수 없으니 당연히 글도 제대로 써지지 않았다. 

열심히 논문을 읽고 있으니 언젠간 제대로 이해해서 질문을 던질 수 있을만큼 성장할 수 있을 것이라 믿는다.

# References

- [논문](https://dl.acm.org/doi/10.1145/3079856.3080246)

- [PR-085: In-Datacenter Performance Analysis of a Tensor Processing Unit](https://www.youtube.com/watch?v=7WhWkhFAIO4)

[I_1]: /assets/review/tpu/pp.jpg
[I_2]: /assets/review/tpu/target.PNG
[I_3]: /assets/review/tpu/mlp.png
[I_4]: /assets/review/tpu/cnn.png
[I_5]: /assets/review/tpu/rnn.png
[I_6]: /assets/review/tpu/arc.PNG
[I_7]: /assets/review/tpu/systolic.png
[I_8]: /assets/review/tpu/sys.PNG
[I_9]: /assets/review/tpu/rep.PNG
[I_10]: /assets/review/tpu/roof.png
[I_12]: /assets/review/tpu/sum.PNG
[I_13]: /assets/review/tpu/table.PNG
[I_14]: /assets/review/tpu/bound.PNG
[I_15]: /assets/review/tpu/watt.PNG