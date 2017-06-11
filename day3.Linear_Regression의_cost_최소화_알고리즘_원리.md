## [Day 3 : Linear Regression의 cost 최소화 알고리즘 원리]

![day3-1](/image_File/day3-1.png)

 ```Simplified hypothesis``` : 쉽게 설명하기위해,   원래의 Linear hypothesis에서 bias를 제거\
![day3-2](/image_File/day3-2.png)

- W의 값에 따라 Cost값이 변함.\
![day3-3](/image_File/day3-3.png)

- Cost가 최소가 되는 지점을 찾는 것이 목적.

우리는 이러한 Cost Function을 만들어 낼 수만 있으면, 기계적으로 찾을 수 있겠다.\
**=>Gradient descent algorithm(경사를 따라 내려가는 알고리즘)**

### Gradient descent algorithm
- Cost Function을 minimize할 때 사용됨
- 이외에도 많은 minimization problems를 해결할 때 사용됨
- Cost(W,b)에서 cost를 minimize시키는 W, b값을 찾을때 사용 됨. 여러개의 값이 있는 cost Function도 가능.\
![day3-4](/image_File/day3-4.png)

- 아무 지점에서나 시작가능
- W를 조금씩 바꿔가면서 cost를 줄여나감.
- 바뀌고나서 다시 기울기(경사도) 계산 및 낮은 쪽으로 이동.\
![day3-5](/image_File/day3-5.png)

Cost Function을 미분할 때 수식을 간단히 하기위해 1/2를 곱함.\
![day3-6](/image_File/day3-6.png)\
(alpha : Learning rate = 0.1)\
현재 W값에 alpha * cost’(W)를 뺏다.\
**=> 기울기가 양수라면 왼쪽으로, 음수라면 오른쪽으로 W값이 이동하겠다.**\
![day3-7](/image_File/day3-7.png)

미분을 하는 절차. 최종 Gradient descent algorithm을 완성함.\
여러번 실행시켜 W를 변화시켜서 minimize된 값을 구하는 것.\
![day3-8](/image_File/day3-8.png)

최종 결과 값.

### <주의해야할 점>

![day3-9](/image_File/day3-9.png)

Cost Function이 위와 같은 형태이면 위치에 따라 Gradient descent algorithm을 적용한 값의 결과가 다르게 나옴. 부적절함.\
![day3-10](/image_File/day3-10.png)

위와 같은 함수만 Gradient descent algorithm 사용가능하겠다. 잘 알아보고 사용해야한다.
