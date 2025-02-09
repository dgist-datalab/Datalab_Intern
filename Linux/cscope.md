# cscope 

## 설치 및 설정법
이 글에서는 cscope라는 tool에 대해서 배웁니다.  
cscope는 vim을 이용하여 많은 양의 코드를 봐야할 때 원하는 함수를 찾기 쉽게 해줍니다.  
  
  
먼저 cscope를 설치합니다.
```terminal
$ sudo apt install cscope
```
  
그 다음에는 `mkcscope.sh`이라는 이름의 shell script를 만들고 아래 내용을 복사해줍시다.
```bash
#!/bin/sh
rm -rf cscope.files cscope.out

find `pwd` \( -name '*.c' -o -name '*.cpp' -o -name '*.cc' -o -name '*.h' -o -name '*.s' -o -name '*.S' \) -print > cscope.files
cscope -i cscope.files -b
```
  
만들고 난 다음 `mkcscope.sh`의 permission을 변경해주고 `/usr/bin` 폴더로 파일을 옵깁니다.
```terminal
$ sudo chmod 755 mkcscope.sh
$ sudo mv mkcscope.sh /usr/bin
```
`mkcscope.sh`는 현재 디렉토리의 코드들의 symbol들을 탐색해서 저장하는 역할을 해줍니다.  
  
이제 vim에서 cscope설정을 하기 위해 `.vimrc`파일을 열고 다음 내용을 추가해줍시다.  
이 때 `PATH`에는 자신이 cscope에 등록하고자하는 코드 디렉토리의 절대경로를 넣어주면 됩니다.
```vimrc
" cscope setting
set csprg=/usr/bin/cscope
set csto=0
set cst 
set nocsverb

if filereadable("PATH/cscope.out") "PATH 예시 - /home/hojun/git/linux
  cs add PATH/cscope.out
endif
set csverb
```
  
cscope에 등록하고자 하는 코드의 디렉토리로 이통해서 `mkcscope.sh`를 실행시켜줍시다. 
```terminal
$ cd /home/hojun/git/linux
$ mkcscope.sh
```
`mkcscope`를 실행하면 데이터가 빌드됩니다.  
  
실행 후에는 `cscope.files`와 `cscope.out` 두 가지 파일이 생성됩니다.
  
이제 vim으로 코드를 열었을 때 error가 생기지 않는다면 cscope 설정에 성공한 것입니다.  

## 사용법
cscope는 vim내에서 command mode로 사용할 수 있습니다.
vim 내에서 다음 command를 입력하면 원하는 함수로 이동합니다.
```text
:cs find {질의종류} {심벌}

cscope의 질의종류
0 or s  -> Find this C symbol
1 or g  -> Find this definition
2 or d  -> Find functions called by this function
3 or c  -> Find functions calling this function
4 or t  -> Find assignments to
6 or e  -> Find this egrep pattern
7 or f  -> Find this File
8 or i  -> Find files #including this file
```

예를 들어, Linux Kernel에서 `sys_write` 함수의 정의를 검색하고 싶다면  
```text
:cs find g sys_write
```
또는 `sys_write` 함수를 사용하는 코드를 찾고 싶다면
```text
:cs find c sys_write
```
라고 입력하면 명령에 해당되는 여러 코드라인을 보여주고 그 중 하나를 선택해 해당 코드라인으로 이동할 수 있습니다.

참고: https://hackereyes.tistory.com/entry/펌-ctags-cscope-설치-및-사용
