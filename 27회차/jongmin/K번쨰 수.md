# 백준 1300번 K번째 수
* https://www.acmicpc.net/problem/1300

<br>

## 시간복잡도


## 풀이



이 배열의 최대 크기는 N^2, 즉 10^5×10^5=10^10 까지 갈 수 있기 때문에

완전 탐색으로 모든 값을 탐색하는 것은 시간상 불가능합니다.

따라서 이분 탐색을 통한 최적화된 접근이 필요합니다.



이 문제는 인덱스가 아니라 값 자체를 기준으로 이분 탐색을 수행합니다.

이때 탐색 구간은

left: 1 (최소 곱셈 결과)

right: N×N (최대 곱셈 결과)

이 구간 안에서 mid 값을 하나 선택하고,

해당 mid 값 이하의 값이 곱셈표에서 몇 개인지를 세는 count(mid) 를 계산합니다.


count(mid)는 곱셈표에서 mid 이하인 수가 몇 개 존재하는지를 알려줍니다.

for (int i = 1; i <= N; i++) { cnt += Math.min(mid / i, N); }
i행에서는 i × 1, i × 2, ..., i × N 까지 존재합니다.

그 중 i × j ≤ mid를 만족하는 j의 개수는 mid / i (단, 최대 N개)

모든 행을 돌면서 mid 이하의 수가 몇 개인지를 누적하면 총 개수가 나옵니다.

- count(mid) ≥ K
  - mid보다 작거나 같은 수가 K개 이상이므로, K번째 수는 mid 이하임
  - 더 작은 값 중에 정답이 있을 수 있으므로 right = mid - 1, answer = mid

- count(mid) < K
  - mid보다 작은 수가 K개보다 적음
  - K번째 수는 mid보다 더 커야 하므로 left = mid + 1

위와 같은 접근법들을 통해서 문제를 해결 할 수 있습니다.


## 코드
```java
import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;

public class Main{
    static int N, K;

    public static void main(String[] args) throws IOException {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        N = Integer.parseInt(br.readLine());
        K = Integer.parseInt(br.readLine());

        long left = 1;
        long right = (long)N * N;
        long answer = 0;
        while (left <= right) {
            long mid = (left + right) / 2;
            if (count(mid) >= K) {
                answer = mid;
                right = mid - 1;
            } else {
                left = mid + 1;
            }
        }
        System.out.println(answer);
    }

    private static long count(long mid) {
        int cnt = 0;
        for (int i = 1; i <= N; i++) {
            cnt += (int)Math.min(mid / i, N);
        }
        return cnt;
    }
}

```

