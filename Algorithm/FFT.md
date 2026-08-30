## FFT (Fast Fourier Transform)

임의의 입력 신호를 다양한 주파수를 갖는 주기함수의 합으로 분해하여 표현하는 수학적 변환.

<img width="470" height="216" alt="image" src="https://github.com/user-attachments/assets/aa3fece8-d3bb-4a19-9d8b-c88e7628c008" />

그림 1. 푸리에 변환 (그림출처: [위키피디아](https://en.wikipedia.org/wiki/Fourier_transform))


- 수식적인 이해

<p align="center">
<img width="250" height="50" alt="image" src="https://github.com/user-attachments/assets/1b1ce180-eec1-4b6f-a368-959c3b149914" />
</p>

$F(x)$ : 계수, $f(x)$ : 원본 입력 신호, $e$<sup>$jπux$</sup>: 주파수 u인 주기함수 성분
<br><br>
정확한 이해는 문서 참조. 어쨌든 푸리에 역변환 시 다시 원래의 함수로 돌아온다는 것이 증명되어 있음.

> (참고) 호너의 정리 : n차 다항식은 서로 다른 n+1개의 점 좌표만 알면 하나로 결정됨
<br><br>
---
<br><br>

### 코딩에서의 활용 예시

> 매우 큰 두 수의 곱셈 (O(NlogN))

<br>

숫자 자리를 각각 다항식 계수로 생각하여 다항식 곱셈 문제로 생각한다.

차수가 $N-1$인 다항식 $A(x)$가 있다고 할 때, 

$$A(x) = a_0 + a_1 x + a_2 x^2 + a_3 x^3 + a_4 x^4 + a_5 x^5 + \dots$$

<br>

> (참고) 호너의 정리 : n차 다항식은 서로 다른 n+1개의 점 좌표만 알면 하나로 결정됨

아까 언급한 이것을 활용하면, (두 다항식의 차수 합 + 1) 개의 수를 대입하면 푸리에 변환이 가능하다는 뜻

(두 식에 대입한 후 나온 두 집합의 각 요소를 곱한 새로운 집합을 통해 곱셈식을 도출 가능)

<br>
근데 대입할 게 너무 많으니. 원래 식을 짝수 항과 홀수 항으로 묶어보자.

<br>

짝수 항 $A_E(x)$: $a_0 + a_2 x + a_4 x^2 + \dots$

홀수 항 $A_O(x)$: $a_1 + a_3 x + a_5 x^2 + \dots$

원래 식 $A(x)$ : $$A(x) = A_E(x^2) + x \cdot A_O(x^2)$$

<br>

x에 -x을 대입해보면, $A_O(x)$와 $A_E(x)$ 중 한 값만 구하면 두 값을 모두 알 수 있음이 도출된다.

$+x$ 대입: $$A(x) = A_E(x^2) + x \cdot A_O(x^2)$$

$-x$ 대입: $$A(-x) = A_E((-x)^2) + (-x) \cdot A_O((-x)^2) = A_E(x^2) - x \cdot A_O(x^2)$$

<br>

따라서 짝수/홀수로 식을 쪼개두면, 전체 N개 점을 대입하는 거대한 문제를 절반 크기의 점만 대입하는 작은 문제 2개로 완전히 쪼개서 O(N log N)만에 모든 값들을 다 구해낼 수 있음.

<br>


정방향 FFT: 복소평면 단위원에서 시계 방향($e^{-i\theta}$)으로 도는 점들을 넣음

역변환 IFFT: 반시계 방향($e^{+i\theta}$)으로 도는 점들을 넣음

정방향 변환 과정에서 $N$번 더해진 것을 원래대로 되돌리기 위해 배열 전체를 $N$으로 나누어 줌


```cpp
#include <vector>
#include <complex>
#include <cmath>

using namespace std;
using cpx = complex<double>;
const double PI = acos(-1);

// inv = false (정방향 FFT 변환)
// inv = true  (역방향 IFFT 변환)
void fft(vector<cpx>& a, bool inv) {
    int n = a.size();

    // 1. 비트 반전 (위치 섞기 - 분할정복을 반복문으로 하기 위함)
    for (int i = 1, j = 0; i < n; i++) {
        int bit = n >> 1;
        for (; j & bit; bit >>= 1) j ^= bit;
        j ^= bit;
        if (i < j) swap(a[i], a[j]);
    }

    // 2. 나비 연산 (실제 병합 및 회전 연산)
    for (int len = 2; len <= n; len <<= 1) { // len 2배씩 커짐 
        // 예를 들어 len=2이면 배열에서 2개씩 묶음. 
        // inv가 true면 +각도(반시계 회전), false면 -각도(시계 회전)
        double angle = 2 * PI / len * (inv ? 1 : -1);
        cpx wlen(cos(angle), sin(angle)); 

        for (int i = 0; i < n; i += len) {
            cpx w(1, 0);
            for (int j = 0; j < len / 2; j++) {
                cpx u = a[i + j];
                cpx v = a[i + j + len / 2] * w;
                
                a[i + j] = u + v;
                a[i + j + len / 2] = u - v;
                w *= wlen;
            }
        }
    }

    // 역변환(IFFT)일 때만 하는 후처리
    if (inv) {
        for (int i = 0; i < n; i++) {
            a[i] /= n;  // 커진 크기를 원상복구
        }
    }
}
```

숫자 → 배열 → FFT → 원소별 곱 → IFFT → 자리올림 → 끝
이 순서인건 알겠는데 이해가 하나도 안된다. 하...
