## [Lab 4-1 : multi-variable linear regression to tensorflow]
￼
![lab4-1](/image_File/lab4-1.png)

X1, X2, X3는 시험 점수, Y는 다음시험의 예상 점수

### <1. 변수 각각 선언>
```
#X, Y data
x1_data = [73., 93., 89., 96., 73.]
x2_data = [80., 88., 91., 98., 66.]
x3_data = [75., 93., 90., 100., 70.]

y_data = [152., 185., 180., 196., 142.]
```

```
# placeholders for a tensor that will be always fed.
x1 = tf.placeholder(tf.float32)
x2 = tf.placeholder(tf.float32)
x3 = tf.placeholder(tf.float32)

Y = tf.placeholder(tf.float32)
```
=>이렇게 선언해놓고, 밑에서 feed_dict로 값을 넣어줄수도 있겠다.

```
#weight, bias
w1 = tf.Variable(tf.random_normal([1]), name='weight1')
w2 = tf.Variable(tf.random_normal([1]), name='weight2')
w3 = tf.Variable(tf.random_normal([1]), name='weight3')
b = tf.Variable(tf.random_normal([1]), name='bias')
```

**=>변수가 많아지면 코드도 엄청 많아짐. 줄이자.**
****
### <2. Matrix로 선언>

```
#X, Y data
x_data = [[73., 80., 75.],
          [93., 88., 93.],
          [89., 91., 90.],
          [96., 98., 100.],
          [73., 66., 70.]]
y_data = [[152.],
          [185.],
          [180.],
          [196.],
          [142.]]
```
```
# placeholders for a tensor that will be always fed.
X = tf.placeholder(tf.float32, shape=[None, 3])
Y = tf.placeholder(tf.float32, shape=[None, 1])

W = tf.Variable(tf.random_normal([3, 1]), name='weight')
b = tf.Variable(tf.random_normal([1]), name='bias')
```

```
#Hypothesis XW+b
hypothesis = x1 * w1 + x2 * w2 + x3 * w3 + b
```
```
#cost/Loss function
cost = tf.reduce_mean(tf.square(hypothesis - Y))
```
```
# Minimize. Need a very small learning rate for this data set
optimizer = tf.train.GradientDescentOptimizer(learning_rate=1e-5)
train = optimizer.minimize(cost)
```
=>1e-5는 0.01, 0.001과 같이 주는것보다 깔끔함.
```
#Launch the graph in a session
sess = tf.Session()
 #Initializes global Variables in the graph
sess.run(tf.global_variables_initializer()) : W,b라는 변수를 사용하기전에 이것을 선언해주어야함.
```
```
#Fit the line
 cost_val, hy_val, _ = sess.run([cost, hypothesis, train],
                                   feed_dict={x1: x1_data, x2: x2_data, x3: x3_data, Y: y_data})
    if step % 10 == 0:
        print(step, "Cost: ", cost_val, "\nPrediction:\n", hy_val)
```
