# 문제 링크
https://school.programmers.co.kr/learn/courses/30/lessons/12939

## 느낀점

- 배열로 바꿔야한다는 생각이 쉽게 떠오르지 않았다
- 코딩테스트는 인텔리에 자동완성이 없기에 내가 무엇을 써야하는지 알아야한다
- 메서드를 암기하는것이 진짜 필요할것같다
- 문제 해결능력이 매우 낮다보니 문제를 보면 어떻게 풀어야할지 감이 안온다
- 분명 성능을 고려하면서 진행을 해야할텐데 그것도 아직은 부족한상태
- 자료구조 이해도가 부족하다


---

## 문제 설명

문자열 s에는 공백으로 구분된 숫자들이 저장되어 있습니다. 
str에 나타나는 숫자 중 최소값과 최대값을 찾아 이를 "(최소값) (최대값)"형태의 문자열을 반환하는 함수, solution을 완성하세요.
예를들어 s가 "1 2 3 4"라면 "1 4"를 리턴하고, 
"-1 -2 -3 -4"라면 "-4 -1"을 리턴하면 됩니다.

최대값과 최소값을 구하는 문제이다


---


## 문제 접근 과정



1. 아직 문제해결능력이 부족하기 때문에 가장 간단한 방법으로 접근
2. 문자열을 공백으로 split하여 배열로 만들고, 최대값과 최소값을 찾는다.
3. 최대값과 최소값을 문자열로 만들어 리턴한다.


---


## 시간복잡도

- 사실 아직 성능을 고려를 어떻게 해야하는지 모르는 상태



## 코드

```java
import java.util.*;

class Solution {

    public String solution(String s) {
        String answer = "";

        String [] arr = s.split(" ");
        int max = Integer.parseInt(arr[0]);
        int min = Integer.parseInt(arr[0]);

        for(int i = 0; i < arr.length; i++) {
            int num = Integer.parseInt(arr[i]);
            if(num > max) {
                max = num;
            }
            if(num < min) {
                min = num;
            }
        }

        answer = min + " " + max;

        return answer;
    }
