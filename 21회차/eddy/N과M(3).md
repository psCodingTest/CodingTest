## 문제 링크
* https://www.acmicpc.net/problem/15651

## 시간복잡도


## 코드
```java
package eddy.N과M_03_;

//https://www.acmicpc.net/problem/15651

import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;
import java.util.StringTokenizer;

public class Main {
    static int[] arr;
    static boolean[] visit;
    static int N;
    static int M;
    static StringBuilder sb = new StringBuilder(); //결과를 한 번에 출력하기 위해

    public static void main(String[] args) throws IOException {


        //입력받기
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        StringTokenizer st = new StringTokenizer(br.readLine());

        N = Integer.parseInt(st.nextToken()); //1부터 N까지
        M = Integer.parseInt(st.nextToken()); // M개를 고른 수열

        arr = new int[M];  //M개를 고른 수열
        dfs(0);
        System.out.println(sb);
    }

    //dfs
    public static void dfs(int index) {
        //M개의 숫자를 골랐을 경우
        if (index == M) {
            for (int val : arr){
                sb.append(val).append(" ");
            }
            sb.append("\n");
            return;
        }

        //시작부터 N까지 숫자 중에서 고르기
        for (int i = 1; i <= N; i++){
            arr[index] = i;
            dfs( index+1);

        }
    }
}


``` 