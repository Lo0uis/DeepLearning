## [Lab 3 : Minimizing Cost]

![lab3-1](/image_File/lab3-1.png)

간략화된 hypothesis로 보자. 원래는 H(x) = Wx + b, b가 생략됨.\
가장 최저의 cost를 구해보자.
```
import tensor flow as tf
import matplotlib.pyplot as plt
```
```
#단순한 형태의 X,Y값을 주자.
X = [1, 2, 3]
Y = [1, 2, 3]
```
```
#placeholder로 선언함으로서, 이값을 변화시키며 test해보겠다.
W = tf.placeholder(tf.float32)
```
```
# Our hypothesis for linear model X * W
hypothesis = X * W
```
```
# cost/loss function 그래프 구성
cost = tf.reduce_mean(tf.square(hypothesis - Y))
```
```
# session을 열고 variables 초기화.
sess = tf.Session()
sess.run(tf.global_variables_initializer())
```
```
#그래프를 그리기위해, W와 Cost값 저장소 생성
W_history = []
cost_history = []
```
```
#-3~-5범위를 0.1간격으로.
for i in range(-30, 50):
	curr_W = i * 0.1
	curr_cost = sess.run(cost, feed_dict={W: curr_W})
	W_history.append(curr_W)
	cost_history.append(curr_cost)
```
```
#x축은 W, y축은 cost인 Cost Function를 시각화 해보자.
plt.plot(W_history, cost_history)
plt.show()
```
![lab3-2](/image_File/lab3-2.png)

이 그래프에서 기울기를 구하기위해, 미분을 하겠다.\
1을기준으로 오른쪽은 기울기가 양수, 왼쪽은 음수.\
기울기가 양수일 경우, W는 왼쪽(-)으로 이동.\
기울기가 음수일 경우, W는 오른쪽(+)으로 이동.\
```W:=W-기울기값```
**=>이것을 tensorflow로 구현 해보자.**

```
learning_rate = 0.1
gradient = tf.reduce_mean((W * X - Y) * X)
descent = W - learning_rate * gradient
update = W.assign(descent)
```
**<중요!!!>**
- tensorflow는 W = W이런식으로 못하기 때문에, W가 갖고있는 함수 중 assign함수를 사용하여 변경된 W를 저장한다. 이때의 명령어를 update라 지정. 앞으로 update하면 W값이 assign된다라고 생각하자.

- 하지만 굳이 미분을 하지 않아도 tensorflow의 함수를 쓰면 된다
```
# Minimize: Gradient Descent Magic
	optimizer = tf.train.GradientDescentOptimizer(learning_rate=0.1)
	train = optimizer.minimize(cost)
```

***gradient(기울기)를 직접 값을 바꿔서 적용시키고 싶을 때는 다음과 같이 하면 된다.**\
밑에 코드는 optimizer를 써서 한 것과 직접 미분해서 한 값의 차이를 보기위해 gvs값을 변경하지않고 바로 apply하였다.

```
# Manual gradient
gradient = tf.reduce_mean((W * X - Y) * X) * 2
```
```
# cost/loss function
cost = tf.reduce_mean(tf.square(hypothesis - Y))
```
```	
# Minimize: Gradient Descent Magic
optimizer = tf.train.GradientDescentOptimizer(learning_rate=0.01)
train = optimizer.minimize(cost)
```
```	
# Get gradients
gvs = optimizer.compute_gradients(cost, [W])
```
```
# Optional: 이 부분을 수정하면 된다. 
```
```
# gvs = [(tf.clip_by_value(grad, -1., 1.), var) for grad, var in gvs]
```
```
# Apply gradients
apply_gradients = optimizer.apply_gradients(gvs)
```
```
# Launch the graph in a session.
sess = tf.Session()
```
```
# Initializes global variables in the graph.
sess.run(tf.global_variables_initializer())
	
for step in range(100):
	print(step, sess.run([gradient, W, gvs]))
	sess.run(apply_gradients)

# Same as sess.run(train)
```
