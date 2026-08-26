## 에라토스테네스의 체
소수 판별기. 소수의 모든 배수를 지워나가는 방식이다.


*시간 복잡도 : O(Nlog(log N))*

```cpp
vector<bool> arr(1000001, true);

arr[0] = arr[1] = false;

for(int i = 2; i * i <= n; i++) {
    if(arr[i]) {
        for(int j = i * i; j <= n; j += i) {
            arr[j] = false;
        }
    }
}
```

