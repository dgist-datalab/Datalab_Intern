# Introduction

이 문서는 2019년 3월부터 8월 사이에 작성된 문서로, Update 상황에 따라서 실습에 약간씩 차이가 있을 수 있습니다.

## TPU

Tensor Processing Unit (TPU)는 구글에서 개발한 Application-Specific Integrated Circuit (ASIC)으로 인공지능에 특화되어 있다.
8-bit 정수의 행렬 곱셈을 가속시킴으로써 Neural Network의 학습 / 추론 과정을 가속시키며, AlphaGo에도 사용된 바가 있다.
이 TPU는 판매되지 않고 Google Cloud 서비스를 통해서만 사용할 수 있었으나,
최근 Edge Computing에 활용할 수 있는 Edge TPU의 판매를 시작하였다. 이 Edge TPU를 이용하여 저전력으로 고성능의 추론 성능을 얻을 수 있다.