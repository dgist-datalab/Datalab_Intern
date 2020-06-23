# Logistic Regression

Logistic regression을 이용하면 스팸 메일 감지 등 Binary classification을 실시할 수 있다.
다음 [강좌](https://deeplearningzerotoall.github.io/season2/lec_tensorflow.html)를 공부하고 따라하면서 간단한 Logistic regression 모델을 제작하여 보자.

이러한 Linear regression의 문제를 직접 예시를 보면서 확인해보도록 하자.

먼저 이 문제를 Linear regression으로 학습시키고, 학습된 Hypothesis를 출력시키는 코드는 다음과 같다.

<!--
```Python
import tensorflow as tf
import numpy as np
import matplotlib.pyplot as plt

learning_rate = 0.005
epoches = 20000

x_data = np.array([[0], [0.1], [0.2], [0.5], [1], [2], [3], [5], [10]])
y_data = np.array([[0], [0], [0], [0], [0], [1], [1], [1], [1]])

x = tf.placeholder(tf.float32, [None, 1])
y = tf.placeholder(tf.float32, [None, 1])

w = tf.Variable(tf.random_uniform([1,1]))
b = tf.Variable(tf.random_uniform([1]))

y_pred = tf.matmul(x, w) + b
predicted = tf.cast(y_pred > 0.5, dtype=tf.float32)
accuracy = tf.reduce_mean(tf.cast(tf.equal(predicted, y), dtype=tf.float32))

cost = tf.reduce_mean(tf.square(y-y_pred))
optimizer = tf.train.GradientDescentOptimizer(learning_rate).minimize(cost)

with tf.Session() as sess:
  sess.run(tf.global_variables_initializer())
  print("Initial State")
  print("w:", w.eval()[0,0], "b:", b.eval()[0])
  print("cost:", cost.eval(feed_dict={x: x_data, y: y_data})){

  print("accuracy:", accuracy.eval(feed_dict={x: x_data, y: y_data}))
  for step in range(epoches+1):
    sess.run(optimizer, feed_dict={x: x_data, y: y_data})
    if step%500 == 0:
      print("step:", step, "cost:", cost.eval(feed_dict={x: x_data, y: y_data}), 
            "accuracy:", accuracy.eval(feed_dict={x: x_data, y: y_data}), sep='\t')
  print("Final State")
  print("w:", w.eval()[0,0], "b:", b.eval()[0])
  print("accuracy:", accuracy.eval(feed_dict={x:x_data, y:y_data}))
  weight = w.eval()[0,0]
  bias = b.eval()[0]

plt.plot(x_data, y_data)
plt.plot([0, 10], [bias, weight*10+bias])
plt.axis([0, 10, -1, 2])
plt.legend(["Data", "Hypothesis"])
plt.show()
```
-->
학습의 결과를 그래프로 그려보면, Linear regression의 cost function은 수렴하였지만, Hypothesis가 모든 데이터를 정확하게 추론하지는 못하였음을 발견할 수 있다.
실행 결과와 그래프로 표현된 Hypothesis는 다음과 같다.

```Text
Initial State
w: 0.28528583 b: 0.7589016
cost: 1.3864064
accuracy: 0.44444445
step:	0	cost:	1.0683331	accuracy:	0.44444445
step:	500	cost:	0.110783085	accuracy:	0.8888889
step:	1000	cost:	0.11033854	accuracy:	0.8888889
step:	1500	cost:	0.11033752	accuracy:	0.8888889
step:	2000	cost:	0.1103375	accuracy:	0.8888889
step:	2500	cost:	0.11033751	accuracy:	0.8888889
step:	3000	cost:	0.11033751	accuracy:	0.8888889
step:	3500	cost:	0.11033751	accuracy:	0.8888889
step:	4000	cost:	0.11033751	accuracy:	0.8888889
step:	4500	cost:	0.11033751	accuracy:	0.8888889
step:	5000	cost:	0.11033751	accuracy:	0.8888889
step:	5500	cost:	0.11033751	accuracy:	0.8888889
step:	6000	cost:	0.11033751	accuracy:	0.8888889
step:	6500	cost:	0.11033751	accuracy:	0.8888889
step:	7000	cost:	0.11033751	accuracy:	0.8888889
step:	7500	cost:	0.11033751	accuracy:	0.8888889
step:	8000	cost:	0.11033751	accuracy:	0.8888889
step:	8500	cost:	0.11033751	accuracy:	0.8888889
step:	9000	cost:	0.11033751	accuracy:	0.8888889
step:	9500	cost:	0.11033751	accuracy:	0.8888889
step:	10000	cost:	0.11033751	accuracy:	0.8888889
step:	10500	cost:	0.11033751	accuracy:	0.8888889
step:	11000	cost:	0.11033751	accuracy:	0.8888889
step:	11500	cost:	0.11033751	accuracy:	0.8888889
step:	12000	cost:	0.11033751	accuracy:	0.8888889
step:	12500	cost:	0.11033751	accuracy:	0.8888889
step:	13000	cost:	0.11033751	accuracy:	0.8888889
step:	13500	cost:	0.11033751	accuracy:	0.8888889
step:	14000	cost:	0.11033751	accuracy:	0.8888889
step:	14500	cost:	0.11033751	accuracy:	0.8888889
step:	15000	cost:	0.11033751	accuracy:	0.8888889
step:	15500	cost:	0.11033751	accuracy:	0.8888889
step:	16000	cost:	0.11033751	accuracy:	0.8888889
step:	16500	cost:	0.11033751	accuracy:	0.8888889
step:	17000	cost:	0.11033751	accuracy:	0.8888889
step:	17500	cost:	0.11033751	accuracy:	0.8888889
step:	18000	cost:	0.11033751	accuracy:	0.8888889
step:	18500	cost:	0.11033751	accuracy:	0.8888889
step:	19000	cost:	0.11033751	accuracy:	0.8888889
step:	19500	cost:	0.11033751	accuracy:	0.8888889
step:	20000	cost:	0.11033751	accuracy:	0.8888889
Final State
w: 0.1192095 b: 0.15569328
accuracy: 0.8888889
```

![linear regression](/img/logistic_1.png)

이제 이 문제를 Logistic regression을 이용하여 풀어보자. 데이터를 그대로 학습시키면 학습이 잘 이루어지지 않기 때문에 데이터를 정규화 시켜줄 필요성이 있다.

데이터의 정규화는 일반적으로 (<!--$$\frac{x - x_{average} }{x_{std}}$$-->)으로 이루어진다.

이 문제를 풀어본 예시 코드는 다음과 같다.

<!--
```Python
import tensorflow as tf
import numpy as np
import matplotlib.pyplot as plt

learning_rate = 0.005
epoches = 20000

x_data = np.array([[0], [0.1], [0.2], [0.5], [1], [2], [3], [5], [10]])
y_data = np.array([[0], [0], [0], [0], [0], [1], [1], [1], [1]])

def sigmoid(x):
  return 1 / (1 + np.exp(-x))

def train(x_data, y_data):
  x_norm = (x_data - np.mean(x_data)) / np.std(x_data)
  y_norm = y_data

  x = tf.placeholder(tf.float32, [None, 1])
  y = tf.placeholder(tf.float32, [None, 1])

  w = tf.Variable(tf.random_normal([1,1]))
  b = tf.Variable(tf.random_normal([1]))

  y_pred = tf.sigmoid(tf.matmul(x, w) + b)
  predicted = tf.cast(y_pred > 0.5, dtype=tf.float32)
  accuracy = tf.reduce_mean(tf.cast(tf.equal(predicted, y), dtype=tf.float32))

  cost = -tf.reduce_mean(y*tf.log(y_pred) + (1-y)*tf.log(1-y_pred))
  optimizer = tf.train.GradientDescentOptimizer(learning_rate).minimize(cost)

  with tf.Session() as sess:
    sess.run(tf.global_variables_initializer())
    print("Initial State")
    print("w:", w.eval()[0,0], "b:", b.eval()[0])
    print("cost:", cost.eval(feed_dict={x: x_norm, y: y_norm}))
    print("accuracy:", accuracy.eval(feed_dict={x: x_norm, y:y_norm}))
    for step in range(epoches+1):
      sess.run(optimizer, feed_dict={x: x_norm, y: y_norm})
      if step%500 == 0:
        print("step:", step, "cost:", cost.eval(feed_dict={x: x_norm, y: y_norm}), 
              "accuracy:", accuracy.eval(feed_dict={x:x_norm, y:y_norm}), sep="\t")

    print("Final State")
    print("w:", w.eval()[0,0], "b:", b.eval()[0])
    print("accuracy:", accuracy.eval(feed_dict={x:x_norm, y:y_norm}))

    weight = w.eval()[0,0]/np.std(x_data)
    bias = b.eval()[0] - np.mean(x_data)*weight

    plt.plot(x_norm, y_norm)
    plt.plot(np.arange(-2,10,0.1), w.eval()[0,0]*np.arange(-2,10,0.1) + b.eval()[0])
    plt.plot(np.arange(-2,10,0.1), sigmoid(w.eval()[0,0]*np.arange(-2,10,0.1) + b.eval()[0]))
    plt.axis([-2,10,-3,3])
    plt.legend(["Normalized data", "W'X+b", "sigmoid(W'X+b)"])
    plt.show()
  return weight, bias

weight, bias = train(x_data, y_data)

plt.plot(x_data, y_data)
plt.plot(np.arange(0, 10, 0.1), weight*np.arange(0,10,0.1)+bias)
plt.plot(np.arange(0, 10, 0.1), sigmoid(weight*np.arange(0,10,0.1)+bias))
plt.axis([0,10,-3,3])
plt.legend(["Original data", "W'X+b", "sigmoid(W'X+b)"])
plt.show()
```
-->

```Text
Initial State
w: -0.08762686 b: -0.83853364
cost: 0.7651719
accuracy: 0.5555556
step:	0	cost:	0.7643202	accuracy:	0.5555556
step:	500	cost:	0.51053536	accuracy:	0.6666667
step:	1000	cost:	0.4196778	accuracy:	0.7777778
step:	1500	cost:	0.37055922	accuracy:	0.7777778
step:	2000	cost:	0.33723614	accuracy:	0.8888889
step:	2500	cost:	0.31211388	accuracy:	0.8888889
step:	3000	cost:	0.29205555	accuracy:	0.8888889
step:	3500	cost:	0.27545375	accuracy:	0.8888889
step:	4000	cost:	0.26136488	accuracy:	0.8888889
step:	4500	cost:	0.24918365	accuracy:	0.8888889
step:	5000	cost:	0.2384975	accuracy:	0.8888889
step:	5500	cost:	0.22901218	accuracy:	0.8888889
step:	6000	cost:	0.22051068	accuracy:	0.8888889
step:	6500	cost:	0.21282858	accuracy:	0.8888889
step:	7000	cost:	0.2058385	accuracy:	0.8888889
step:	7500	cost:	0.19943966	accuracy:	0.8888889
step:	8000	cost:	0.1935512	accuracy:	0.8888889
step:	8500	cost:	0.18810739	accuracy:	0.8888889
step:	9000	cost:	0.1830539	accuracy:	0.8888889
step:	9500	cost:	0.17834553	accuracy:	1.0
step:	10000	cost:	0.17394434	accuracy:	1.0
step:	10500	cost:	0.16981788	accuracy:	1.0
step:	11000	cost:	0.16593865	accuracy:	1.0
step:	11500	cost:	0.16228266	accuracy:	1.0
step:	12000	cost:	0.15882927	accuracy:	1.0
step:	12500	cost:	0.1555604	accuracy:	1.0
step:	13000	cost:	0.15246028	accuracy:	1.0
step:	13500	cost:	0.14951481	accuracy:	1.0
step:	14000	cost:	0.14671165	accuracy:	1.0
step:	14500	cost:	0.1440399	accuracy:	1.0
step:	15000	cost:	0.14148939	accuracy:	1.0
step:	15500	cost:	0.13905148	accuracy:	1.0
step:	16000	cost:	0.13671805	accuracy:	1.0
step:	16500	cost:	0.13448226	accuracy:	1.0
step:	17000	cost:	0.13233723	accuracy:	1.0
step:	17500	cost:	0.13027719	accuracy:	1.0
step:	18000	cost:	0.12829666	accuracy:	1.0
step:	18500	cost:	0.12639087	accuracy:	1.0
step:	19000	cost:	0.12455518	accuracy:	1.0
step:	19500	cost:	0.12278541	accuracy:	1.0
step:	20000	cost:	0.121078014	accuracy:	1.0
Final State
w: 5.549216 b: 1.1411173
accuracy: 1.0
```

![Normalized data](/img/logistic_2.png)

![Restored data](/img/logistic_3.png)
