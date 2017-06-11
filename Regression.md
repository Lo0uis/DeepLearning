## [Day 2 : Regression]

- ```Regression``` : (0-100)과 같이 범위가 넓은 값을 예측.

```regression```은 training Data를 가지고 train을 함. 다 하게 되면 model(학습된 데이터)을 생성.\
x값이 들어오면 ```regression```은 model을 통해 y값을 내보냄.\
=> ```Linear Regression```
(x는 예측을 위한 자료(feature), y는 예측된 자료)

가설을 세움. Linear한 모델이 우리가 가진 데이터에 맞겠다. 하는 가설.
(실제로 대부분 우리 주위의 현상들은 Linear로 되어있음.)\
=> ```Linear Hypothesis```
```
H(x) = Wx +b
```

여러 선 중에서 어떤 선이 가장 갖고 있는 데이터와 잘 맞는 선인지 찾아야함.\
=> 실제데이터와 가설로 세운 선과의 거리를 계산.\
=> Cost function
(H(x) - y)^2 =>거리의 차를 제곱함.

** *거리의 차를 제곱하게되면, 전부 양수로 표현가능. 또한 거리차이가 많이나는 것은 패널티를 더 주게되는 효과도 있다.**
￼

```Linear Hypothesis```와 ```Cost Function```을 합치면 다음과 같다.
￼
```Cost Function```은 W와 b의 Function이 된다.\
```Linear Regression```은 ```Cost Fuction```의 값을 작게하는게 목표.
즉, 가장 작게만드는 W와 b 찾기.
