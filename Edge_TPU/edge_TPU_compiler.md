# Edge TPU Compiler 사용하기

Edge TPU는 요구사항을 충족시킨 Tensorflow Lite 모델을 Edge TPU에서 구동시킬 수 있도록 해 주는 [Edge TPU Model Compiler](https://coral.withgoogle.com/web-compiler/)를 제공한다. 본 페이지에서는 Edge TPU에서 제공하는 [사전에 학습된 모델](https://coral.withgoogle.com/models/)을 가지고 Edge TPU에서 사용할 수 있는 모델을 직접 컴파일해보자 한다. 직접 작성한 TensorFlow Lite 모델도 동일한 방법으로 변환시킬 수 있다.

1. [Edge TPU 홈페이지](https://coral.withgoogle.com/models/)에 접속하여 [MobileNet V1 (ImageNet)의 All model files](http://download.tensorflow.org/models/mobilenet_v1_2018_08_02/mobilenet_v1_1.0_224_quant.tgz)을 다운로드 받는다.

2. 압축 파일을 풀고 홈페이지 내 [Edge TPU Model Compiler](https://coral.withgoogle.com/web-compiler/)에 `mobilenet_v1_1.0_224_quant.tflite` 파일을 업로드한다.

3. 성공하였다면 다음과 같은 log와 함께 Edge TPU에서 사용할 수 있는 Tensorflow Lite 파일을 다운로드 받을 수 있다.
    ```
    Compiler Version: 0.14.5
    Output Model Size: 4.32MB
    Total no. of operations: 31
    No. of operations that will run on Edge TPU: 31 (100%)
    No. of operations that will run on CPU: 0 (0%)

    Consolidated Operation Log:
    --------------------------

    Operation type                           Compilation status   No. of occurrences
    --------------                           ------------------   ------------------
    kTfLiteBuiltinDepthwiseConv2d            SUCCEEDED            13                
    kTfLiteBuiltinReshape                    SUCCEEDED            1                 
    kTfLiteBuiltinAveragePool2d              SUCCEEDED            1                 
    kTfLiteBuiltinSoftmax                    SUCCEEDED            1                 
    kTfLiteBuiltinConv2d                     SUCCEEDED            15                

    More details
    --------------
    Start Time     2019-05-07T10:16:41.818226Z
    State          SUCCEEDED
    Duration       3.426454521s
    Type           type.googleapis.com/google.cloud.iot.edgeml.v1beta1.CompileOperationMetadata
    Name           operations/compile/10669477871472753422
    ```