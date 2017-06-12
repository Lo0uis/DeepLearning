## [Lab 4-2 : Loading Data from File]
￼
![lab4.2-1](/image_File/lab4.2-1.png)\
(test_score.csv)

csv는 ','로 구분되는 data file.
데이터가 많아지면, 소스코드에 직접 쓰기 불편함.\
**=>파일에 저장해서 불러오자.**

### <1개 파일 갖고오기>
numpy의 loadtxt를 이용하자. 이때, csv파일의 데이터값은 모두 같은 타입이어야 한다.
```
import numpy as np
```
```
#X, Y data
xy = np.loadtxt('test_score.csv', delimiter=',', dtype=np.float32)
x_data = xy[:, 0:-1]
y_data = xy[:, [-1]]
```
=>파일의 값들은 x,y 구분이 없기 때문에, 직접 ```slicing``` 해줘야 한다.\
**```slicing```에 관한 자료는 [이곳](http://cs231n.github.io/python-numpy-tutorial/)을 참조하기 바람.**

**또한 Numpy를 사용하면, 더 많은 기능을 할 수도 있다.\
[이곳](http://slides.com/wigging/numpy#/9) 참조.**

```
# 갖고 온 데이터가 맞는지 가장 먼저 확인해볼 것.
print(x_data.shape, x_data, len(x_data))
print(y_data.shape, y_data)
```
****
### <여러 파일 갖고오기>
파일을 여러개 불러올 때는 용량이 커서 프로그램이 실행이 안될 수 있다.

![lab4.2-2](/image_File/lab4.2-2.png)

### Queue Runner를 사용하자.
**=> 메모리를 굉장히 절약할 수 있겠다.**
1. 파일들을 큐에 쌓는다.
```
filename_queue = tf.train.string_input_producer(
    ['test_score_01.csv', 'test_score_02.csv', ...], shuffle=False, name='filename_queue')
```
2. Reader로 연결하고 Decoding 후 다시 큐에 쌓음.
```
#Reader를 정의해줌. text파일을 읽을 때 일반적으로 아래와 같이 함.
reader = tf.TextLineReader()
key, value = reader.read(filename_queue)
```
```
#읽어온 value를 decode_csv로 parsing하자.
#읽어올 때의 data type을 정해줄 수 있다. float은 [0.]으로 정의.
record_defaults = [[0.], [0.], [0.], [0.]]
xy = tf.decode_csv(value, record_defaults=record_defaults)
```
3. 학습을 할 때 파일 전체가 필요한 것은 아니기 때문에, 필요한 batch만큼만 읽어와서 사용할 수 있다.
```
# collect batches of csv in
#X_data와 Y_data를 10개씩 가져옴.
train_x_batch, train_y_batch = \
    tf.train.batch([xy[0:-1], xy[-1:]], batch_size=10)
```
****
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
```
#Launch the graph in a session
sess = tf.Session()
```
```
 #Initializes global Variables in the graph
sess.run(tf.global_variables_initializer())
```
### 1개 파일 갖고올 때
```
 for step in range(2001):
    cost_val, hy_val, _ = sess.run(
        [cost, hypothesis, train], feed_dict={X: x_data, Y: y_data})
    if step % 10 == 0:
        print(step, "Cost: ", cost_val, "\nPrediction:\n", hy_val)
```
### 여러개 파일 갖고올 때
```
# Start populating the filename queue.
coord = tf.train.Coordinator()
threads = tf.train.start_queue_runners(sess=sess, coord=coord)

for step in range(2001):
    x_batch, y_batch = sess.run([train_x_batch, train_y_batch])
    cost_val, hy_val, _ = sess.run(
        [cost, hypothesis, train], feed_dict={X: x_batch, Y: y_batch})
    if step % 10 == 0:
        print(step, "Cost: ", cost_val, "\nPrediction:\n", hy_val)

coord.request_stop()
coord.join(threads)
```
****
계산된 hypothesis를 가지고 값을 예측해 볼수도 있다.
```
# Ask my score
print("Your score will be ", sess.run(
    hypothesis, feed_dict={X: [[100, 70, 101]]}))

print("Other scores will be ", sess.run(hypothesis,
                                        feed_dict={X: [[60, 70, 110], [90, 100, 80]]}))
```
****
위에선, batch의 순서를 shuffle을 false로 했지만, shuffle을 사용해볼 수도 있다. 
```
# min_after_dequeue defines how big a buffer we will randomly sample
#   from -- bigger means better shuffling but slower start up and more
#   memory used.
# capacity must be larger than min_after_dequeue and the amount larger
#   determines the maximum we will prefetch.  Recommendation:
#   min_after_dequeue + (num_threads + a small safety margin) * batch_size
min_after_dequeue = 10000
capacity = min_after_dequeue + 3 * batch_size
example_batch, label_batch = tf.train.shuffle_batch(\
[example, label], batch_size=batch_size, capacity=capacity, min_after_dequeue=min_after_dequeue)
  return example_batch, label_batch
```

