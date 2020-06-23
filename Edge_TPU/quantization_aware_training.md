# Quantization-aware Training

Edge TPU에서 동작하는 모델을 작성하기 위해서는 [Quantization-aware Training](https://github.com/tensorflow/tensorflow/tree/r1.13/tensorflow/contrib/quantize#quantization-aware-training)을 실시해야만 한다. 아래 예시와 같은 코드를 이용하여 Quantization-aware Training을 실시할 수 있는데, 핵심은 Training을 위한 model과 Evaluation을 위한 model을 분리해서 구성하고, 따로 Quantization을 시켜준 이후, [Freezing 및 TensorFlow Lite 모델로 변환시키는 과정](/Edge_TPU/tflite_convert.md)을 통해 Fully-quantized model을 생성해야 한다는 것이다. 참고로 모델을 분리시키는 것은 Training용 모델을 학습시킨 후, Evaluation용 모델에서 그 Parameter들을 loading 하는 방식으로 생성할 수 있다.  
Quantization-aware Training의 자세한 내용은 아래 논문 [Quantization and Training of Neural Networks for Efficient Integer-Arithmetic-Only Inference](https://arxiv.org/abs/1712.05877) 에 설명되어 있으므로 참고하도록 하자.

참고로 이미 학습된 모델을 [Post-training quantization](https://www.tensorflow.org/lite/performance/post_training_quantization)을 통해 Quantization을 시킬 수도 있다. 이 경우 모델을 손쉽고 빠르게 Quantization을 시킬 수는 있으나, Quantization-aware training 대비 정확도가 떨어진다는 단점이 있다.

```Python
# Build forward pass of model.

# Call the training rewrite which rewrites the graph in-place with
# FakeQuantization nodes and folds batchnorm for training. It is
# often needed to fine tune a floating point model for quantization
# with this training tool. When training from scratch, quant_delay
# can be used to activate quantization after training to converge
# with the float graph, effectively fine-tuning the model.
g = tf.get_default_graph()
tf.contrib.quantize.create_training_graph(input_graph=g,
                                          quant_delay=2000000)

# Call backward pass optimizer as usual.
optimizer = tf.train.GradientDescentOptimizer(learning_rate)
optimizer.minimize(loss)
```

```Python
# Build eval model

# Call the eval rewrite which rewrites the graph in-place with
# FakeQuantization nodes and fold batchnorm for eval.
g = tf.get_default_graph()
tf.contrib.quantize.create_eval_graph(input_graph=g)

# Save the checkpoint and eval graph proto to disk for freezing
# and providing to TFLite.
with open(eval_graph_file, ‘w’) as f:
  f.write(str(g.as_graph_def()))
saver = tf.train.Saver()
saver.save(sess, checkpoint_name)
```