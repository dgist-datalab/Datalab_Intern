# Bloom Filter

## Concept

Bloom Filter는 1970년에 Burton Howard Bloom에 의해 고안되어 원소가 집합에 속하는지 여부를 검사하는데 사용되는 공간 효율적인 확률적 자료 구조이다.

Bloom Filter에 의해 어떤 원소가 집합에 속한다고 판단된 경우 실제로는 원소가 집합에 속하지 않는 False Positive가 발생 가능하지만, 반대로 원소가 집합에 속하지 않는 것으로 판단되었는데 실제로는 원소가 집합에 속하는 False Negative는 절대로 발생하지 않는다는 특성이 있다.

집합에 원소를 추가하는 것은 가능하나, 집합에서 원소를 삭제하는 것은 불가능하다 (이를 해결하기 위해,  Counting Bloom Filter라는 것이 존재한다).
또한, 집합 내의 원소가 증가할수록 False Positive의 발생 확률도 증가한다.

## Structure

Bloom Filter는 m-bit 크기의 비트 배열 구조로 구성되어 있으며 그 값은 모두 0으로 초기 설정된다. 또한, k 개의 서로 다른 Hash Fuction을 사용한다.

k 개의 함수는 들어온 원소를 hash했을 때의 값이 배열에 균등하게 분포되게 하며 그 값이 지정하는 위치를 배열에서 1로 바꾸게 한다.

일반적으로 k는 m보다 훨씬 작은 상수로 지정되며 배열에 들어갈 원소의 개수에 비례한다.

False Positive 확률에 의해 k와 m을 결정한다.

아래는 m이 18이고 k가 3일때의 BloomFilter에 대한 예시이다.

![BF IMG](/img/bf_concept.png)

각 원소가 해당하는 bit를 같은 색의 화살표가 가르키고 있고 w라는 원소는 집합에 포함되지 않기 때문에 가르키는 위치에 값이 0인 경우가 존재하는 것을 확인할 수 있다.

출처 - [BloomFilter Wikipedia](https://en.wikipedia.org/wiki/Bloom_filter)
