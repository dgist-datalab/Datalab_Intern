# Linear Regression



## Problem

다음 [링크](https://github.com/girishkuniyal/Predict-housing-prices-in-Portland)에서 데이터를 다운받고, Python으로 읽어들여서 다음 [자료](http://cs229.stanford.edu/notes-spring2019/cs229-notes1.pdf)의 Page 6에서 설명하는 결과를 Linear regression으로 구하고 다음과 같은 그래프를 그리시오.

![Linear Regression](/img/linear_regression_1.png)

### Solution Examples
Linear regression을 위한 모델 정의와 Optimizer 정의는 다음과 같이 할 수 있다.

```Python
# Model definition
x = tf.placeholder(tf.float32, [None, 1])
y = tf.placeholder(tf.float32, [None, 1])

w = tf.Variable(tf.random_uniform([1,1]))
b = tf.Variable(tf.random_uniform([1]))
y_pred = tf.matmul(x, w) + b

# Optimizer
cost = tf.reduce_mean(tf.square(y-y_pred))
optimizer = tf.train.GradientDescentOptimizer(learning_rate).minimize(cost)
```

이렇게 정의한 모델은 다음과 같은 예시 코드를 통해 실행시킬 수 있다.
`feed_dict`를 통해 Training과 Evaluation set을 사용할 수 있다.

```Python
with tf.Session() as sess:
  sess.run(tf.global_variables_initializer())
  for step in range(epoch+1):
    sess.run(optimizer, feed_dict={x: X, y: Y})
    if step % 2000 == 0:
      print("step:", step)
      print("cost:", cost.eval(feed_dict={x: X, y: Y}))
  print("w:", w.eval(), "b:", b.eval())
  weight = w.eval()[0,0]
  bias = b.eval()[0]
```

하지만 이렇게 모델을 구성하였을 때, 항상 성공적으로 데이터를 학습하는 것은 아니라는 것을 확인할 수 있다. 다음 그림은 Cost function이 Global minimum에 수렴하지 못한 경우에 대한 그림이다.  
![Fig](/img/linear_regression_2.png)  
이를 방지하기 위해서는 Linear regression을 실시하기 전에 (<!--Data를 Normailization-->) 시켜주는 것이 필요하다.
이를 보완한 코드는 다음과 같다.

생략
<!--
```Python
def train(X, Y):
  epoch = 10000
  learning_rate = 0.01

  X_norm = X / X.max()
  Y_norm = Y / Y.max()

  # Model definition
  x = tf.placeholder(tf.float32, [None, 1])
  y = tf.placeholder(tf.float32, [None, 1])

  w = tf.Variable(tf.random_uniform([1,1]))
  b = tf.Variable(tf.random_uniform([1]))
  y_pred = tf.matmul(x, w) + b

  # Optimizer
  cost = tf.reduce_mean(tf.square(y-y_pred))
  optimizer = tf.train.GradientDescentOptimizer(learning_rate).minimize(cost)

  with tf.Session() as sess:
    sess.run(tf.global_variables_initializer())
    tf.summary.FileWriter("./log", sess.graph)
    for step in range(epoch+1):
      sess.run(optimizer, feed_dict={x: X_norm, y: Y_norm})
      if step % 2000 == 0:
        print("step:", step)
        print("cost:", cost.eval(feed_dict={x: X_norm, y: Y_norm}))
    print("w:", w.eval(), "b:", b.eval())
    weight = w.eval()[0,0]
    bias = b.eval()[0]
  weight = weight*(Y.max()/X.max())
  bias = bias*Y.max()
  return weight, bias
```
-->

그리고 데이터 입출력까지 포함한 코드 전체 구현은 다음과 같다.

생략
<!--
```Python
import numpy as np
import matplotlib.pyplot as plt
import tensorflow as tf

data = np.loadtxt("ex1data2.txt", dtype=np.float32, delimiter=",")

# Should reshape to prevent error from shape (N, ?)
size = data[:,0]
size.shape = (data.shape[0],1)

bedroom = data[:,1]
bedroom.shape = (data.shape[0],1)

price = data[:,2]
price.shape = (data.shape[0],1)

def train(X, Y):
  epoch = 10000
  learning_rate = 0.01

  X_norm = X / X.max()
  Y_norm = Y / Y.max()

  # Model definition
  x = tf.placeholder(tf.float32, [None, 1])
  y = tf.placeholder(tf.float32, [None, 1])

  w = tf.Variable(tf.random_uniform([1,1]))
  b = tf.Variable(tf.random_uniform([1]))
  y_pred = tf.matmul(x, w) + b

  # Optimizer
  cost = tf.reduce_mean(tf.square(y-y_pred))
  optimizer = tf.train.GradientDescentOptimizer(learning_rate).minimize(cost)

  with tf.Session() as sess:
    sess.run(tf.global_variables_initializer())
    tf.summary.FileWriter("./log", sess.graph)
    for step in range(epoch+1):
      sess.run(optimizer, feed_dict={x: X_norm, y: Y_norm})
      if step % 2000 == 0:
        print("step:", step)
        print("cost:", cost.eval(feed_dict={x: X_norm, y: Y_norm}))
    print("w:", w.eval(), "b:", b.eval())
    weight = w.eval()[0,0]
    bias = b.eval()[0]
  weight = weight*(Y.max()/X.max())
  bias = bias*Y.max()
  return weight, bias

# Plotting results
plt.figure(figsize = (21,6), dpi=72)

weight, bias = train(size, price)
print(weight, bias)

plt.subplot(131)
plt.scatter(size, price)
plt.title("Relation between size and price")
plt.xlabel("Size")
plt.ylabel("Price")

plt.plot(np.array([0,5000]), np.array([bias, 5000*weight+bias]))

weight, bias = train(bedroom, price)
print(weight, bias)

plt.subplot(132)
plt.scatter(bedroom, price)
plt.title("Relation between number of bedroom and price")
plt.xlabel("Bedroom")
plt.ylabel("Price")

plt.plot(np.array([0,5]), np.array([bias, 5*weight+bias]))

weight, bias = train(bedroom, size)
print(weight, bias)

plt.subplot(133)
plt.scatter(bedroom, size)
plt.title("Relation between size and number of bedroom")
plt.xlabel("Size")
plt.ylabel("Bedroom")

plt.plot(np.array([0,5]), np.array([bias, 5*weight+bias]))

plt.show()
```
-->

이 코드의 실행 결과는 다음과 같다.  
![Fig2](/img/linear_regression_3.png)

## Extra Problem

위의 데이터셋을 활용하여 가격을 방의 개수와 넓이와 관련된 함수로 표현해 보자.

### Solution Exmaples

생략
<!--
```Python
import numpy as np
import matplotlib.pyplot as plt
from mpl_toolkits.mplot3d import Axes3D

import tensorflow as tf

data = np.loadtxt("ex1data2.txt", dtype=np.float32, delimiter=",")

# Should reshape to prevent error from shape (N, ?)
size = data[:,0]
size.shape = (data.shape[0],1)

bedroom = data[:,1]
bedroom.shape = (data.shape[0],1)

price = data[:,2]
price.shape = (data.shape[0],1)

def train(X, Y, Z):
  epoch = 10000
  learning_rate = 0.01

  X_norm = X / X.max()
  Y_norm = Y / Y.max()
  Z_norm = Z / Z.max()

  # Model definition
  x = tf.placeholder(tf.float32, [None, 1])
  y = tf.placeholder(tf.float32, [None, 1])
  z = tf.placeholder(tf.float32, [None, 1])

  w_1 = tf.Variable(tf.random_uniform([1,1]))
  w_2 = tf.Variable(tf.random_uniform([1,1]))
  b = tf.Variable(tf.random_uniform([1]))
  z_pred = tf.matmul(x, w_1) + tf.matmul(y, w_2) + b

  # Optimizer
  cost = tf.reduce_mean(tf.square(z-z_pred))
  optimizer = tf.train.GradientDescentOptimizer(learning_rate).minimize(cost)

  with tf.Session() as sess:
    sess.run(tf.global_variables_initializer())
    
    for step in range(epoch+1):
      sess.run(optimizer, feed_dict={x: X_norm, y: Y_norm, z: Z_norm})
      if step % 2000 == 0:
        print("step:", step)
        print("cost:", cost.eval(feed_dict={x: X_norm, y: Y_norm, z:Z_norm}))
    
    print("w_1:", w_1.eval(), "w_2:", w_2.eval(), "b:", b.eval())
    weight_1 = w_1.eval()[0,0]
    weight_2 = w_2.eval()[0,0]
    bias = b.eval()[0]

  weight_1 = weight_1*(Z.max()/X.max())
  weight_2 = weight_2*(Z.max()/Y.max())
  bias = bias*Z.max()
  return weight_1, weight_2, bias

weight_1, weight_2, bias = train(size, bedroom, price)
print(weight_1, weight_2, bias)

for i in range(len(price)):
  print("size:", size[i], "bedroom:", bedroom[i], "price:", price[i], 
        "predicted_price", weight_1*size[i] + weight_2*bedroom[i] + bias)
```
-->

### Results
```Text
step: 0
cost: 0.27548373
step: 2000
cost: 0.008432454
step: 4000
cost: 0.008361867
step: 6000
cost: 0.0083486
step: 8000
cost: 0.008344767
step: 10000
cost: 0.008343329
w_1: [[0.8953951]] w_2: [[-0.07044698]] b: [0.13103214]
139.94798 -9861.168 91709.39
size: [2104.] bedroom: [3.] price: [399900.] predicted_price [356576.44]
size: [1600.] bedroom: [3.] price: [329900.] predicted_price [286042.66]
size: [2400.] bedroom: [3.] price: [369000.] predicted_price [398001.06]
size: [1416.] bedroom: [2.] price: [232000.] predicted_price [270153.38]
size: [3000.] bedroom: [4.] price: [539900.] predicted_price [472108.62]
size: [1985.] bedroom: [4.] price: [299900.] predicted_price [330061.47]
size: [1534.] bedroom: [3.] price: [314900.] predicted_price [276806.1]
size: [1427.] bedroom: [3.] price: [198999.] predicted_price [261831.66]
size: [1380.] bedroom: [3.] price: [212000.] predicted_price [255254.11]
size: [1494.] bedroom: [3.] price: [242500.] predicted_price [271208.2]
size: [1940.] bedroom: [4.] price: [239999.] predicted_price [323763.8]
size: [2000.] bedroom: [3.] price: [347000.] predicted_price [342021.88]
size: [1890.] bedroom: [3.] price: [329999.] predicted_price [326627.56]
size: [4478.] bedroom: [5.] price: [699900.] predicted_price [669090.6]
size: [1268.] bedroom: [3.] price: [259900.] predicted_price [239579.94]
size: [2300.] bedroom: [4.] price: [449900.] predicted_price [374145.06]
size: [1320.] bedroom: [2.] price: [299900.] predicted_price [256718.39]
size: [1236.] bedroom: [3.] price: [199900.] predicted_price [235101.6]
size: [2609.] bedroom: [4.] price: [499998.] predicted_price [417389.]
size: [3031.] bedroom: [4.] price: [599000.] predicted_price [476447.06]
size: [1767.] bedroom: [3.] price: [252900.] predicted_price [309413.97]
size: [1888.] bedroom: [2.] price: [255000.] predicted_price [336208.8]
size: [1604.] bedroom: [3.] price: [242900.] predicted_price [286602.44]
size: [1962.] bedroom: [4.] price: [259900.] predicted_price [326842.66]
size: [3890.] bedroom: [3.] price: [573900.] predicted_price [606523.5]
size: [1100.] bedroom: [3.] price: [249900.] predicted_price [216068.67]
size: [1458.] bedroom: [3.] price: [464500.] predicted_price [266170.06]
size: [2526.] bedroom: [3.] price: [469000.] predicted_price [415634.5]
size: [2200.] bedroom: [3.] price: [475000.] predicted_price [370011.44]
size: [2637.] bedroom: [3.] price: [299900.] predicted_price [431168.75]
size: [1839.] bedroom: [2.] price: [349900.] predicted_price [329351.38]
size: [1000.] bedroom: [1.] price: [169900.] predicted_price [221796.2]
size: [2040.] bedroom: [4.] price: [314900.] predicted_price [337758.6]
size: [3137.] bedroom: [3.] price: [579900.] predicted_price [501142.7]
size: [1811.] bedroom: [4.] price: [285900.] predicted_price [305710.5]
size: [1437.] bedroom: [3.] price: [249900.] predicted_price [263231.12]
size: [1239.] bedroom: [3.] price: [229900.] predicted_price [235521.44]
size: [2132.] bedroom: [4.] price: [345000.] predicted_price [350633.8]
size: [4215.] bedroom: [4.] price: [549000.] predicted_price [642145.44]
size: [2162.] bedroom: [4.] price: [287000.] predicted_price [354832.25]
size: [1664.] bedroom: [2.] price: [368500.] predicted_price [304860.5]
size: [2238.] bedroom: [3.] price: [329900.] predicted_price [375329.5]
size: [2567.] bedroom: [4.] price: [314000.] predicted_price [411511.2]
size: [1200.] bedroom: [3.] price: [299000.] predicted_price [230063.47]
size: [852.] bedroom: [2.] price: [179900.] predicted_price [191222.73]
size: [1852.] bedroom: [4.] price: [299900.] predicted_price [311448.38]
size: [1203.] bedroom: [3.] price: [239500.] predicted_price [230483.31]
```
