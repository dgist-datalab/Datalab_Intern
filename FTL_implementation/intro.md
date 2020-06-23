# Introduction

## 개요

Flash memory controller 중 일부에 해당하는 FTL(Flash Translation Layer)에 대해 공부합니다.

NAND Flash memory의 특성에 따른 기초적인 제약조건만을 만들어 놓은 FTLBox에서 FTL을 구현해본 뒤,

DataLab에서 제작하여 사용하고 있는 User-space Flash Simulation Platform인 FlashDriver에서 더 심화된 FTL을 구현하는 것을 목표로 합니다.

본 일정(특히 3주차 이후)은 진행상황에 따라 변경될 수 있습니다.

## 일정

| 날짜 | 내용 |
| ---- | ---- |
| 1주차 | Flash memory와 FTL에 대한 공부 / FTLBox에서 Page FTL 구현 시작 |
| 2주차 | FTLBox에서 Page FTL 구현 완료 / FlashDriver에서 Block FTL 구현 시작 |
| 3주차 | FlashDriver에서 Block FTL 구현 완료 / 간단한 Demand FTL 구현 시작 |
| 4주차 | 간단한 Demand FTL 구현 완료 / GC를 지원하는 Demand FTL 구현 시작 | 
| 5주차 | GC를 지원하는 Demand 구현 완료 / 인턴 마무리 |


## 페이지 모음

  + [Todo List](./todo.md)
  + [OSTEP](./OSTEP.md)
  + [FTLBox](./ftl_box.md)
  + [FlashDriver](./flashdriver.md)