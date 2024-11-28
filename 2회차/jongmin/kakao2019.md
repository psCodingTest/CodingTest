# 문제 링크
https://school.programmers.co.kr/learn/courses/30/lessons/42889

##  느낀점

- 레벨 1 의 문제이긴 하지만 확실히 카카오 코테이기 때문에 시간이 좀 걸렸던것 같습니다.
- 코드를 다 작성한뒤 보니 너무 지저분한 요소가 많아 신경을 쓰면서 작성하도록 유의해야 할 것 같습니다.

---

## 문제 설명

- 스테이지의 개수 `N` 과 해당 스테이지에 참여한 사람들이 어디까지 도달하였는지에 대한 `stage` 라는 Integer 배열이 주어집니다
- 각 스테이지 마다 `실패율`을 계산하고 `실패율`이 `높은 스테이지`부터 `내림차순`으로 스테이지의 번호가 담겨있는 배열을 return 하도록 하는것이 문제의 요점입니다.


---


## 문제 접근 과정

1. 저는 처음으로 각 스테이지마다 몇명의 사람들이 도전하고 있는지에 대해 구하기 위해 HashMap 을 사용하였습니다
2. 두번째로 내림차순의 조건을 설정하였습니다 만약 실패율이 똑같은 경우에는 작은 번호의 스테이지가 오도록 설정하였습니다
3. Stage 라는 객체를 생성하여 활용하였습니다.

---


## 시간복잡도
스테이지별 도전자를 카운트 할때 사용한 반복문 BigO(N) \
우선순위큐에 삽입하기 위해 사용된 반복문 BigO(N log N)

총 BigO(N + N log N) 정도의 시간이 걸립니다.
## 코드

```java
import java.util.*;

class Solution {
    public int[] solution(int N, int[] stages) {

		Map<Integer, Integer> map = new HashMap<>();
		int people = stages.length;

		// 스테이지별 도전 중인 사용자 수 카운트
		for (int i = 0; i < stages.length; i++) {
			map.put(stages[i], map.getOrDefault(stages[i], 0) + 1);
		}

		Queue<Stage> queue = new PriorityQueue<>(new Comparator<Stage>() {
			@Override
			public int compare(Stage o1, Stage o2) {
				if (o1.clear == o2.clear) {
					return o1.stage - o2.stage; // 실패율이 같으면 스테이지 번호가 작은 순으로
				} else {
					return Float.compare(o2.clear, o1.clear); // 실패율이 높은 순으로
				}
			}
		});

		for (int i = 1; i <= N; i++) {
			if (map.get(i) == null) {
				queue.offer(new Stage(i, 0));
				continue;
			}
			Integer ing = map.get(i);
			queue.offer(new Stage(i, (float) ing / people));
			people -= ing;
		}

		int[] answer = new int[N];
		int i = 0;
		while (!queue.isEmpty()) {
			Stage poll = queue.poll();
			answer[i] = poll.stage;
			i++;
		}

		return answer;
	}

	public static class Stage {
		int stage;
		float clear;

		public Stage(int stage, float clear) {
			this.stage = stage;
			this.clear = clear;
		}
	}
}