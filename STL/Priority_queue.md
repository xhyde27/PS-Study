## 우선순위 큐
들어온 순서와 상관없이 우선순위가 가장 높은 데이터부터 나가는 큐.

```cpp
priority_queue <int> pq; // 내림차순으로 나옴
```

### 오름차순으로 나오게 하려면..
```
priority_queue <int, vector<int>, greater<int>> pq; 

pq.push(1);
pq.push(10);
pq.push(7);

cout << pq.top(); // 1 출력
```
```
struct Data{
    int x, y;
    bool operator<(const Data &Right)const{
        return x > Right.x;
    }
};

priority_queue <Data> pq;
```
