# 산책 (small)

- https://www.acmicpc.net/problem/22868

<br>

## 시간복잡도

## 코드

```java
import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;
import java.util.StringTokenizer;

public class Main {
    public static void main(String[] args) throws IOException {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        StringTokenizer st = new StringTokenizer(br.readLine());
        int N = Integer.parseInt(st.nextToken());
        int K = Integer.parseInt(st.nextToken());
        char[] arr = br.readLine().toCharArray();
        int result = 0;
        boolean[] visited = new boolean[N + 1];
        for (int i = 0; i < arr.length; i++) {
            if (arr[i] == 'H')
                for (int j = Math.max(i - K, 0); j <= Math.min(i + K, N - 1); j++) {
                    if (arr[i] != arr[j] && !visited[j]) {
                        result++;
                        visited[j] = true;
                        break;
                    }
                }
        }
        System.out.println(result);
    }
}
```