## 세그먼트 트리 (Segment Tree)
구간 정보를 이진 트리 형태의 노드들에 나누어 저장해 특정 구간의 합을 구하는 자료 구조

트리 구조라서 시간복잡도: O(log N)

```cpp
vector<long long> arr = {1, 2, 3, 4, 5, 6, 7, 8, 9, 10};
vector<long long> tree;
```
### 트리 생성 함수
```cpp
long long init(int start, int end, int index) {
	
	if(start == end) {
		tree[index] = arr[start];
		return tree[index];
	}
	 
	int mid = (start + end) / 2;
	tree[index] = init(start, mid, index * 2) + init(mid + 1, end, index * 2 + 1);
	
	return tree[index];
}
```
### 구간 합 구하는 함수
```cpp
long long interval_sum(int start, int end, int index, int left, int right) {
	int mid = (start + end) / 2;
	
	if(left > end || right < start) {
		return 0;
	}
	if(left <= start && end <= right) {
		return tree[index];
	}
	else {
		mid = (start + end) / 2;
		return interval_sum(start, mid, index * 2, left, right) + interval_sum(mid + 1, end, index * 2 + 1, left, right);
	}
}
```
### 값 수정 함수
```cpp
// diff: 기존 값과의 차이 (val - arr[what])
void update(int start, int end, int index, int what, long long diff) {
	if(start > what || what > end) return ;
	
	tree[index] += diff;
	
	if(start == end) return ;
	
	int mid = (start + end) / 2;
	update(start, mid, index * 2, what, diff);
	update(mid + 1, end, index * 2 + 1, what, diff);
}
```
### 실행 예시
```cpp
// 0부터 9까지 구간 합 (1~10의 합 = 55)
cout << interval_sum(0, n - 1, 1, 0, 9) << "\n";
    
// arr[0]을 +4 만큼 수정 (기존 1 -> 5)
update(0, n - 1, 1, 0, 4);
	
// 0부터 9까지 구간 합 (1~10의 합 = 55)
cout << interval_sum(0, n - 1, 1, 0, 9) << "\n";
```
