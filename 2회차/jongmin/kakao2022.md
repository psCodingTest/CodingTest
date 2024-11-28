# 문제 링크
https://school.programmers.co.kr/learn/courses/30/lessons/92335

##  느낀점

- 진수변경 메서드를 몰랐다면 시간이 걸렸을 것 같습니다
- 시간 단축 하도록 노력해야한다고 느꼇습니다.

---

## 문제 설명

- 양의 정수 n이 주어집니다. 이 숫자를 `k진수`로 바꿨을 때, 변환된 수 안에 아래 조건에 맞는 `소수`가 몇 개인지 알아보려 합니다.
- 0P0처럼 소수 양쪽에 0이 있는 경우
- P0처럼 소수 오른쪽에만 0이 있고 왼쪽에는 아무것도 없는 경우
- 0P처럼 소수 왼쪽에만 0이 있고 오른쪽에는 아무것도 없는 경우
- P처럼 소수 양쪽에 아무것도 없는 경우

---


## 문제 접근 과정

1. toString 을 활용하여 진법을 변환하는 과정을 축소하였습니다.
2. 이후 소수판별 메서드를 사용하여 소수를 판별하도록 하였습니다.
---


## 시간복잡도


## 코드

```java
class Solution {
    static int cnt = 0;
	public static int solution(int n, int k){
		String string = Integer.toString(n, k);
		double length = string.length();
		int left = 0;
		int right =1;
		while (right < length){
			if (string.charAt(right) == '0' ) {
				String substring = string.substring(left, right);
				isPrime(Long.parseLong(substring));
				left = right;
			}
			right +=1;
		}
		isPrime(Long.parseLong(string.substring(left, (int)length)));

		return cnt;
	}

	private static void isPrime(double n) {
		if (n < 2) return;
		for (double i = 2; i <= Math.sqrt(n); i++) {
			if (n % i == 0) {
				return;
			}
		}
		cnt++;
	}
}