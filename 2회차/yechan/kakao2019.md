# 문제 링크
https://school.programmers.co.kr/learn/courses/30/lessons/42889

---

## 문제 설명

슈퍼 게임 개발자 오렐리는 큰 고민에 빠졌다. 그녀가 만든 프랜즈 오천성이 대성공을 거뒀지만, 요즘 신규 사용자의 수가 급감한 것이다. 원인은 신규 사용자와 기존 사용자 사이에 스테이지 차이가 너무 큰 것이 문제였다.

이 문제를 어떻게 할까 고민 한 그녀는 동적으로 게임 시간을 늘려서 난이도를 조절하기로 했다. 역시 슈퍼 개발자라 대부분의 로직은 쉽게 구현했지만, 실패율을 구하는 부분에서 위기에 빠지고 말았다. 오렐리를 위해 실패율을 구하는 코드를 완성하라.

실패율은 다음과 같이 정의한다.
스테이지에 도달했으나 아직 클리어하지 못한 플레이어의 수 / 스테이지에 도달한 플레이어 수
전체 스테이지의 개수 N, 게임을 이용하는 사용자가 현재 멈춰있는 스테이지의 번호가 담긴 배열 stages가 매개변수로 주어질 때, 실패율이 높은 스테이지부터 내림차순으로 스테이지의 번호가 담겨있는 배열을 return 하도록 solution 함수를 완성하라.

---


## 문제 접근 과정

1. 스테이지마다 인원 존재 -> `Map으로 각 스테이지 인원 계산`
2. 각 Stage 마다 실패율 계산 -> `Node 배열로 각 스테이지 실패율 계산`
3. 실패율 기반 정렬  -> `Collections 활용 정렬`
4. 정렬의 조건이 복잡함 -> `익명객체 활용 구현`

---


## 시간복잡도

- 최악의 경우, Stage 실패자 Map 체크 = 200,000
- Stage 실패율 계산 = 500 + a
- Collection 정렬의 경우 (삽입 + 합병)을 합친 `Tim 정렬`을 사용 = 대충 `500 log 500` = 약 5000 

수가 매우 적기 때문에 모든 경우의 수 다 판단해도 괜찮다고 판단.

---

## 느낀점

- Collection 정렬의 내부 구현과정을 처음 알았다.
- 다양한 테스트 케이스 생각해보기
- 문자열 파싱 관련 메서드 암기하기....






## 코드

```java
import java.util.*;

class Solution {
    
    //각 Stage의 정보
    static class Node {
        int index; // stage 번호
        double failValue; // 실패율
        
        Node(int index, double failValue){
            this.index = index;
            this.failValue = failValue;
        }
    }
    
    public int[] solution(int N, int[] stages) {
        ArrayList<Node> list = new ArrayList<>();
        HashMap<Integer, Integer> map = new HashMap<>();
        int allPerson = stages.length;
        
        // 각 스테이지 실패자 체크
        for(int i=0; i<stages.length; i++){
            map.put(stages[i], map.getOrDefault(stages[i], 0)+1);
        }
        
        // 스테이지마다 실패율 계산
        for(int i=1; i<N+1; i++){
            int failPerson = map.getOrDefault(i, 0);
            double failValue = (double)failPerson/allPerson; // 실패율 계산
            
            // 스테이지는 남았는데, 이미 플레이어들이 다 떨어진 경우 에러 변환
            if(failPerson==0 && allPerson==0){
                failValue = (double)0;
            }
                
            list.add(new Node(i, failValue));
            allPerson -= failPerson; // 실패자 빼기 연산
        }
        
        // 실패율 기반 오름차순 정렬 (만약 실패율 같을 시, Stage 번호 기반 정렬)
        Collections.sort(list, new Comparator<Node>(){
            @Override
            public int compare(Node o1, Node o2){
                if(o2.failValue == o1.failValue){
                    return o1.index-o2.index;
                }
                return o2.failValue > o1.failValue ? 1 : -1;
            }
        });
        
        // 결과 출력
        int[] answer = new int[N];
        for(int i=0; i<N; i++){
            answer[i] = list.get(i).index;
        }
        
        return answer;
    }
}
