## [Lab 1 : tensorflow]

- ```tf.constant(“”)``` : constant(노드) 생성
- ```tf.Session()``` : 노드의 Session 생성
- ```print(tf.Session().run(tf.constant(“”)))``` : constant출력.

*Sess.run을 해야 해당 값을 출력함. 그냥 출력하면 tensor값이 출력됨.

**< tf 작동과정 >**

1. 그래프 빌드(설계)하기
2. sess.run을 통해 그래프 실행
3. 결과로 연산 또는 값 리턴

- ```Placeholder``` : 입력을 받을 준비가 된 것.

예를 들어,
```
a = tf.Placeholder(tf.float32)
b = tf.Placeholder(tf.float32)
adder_node = a + b
print(sess.run(adder_node, feed_dict = {a:3, b:4.5})) : feed_dict에서 a,b에 값을 넣어준다.
이때 상수값외에도 배열이나 리스트도 가능하다.
print(sess.run(adder_node, feed_dict = {a:[1, 3], b:[2, 4]})) = [   3.   7.   ]
```

**< Tensor에 대해 알아보자 >**

- 잘 사용하면 프로그래밍이 쉽겠다.
- array로 되어있음

- Rank : 몇차원으로 되어있나
	- scalar : 0차원(rank = 0)
	- Vector : 1차원(rank = 1)
	- Matrix : 2차원(rank = 2)
	- n-Tensor : n차원(rank = n)

- Shape : 각각의 elements가 몇개 들어있나

- Type : 대부분 float32, int32