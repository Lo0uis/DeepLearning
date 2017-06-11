## [Lab 2 : Linear Regression to tensorflow]
￼
![lab2-1](/image_File/lab2-1.png)

H(x)는 예측 value, y는 true value
W, b값에 따라 값이 달라짐. **=> 이 값들을 이용해 제일 작은 값 찾기.**
```
#X, Y data
x_train = [1, 2, 3]
y_train = [1, 2, 3]
```

```
X = tf.placeholder(tf.float32, shape = [none])
Y = tf.placeholder(tf.float32, shape = [none])
```
=>이렇게 선언해놓고, 밑에서 feed_dict로 값을 넣어줄수도 있겠다.

```
#weight, bias
tf.variable : 텐서플로우형 변수 선언. (여기서 변수는 사용자가 아니라 텐서플로우가 사용하는 변수)
(자체적으로 텐서플로우가 값을 변경시킴. 즉, trainable한 variable이다.)
shape과 값을 주면 된다. 이때 우리는 값을 모르기 때문에 랜덤한 값을 넣어준다.
tf.random_normal([1]) : 값이 1개인 일차원 array (Rank = 1)
```
```
#Hypothesis XW+b
hypothesis = x_train * W + b
```
```
#cost/Loss function
cost = tf.reduce_mean(tf.square(hypothesis - y_train))
#여기서 reduce_mean은
t = [1. , 2. , 3. , 4.]
tf.reduce_mean(t) = 2.5 #평균을 내줌.
```
```
#cost Minimize(Gradient Descent) - magic으로 보자. 아직설명안함.
optimizer = tf.train.GradientDescentOptimizer(learning_rate = 0.01)
train = optimizer.minimize(cost) #cost를 최소화함.
```
```
#Launch the graph in a session
sess = tf.Session()
 #Initializes global Variables in the graph
sess.run(tf.global_variables_initializer()) : W,b라는 변수를 사용하기전에 이것을 선언해주어야함.
```
```
#Fit the line - 1
for step in range(2001):
	sess.run(train)
	if step %20 == 0 :
		print(step, sess.run(cost), sess.run(W), sess.run(b))
```
=>점점 cost가 작아짐을 확인.
```
#Fit the line - 2
for step in range(2001) :
	cost_val, W_val, b_val, _ = \
		sess.run([cost, W, b, train])
	if step %20 == 0 :
		print(step, cost_val, W_val, b_val)
```
이때, sess.run([cost, W, b, train], feed_dict{X: [1,2,3], Y : [1,2,3]}) 하면 placeholder사용가능.

### <tf 작동과정>

** 1. 그래프 빌드(설계)하기**

￼![lab2-2](/image_File/lab2-2.png)

**2. sess.run을 통해 그래프 실행\
=> feed_dict={X:[1,2,3], Y : [1,2,3]}**

**3. 결과로 연산 또는 값 리턴\
=> w와b의값을 리턴받아 함수 완성.**
