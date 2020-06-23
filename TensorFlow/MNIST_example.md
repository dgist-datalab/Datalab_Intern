# Keras로 작성한 MNIST 예제

MNIST(Modified National Institute of Standards and Technology database)는 28*28 크기의 흑백 손글씨 이미지로 이루어진 데이터베이스이다.
60000개의 학습 이미지와 10000개의 테스트 이미지로 구성되며, 딥러닝을 공부하는 사람들이 많이 사용하는 "Hello, world!" 급의 위치에 있는 데이터베이스이기도 하다.

TensorFlow는 홈페이지에서 MNIST를 학습하는 간단한 [예제](https://www.tensorflow.org/overview/)를 제공하고 있으며, 이는 다음과 같다.
그리고 이 코드로부터 Keras를 이용하면 매우 손쉽게 모델을 정의할 수 있는 것을 발견할 수 있다.

```Python
import tensorflow as tf
mnist = tf.keras.datasets.mnist

(x_train, y_train),(x_test, y_test) = mnist.load_data()
x_train, x_test = x_train / 255.0, x_test / 255.0

model = tf.keras.models.Sequential([
  tf.keras.layers.Flatten(input_shape=(28, 28)),
  tf.keras.layers.Dense(128, activation='relu'),
  tf.keras.layers.Dropout(0.2),
  tf.keras.layers.Dense(10, activation='softmax')
])

model.compile(optimizer='adam',
              loss='sparse_categorical_crossentropy',
              metrics=['accuracy'])

model.fit(x_train, y_train, epochs=5)
model.evaluate(x_test, y_test)
```

이 코드를 실행시켜 MNIST 데이터셋을 학습시켜보고, 코드를 수정하여 Keras Model을 Export 시켜 보자.

# Keras를 사용하지 않고 MNIST 작성해보기

Keras를 사용하면 손쉽게 Neural Network 모델을 정의하고 학습시킬 수 있다. 하지만 Edge TPU에서 동작하는 모델을 만들기 위해서는 Quantization Aware Training을 실시해야 하는데, 
아직 Keras에서는 이를 지원하지 않고 있다. 향후 Edge TPU 위에서 동작하는 모델을 만들기 위해 MNIST 데이터를 분류하는 CNN 모델을 Keras를 사용하지 않고 작성해 보고, 
이를 TensorBoard로 시각화 시켜보자. 그리고 시각화를 마친 코드는 Export 시키는 것으로 마무리 시켜보자.
모델 구성은 위의 Keras 코드의 구성을 그대로 따라도 좋고, 직접 모델을 설계해도 좋다.
다음 [자료](https://hunkim.github.io/ml/) 등에 관련 내용이 설명되어 있다.

# 예시

```Python
import tensorflow as tf

from tensorflow.examples.tutorials.mnist import input_data
mnist = input_data.read_data_sets("/tmp/data", one_hot=True)

learning_rate = 0.001
training_epochs = 5
batch_size = 100
display_step = 1

X = tf.placeholder(tf.float32, [None,784], name="Input")
Y = tf.placeholder(tf.float32, [None,10], name="Output")

W1 = tf.get_variable("W1", shape=[784,512], initializer=tf.contrib.layers.xavier_initializer())
W2 = tf.get_variable("W2", shape=[512,10], initializer=tf.contrib.layers.xavier_initializer())

L1 = tf.nn.relu(tf.matmul(X, W1), name="Layer_1")
L1_drop = tf.nn.dropout(L1, 0.2, name="Dropout")
H = tf.matmul(L1, W2, name="Hypothesis")

g = tf.get_default_graph()
tf.contrib.quantize.create_training_graph(input_graph = g, quant_delay=2000000)

cost = tf.reduce_mean(tf.nn.softmax_cross_entropy_with_logits_v2(logits=H, labels=Y))
train_step = tf.train.AdamOptimizer(learning_rate).minimize(cost)

with tf.Session() as sess:
  init = tf.global_variables_initializer()
  sess.run(init)

  writer = tf.summary.FileWriter("./log/", sess.graph)

  for i in range(training_epochs):
    total_batch = int(mnist.train.num_examples / batch_size)

    for j in range(total_batch):
      batch_xs, batch_ys = mnist.train.next_batch(batch_size)
      sess.run(train_step, feed_dict={X: batch_xs, Y: batch_ys})

    correct_prediction = tf.equal(tf.argmax(H, 1), tf.argmax(Y, 1))
    accuracy = tf.reduce_mean(tf.cast(correct_prediction, tf.float32))

    print(i+1, "th training")
    print(accuracy.eval(feed_dict={X: mnist.test.images, Y:mnist.test.labels}))
  
  saver = tf.train.Saver()
  saver.save(sess, "data/trained_model.ckpt")

  writer.close()
```