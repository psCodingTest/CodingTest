# 문제 링크
https://school.programmers.co.kr/learn/courses/30/lessons/181841

## 느낀점

- 0단계 문제이기에 확실히 쉽지만 0단계 문제들은 꼭 하나하나 특정 메소드를 사용할수 있는지를 확인하는것같다
- contains를 사용하면 쉽게 풀수있는 문제지만 생각보다 쉽게 떠오르지 않았다
- 알고리즘문제를 푸는것은 실무와는 진짜 다른거같다
- for문을 Stream으로 바꾸는 연습도 하고싶은데 이게 성능에도 영향이 어떨지 잘 모르겠다

---

## 문제 설명

문자열들이 담긴 리스트가 주어졌을 때, 모든 문자열들을 순서대로 합친 문자열을 꼬리 문자열이라고 합니다. 
꼬리 문자열을 만들 때 특정 문자열을 포함한 문자열은 제외시키려고 합니다. 
예를 들어 문자열 리스트 ["abc", "def", "ghi"]가 있고 문자열 "ef"를 포함한 문자열은 제외하고 꼬리 문자열을 만들면 "abcghi"가 됩니다.
문자열 리스트 str_list와 제외하려는 문자열 ex가 주어질 때, 
str_list에서 ex를 포함한 문자열을 제외하고 만든 꼬리 문자열을 return하도록 solution 함수를 완성해주세요.


---


## 문제 접근 과정


1. 문자열 리스트를 순회하면서, ex를 포함하지 않는 문자열을 찾는다.
2. 찾은 문자열을 answer에 더한다.
3. answer를 리턴한다.
4. ex를 포함하는 문자열을 찾는 방법은 contains를 사용하면 될것같다.


---


## 시간복잡도

- 사실 아직 성능을 고려를 어떻게 해야하는지 모르는 상태



## 코드

```java
import java.util.*;

class Solution {

    public String solution(String[] str_list, String ex) {
        String answer = "";


        for (String str : str_list) {
            if (!str.contains(ex)) {
                answer += str;
            }
        }

        return answer;
    }
