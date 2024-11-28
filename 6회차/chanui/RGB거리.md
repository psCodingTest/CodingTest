# RGB 거리
* https://www.acmicpc.net/problem/1149

## cpp 코드
```cpp
#include<bits/stdc++.h>
using namespace std;

int N;
int rgb[3][1001];
int dp[3][1001]={0,};

int main() {
    ios_base::sync_with_stdio(0);
    cin.tie(0); cin>>N;
    for(int i=1; i<=N; i++){
        for(int j=0; j<3; j++){
            cin>>rgb[j][i];
        }
    }
    
    dp[0][1] = rgb[0][1];
    dp[1][1] = rgb[1][1];
    dp[2][1] = rgb[2][1];

    for(int i=2; i<=N; i++){
        dp[0][i] = min(dp[1][i-1],dp[2][i-1]) + rgb[0][i];
        dp[1][i] = min(dp[0][i-1],dp[2][i-1]) + rgb[1][i];
        dp[2][i] = min(dp[0][i-1],dp[1][i-1]) + rgb[2][i];
    }

    int ans = min(dp[0][N],min(dp[1][N],dp[2][N]));
    cout<<ans;
}
```