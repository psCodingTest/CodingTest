# 문제 링크

https://school.programmers.co.kr/learn/courses/30/lessons/258711

---

## 문제 설명

3개의 그래프 `막대`, `도넛`, `8자` 가 존재하며 한 정점을 기준으로 해당 그래프들이 몇개가 존재하는지에 대해 구하는 문제입니다.

---


## 문제 접근 과정

1. `그래프` 문제이기 떄문에 `인접 리스트`를 구현하여 풀어야 겠다고 생각하였습니다
2. 각 특징 즉 `막대`, `도넛`, `8자`의 특징을 판별하는 조건문을 만들어야 겠다고 생각하였습니다
---


## 시간복잡도
시간복잡도는 간선이 1,000,000 개가 존재하기 때문에 지수로는 풀 수 없다고 생각하였으며
O(N log N) 이내로 풀어야겠다고 생각했습니다
---

## 코드
```java
import java.util.*;


class Solution {
    static final int MAX_CAPACITY = 1_000_001;
    static boolean[] isVisited = new boolean[MAX_CAPACITY];
	static List<List<Integer>> list = new ArrayList<>(MAX_CAPACITY);
	static int eightCount = 0;
	static int donutCount = 0;
	static int barCount = 0;
	static boolean isRecursion;
    
    public int[] solution(int[][] edges) {
		int vertex = 0; // 정점

	    for (int i = 0; i <= MAX_CAPACITY; i++) {
			list.add(new ArrayList<>());
		}

		Set<Integer> set = new HashSet<>(MAX_CAPACITY); // 정점을 판별하기 위한 자료구조

		for (int i = 0; i < edges.length; i++) {
			int from = edges[i][0];
			int to = edges[i][1];
			list.get(from).add(to);
			set.add(to);
          
		} // 삽입

		for (int[] edge : edges) {
			int from = edge[0];
			if (!set.contains(from) && list.get(from).size() >= 2) {
				vertex = from;
				break;
			}
		} // 정점을 찾기위한 전처리 과정
		for (int i = 0; i < list.get(vertex).size(); i++) {
			isRecursion = false;
			dfs(list.get(vertex).get(i));
			if (!isRecursion) {
				barCount++;
			}
		}
		return new int[] {vertex, donutCount, barCount, eightCount};
	}

	private static void dfs(int start) {
		Stack<Integer> stack = new Stack<>(); // 자료구조
		stack.push(start);
		while (!stack.isEmpty()) {
			Integer curr = stack.pop();
			for (int i = 0; i < list.get(curr).size(); i++) {
				Integer nextNode = list.get(curr).get(i);
				if (list.get(curr).size() >= 2) {
					eightCount++;
					isRecursion = true;
					return;
				}
				if (isVisited[nextNode]) { // 만약 다음노드를 이미 방문을 했다면?
					donutCount++;
					isRecursion = true;
					return;
				}
				isVisited[curr] = true;
				stack.push(nextNode); // 다음 노드를 넣어준다
			}
		}
	}
}
```


##  느낀점
- 해당 문제를 풀때 거의 5시간을 소비하여 풀어 시간을 단축하자는 생각을 했습니다.
- 또한 정점이 저는 `edge` 의 `첫번쨰요소`의 0번째 지점인줄 알고 해당 노드를 기준으로 탐색을 시도했는데 알고보니 아니였습니다. 그래서 문제를 좀 더 신중히 읽는 습관을 들이는게 필요하다고 생각했습니다.
- 계속 한가지 방식으로 고집하지말자고 생각하였습니다 문제를 풀때 막힌 부분에서 여태까지 한 코드가 아까워 계속 기존의 코드를 유지한채 수정을 하다보니 무엇이 잘못된지를 파악을 잘 못했습니다 결국에는 조건절을 다 지우고 다시 작성하니 잘 되었습니다

