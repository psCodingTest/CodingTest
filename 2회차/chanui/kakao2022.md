## k진수에서 소수 개수 구하기
* [문제 링크](https://school.programmers.co.kr/learn/courses/30/lessons/92335)

## 배운점
* 프로그래머스 시간초과 기준은 10초라는 것을 배웠습니다.

## 문제 접근 과정
1. 소수 판별 알고리즘이 필요하다고 생각함
2. P대상이 되는 수를 구할때 stoi나 stol 쓰면 되겠다고 생각함
3. n최대가 100만이고 k최소가 3이므로 P대상중 int32범위를 벗어나는 수가 있을거라고 생각
4. k진수중 19자리(int64)가 넘어가는 경우는 없을거라고 생각하고 소수 판별에 $O(\sqrt{N})$ 풀이 생각함
5. 통과 이후 예외 케이스 확인함
   * 최악의 경우의 수 ex. 797161, 3 -> 1111111111111 : 13자리 이므로 시간안에 충분히 들어옴

## 시간복잡도
* 소수 판별 필요한 시간복잡도 - $O(\sqrt{N})$

## CPP 코드
~~~cpp
#include <iostream>
#include <string>
#include <algorithm>
#include <vector>
using namespace std;
using ll = long long;

bool isPrime(ll num) {
    if(num < 2LL) return false;
    for(ll i=2; i*i<=num; i++){
        if(num%i == 0) return false;
    }
    return true;
}

ll stolReverse(string& str) {
    reverse(str.begin(), str.end());
    return stol(str);
}


int solution(int n, int k) {
    vector<ll> nums;
    string res = "";

    while(n) {
        if(n%k == 0) {
            if(res.size() > 0) {
                nums.push_back(stolReverse(res));
                res.clear();
            }
        } else {
            res.push_back('0' + n%k);
        }
        n /= k;
    }
    
    if(res.size()!=0) {
         nums.push_back(stolReverse(res));
    }
    
    int ans = 0;
    for(int i=0; i<nums.size(); i++) {
        if(isPrime(nums[i])) ans++;
    }
    
    return ans;
}
~~