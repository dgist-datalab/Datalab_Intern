# Transfer Learning을 시킨 후 Edge TPU에서 추론해 보기

이미 학습되어 있는 Model을 Transfer Learning으로 재학습시켜서 Edge TPU에서 추론을 실시해 보자. 이 문서는 [다음 문서](https://coral.withgoogle.com/docs/edgetpu/retrain-classification/)의 내용을 기반으로 하고 있다.

먼저 [Docker를 설치](https://docs.docker.com/install/)하여 Edge TPU에서 제공하는 image를 build시켜 model을 구해야 한다.

1. 다음 명령어를 통해 설치되어 있는 기존 docker를 삭제한다.
```
sudo apt-get remove docker docker-engine docker.io containerd runc
```
2. 다음 명령어를 이용하여 docker의 설치 경로를 설정한다.
```
sudo apt-get update
```
```
sudo apt-get install \
    apt-transport-https \
    ca-certificates \
    curl \
    gnupg-agent \
    software-properties-common
```
```
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
```
```
sudo apt-key fingerprint 0EBFCD88
```
명령어가 제대로 수행되었다면 아래의 출력을 얻을 수 있다.
```
pub   rsa4096 2017-02-22 [SCEA]
      9DC8 5822 9FC7 DD38 854A  E2D8 8D81 803C 0EBF CD88
uid           [ unknown] Docker Release (CE deb) <docker@docker.com>
sub   rsa4096 2017-02-22 [S]
```
```
sudo add-apt-repository \
   "deb [arch=amd64] https://download.docker.com/linux/ubuntu \
   $(lsb_release -cs) \
   stable"
```
3. 다음 명령어를 통해 docker를 설치하고, 설치가 제대로 이루어졌는지의 여부를 확인한다.
```
sudo apt-get update
```
```
sudo apt-get install docker-ce docker-ce-cli containerd.io
```
```
sudo docker run hello-world
```
4. docker 파일을 받아서 빌드하고 실행한다.
```
CLASSIFY_DIR=${HOME}/edgetpu/classify && mkdir -p $CLASSIFY_DIR
```
```
cd $CLASSIFY_DIR
wget -O Dockerfile "http://storage.googleapis.com/cloud-iot-edge-pretrained-models/docker/classify_docker"
sudo docker build - < Dockerfile --tag classify-tutorial
```
```Text
docker run --name edgetpu-classify \
--rm -it --privileged -p 6006:6006 \
--mount type=bind,src=${CLASSIFY_DIR},dst=/tensorflow/models/research/slim/transfer_learn \
classify-tutorial
```

5. Transfer learning을 위한 데이터셋을 다음 스크립트를 실행시킴으로써 다운받는다.
```Text
# From the Docker /tensorflow/models/research/slim/ directory
./prepare_checkpoint_and_dataset.sh --network_type mobilenet_v1
```

6. Transfer learning을 실시한다. 이 과정은 시간이 많이 걸리니 인내심을 가지고 기다릴 필요성이 있다.
```Text
./start_training.sh --network_type mobilenet_v1
```

7. 저장된 Checkpoint와 GraphDef를 TensorFlow Lite 모델로 변환시킨다.
```Text
# From the Docker /tensorflow/models/research/slim/ directory
./convert_checkpoint_to_edgetpu_tflite.sh --network_type mobilenet_v1 --checkpoint_num 300
```

8. 다음과 같은 명령어를 통해 Edge TPU Compiler를 설치하고 모델을 Edge TPU에서 실행시킬 수 있도록 컴파일한다. 그리고 컴파일의 결과물을 `edgetpu.tflite`로 명명하여 `${CLASSIFY_DIR}/models/`에 복사한다.(2019년 6월 24일 기준 sudo apt update가 동작하지 않으니 참고 바람. 이 시점에도 동작하지 않으면 온라인 Edge TPU Compiler를 이용하길 바람)
```Text
# Install the compiler:
echo "deb https://packages.cloud.google.com/apt coral-edgetpu-stable main" | sudo tee /etc/apt/sources.list.d/coral-edgetpu.list
sudo apt update
sudo apt install edgetpu
# Change directories to where the new model is:
cd $HOME/edgetpu/classify/models
# Compile the model:
edgetpu_compiler output_tflite_graph.tflite
```

9. 다음 이미지와 라벨을 다운받아 Edge TPU에서 모델이 잘 동작하는지 확인한다.
```Text
wget https://c2.staticflickr.com/9/8374/8519435096_45e27efd0d_o.jpg -O ${CLASSIFY_DIR}/flower.jpg
```
```Text
cd /usr/local/lib/python3.6/dist-packages/edgetpu/demo
python3 classify_image.py \
--model ${CLASSIFY_DIR}/models/edgetpu.tflite \
--label ${CLASSIFY_DIR}/models/labels.txt \
--image ${CLASSIFY_DIR}/flower.jpg
```