# Installing Linux

Linux에는 여러 가지 종류가 있는데, 우리는 그 중 ubuntu를 설치하여 사용하려고 한다.

Windows 운영체제를 사용중이라면 VMware Player, VirtualBox, Mac 운영체제를 사용중이라면 Parallels, VirtualBox 같은 프로그램을 이용하여 가상 머신 위에 Linux를 설치할 수 있다. (또는, 디스크 파티션을 나누어 듀얼부팅이 되도록 설치하는 방법도 있다)

VirtualBox의 경우에는 다음 [문서](../Edge_TPU/install_ubuntu.html)에 매우 간략하게 설명이 되어있으니, 참고해도 된다.

가장 먼저 Linux를 설치하는 방법을 알아보고, [다음 자료](/pdf/LAB1.pdf)를 천천히 따라하며 Linux를 설치하여 보자. VMware Player를 기준으로 설명되어 있다.


# Terminal Commands

다음 명령어들의 용도와 사용법을 익숙해 질 때까지 직접 실습해보자.

```
pwd, ls, mkdir, rmdir, cd, touch, cp, mv, rm,
man,
tar,
sudo, su,
apt-get, apt,
clear,
scp, ssh,
grep
```

그리고 절대 경로(from root directory)와 상대 경로(from current directory)의 개념에 대해서도 예시와 함께 조사해 보자.

- 예제

![그림](/img/path.jpg)

다음 그림과 같이 디렉토리 구성이 되어있다. 사용자는 현재 /home/cory 폴더에 위치한다. /home/jono/work 폴더를 상대 경로로 나타내보자.


# Git

다음 [책](https://git-scm.com/)([번역본](https://git-scm.com/book/ko/v2))의 Chapter 1부터 3까지를 읽고 그 내용을 정리해 보자.

# Vim

Vim은 터미널 환경에서 사용할 수 있는 텍스트 편집기로 많은 프로그래머들의 사랑을 받고 있다.
터미널 창에 `vimtutor`를 입력하면 각종 단축키들의 사용법을 설명하고 있는 튜토리얼을 진행할 수 있으니, 직접 튜토리얼을 진행해 보도록 하자.

# Ctags & Cscope

ctags와 cscope는 vim에서 원하는 함수나 변수를 검색할 때 사용하는 툴이다.  
아래 링크에 설치법과 사용법이 나와있다.  
[ctags](./ctags.md)  
[cscope](./cscope.md)  

# Python Libraries

Ubuntu에는 Python 2와 Python 3이 기본적으로 설치되어 있으며 pip를 이용하여 Python 2 라이브러리를, pip3을 이용하여 Python 3 라이브러리를 손쉽게 설치할 수 있다.
pip3 명령어(Windows의 경우 pip만 입력해도 된다.)를 이용하여 다음 몇가지 유용한 파이썬 라이브러리들을 설치해 보도록 하자.

```Text
pip3 install matplotlib
pip3 install numpy
pip3 install scipy
pip3 install pandas
```

상당수의 파이썬 라이브러리들은 pip(pip3) 명령어를 이용하여 설치할 수 있으며, Anaconda 배포판을 사용하고 있다면 conda 명령어를 사용할 수도 있다.
참고로 이 가이드라인은 pip(pip3)를 이용하는 것을 기본 전제로 하고 있다.

# GCC/G++

리눅스에서의 C / C++ 프로그램들은 보통 GCC/G++ 컴파일러를 이용하여 컴파일 된다. 간단한 `Hello, world!\n`를 출력하는 C / C++ 프로그램을 작성해 본 후 GCC / G++ 을 이용하여 이를 컴파일 해 보자. 컴파일 및 실행 예시는 다음과 같다.

```Text
gcc helloworld.c
./a.out
```

```Text
g++ helloworld.cpp
./a.out
```

# Makefile

C / C++ 프로그램의 규모가 커졌을 때 유용한 프로그램이다.
이 부분은 향후 업데이트 될 예정이다.

# Shell Script

터미널에 다수의 명령어를 입력할 때 유용하게 활용할 수 있다.
이 부분 또한 향후 업데이트 될 예정이다.

# Raspberry Pi
[Raspberry Pi 홈페이지](https://www.raspberrypi.org/downloads/)에서 Raspbian을 포함한 라즈베리 파이에 설치 가능한 다양한 운영체제를 제공하고 있다. Raspbian을 직접 라즈베리 파이에 설치해 보고, 여러 설정을 맞추어 보고, 터미널 환경에서 간단한 프로그래밍도 실시해 보는 등 Raspberry Pi에 익숙해 지는 시간을 가지도록 하자.
