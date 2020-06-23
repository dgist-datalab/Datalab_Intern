# EdgeTPU의 요구사항

EdgeTPU는 Low power device 환경을 위한 기기인 만큼, 그 기능에 제약이 많다. EdgeTPU 위에서 돌아가는 모델은 Tensorflow를 통해 학습시킨 후 Tensorflow Lite 모델로 전환되어야만 하며, 이 과정을 간략히 나타내면 아래 그림과 같다.
![EdgeTPU Workflow](/img/compile-workflow.png)

그리고 이 이외에도 지켜야 하는 점들이 몇몇 가지가 있다.
+ Tensor parameter들은 quantized (8-bit fixed-point numbers, 8-bit integer)되어 있으며, Quantization-aware Training을 이용해야만 한다. (Post-training Quantization은 지원하지 않는다.)
+ Dynamic Tensor size를 지원하지 않는다. (Constant size이어야 함)
+ Model parameters (예: bias tensor) 들은 compile-time에 상수이어야 한다.
+ Tensor는 1, 2, 3차원 중 하나이다. 3차원 이상일 경우에는 가장 안쪽의 3차원의 값들만 유지된다. (If a tensor has more than 3 dimensions, then only the 3 innermost dimensions may have a size greater than 1.)
+ 아래 표에서 설명되는 Edge TPU에서 지원하는 연산자들만 사용해야 한다.

| Operation name         | Known limitations |
| ---------------------- | ----------------- |
| Logistic               |                   |
| Relu                   |                   |
| Relu6                  |                   |
| ReluN1To1              |                   |
| Tanh                   |                   |
| Add                    |                   |
| Maximum                |                   |
| Minimum                |                   |
| Mul                    |                   |
| Sub                    |                   |
| AveragePool2d	         | No fused activation function. |
| Concatenation	         | No fused activation function.  If any input is a compile-time constant tensor, there must be only 2 inputs, and this constant tensor must be all zeroes (effectively, a zero-padding op). |
| Conv2d                 |                   |
| DepthwiseConv2d        |                   |
| FullyConnected         | Only default format supported for fully-connected weights. Output tensor is one-dimensional.
| L2Normalization        |                   |
| MaxPool2d	             | No fused activation function. |
| Mean	                 | Supports reduction along x- and/or y-dimensions only. |
| Pad	                   | Supports padding along x- and/or y-dimensions only. |
| Reshape                |                   |
| ResizeBilinear         | Input/output is a 3-dimensional tensor. Depending on input/output size, this operation may not be mapped to the Edge TPU to avoid loss in precision. |
| ResizeNearestNeighbor	 | Input/output is a 3-dimensional tensor. Depending on input/output size, this operation may not be mapped to the Edge TPU to avoid loss in precision. |
| Slice                  |                   |
| Softmax                | Supports only 1D input tensor with a max of 16,000 elements. |
| SpaceToDepth           |                   |
| Split                  |                   |
| Squeeze                | Supported only when input tensor dimensions that have leading 1s (that is, no relayout needed). For example input tensor with [y][x][z] = 1,1,10 or 1,5,10 is ok. But [y][x][z] = 5,1,10 is not supported. |
| StridedSlice           | Supported only when all strides are equal to 1 (that is, effectively a Stride op), and with ellipsis-axis-mask == 0, and new-axis-max == 0. |

