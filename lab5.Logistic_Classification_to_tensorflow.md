## [Lab 5 : Logistic Classification to tensorflow]
￼
![lab5-1](/image_File/lab5-1.png)

Hypothesis Function : ```sigmoid function```
Cost Function : y(실제 값)와 H(x)(예측 값)의 값이 가까울수록 작은값을 가지는 함수.\
=>Cost를 작게 만드는 W를 구하자(최적화).

### <1. 코드 내부에서 변수 선언>
```
#X, Y data ; x_data = [x1,x2]   y_data = [0] or [1] 즉, binary값.
x_data = [[1, 2],
          [2, 3],
          [3, 1],
          [4, 3],
          [5, 3],
          [6, 2]]
y_data = [[0],
          [0],
          [0],
          [1],
          [1],
          [1]]
```
```
# placeholders for a tensor that will be always fed.
X = tf.placeholder(tf.float32, shape=[None, 2])
Y = tf.placeholder(tf.float32, shape=[None, 1])
```
```
#weight, bias
W = tf.Variable(tf.random_normal([2, 1]), name='weight')
b = tf.Variable(tf.random_normal([1]), name='bias')
```
Weight에서 [2,1]은 들어오는 값이 2개(x갯수), 나가는 값이 1개(y갯수)를 의미.\
Bias는 항상 나가는 값의 갯수와 동일.

****
### <2. 파일에서 데이터 가져오기>
```
import numpy as np
```
```
#X, Y data
xy = np.loadtxt('test_diabetes.csv', delimiter=',', dtype=np.float32)
x_data = xy[:, 0:-1]
y_data = xy[:, [-1]]
```
```
# 갖고 온 데이터가 맞는지 가장 먼저 확인해볼 것.
print(x_data.shape, y_data.shape)
```
```
# placeholders for a tensor that will be always fed.
X = tf.placeholder(tf.float32, shape=[None, 8])
Y = tf.placeholder(tf.float32, shape=[None, 1])
```
x는 8개가 들어올 것.
```
W = tf.Variable(tf.random_normal([8, 1]), name='weight')
b = tf.Variable(tf.random_normal([1]), name='bias')
```
****
![lab5-2](/image_File/lab5-2.png)
```
#Hypothesis using sigmoid: tf.div(1., 1. + tf.exp(tf.matmul(X, W)))
hypothesis = tf.sigmoid(tf.matmul(X, W) + b)
```
![lab5-3](/image_File/lab5-3.png)
```
#cost/Loss function
cost = -tf.reduce_mean(Y * tf.log(hypothesis) + (1 - Y) * tf.log(1 - hypothesis))
```
![lab5-4](/image_File/lab5-4.png)
```
# Minimize. Need a very small learning rate for this data set
#우리는 tensorflow를 사용하기때문에 미분을 할 필요가 없다.
train = tf.train.GradientDescentOptimizer(learning_rate=0.01).minimize(cost)
```
```
# Accuracy computation
# True if hypothesis>0.5 else False
predicted = tf.cast(hypothesis > 0.5, dtype=tf.float32)
accuracy = tf.reduce_mean(tf.cast(tf.equal(predicted, Y), dtype=tf.float32))
```
0.5를 기준으로 True / False 결정.\
```tf.cast```는 True / False를 1 / 0 값으로 변경시켜줌.
```
# Launch graph
with tf.Session() as sess:
    # Initialize TensorFlow variables
    sess.run(tf.global_variables_initializer())
```
```
for step in range(10001):
  cost_val, _ = sess.run([cost, train], feed_dict={X: x_data, Y: y_data})
  if step % 200 == 0:
    print(step, cost_val)
```
```
# Accuracy report
h, c, a = sess.run([hypothesis, predicted, accuracy], feed_dict={X: x_data, Y: y_data})

print("\nHypothesis: ", h, "\nCorrect (Y): ", c, "\nAccuracy: ", a)
```
hypothesis는 0~1사이의 값, predicted는 0또는 1의 값이 나올 것.\
accuracy가 얼마 나오나 보자.
