## [Day 4 : multi-variable linear regression]

< reView >

###Linear Regression을 설계하기 위해 필요한 3가지
- Hypothesis(우리의 가설이 무엇인지)
- Cost Function(Cost를 어떻게 계산할 것 인지. 잘했는지/못했는지)
- Gradient descent Algorithm(최적화 알고리즘)
￼
![day4-1](/image_File/day4-1.png)

- 우리는 가설에서의 W, b를 학습한다. W, b의 함수 : Cost Function
- Cost Function에서 H(x)는 prediction(예측값), y는 true(실제값)
- Cost Function을 그려보면 밥그릇 모양.\ 
가장 낮은 부분(최적화된 곳)을 찾는 것이 Gradient descent algorithm.

****

이번엔 여러개의 입력에 대한 예측값을 구해보자.\
![day4-2](/image_File/day4-2.png)￼

학습해야할 데이터가 많아진 것 뿐. 원리는 똑같다.\
![day4-3](/image_File/day4-3.png)\
Cost Fuction 또한 마찬가지.

H의 입력값이 많아지면, 길어지기 때문에, 보기좋게 Matrix로 구현시켜보자.\
![day4-4](/image_File/day4-4.png)\
 
실제 데이터 예시를 보자.\
![day4-5](/image_File/day4-5.png)\
(73, 80, 75, 152) 이렇게 가로줄 하나를 instance라 한다.\
총 5개의 instance. \
각각을 식에 대입하여 계산하면, 비효율적.\
![day4-6](/image_File/day4-6.png)\

행렬의 특성을 이용해서 식을 변형시켜보았다.\
X = [5, 3] matrix = [instance의 갯수, x의 variable의 갯수]\
W = [3, 1] matrix : 안주어지는 경우가 많음. 충분히 유추해볼 수 있겠다.\
H(x) = [5, 1] matrix = [instance의 갯수, 결과값은 당연히 1]\
=>weight의 크기를 결정해야함.\
***가끔씩 x의 instance갯수가 미지수로 나오는 경우도 있음. 충분히 가변적이다. => 출력에도 영향 미침.**\
***실제 구현에서는 그 값을 -1 또는 none(tensorflow)이라고 표시.**\
**=>array의 갯수가 위와 같으면, n개 즉, 여러개가 들어올 수 있겠다. 라고 생각하기.**\
![day4-7](/image_File/day4-7.png)\
output이 2개이상일 때도, 행렬을 이용해서 하면 훨씬 보기쉽겠다.
****
###<총정리>
![day4-8](/image_File/day4-8.png)\
x가 하나로 주어지지않고 여러개로 주어지는 경우가 많기 때문에, 행렬을 많이 사용하게 될 것이다.

* 주의할 점
    * theory : 수식을 쓸 때는 Wx이렇게 많이 하지만, 구현할 때는 tensorflow에서 XW로 하면 행렬로 인식하기때문에, XW로 할 것!! 의미는 똑같음.
