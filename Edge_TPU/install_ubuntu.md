# Install Guide (Ubuntu 18.04)

1. [Ubuntu 홈페이지](https://www.ubuntu.com/download)에 가서 Ubuntu 18.04 Desktop Version ISO file을 다운로드 받는다.

2. [VirtualBox](https://www.virtualbox.org/) 혹은 [VMware Workstation Player](https://www.vmware.com/kr/products/workstation-player.html)를 다운받고 설치한다. 참고로 이 가이드라인은 VirtualBox를 기준으로 설명하고 있다. 혹시나 VMware Workstation Pro, Parallels Desktop 같은 상용 소프트웨어의 라이센스를 소지하고 있다면, 그 프로그램을 사용해도 된다.

3. VirtualBox 혹은 VMware Player에 가상 데스크탑을 생성하고, 다운로드 받은 ISO 파일을 이용하여 Ubuntu를 설치한다. 기본 설정으로 설치하면 무난하게 설치를 마칠 수 있으며, 혹시나 설치 과정에서 막힐 경우 네이버에 'Vmware Player Ubuntu', 'VirtualBox Ubuntu' 같은 키워드로 검색하면 상세한 도움을 받을 수 있다.

4. 리눅스 터미널에서 사용할 수 있는 필수적인 프로그램(git, build-essential, vim, valgrind, etc...)들을 `sudo apt-get install` 명령어를 통해 설치한다.

5. 다음 [문서](https://gabii.tistory.com/entry/Ubuntu-1804-LTS-%ED%95%9C%EA%B8%80-%EC%84%A4%EC%B9%98-%EB%B0%8F-%EC%84%A4%EC%A0%95) 등을 참고하여 한글 설정을 완료한다. 간략히 요악하면 Language Support에서 한글을 설치하고, 터미널에서 ibus-setup을 실행하여 한글을 설치한 후, Language Support의 Input Method에 한글을 추가하면 된다.

6. `sudo apt-get install python3.6` 명령어를 통해 3.6 버전의 Python Interpreter를 설치하고 `pip3 install tensorflow` 명령어를 통해 Tensorflow를 설치한다. 필요에 따라서 `pip3 install` 명령어로 Pandas, Matplotlib 같은 다른 유용한 패키지들을 설치해도 좋다.

7. `sudo passwd root`를 통해 root 계정의 비밀번호를 설정한다.
이를 설정해야 `su`를 통해 root 사용자 권한을 획득할 수 있다.

8. [Coral TPU Document](https://coral.withgoogle.com/docs/accelerator/get-started/#set-up-on-linux-or-raspberry-pi)를 따라 TPU를 설치하고 예시 모델을 실행한다.
Coral TPU Document를 따라 TPU를 설치하고 예시 모델을 실행한다.
먼저 다음 명령어는 TPU를 Ubuntu에 설치하는 과정이다. 이 과정을 수행한 후에 Coral TPU를 Raspberry Pi에 연결하면 된다. 만약 TPU를 먼저 연결하였다면, 장치를 Raspberry Pi로부터 제거한 후 다시 연결해주어야 한다.
```Text
cd ~/
wget https://dl.google.com/coral/edgetpu_api/edgetpu_api_latest.tar.gz -O edgetpu_api.tar.gz --trust-server-names
tar xzf edgetpu_api.tar.gz
cd edgetpu_api
bash ./install.sh
```
TPU를 설치한 후 다음 명령어를 통해 예시 코드를 실행함으로써 제대로 설치되었는지의 여부를 확인할 수 있다.
```Text
# on Raspberry Pi:
cd ~/Downloads/
wget https://storage.googleapis.com/cloud-iot-edge-pretrained-models/canned_models/mobilenet_v2_1.0_224_inat_bird_quant_edgetpu.tflite \
http://storage.googleapis.com/cloud-iot-edge-pretrained-models/canned_models/inat_bird_labels.txt \
https://coral.withgoogle.com/static/images/parrot.jpg
cd /usr/local/lib/python3.6/dist-packages/edgetpu/demo
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
