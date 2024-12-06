## 문제 링크
https://www.acmicpc.net/problem/1197


## 접근 과정
- 제목에서 알 수 있듯이 MST 문제이기 떄문에 MST 에서 주로 사용하는 prim, kruskal 알고리즘을 사용하여 풀었습니다

## 시간 복잡도

O(V + E)

## 코드
```java
import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;
import java.util.ArrayList;
import java.util.List;
import java.util.PriorityQueue;
import java.util.Queue;
import java.util.StringTokenizer;

public class Main {
	static int[] dist;

	public static void main(String[] args) throws IOException {
		BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
		StringTokenizer st = new StringTokenizer(br.readLine());
		int V = Integer.parseInt(st.nextToken());
		int E = Integer.parseInt(st.nextToken());

		List<List<Node>> graph = new ArrayList<>();
		dist = new int[V + 1];
		for (int i = 0; i <= V; i++)
			graph.add(new ArrayList<>());

		for (int i = 0; i < E; i++) {
			st = new StringTokenizer(br.readLine());
			int from = Integer.parseInt(st.nextToken());
			int to = Integer.parseInt(st.nextToken());
			int cost = Integer.parseInt(st.nextToken());

			graph.get(from).add(new Node(to, cost));
            graph.get(to).add(new Node(from, cost));
		}

		prim(graph, V);
	}

	private static void prim(List<List<Node>> graph, int v) {
		Queue<Node> q = new PriorityQueue<>();
		boolean[] isVisited = new boolean[v + 1];
		q.offer(new Node(1, 0));
		int total = 0;
		while (!q.isEmpty()) {
			Node curr = q.poll();
			if (isVisited[curr.index]) continue;
			isVisited[curr.index] = true;
			total += curr.cost;
			for (Node node : graph.get(curr.index)) {
				if (!isVisited[node.index]) {
					q.offer(new Node(node.index, node.cost));
				}
			}
		}
		System.out.println(total);
	}

	private static class Node implements Comparable<Node> {
		int index;
		int cost;

		public Node(int index, int cost) {
			this.index = index;
			this.cost = cost;
		}

		@Override
		public int compareTo(Node o) {
			return this.cost - o.cost;
		}
	}
}

```

```java
import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;
import java.util.ArrayList;
import java.util.Collections;
import java.util.List;
import java.util.StringTokenizer;

public class Main {
	static int[] parent;

	public static void main(String[] args) throws IOException {
		BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
		StringTokenizer st = new StringTokenizer(br.readLine());
		int V = Integer.parseInt(st.nextToken());
		int E = Integer.parseInt(st.nextToken());

		List<Node> edges = new ArrayList<>();
		parent = new int[V + 1];

		for (int i = 1; i <= V; i++) {
			parent[i] = i;
		}

		for (int i = 0; i < E; i++) {
			st = new StringTokenizer(br.readLine());
			int from = Integer.parseInt(st.nextToken());
			int to = Integer.parseInt(st.nextToken());
			int cost = Integer.parseInt(st.nextToken());
			edges.add(new Node(from, to, cost));
		}

		Collections.sort(edges, (o1, o2) ->
			o1.cost - o2.cost
		);

    		int result = 0;
		for (Node edge : edges) {
			if (find(edge.from) != find(edge.to)) {
				union(edge.from, edge.to);
				result += edge.cost;
			}
		}

		System.out.println(result);
	}

	private static void union(int a, int b) {
		int parentA = find(a);
		int parentB = find(b);
		if (parent[parentA] < parent[parentB]) {
			parent[parentA] = parent[parentB];
		} else {
			parent[parentB] = parent[parentA];
		}
	}

	private static int find(int a) {
		if (parent[a] != a) {
			parent[a] = find(parent[a]);
		}
		return parent[a];
	}

	private static class Node {
		int from;
		int to;
		int cost;

		public Node(int from, int to, int cost) {
			this.from = from;
			this.to = to;
			this.cost = cost;
		}
	}
}

```
##  느낀점

- 기존에는 Prim 방식을 주로 활용하다 보니 구현은 쉽게 하였지만 Kruskal 을 사용하여 구현했을시 어려움을 겪었습니다
- 또한 Kruskal 을 활용하여 인접배열로 구현할 시 메모리 초과가 발생하여 고치는데 시간이 걸렸습니다.
