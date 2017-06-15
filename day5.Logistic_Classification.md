## [Day 5 : Logistic (regression) classification]

- Logistic classification알고리즘은 다른 classification알고리즘과 비교해서, 정확도가 굉장히 높다.
- neural network과 Deep Learning의 중요한 component.

이전의 regression은 단순히 숫자를 예측하는 것이었다면, classification은 정해진 것을 고르는 것.\
기계어로 학습 시키기위해 1과 0으로 하자.

- spam E-mail Detection : spam(1) of ham(0)
- Facebook feed : show(1) or hide(0)
- credit card fraudulent transaction detection : legitimate(1) or fraud(0)

Linear Regression은 logistic classification을 구현하는데 부적합.
- 0~1 범위를 넘어설 수 있음.
- 학습을 시킬때마다 weight가 변하고, 측정했던 값이 바뀜. 1이었던 변수가 0으로 바뀔 수 있음\
**=>```Sigmoid(logistic) function```를 사용하자.**

### Sigmoid(logistic) Funtion

![day5-1](/image_File/day5-1.png)

g(z)는 아무리 큰값, 작은 값을 가지더라도 0~1 범위에 닫혀있다.\
따라서 우리는 ```z = WX```, ```H(x) = g(z)``` 라 할 수 있다.

![day5-2](/image_File/day5-2.png)

W^TX대신 WX라 써도 된다. 똑같다.
****
## Cost Function & gradient decent
이전의 cost Function은 다음과 같다.
![day5-3](/image_File/day5-3.png)

하지만 Logistic classification은 hypothesis를 새롭게 정의했기 때문에, 그래프에 변형이 생긴다.
=>그에 맞는 Cost function을 재정의 해주자.

![day5-4](/image_File/day5-4.png)

이전의 cost Function을 사용하면 ```exponential term```이 생긴다.\
이것을 없애기 위해 log function을 사용했다.

![day5-5](/image_File/day5-5.png)

y=1일 때, C(H(x), y)= -log(H(x))
- H(x)=1 -> cost(1)=0
- H(x)=0 -> cost(0)=INF   (우리의 예측이 틀리면 Cost 값을 높게 주어 틀림을 알림.)

![day5-6](/image_File/day5-6.png)

y=0일 때, C(H(x), y)= -log(1-H(x))
- H(x)=0 -> cost(1)=0
- H(x)=1 -> cost(0)=INF

=>둘을 붙이면, 이전의 그래프 형태와 비슷해지겠다.\
**But, tensorflow에서 구현하기 힘듬. 둘을 합친 하나의 수식을 생성하자.**

![day5-7](/image_File/day5-7.png)\


cost function을 구했으니, gradient decent algorithm을 구하자.

![day5-8](/image_File/day5-8.png)