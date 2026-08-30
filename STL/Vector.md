## Vector

### Vector 초기화 

```cpp
vector<int> v1;        // int형 벡터 선언
vector<int> v2(5);     // 사이즈 5, 0으로 채워짐.
vector<int> v3(5, -1); // 사이즈 5, -1로 채워짐.

vector<bool> v(n+1, true); // 크기 n+1, 값 true로 초기화
```

### 인덱스 접근 방법

1. 일반적인 방식
```cpp
for (int i = 0; i < v.size(); i++) {
    int k = v[i];
}
```

2. iterator 사용
```cpp
for (auto i = v.begin(); i != v.end(); i++) {
    int k = *i;
}
```

3. 범위 기반 (C++11 이상)
```cpp
for (int k : v) {
    // blabla
}
```
---
뭐 풀다가 적었더라

[문제: #38 서로소 그래프와 쿼리](https://www.doj.kr/ko/problems/38)

[문제: #58 다다스와 친구들](https://www.doj.kr/ko/problems/58)
