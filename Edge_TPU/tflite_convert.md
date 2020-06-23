# Saved model을 EdgeTPU로 Evaluation 해보기.

1. 학습시키고 저장한 Tensorflow 모델은 EdgeTPU에서 사용하기 위해 GraphDef와 Checkpoint를 Freeze 시킨 후 Frozen 된 모델을 Tensorflow Lite 모델로 전환시켜야 한다. 이때 사용하는 툴이 Tensorflow Lite Converter(TOCO)이며, 다음과 같이 모델을 전환시킬 수 있다. 실제 사용되는 Parameter들의 내용은 작성한 모델의 구성에 따라 달라질 수 있다. 
```Text
freeze_graph \
  --input_graph=./data/eval_graph.pd \
  --input_checkpoint=./data/trained_model.ckpt \
  --output_graph=./data/frozen_graph.pd \
  --output_node_names=Hypothesis
toco \
  --output_file=./data/mnist_quant.tflite \
  --graph_def_file=./data/frozen_graph.pd \
  --input_type=QUANTIZED_UINT8 \
  --inference_type=QUANTIZED_UINT8 \
  --input_arrays=Input \
  --output_arrays=Hypothesis \
  --mean_values=128 \
  --std_dev_values=127 \
  --default_ranges_min=0 \
  --default_ranges_max=255 \
```
보다 자세한 내용은 [Tensorflow Lite Converter 문서](https://www.tensorflow.org/lite/convert/cmdline_reference)에서 확인할 수 있다. 혹은 Python API도 제공하므로 이를 활용해도 된다.
