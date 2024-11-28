# 문제 링크
https://school.programmers.co.kr/learn/courses/30/lessons/81302

## 느낀점

- 문제 꼼꼼하게 읽기
- DFS, BFS 문제의 경우 함수 따로 분리하기
- 상,하,좌,우로 이동가능할땐 Visited 배열 활용하기



---

## 문제 설명

주어진 대기실 구조에서 사람 간 `거리두기`가 제대로 지켜지고 있는지 판별하는 문제입니다. 각 대기실은 `5x5` 크기의 배열로 주어지며, `사람(P)`, `빈 테이블(O)`, `파티션(X)`으로 구성되어 있습니다.

각 대기실에서 서로 간의 맨해튼 거리가 2 이하일 경우, 거리두기가 제대로 지켜지지 않으므로, `모든 사람간의 맨헤튼 거리가 2를 넘어야합니다`.


---


## 문제 접근 과정

1. 문제에서 대기실 크기가 5*5라고 기술 -> `그래프 문제`
2. 사용자(P)간 거리제한 존재 -> `최단거리(BFS)`
3. 맨해튼 거리 조건 존재 -> `BFS로 최대 2까지만 탐색`
4. 사용자가 상,하,좌,우로 이동가능 -> `Visited 배열 사용`

---


## 시간복잡도

- 대기실 개수 = 5
- 대기실 세로길이 = 5
- 대기실 가로길이 = 5

대기실 탐색 시간 = O(5*5) = O(25)  
최악의 BFS 탐색 시간 = O(25)   
따라서, 최악의 경우 전체 시간 복잡도는 `O(250)`



## 코드

```java
import java.util.*;

class Solution {
    static char[][] array;

    public int[] solution(String[][] places) {
        int[] answer = new int[places.length];

        for (int i = 0; i < places.length; i++) {
            answer[i] = 1; // 기본값을 1로 설정 (모두 지킨다고 가정)

            // 대기실 구조를 2차원 배열에 저장
            array = new char[5][5];
            for (int j = 0; j < 5; j++) {
                String place = places[i][j];
                for (int k = 0; k < 5; k++) {
                    array[j][k] = place.charAt(k);
                }
            }

            // 각 대기실에서 'P' 위치 탐색
            for (int j = 0; j < 5; j++) {
                for (int k = 0; k < 5; k++) {
                    if (array[j][k] == 'P') {
                        // 거리두기를 지키지 않는 경우(BFS) 발견 시 0으로 설정하고 탐색 종료
                        if (!bfs(j, k)) {
                            answer[i] = 0;
                            break;
                        }
                    }
                }
                if (answer[i] == 0) break; // 거리두기를 지키지 않은 경우 더 이상 탐색하지 않음
            }
        }
        return answer;
    }

    public boolean bfs(int i, int j){
        int[] x_p = {1,-1,0,0};// 가로
        int[] y_p = {0,0,1,-1};// 세로

        Queue<Integer[]> queue = new LinkedList<>();
        boolean[][] visited =new boolean[5][5];

        // 시작점 삽입
        queue.add(new Integer[]{i,j});
        visited[i][j] = true;

        // BFS 시작
        while(!queue.isEmpty()){
            Integer[] current = queue.poll();

            if(array[current[0]][current[1]] == 'P' && (current[0]!=i||current[1]!=j)){
                return false;
            }

            // 다음 대기실 탐색(상,하,좌,우)
            for(int q=0; q<4; q++){
                int x = current[1]+x_p[q];
                int y = current[0]+y_p[q];

                //만약, 5*5 벗어나거나 방문한 곳일시, 스킵
                if(0<=x && x<5 && 0<=y && y<5 && visited[y][x] == false){
                    //파티션이 아니고, 맨해튼 거리가 2 이하 일때만 Queue 삽입
                    if(array[y][x]!='X' && Math.abs(j-x) + Math.abs(i-y) <= 2 ){
                        queue.add(new Integer[]{y,x});
                        visited[y][x] = true;
                    }
                }
            }
        }
        return true;
    }
}

