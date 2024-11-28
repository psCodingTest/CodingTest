# 문제 링크
https://school.programmers.co.kr/learn/courses/30/lessons/92335


## 문제 설명

양의 정수 n이 주어집니다. 이 숫자를 k진수로 바꿨을 때, 변환된 수 안에 아래 조건에 맞는 소수(Prime number)가 몇 개인지 알아보려 합니다.

0P0처럼 소수 양쪽에 0이 있는 경우
P0처럼 소수 오른쪽에만 0이 있고 왼쪽에는 아무것도 없는 경우
0P처럼 소수 왼쪽에만 0이 있고 오른쪽에는 아무것도 없는 경우
P처럼 소수 양쪽에 아무것도 없는 경우
단, P는 각 자릿수에 0을 포함하지 않는 소수입니다.
예를 들어, 101은 P가 될 수 없습니다.
예를 들어, 437674을 3진수로 바꾸면 211020101011입니다. 여기서 찾을 수 있는 조건에 맞는 소수는 왼쪽부터 순서대로 211, 2, 11이 있으며, 총 3개입니다. (211, 2, 11을 k진법으로 보았을 때가 아닌, 10진법으로 보았을 때 소수여야 한다는 점에 주의합니다.) 211은 P0 형태에서 찾을 수 있으며, 2는 0P0에서, 11은 0P에서 찾을 수 있습니다.

정수 n과 k가 매개변수로 주어집니다. n을 k진수로 바꿨을 때, 변환된 수 안에서 찾을 수 있는 위 조건에 맞는 소수의 개수를 return 하도록 solution 함수를 완성해 주세요.


---


## 문제 접근 과정

1. 소수 판별 문제 -> `에라토스테네스의 체 알고리즘 활용`
2. 정수n을 k진수로 변환 -> `진법 변환 알고리즘 사용`
3. 소수의 조건 -> `0을 기준으로 분리 후 진법 계산`
4. n을 변환했을때 값 크기 -> `int 벗어나므로 Long 활용` -> `초반에 판단 X`


---

## 시간복잡도

- 최악의 경우 n = 1,000,000, k=3
- 10자리 이상, (실제론 13자리 정도라네요)
- 따라서, 변환과정에서 소요시간 적음
- 또한, 10자리 정도기에, 반복문으로 충분히 처리 가능하다 생각
  
(다만, int 범위를 벗어날거란 생각은 못했음)

---

## 느낀점

- 큰 숫자 예상될땐,항상 int와 long 범위 고려하기 
- 시간 단축위해 무조건적으로 `에라토스테네스 체 배열` 사용 x
- 주어진 문제 조건을 잘못 읽는 경우 주의하기.
- 지금 생각해보니, StringBuffer로도 풀 수 있었을 것 같음.



---

## 코드

```java
import java.util.*;

class Solution {
    
    public int solution(int n, int k) {
        int answer = 0;
        ArrayList<Integer> list = new ArrayList<>();

        // n을 k진수로 변환하여 list에 저장
        while(n > 0){
            list.add(n % k);
            n /= k;
        }

        long value = 0;
        long index = 1;

        for(int i = 0; i < list.size(); i++){
            if(list.get(i) == 0){
                // 0을 만나면 현재 value가 소수인지 확인
                if (value > 1 && isPrime(value)) {
                    answer++;
                }
                value = 0;  // value 초기화
                index = 1;
            }
            // 0이 아닌 숫자일 경우 진법 업데이트
            else { 
                value = value + list.get(i) * index;
                index *= 10;
            }
        }

        // 마지막 숫자 처리 & 소수확인
        if (value > 1 && isPrime(value)) {
            answer++;
        }

        return answer;
    }

    // 주어진 숫자가 소수인지 판별하는 함수 (에라토스테네스의 체)
    public boolean isPrime(long num) {
        if (num < 2) {
            return false;
        }
        for (long i = 2; i <= Math.sqrt(num); i++) {
            if (num % i == 0) {
                return false;
            }
        }
        return true;
    }
}
```
---

## 처음 풀이

```java
import java.util.*;

class Solution {
    static boolean[] primeNumber = new boolean[1000001];
    
    public int solution(int n, int k) {
        int answer = 0;
        ArrayList<Integer> list = new ArrayList<>();
        checkPrimeNumber();
        
        // n을 k진수로 변환한 결과를 리스트에 저장
        while(n > 0) {
            list.add(n % k);
            n /= k;
        }
        
        int value = 0;
        int index = 1;
        
        for(int i = 0; i < list.size(); i++) {
            if(list.get(i) == 0) {
                // 0을 만나면 현재 value가 소수인지 확인
                if (value > 1 && !primeNumber[value]) {
                    answer++;
                }
                value = 0;  // value 초기화
                index = 1;
            } else {
                value = value + list.get(i) * index;
                index *= 10;
            }
        }
        
        // 마지막 숫자 처리
        if (value > 1 && !primeNumber[value]) {
            answer++;
        }
        
        return answer;
    }
    
    // 소수 판별을 위한 에라토스테네스의 체
    public void checkPrimeNumber() {
        primeNumber[0] = true;
        primeNumber[1] = true;
        
        for (int i = 2; i <= (int) Math.sqrt(1000001); i++) {  // 범위 수정
            if (!primeNumber[i]) {
                for (int j = i * 2; j < 1000001; j += i) {
                    primeNumber[j] = true;
                }
            }
        }
    }
}
