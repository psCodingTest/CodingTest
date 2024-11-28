# 문제 링크
https://school.programmers.co.kr/learn/courses/30/lessons/17680
---

## 문제 설명

지도개발팀에서 근무하는 제이지는 지도에서 도시 이름을 검색하면 해당 도시와 관련된 맛집 게시물들을 데이터베이스에서 읽어 보여주는 서비스를 개발하고 있다.
이 프로그램의 테스팅 업무를 담당하고 있는 어피치는 서비스를 오픈하기 전 각 로직에 대한 성능 측정을 수행하였는데, 제이지가 작성한 부분 중 데이터베이스에서 게시물을 가져오는 부분의 실행시간이 너무 오래 걸린다는 것을 알게 되었다.
어피치는 제이지에게 해당 로직을 개선하라고 닦달하기 시작하였고, 제이지는 DB 캐시를 적용하여 성능 개선을 시도하고 있지만 캐시 크기를 얼마로 해야 효율적인지 몰라 난감한 상황이다.

어피치에게 시달리는 제이지를 도와, DB 캐시를 적용할 때 캐시 크기에 따른 실행시간 측정 프로그램을 작성하시오.

---


## 문제 접근 과정

1. 소문자와 대문자 구분 x -> `도시이름 소문자로 통일`
2. LRU이며, 캐시크기가 존재 -> `Map으로 <도시,index>로 설정`
3. 캐시 구현 -> `캐시 hit, 캐시 안 찬 상태에서 miss, 캐시 꽉 찬 상태에서 miss`
4. 캐시 꽉찼을 때 어떤걸 밀어낼지? -> `Map 반복문으로 index차이 가장 큰걸 밀어냄`

---


## 시간복잡도

- 최악의 경우 도시이름 = 100,000, 캐시 사이즈 = 30임.
- 이 경우, 정말 최악의 경우로 잡아도 3,000,000의 시간 복잡도
- 따라서, 문제없이 통과할 것이라 생각

## 느낀점

- LRU 굉장히 쉬운 알고리즘이라 생각했는데, 구현 은근 시간 걸렸음





## 코드

```java
import java.util.*;

class Solution {
    public int solution(int cacheSize, String[] cities) {
        
        Map<String,Integer> map = new HashMap<>();
       
        
        int time =0; // 실행시간
        
        for(int i=0; i<cities.length; i++){
            String city = cities[i].toLowerCase(); // 소문자 만들기
            
            //캐시 hit
            if(map.containsKey(city)){ 
                map.put(city, i);
                time+=1;
            }
            //캐시 miss
            else{
                if(cacheSize > 0){ // 0이 아닐경우에만 실행
                    if(map.size() < cacheSize){ // 캐시가 안 찼을 경우, 그냥 삽입
                        map.put(city, i);
                    }
                    else{ // 캐시가 꽉 찼을 경우, 하나 밀어내고 삽입
                        int min = Integer.MAX_VALUE;
                        String minKey=null;
                        for(String key : map.keySet()){
                            if(min > map.get(key)){
                                min = map.get(key);
                                minKey = key;
                            }
                        }

                        map.remove(minKey);
                        map.put(city, i);
                    }
                }
                
                time+=5;
            }
        }
        
        return time;
    }
}

//Set으로 현재 Cache 있는지 확인
//PriorityQueue로 오름차순 정렬 (여기서 index가 제일 낮은게 탈락 되는 구조)

