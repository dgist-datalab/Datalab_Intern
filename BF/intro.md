# Introduction

Bloom Filter는 1970년에 Burton Howard Bloom에 의해 고안되어 원소가 집합에 속하는지 여부를 검사하는데 사용되는 공간 효율적인 확률적 자료 구조이다.

Bloom Filter에 의해 어떤 원소가 집합에 속한다고 판단된 경우 실제로는 원소가 집합에 속하지 않는 False Positive가 발생 가능하지만, 반대로 원소가 집합에 속하지 않는 것으로 판단되었는데 실제로는 원소가 집합에 속하는 False Negative는 절대로 발생하지 않는다는 특성이 있다.

이러한 특성을 갖는 Bloom Filter를 사용한 FTL을 이해하고 이를 최적화하는 것을 목표해본다.

## Goals

BloomFilter의 구조를 이해하고 이를 C를 사용하여 직접 구현해본다. (Byte-based, Bit-based인 두가지 방식)

Bloom Filter를 이용한 간단한 Storage Management를 구현해본다.

Bloomfilter를 이용한 FTL을 이해하고 해당 FTL에서 Bloomfilter를 최적화 할 수 있는 기법을 구상해본다.

## Prerequisite

* C/C++
* Data Structure
* Flash Translation Layer (FTL)

## Weekly Plans

| 기간 | 내용 |
| ---- | ---- |
| 1주차 | Bloom Filter 개념 공부, Byte/Bit based version 구현 |
| 2주차 | Bloom Filter를 이용한 간단한 Storage Management 구현 |
| 3주차 | BloomFTL 초기형 개요 설명 및 코드 이해 |
| 4주차 | BloomFTL의 Bloom Filter 탐색 알고리즘적 최적화 |
| 5주차 | SIMD Instruction의 설명과 적용 방법 탐색 / 연구 성과 발표 |


## 페이지 모음

  + [Bloom Filter Concept](./bf_concept.md)
  + [Bloom Filter Code Example](./bf_example.md)
  + [BloomFTL](./bloomftl.md)
  + [Performance Evaluation](./bf_eval.md)
  + [SIMD](./simd_instruction.md)
