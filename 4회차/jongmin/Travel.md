# 문제 링크
https://school.programmers.co.kr/learn/courses/30/lessons/43164


## 문제 설명
주어진 항공권을 모두 이용하여 여행경로를 짜려고 합니다. 항상 `"ICN"` 공항에서 출발합니다.

항공권 정보가 담긴 2차원 배열 tickets가 매개변수로 주어질 때,\
방문하는 공항 경로를 배열에 담아 return 하도록 solution 함수를 작성해주세요.

---


## 문제 접근 과정

1. DFS를 사용한 문제로 DFS 를 구현하였습니다
---


## 시간복잡도
주어진 공항의 수는 최대 `10000`개 이기 떄문에 `N^2` 이 되어도 통과가 된다는 뜻이기에
시간복잡도는 깊게 생각하지 않았습니다


## 코드

```java
public class TravelPath {
	static boolean[] isVisited;
	static List<String> output;
	static Stack<Node> stack;
public static String[] solution(String[][] tickets) {

		stack = new Stack<>(); // DFS를 구현하기 위한 Stack 자료구조
		isVisited = new boolean[tickets.length]; // 방문 표시

		for (String[] ticket : tickets) {
			stack.push(new Node(ticket[0], ticket[1])); // 노드에 삽입
		}
		output = new ArrayList<>(); // 출력물을 저장할 output

		dfs(0, stack.get(0).from, stack.get(0).from, tickets.length); // DFS
		Collections.sort(output); // 오름차순 정렬
		return output.get(0).split(" "); // 정답반환
	}

	private static void dfs(int depth, String now, String path, int r) {
		if (depth == r) {
			output.add(path); // dfs 종료
		}
		for (int i = 0; i < r; i++) {
			if (!isVisited[i] && now.equals(stack.get(i).from)) { // 방문을 안했으며 현재(INC) 이 stack.get(i) 의 from 과같으면?
				isVisited[i] = true; // 방문처리
				Node node = stack.get(i); // 노드를 빼서
				dfs(depth + 1, node.to, path + " " + node.to, r); // "ICN ATL" 과 같은형식의 문자열로 만들어준다
				isVisited[i] = false; // 방문처리 해제후 다시 올라간다
			}
		}
	}

	public static class Node {
		String from;
		String to;

		public Node(String from, String to) {
			this.from = from;
			this.to = to;
		}
	}
}
```

##  느낀점

- **BFS** 종류의 문제를 중점으로 풀었어서 DFS 문제를 오랜만에 풀어보아서 감이 안잡혀서 시간을 잡아먹었습니다 
-  그래서 알고리즘 공부를 할때 확실히 깊이 있게 공부해야한다는것을 다시한번 깨달았습니다

---