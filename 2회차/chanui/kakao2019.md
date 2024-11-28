## 실패율 - [문제 링크](https://school.programmers.co.kr/learn/courses/30/lessons/42889)

## 느낀점
* 문제 풀면서 c++ 문법이 기억나지 않았는데... 꾸준히 문제를 풀어야 겠다고 생각했습니다.

## 문제 접근 과정
1. 실패율(실수형)에 대한 대소비교가 필요하므로 double을 사용해야 겠다고 생각함
2. 스테이지별 플레이어수가 필요하고, 스테이지 최대 개수는 500개이므로 배열에 따로 저장해두면 편하겠다고 생각함
3. 스테이지 출력은, 실패율 내림차순이고 실패율이 같은 경우 스테이지 번호 오름차순이므로 pair로 묶어서 정렬하면 편하겠다고 생각함
4. stage 길이가 최대 20만이라 stage를 이용한 $N^2$ 시간 풀이가 들어가면 안된다고 생각함
5. 스테이지에 도달한 유저가 없는 경우 해당 스테이지의 실패율은 0이므로 예외처리 해야겠다고 생각함
6. 예외 케이스가 더 있는지 확인함 
   * ex. 입력이 작은 경우: N=3, stages=[1]

## 시간복잡도
* 정렬에 필요한 시간복잡도 - $O(N{log{N}})$

## CPP 코드
~~~cpp
#include <iostream>
#include <string>
#include <vector>
#include <algorithm>
#define xx first
#define yy second
using namespace std;
using pdi = pair<double,int>;

vector<pdi> m;
double sz[501]{};

vector<int> solution(int N, vector<int> stages) {
    vector<int> ans;

    for(int i=0; i<stages.size(); i++) sz[stages[i]] += 1;

    double now = stages.size(), f=0;
    for(int i=1; i<=N; i++) {
        if(now==0) f = 0;
        else f = sz[i] / now;
        m.push_back({f,i});
        now -= sz[i];
    }

    sort(m.begin(), m.end(), [&](pdi a, pdi b) -> bool{
        if(a.xx == b.xx) return a.yy < b.yy;
        return a.xx > b.xx;
    });

    for(int i=0; i<m.size(); i++) {
        ans.push_back(m[i].yy);
    }
    
    return ans;
}
~~~