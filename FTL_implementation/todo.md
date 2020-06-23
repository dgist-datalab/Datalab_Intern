# Todo list

1. Flash memory와 FTL에 대해 이해하기 위해, Three easy pieces(최신 OS 교과서)의 SSD 파트 공부하기

2. FTLbox github의 Flash Firmware 관련 Lecture note를 보고 FTLBox에서 Page FTL을 구현해 보기 (Lecture note 중 04-Storage-Frimware-Part1.pdf, SE380-algorithm-project-02.pdf 를 보면 도움이 될 것)

3. FTLbox는 FTL을 간단하게 만들어보기 위한 것으로, 몇 줄 안되는 간단한 코드로 구성되어 있음. FTLbox의 3가지 interface(read, write, erase)를 사용하여 FTL을 구현하면 됨. (FTLBox에 대한 자세한 설명은 FTLbox github에 있으며, 이제 막 만든 것이므로 잘못된 점이 존재할 수 있음)

4. FTLBox에서의 Page FTL 구현이 완료되면, FlashDriver에서 Block FTL을 구현해보기. 다만 FlashDriver는 혼자서 시작하기에는 어려운 점이 있으므로 도움을 요청하는 것이 좋음. FlashDriver를 시작하기 전에 FTLBox에서 원하는 FTL을 추가로 구현하여도 되며, 다른 FTL의 경우에는 FTLBox github의 Lecture note에서 Storage Firmware 파트와 관련 논문을 참고하면 된다. (PFTL, BFTL, DFTL, FAST 등)



## 참고자료
[Three easy pieces]  
http://pages.cs.wisc.edu/~remzi/OSTEP

[Three easy pieces의 SSD 파트]  
http://pages.cs.wisc.edu/~remzi/OSTEP/file-ssd.pdf

[FTLbox github]  
https://github.com/jwya2149/FTLBox

[FlashDriver github]  
https://github.com/kukania/FlashDriver