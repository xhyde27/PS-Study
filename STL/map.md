## map

key와 value를 pair 형태로 저장하는 자료구조. 기본적으로 오름차순 배열됨 (O(log n))

```cpp
map<int, int> m;

m[a] = b;
m.insert(a, b);
```

<br>

없는 요소 접근 시 자동으로 value에 0을 삽입함. 삽입하지 않고 없는지 체크하는 방법은 아래와 같음.

```cpp
if(m.find(2) != m.end()){
    printf("YES");
}
else{
    printf("NO");
}
```

<br>

### 모든 요소를 도는 방법

```cpp
for(auto element : m) {
    cout << m.first << " " << m.second << "\n";
}
```
내 컴파일러로 이건 안 돌아가긴 한데 이런 방식도 있음.
```cpp
for(auto [x, y] : m) {
    cout << x << " " << y << "\n";
}
```
