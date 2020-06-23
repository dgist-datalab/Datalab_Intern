# Numpy / Matplotlib Tutorial

Numpy, Scipy, Pandas, Matplotlib 같은 여러 Python libraries들은 pip (Ubuntu의 경우에는 pip3) 명령어를 통해 쉽게 설치할 수 있다. 

```Text
pip install numpy, scipy, pandas, matplotlib
```

이중 Numpy 라이브러리는 여러 배열 처리를 MATLAB 처럼 효율적으로 할 수 있게 해 주는 다른 라이브러리에서도 데이터형으로 많이 채택하는 매우 중요한 라이브러리 중 하나이며, Matplotlib 또한 이 데이터를 쉽게 시각화를 시켜 준다는 점에서 많이 사용되는 라이브러리 중 하나이다. 
아래 문서들을 참고하여 numpy, matplotlib의 사용법을 간략히 익히고, 제시된 문제를 풀어보며 그 사용법을 연습해보자.

[CS231n Numpy Tutorial (Kor)](http://aikorea.org/cs231n/python-numpy-tutorial/)
[CS231n Numpy Tutorial (Eng)](http://cs231n.github.io/python-numpy-tutorial/)

## 연습문제

다음 코드 

```Python
import matplotlib.pyplot as plt
from scipy.misc import electrocardiogram
from scipy.signal import find_peaks
x = electrocardiogram()[2000:4000]
```

를 통해 ECG (심전도, Electrocardiogram) 데이터를 로딩한 후, 다음 [문서](https://docs.scipy.org/doc/scipy/reference/generated/scipy.signal.find_peaks.html)를 참고하여 R wave를 찾고 R-R interval을 계산하여 그를 토대로 Heart rate (단위: BPM)을 계산하시오. Heart rate는 다음 관계를 통해 계산할 수 있다.

$$\text{Heart Rate} = \frac{60*1000}{\text{R-R Interval}}$$