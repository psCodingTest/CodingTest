# 문제 링크
https://school.programmers.co.kr/learn/courses/30/lessons/67257

## 느낀점

- 연산 문제라고 Stack, Queue로만 해결하려고 하지않기
- 문제가 길다면 중간에 컴파일로 문법체크하기(프로그래머스 기준)
- 문자열 파싱 관련 메서드 암기하기....



---

## 문제 설명

주어진 수식에서 연산자의 우선순위를 바꿔가며 연산 결과의 절댓값이 가장 큰 경우를 찾아내는 문제입니다. 
주어진 연산식은 `+, -, *`로 이루어져 있으며, 이 연산들의 우선순위를 변경하여 나올 수 있는 결과 중 최대 절댓값을 구해야 합니다.



---


## 문제 접근 과정

1. 연산자 우선순위가 6종류 -> `2차원 배열`
2. String 기반 입력값 -> `문자열 파싱`
3. 연산자와 숫자 분리 -> `두개의 List 활용`
4. 결과값 절대값 -> `Math.abs()활용`

---


## 시간복잡도

- 3 < 문자열 길이 < 100  
- 연산자 우선순위 경우의 수 = 6

수가 매우 적기 때문에 모든 경우의 수 다 판단해도 괜찮다고 판단.
즉, DP나 그리디 기법은 아예 배제



## 코드

```java
import java.util.*;

class Solution {
    static char[][] operat = {
        {'*', '+', '-'},
        {'+', '*', '-'},
        {'+', '-', '*'},
        {'-', '+', '*'},
        {'-', '*', '+'},
        {'*', '-', '+'},
    };

    public long solution(String expression) {
        // numbers와 operation을 함수 내부에서 초기화하여 매번 새롭게 계산
        List<Long> numbers = new ArrayList<>();
        List<Character> operation = new ArrayList<>();
        
        int start = 0;
        // 문자열 파싱하여 숫자와 연산자 분리
        for (int i = 0; i < expression.length(); i++) {
            char value = expression.charAt(i);
            if (value == '-' || value == '*' || value == '+') {
                operation.add(value); // 연산자 삽입

                // 숫자 삽입
                numbers.add(Long.parseLong(expression.substring(start, i)));
                start = i + 1;
            }
        }
        // 마지막 숫자 삽입
        numbers.add(Long.parseLong(expression.substring(start)));

        // 연산자 우선순위에 따라 최대 절댓값 계산
        long max = 0;
        for (int i = 0; i < 6; i++) {
            // numbers와 operation의 복사본을 사용하여 연산
            max = Math.max(max, Math.abs(calculate(new ArrayList<>(numbers), new ArrayList<>(operation), operat[i])));
        }

        return max;
    }

    // 연산자 우선순위에 따라 계산하는 함수
    public long calculate(List<Long> numbers, List<Character> operation, char[] priority) {
        for (char op : priority) {
            for (int i = 0; i < operation.size(); i++) {
                if (operation.get(i) == op) {
                    long result = performOperation(numbers.get(i), numbers.get(i + 1), op);
                    numbers.set(i, result); // 계산 결과를 현재 위치에 저장
                    numbers.remove(i + 1); // 계산한 숫자는 리스트에서 제거
                    operation.remove(i); // 사용한 연산자 제거
                    i--; // 리스트 크기 줄어들었으니 인덱스 조정
                }
            }
        }
        return numbers.get(0); // 최종 결과 반환
    }

    // 실제 연산을 수행하는 함수
    public long performOperation(long num1, long num2, char operation) {
        switch (operation) {
            case '*':
                return num1 * num2;
            case '+':
                return num1 + num2;
            case '-':
                return num1 - num2;
            default:
                throw new IllegalArgumentException("Unknown operation: " + operation);
        }
    }
}
