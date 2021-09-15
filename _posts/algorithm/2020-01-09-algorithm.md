---
layout: post
title: 알고리즘 기초-수학2
category: algorithm
tags: [algorithm]
comments: true
---

# 알고리즘 기초-수학2
## 소수 (Prime Number)
- 소수 : 약수가 1과 자기 자신 밖에 없는 수
- 소수는 여러 알고리즘 문제에 적용 되므로 중요하다!
  - N이 소수가 되기 위한 조건
    - N이 2보다 크거나 같아야 한다.
    - N - 1 보다 작거나 같은 자연수로 나누어 떨어지면 안된다.
- 소수와 관련된 알고리즘 2가지
  - 1. 어떤 수 N이 소수인지 아닌지 판별하는 방법
  - 2. N보다 작거나 같은 모든 자연수 중에서 소수를 찾아내는 방법

소수인지 아닌지 판별하는 함수(with N)
```c
bool prime(int n)
{
	if (n < 2) {
		return false;
	}

	for (int i = 2; i <= n - 1; i++) {
		if (n % i == 0) {
			return false;
		}
	}
	return true;
}
```

- 하지만 좀 더 간단하게 소수임을 검사할 수 있다.
- N이 소수가 되기위해 2보다 크거나 같고, __N/2__ 보다 작거나 같은 자연수로 나누어 떨어지면 안된다.
  - N의 약수 중에서 가장 큰 것은 N/2 보다 작거나 같기 때문
  - N = a * b 로 나타낼 수 있는데, a가 작을수록 b가 크다.
  - 가능한 a중에서 가장 작은 값이 2이기 때문에, b 는 N/2를 넘지 않는다.

소수인지 아닌지 판별하는 함수 (with N/2)
```c
bool prime(int n)
{
  if (n < 2) {
  	return false;
  }

  for (int i = 2; i <= n/2; i++) {
  	if (n % i == 0) {
  		return false;
  	}
  }
	return true;
}
```   

- N/2 까지만 하는 방법 보다도 빠른 방법! __루트__
- N이 소수가 되려면 2보다 크거나 같고, 루트N 보다 작거나 같은 자연수로 나누어 떨어지면 안된다.
  - N이 소수가 아니라면 , N = a * b 로 나타낼 수 있다.(a <= b)
  - a > b라면 두 수를 바꿔서 항상 a <= b로 만들 수 있다.
  - 두 수 a와 b의 차이가 가장 작은 경우는 루트 N 이다.
  - 따라서 루트 N까지만 검사를 해보면 된다.


소수인지 아닌지 판별하는 함수 (with 루트N)
```c
bool prime(int n)
{
  if (n < 2) {
  	return false;
  }

  for (int i = 2; i * i <= n; i++) {
  	if (n % i == 0) {
  		return false;
  	}
  }
	return true;
}
```
### 백준 [1978] 소수 찾기
- 숫자의 갯수 N과 N개의 숫자들이 입력으로 주어진다.
- 주어진 입력에서 소수의 갯수를 출력한다.
- 아래의 코드는 가장 기본적인 방법인 N까지 검사하는 방식으로 구현하였다.
- 더 빠른 방법으로 하고 싶으면 함수 안에 i의 범위를 조절하면 된다.

```c
#include <iostream>
using namespace std;

int PrimeNumber(int num);

int main() {
	int n, num = 0, res;
	int ary[101];

	cin >> n;

	for (int i = 0; i < n; i++) {
		cin >> ary[i];
	}

	for (int i = 0; i < n; i++) {
		res = PrimeNumber(ary[i]);
		if (res == 2) num++;
		else continue;
	}

	cout << num << endl;

	return 0;

}

int PrimeNumber(int a)
{
	int cnt = 0;
	for (int i = 1; i <= a; i++) {
		if (a % i == 0) cnt++;
	}

	return cnt;
}
```

## 에라토스테네스의 체
- 1 부터 N까지 범위 안에 들어가는 모든 소수를 구하려면 에라토스테네스의 체를 사용한다.
- 방법
  - 1. 2부터 N까지 모든 수를 써 놓는다.
  - 2. 아직 지워지지 않은 수 중에서 가장 작은 수를 찾는다.
  - 3. 그 수는 소수이다.
  - 4. 이제 그 수의 배수를 모두 지운다.

- 쉽게 말하자면 2를 제외한 모든 2의 배수를 지운다.
- 지우고 남은 것중에 가장 작은 숫자를 고른다. 이 경우에는 3이 선택된다.
- 3을 제외한 모든 3의 배수를 지운다.
- 이것을 반복하고 지워지지 않은 숫자들이 소수이다.

```c
int prime[100];
int pn = 0;
bool check[101];
int n = 100;
for (int i = 2; i <= n; i++) {
	if (check[i] == false) {
		prime[pn++] = i;
		for (int j = i * i; j <= n; j += i) {
			check[j] = true;
		}
	}
}
```  
