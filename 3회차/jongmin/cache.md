# 문제 링크
https://school.programmers.co.kr/learn/courses/30/lessons/17680


---

## 문제 설명

이 문제는 **LRU 캐시 교체 알고리즘**을 적용해 도시 이름을 검색하고 캐시의 크기에 따른 실행시간을 측정하는 프로그램을 작성하는 문제입니다.

주어진 조건에 따라 **캐시 히트**(도시가 캐시에 있을 경우) 시 1의 실행시간, **캐시 미스**(도시가 캐시에 없을 경우) 시 5의 실행시간이 추가됩니다.

---


## 문제 접근 과정

1. 문제의 핵심인 LRU 알고리즘의 캐시를 구현하기 위해서 **Queue** 자료구조를 선정하였습니다.
---


## 시간복잡도
최대 도시 수는 100,000개이기 때문에 BigO(N * K) 이내로 풀어야 겠다고 생각 했습니다


## 코드

```java
/*
오전 10:02
오전 10:38
 */
 
import java.util.*;

class Solution {
    public int solution(int cacheSize, String[] cities) {
   
		int answer = 0;
		if (cacheSize == 0) {
			return cities.length * 5;
		}

		Queue<String> q = new LinkedList<>(); // 캐시

		String[] lowerCities = Arrays.stream(cities)
			.map(String::toLowerCase)
			.toArray(String[]::new); // 전처리

		for (int i = 0; i < lowerCities.length; i++) {
			if (!q.contains(lowerCities[i])) { // 캐시 미스
				if (q.size() >= cacheSize) { // 캐시 미스이면서 사이즈가 꽉 찼을 경우
					q.poll();
					q.offer(lowerCities[i]);
				} else { // 아닐 경우
					q.offer(lowerCities[i]);
				}
				answer += 5; // 캐시 미스로 인해 +5
			} else {  // 캐시 히트
				q.remove(lowerCities[i]); // 삭제해주고
				q.offer(lowerCities[i]); // 교체
				answer += 1;
			}
		}
		return answer;
	}
}

```

##  느낀점

- 예외케이스를 생각하지 못해 시간이 걸렸습니다 앞으로 예외케이스를 좀더 깊이 고려해야한다고 느꼇습니다.
