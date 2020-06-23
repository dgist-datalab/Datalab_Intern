# Install Guide (Raspberry Pi)

1. [Raspberry Pi 홈페이지](https://www.raspberrypi.org/)에 가서 Raspbian을 다운로드 받는다.
이때 여러 종류의 Raspbian 중에서 Raspbian Stretch Lite을 선택하면 된다. Raspbian을 다운받은 후에는 [Raspbian 설치 가이드라인](https://www.raspberrypi.org/documentation/installation/installing-images/README.md)에서 안내하는 대로 [Etcher](https://www.balena.io/etcher/)를 설치하고, Micro SD card에 Raspbian 이미지를 기록한다.  

  **Note** Raspberry Pi는 제약된 하드웨어 성능 때문에 GUI 환경에서 작업을 수행하는 데 있어 메모리 부족 같은 문제점들이 자주 발생한다. 이러한 이유로 CUI 환경에서 작업을 수행하길 권장한다.

2. Raspbian이 기록된 Micro SD card를 삽입한 후, Micro 5pin Cable과 키보드, HDMI Cable을 연결한다.
연결이 끝나면 자동으로 부팅이 진행되고, 모니터에 로그인 화면이 출력된다.
이때 사용할 수 있는 기본 ID / Password는 다음과 같다.
```Text
ID: pi
Password: raspberry
```

3. 리눅스 터미널에서 사용할 수 있는 필수적인 프로그램(git, build-essential, vim, valgrind, etc...)들을 `sudo apt-get install` 명령어를 통해 설치한다.

4. `sudo raspi-config` 명령어를 통해 필요한 설정들을 해 준다.
  1. InterfaceOption에서 SSH가 사용 가능하도록 설정한다.
  2. Change User Password
  3. Localization Option: Timezone을 Asia의 Seoul로 변환한다.
  4. 앞에서 언급한 내용 이외에 필요한 설정

  모든 설정을 끝낸 이후 exit를 통해 터미널로 빠져나온 후, `reboot`명령어를 통해 재부팅을 실시한다.

5. `sudo passwd root`를 통해 root 계정의 비밀번호를 설정한다.
이를 설정해야 `su`를 통해 root 사용자 권한을 획득할 수 있다.

6. `ifconfig`를 입력하여 내부 네트워크 상에서의 ip 주소를 확인하고 이를 기억한다.
이후 putty나 ssh를 이용하여 라즈베리파이에 원격으로 접속하여 작업을 계속해서 진행한다.  

  **Note** git clone https://github.com/tensorflow/tensorflow.git 같은 외부 텍스트를 복사, 붙여넣기를 이용하는 것이 편한 경우가 자주 존재한다. 라즈베리파이는 터미널 모드로 동작하기 때문에 이 작업이 용이하지 않으나, 외부 컴퓨터에서 복사 붙여넣기를 하면 이런 작업도 손쉽게 처리할 수 있다.

7. [Tensorflow Lite Documentation](https://www.tensorflow.org/lite/guide)의 [Raspberry Pi installation instruction](https://www.tensorflow.org/lite/guide/build_rpi)을 따라서 Tensorflow Lite static library를 컴파일한다. 이 작업은 시간이 오래 걸리는 편이다. 다음 명령어를 순서대로 입력하면 Home directory 아래에 Tensorflow repository가 복사되고, Raspberry Pi static library `~/tensorflow/tensorflow/lite/tools/make/gen/lib/rpi_armv7/libtensorflow-lite.a`가 생성된다.
```Text
cd ~
git clone https://github.com/tensorflow/tensorflow.git
cd tensorflow
./tensorflow/lite/tools/make/download_dependencies.sh
./tensorflow/lite/tools/make/build_rpi_lib.sh
```

8. [Coral TPU Document](https://coral.withgoogle.com/docs/accelerator/get-started/#set-up-on-linux-or-raspberry-pi)를 따라 TPU를 설치하고 예시 모델을 실행한다.
Coral TPU Document를 따라 TPU를 설치하고 예시 모델을 실행한다.
먼저 다음 명령어는 TPU를 Raspberry Pi에 설치하는 과정이다. 이 과정을 수행한 후에 Coral TPU를 Raspberry Pi에 연결하면 된다. 만약 TPU를 먼저 연결하였다면, 장치를 Raspberry Pi로부터 제거한 후 다시 연결해주어야 한다.
```Text
cd ~/
wget https://dl.google.com/coral/edgetpu_api/edgetpu_api_latest.tar.gz -O edgetpu_api.tar.gz --trust-server-names
tar xzf edgetpu_api.tar.gz
cd edgetpu_api
bash ./install.sh
```
TPU를 설치한 후 다음 명령어를 통해 예시 코드를 실행함으로써 제대로 설치되었는지의 여부를 확인할 수 있다.
```Text
cd ~/Downloads/
wget https://storage.googleapis.com/cloud-iot-edge-pretrained-models/canned_models/mobilenet_v2_1.0_224_inat_bird_quant_edgetpu.tflite \
http://storage.googleapis.com/cloud-iot-edge-pretrained-models/canned_models/inat_bird_labels.txt \
https://coral.withgoogle.com/static/images/parrot.jpg
cd /usr/local/lib/python3.5/dist-packages/edgetpu/demo
python3 classify_image.py \
--model ~/Downloads/mobilenet_v2_1.0_224_inat_bird_quant_edgetpu.tflite \
--label ~/Downloads/inat_bird_labels.txt \
--image ~/Downloads/parrot.jpg
```
이 예시 코드의 실행 결과는 다음과 같다.

  ```Text
  ---------------------------
  Ara macao (Scarlet Macaw)
  Score :  0.761719
  ```

9. 별도의 NIC를 통해 접속할 때에는 윈도우는 다른 문서를 참고하면 되고, 우분투의 경우 nm-connection-editor를 실행시켜 IPv4 설정에서 "다른 컴퓨터와 공유"를 선택하면 된다.