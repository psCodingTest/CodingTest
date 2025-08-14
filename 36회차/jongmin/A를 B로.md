# A를 B로

- https://www.acmicpc.net/problem/13019

<br>

## 시간복잡도

## 코드

```java
import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;

public class Main {
    public static void main(String[] args) throws IOException {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        char[] A = br.readLine().toCharArray();
        char[] B = br.readLine().toCharArray();


        int[] countA = new int[26];
        int[] countB = new int[26];

        for (char c : A) countA[c - 'A']++;
        for (char c : B) countB[c - 'A']++;

        for (int i = 0; i < 26; i++) {
            if (countA[i] != countB[i]) {
                System.out.println(-1);
                return;
            }
        }


        int result = 0;
        int i = A.length - 1;
        int j = B.length - 1;

        while (i >= 0 && j >= 0) {
            if (A[i] == B[j]) {
                i--;
                j--;
            } else {
                i--;
                result++;
            }
        }
        System.out.println(result);
    }
}
```