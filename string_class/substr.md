## substr

`문자열.substr(시작인덱스, 길이)`<br><br>
문자열에서 원하는 위치에 있는 문자열을 가져올 때 사용한다.<br>

```cpp
#include <iostream>
#include <string>

int main() {
    std::string str = "Hello World";
    // 6번째 인덱스부터 5글자 자르기 -> "World"
    std::string sub = str.substr(6, 5); 
}

```
<br>

길이는 생략 가능. 생략 시 끝까지 불러옴.
```
std::string sub = str.substr(6);
```
[문제: #39 폴리폴리펩타이드](https://www.doj.kr/ko/problems/39)
