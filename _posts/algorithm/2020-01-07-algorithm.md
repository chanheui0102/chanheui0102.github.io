---
layout: post
title: 알고리즘 기초-수학1
category: algorithm
tags: [algorithm]
comments: true
---

# 알고리즘 기초-수학
## 나머지 연산
- 자료형이 표현 할 수 있는 범위가 다르므로 큰 수의 문제를 다룰 때는 나머지 연산을 사용한다.
  - C++의 경우 정수를 표현하는 int 와 lon long이 대표적인 자료형이다.
    - `int`: $2^{31} -1$
    - `long long`: $2^{63} -1$
  - 하지만 문제가 저 범위를 벗어나는 숫자를 다루게 될 때 나머지 연산을 사용한다.
  - 큰 숫자에 대한 나머지 연산을 하기 위해서는 중간 중간에 나머지 연산을 추가해야 한다.
  - 큰 숫자의 덧셈, 뺄셈, 곱셈의 경우 분배법칙이 성립한다.
  - 하지만 나누기의 경우에는 성립하지 않는다.
- 문제에서 정답을 __"정답을 ~로 나눈 나머지를 출력하라."__ 라는 문제의 경우에는 자료형이 int 나 long long으로 표현 불가능한 범위의 숫자를 다루기 때문이다.
  - 때문에 연산의 중간 중간에 나머지 연산을 추가로 넣어야한다.
  - (A + B) % C = (A % C + B % C) % C
  - (A * B) % C = (A % C * B % C) % C
  - 이것을 이용하여 자료형의 범위를 넘어서지 않도록 한다.

### 백준 [10430] 나머지
- (A+B)%C, (A%C + B%C)%C

- (A×B)%C, (A%C × B%C)%C

- 세 수 A, B, C가 주어졌을 때, 위의 네 가지 값을 구하는 프로그램을 작성하시오.

```c
#include <iostream>

using namespace std;

int main()
{
	int A, B, C;

	cin >> A >> B >> C;

	cout << (A + B) % C << endl;
	cout << (A%C + B%C) % C << endl;
	cout << (A * B) % C << endl;
	cout << (A%C * B%C) % C;
}
```

## 최대공약수 (GCD)
- 최대공약수 G는 A와 B의 공통된 약수 중에서 가장 큰 정수
- 최대 공약수는 기약분수의 형태로 나타내라는 문제등에 이용
- 가장 쉬운 방법은 2부터 min(A, B)까지 모든 정수로 나누어 보는 방법
- 최대공약수가 1인 두 수를 서로소(Coprime)라고 한다.
- 하지만 모든 정수로 나누어보는 방법은 비효율적이므로 __유클리드 호제법__ 을 이용!
- __유클리드 호제법__
  - A를 B로 나눈 나머지를 r 이라고 한다.
  - GCD(A, B) = GCD(B, r)
  - r 이 0이면 그때의 B가 최대공약수
  - GCD(A, B) -> GCD(B, A%B) -> ... GCD(result, 0)

```c
int gcd(int a, int b)
{
  if (b == 0)
    return a;
  else
    return gcd(b, a%b);
}
```

- 세 수의 최대공약수는 다음과 같이 구할 수 있음
  - GCD(a, b, c) = GCD(GCD(a, b), c)
  - N개의 수에 대해서도 동일하게 적용 가능(분배/결합법칙 성립)

## 최소공배수 (LCM)
- 배수 중 가장 작은 공통 수
- 최대공약수(GCD)를 이용해서 쉽게 구할 수 있음
  - 두 수 a, b의 최대공약수를 g라고 했을 때
  - LCM = a/g \* b/g * g = a\*b/c 성립

### 백준 2609
- 두 개의 자연수를 입력받아 최대 공약수와 최소 공배수를 출력하는 프로그램을 작성하시오.

```c
#include <iostream>

using namespace std;

int gcd(int a, int b)
{
	while (b != 0) {
		int r = a%b;
		a = b;
		b = r;
	}
	return a;
}

int main()
{
	int num1, num2;
	int g , l;
	cin >> num1 >> num2;

	g = gcd(num1, num2);
	l = g * (num1 / g) * (num2 / g);

	cout << g << endl;
	cout << l << endl;
}
```

### 백준 1934
- 두 자연수 A와 B에 대해서, A의 배수이면서 B의 배수인 자연수를 A와 B의 공배수라고 한다. 이런 공배수 중에서 가장 작은 수를 최소공배수라고 한다. 예를 들어, 6과 15의 공배수는 30, 60, 90등이 있으며, 최소 공배수는 30이다.
- 두 자연수 A와 B가 주어졌을 때, A와 B의 최소공배수를 구하는 프로그램을 작성하시오.

```c
#include <iostream>

using namespace std;

int gcd(int a, int b)
{
	while (b != 0) {
		int r = a%b;
		a = b;
		b = r;
	}
	return a;
}

int main()
{
	int T;

	cin >> T;
	for (int i = 0; i < T; i++) {
		int num1, num2, g, l = 0;
		cin >> num1 >> num2;

		g = gcd(num1, num2);
		l = g * (num1 / g) * (num2 / g);

		cout << l << endl;
	}
}
```

### 백준 9613
- n개의 수에 대해 가능한 모든 쌍의 GCD 합을 구하기
  - 합을 구하는 경우, 숫자가 매우 커져 오답이 발생 할 수 있다.
  - 그럴땐 `int` 대신 `long long`을 사용하자.

```c
#include <iostream>
#include <vector>
using namespace std;

int gcd(int a, int b)
{
	while (b != 0) {
		int r = a%b;
		a = b;
		b = r;
	}
	return a;
}

int main()
{
	int T;
	int N;
	long long g = 0;

	cin >> T;

	for (int i = 0; i < T; i++) {
		cin >> N;

		vector<int> v;

		for (int j = 0; j < N; j++) {
			int a;
			cin >> a;

			v.push_back(a);
		}

		for (int k = 0; k < N - 1; k++) {
			for (int l = k + 1; l < N; l++) {
				g += gcd(v[k], v[l]);
			}
		}
		cout << g << endl;
		g = 0;
	}
}
```

- 알고리즘 수학강의의 내용은 더 있지만, 포스팅하는 오늘 예제 문제가 있는 백준 사이트가 문제가 조금 있어서 나머지는 다음 포스팅에 이어서...
