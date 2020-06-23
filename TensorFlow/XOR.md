# TensorFlow를 이용하여 XOR 문제 해결해 보기

TensorFlow Library를 이용하여 XOR 문제를 해결해 보자. 필요하다면 Keras를 사용해도 되고, 원한다면 Numpy Library만으로도 해결할 수 있다.
XOR 문제에 대한 설명은 김성훈 홍콩과기대 교수님의 [모두를 위한 딥러닝 강좌](https://www.youtube.com/watch?v=n7DNueHGkqE&feature=youtu.be)에 잘 설명되어 있다.

![Neural Networks for XOR Problems (Ref. Project of SE389, Fall 2018)](/img/XOR_1.png)

Ref of Figure. Final Project of SE389, Fall 2018

# 해설

이 문제를 해결하기 위해 Fig와 동일한 형태의 Neural Network를 작성하여 문제를 풀어보면
학습이 기대했던 것 만큼 잘 이루어지지는 못한 사실을 확인할 수 있을 것이다.
이 문제는 Neural network를 넓거나 깊게 구성하는 것을 통해 보완 / 해결할 수 있다.
이를 구현한 예시 코드는 다음과 같다.

```Python
import tensorflow as tf
import numpy as np
import matplotlib.pyplot as plt

step_lst = []
loss_lst = []

# Definition of Neural Network
X = tf.placeholder(tf.float32, shape=(None,2), name="X")
Y = tf.placeholder(tf.float32, shape=(None,1), name="Y")

init = tf.contrib.layers.xavier_initializer()

W1 = tf.get_variable(name="Weight_1", shape=[2,4], initializer=init)
B1 = tf.get_variable(name="Bias_1", shape=[4], initializer=init)
L1 = tf.nn.sigmoid(tf.matmul(X, W1)+B1, name="Layer_1")

W2 = tf.get_variable(name="Weigh_2t", shape=[4,1], initializer=init)
B2 = tf.get_variable(name="Bias_2", shape=[1], initializer=init)
H = tf.nn.sigmoid(tf.matmul(L1, W2)+B2, name="Hypothesis")

# Definition of Cost function & Optimizer
cost = -tf.reduce_mean(Y*tf.log(H) + (1-Y)*tf.log(1-H))
optimizer = tf.train.GradientDescentOptimizer(0.1)
train = optimizer.minimize(cost)

with tf.Session() as sess:
  sess.run(tf.global_variables_initializer())
  
  Y_pred = tf.cast(H > 0.5, dtype=tf.float32)
  accuracy = tf.reduce_mean(tf.cast(tf.equal(Y, Y_pred), dtype=tf.float32))
  
  # Definition of training & test set
  x = np.array([[0,0],[0,1],[1,0],[1,1]])
  y = np.array([[0],[1],[1],[0]])

  for step in range(10000+1):
    sess.run(train, feed_dict={X:x, Y:y})

    if step%100 == 0:
      if step%1000 == 0:
        print("step:", step)
        print("accuracy:", accuracy.eval(feed_dict={X:x, Y:y}))

      # Save loss values for plotting
      step_lst.append(step)
      loss_lst.append(cost.eval(feed_dict={X:x, Y:y}))

plt.plot(np.array(step_lst), np.array(loss_lst))
plt.xlabel("Epoch")
plt.ylabel("Loss")
plt.show()
```

# 실행 결과

```Text
step: 0
accuracy: 0.5
step: 1000
accuracy: 0.5
step: 2000
accuracy: 0.75
step: 3000
accuracy: 1.0
step: 4000
accuracy: 1.0
step: 5000
accuracy: 1.0
step: 6000
accuracy: 1.0
step: 7000
accuracy: 1.0
step: 8000
accuracy: 1.0
step: 9000
accuracy: 1.0
step: 10000
accuracy: 1.0
```

![XOR](/img/XOR_2.png)
