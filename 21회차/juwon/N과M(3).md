# 백준 9084: N과 M (3)

- https://www.acmicpc.net/problem/15651

<br>

## 시간복잡도

- O(M^N)

<br>

## 풀이

- 중복 조합을 구현하면 됩니다.
- 재귀 함수를 사용했습니다.

  <br>

## 어려웠던 부분

<br>

## 코드

```java
import java.io.BufferedReader;
import java.io.BufferedWriter;
import java.io.IOException;
import java.io.InputStreamReader;
import java.io.OutputStreamWriter;
import java.util.StringTokenizer;

public class Main {

	public static BufferedReader br = new BufferedReader(
			new InputStreamReader(System.in));
	public static BufferedWriter bw = new BufferedWriter(
			new OutputStreamWriter(System.out));
	public static StringTokenizer st;
	public static int N, M;
	public static StringBuilder result = new StringBuilder();

	// 중복 조합을 구현
	// O(M^N)
	// 사전순
	public static void main(String[] args) throws IOException {
		st = new StringTokenizer(br.readLine());
		N = Integer.parseInt(st.nextToken());
		M = Integer.parseInt(st.nextToken());

		int[] arr = new int[N];
		int[] output = new int[M];
		for (int i = 0; i < N; i++) {
			arr[i] = i + 1;
		}


		combination(arr, output, N, M, 0);

		bw.write(result.toString());
		bw.close();
		br.close();
	}

	public static void combination(int[] arr,int[] output, int n, int r, int idx) {
		if (idx == r) {
			for (int i = 0; i < output.length; i++) {
				result.append(output[i]).append(" ");
			}

			result.append("\n");
			return;
		}

		for (int i = 0; i < n; i++) {
			output[idx] = arr[i];
			combination(arr, output, n, r, idx + 1);
		}
	}


}



```
