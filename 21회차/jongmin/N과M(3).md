## 문제 링크
* https://www.acmicpc.net/problem/15651

## 시간복잡도

해당 문제의 최대 크기는 7이기 때문에 시간복잡도상 가장높은 7!^2 을 해도 시간범위 내에들기떄문에 완탐으로 풀었습니다.


## 코드
```java

import java.io.BufferedReader;
import java.io.BufferedWriter;
import java.io.IOException;
import java.io.InputStreamReader;
import java.io.OutputStreamWriter;
import java.util.ArrayList;
import java.util.List;
import java.util.StringTokenizer;

public class Boj15651 {
	static int N, M;
	static BufferedWriter bw = new BufferedWriter(new OutputStreamWriter(System.out));

	public static void main(String[] args) throws IOException {
		BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
		StringTokenizer st = new StringTokenizer(br.readLine());

		N = Integer.parseInt(st.nextToken());
		M = Integer.parseInt(st.nextToken());
		List<Integer> comb = new ArrayList<>();
		dfs(comb);
		bw.flush();
		bw.close();
	}

	private static void dfs(List<Integer> comb) throws IOException {
		if (comb.size() == M) {
			for (int i = 0; i < comb.size(); i++) {
				bw.write(comb.get(i) + " ");
			}
			bw.newLine();
			return;
		}
		for (int i = 1; i <= N; i++) {
			comb.add(i);
			dfs(comb);
			comb.remove(comb.size() - 1);
		}
	}
}

``` 