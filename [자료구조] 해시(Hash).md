# [자료구조] 해시(Hash)



* 데이터를 효율적으로 관리하기 위해 임의의 길이 데이터를 고정된 길이의 데이터로 매핑하는 것
* 해시함수를 구현하여 데이터 값을 해시 값으로 매핑한다.

* 용어
  * 키**(key)** - 매팅 전 원래 데이터의 값
  * 해시값**(hash value)** - 매핑 후 데이터의 값
  * 해싱**(hashing)** - 매핑하는 과정 자체



#### collision 현상

```
lim -> 해싱함수 -> 5
Kim -> 해싱함수 -> 4
Park -> 해싱함수 -> 3
Jung -> 해싱함수 -> 2
...
Lee -> 해싱함수 -> 5// Lim과 해싱값 충동
```

* 데이터가 많아지게 되면, 다른 데이터와 같은 해시 값으로 충돌현상이 발생

* 완전히 같은 해시코드로 충돌이 발생할 수 도 있다.



#### 사용 이유

* 해시 충동 발생 가능성이 있음에도 적은 리소스로 많은 데이터를 효율적으로 관리하기 위해서

- 언제나 동일한 해시값 리턴, index를 알면 빠른 데이터 검색이 가능
- 해시테이블의 시간복잡도 O(1) - (이진탐색트리는 O(logN))



#### 해시함수

특정 값에 치우치지 않고 해시값을 고르게 만들어낼 수 있다.

입력값의 길이가 달라도 출력값은 언제나 고정된 길이로 반환

동일한 값이 입력되면 언제나 동일한 출력값을 보장





#### collision 충돌문제 해결

1. **체이닝** : 열결리스트로 노드를 계속 추가해나가는 방식(제한 없이 계속 연결 가능, but 메모리 문제)
2. **Open Addressing** :  해시 함수로 얻은 주소가 아닌 다른 주소에 데이터를 저장할 수 있도록 허용 (해당 키 값에 저장되어있으면 다음 주소에 저장)
3. **선형 탐사** : 정해진 고정 폭으로 옮겨 해시값의 중복을 피함
4. **제곱 탐사** : 정해진 고정 폭을 제곱수로 옮겨 해시값의 중복을 피함



참고자료

* https://ratsgo.github.io/data%20structure&algorithm/2017/10/25/hash/

* https://velog.io/@cyranocoding/Hash-Hashing-Hash-Table%ED%95%B4%EC%8B%9C-%ED%95%B4%EC%8B%B1-%ED%95%B4%EC%8B%9C%ED%85%8C%EC%9D%B4%EB%B8%94-%EC%9E%90%EB%A3%8C%EA%B5%AC%EC%A1%B0%EC%9D%98-%EC%9D%B4%ED%95%B4-6ijyonph6o
* 태원님 노션 - hash, hashing`