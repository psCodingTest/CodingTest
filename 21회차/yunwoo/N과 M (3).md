# 백준 15651번: N과 M (3)
* https://www.acmicpc.net/problem/15651

<br>

## 시간복잡도
* (1 ≤ M ≤ N ≤ 7)
* O(N**M)

<br>

## 풀이
* 백트래킹
* 중복 순열

<br>

## 어려웠던 부분
* 출력 시간초과 -> BufferedWriter를 적극 활용하자

<br>

## 코드
```java
import java.io.*;

public class Main {
    public static void main(String[] args) throws IOException {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        String[] s = br.readLine().split(" ");
        int N = Integer.parseInt(s[0]);
        int M = Integer.parseInt(s[1]);
        //중복이 가능한 순열

        BufferedWriter bw = new BufferedWriter(new OutputStreamWriter(System.out));
        dfs(N, M, 0, "", bw);
        bw.flush();
        bw.close();
    }

    private static void dfs(int N, int M, int depth, String array, BufferedWriter bw) throws IOException {
        if (depth == M) {
            bw.write(array);
            bw.newLine();
            return;
        }

        for (int n = 1; n <= N; n++) {
            dfs(N, M, depth+1, array + n + " ", bw);
        }
    }
}
```

