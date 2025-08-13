# A를 B로

- https://www.acmicpc.net/problem/13019

<br>

## 시간복잡도

## 코드

```java
import java.io.*;
import java.util.*;


public class Main {

    public static void main(String[] args) throws IOException {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        String A = br.readLine();
        String B = br.readLine();

        boolean result = isAvailable(A, B);
        if (!result){
            System.out.println(-1);
            return;
        }

        char[] arrA = A.toCharArray();
        char[] arrB = B.toCharArray();

        int idxA = A.length()-1;
        int idxB = B.length()-1;

        int count = 0;
        while(idxA>=0 && idxB>=0){
            if (arrA[idxA]!=arrB[idxB]){
                count++;
                idxA--;
            } else if (arrA[idxA]==arrB[idxB]){
                idxA--;
                idxB--;
            }
        }
        System.out.println(count);
    }

    static boolean isAvailable(String a, String b) {
        char[] x = a.toCharArray(), y = b.toCharArray();
        Arrays.sort(x);
        Arrays.sort(y);
        return Arrays.equals(x, y);
    }
}
```