Convolution 곱셈 단축

Weight * Image

Weight는 column wise add
Image는 row wise add

예를 들어
Weight가
0 1 0
1 0 1
1 1 1 

이라면
합하고 나면
2 2 2

Image가
0 0 1
1 0 0 
0 1 1
이라면

합하고 나면
1
1
2

이제 
2 2 2 * (1,1,2) = 8

Convolution은 전부 합하는 거니까 이 연산이 가능함.
전체 연산수가 확 줄기는 하는데, 아키텍쳐에 적용이 가능한 것 같지는 않음. 그냥 아이디어임