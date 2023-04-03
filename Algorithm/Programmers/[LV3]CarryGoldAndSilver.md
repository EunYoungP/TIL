2023.04.03

# __[프로그래머스 LV3] 금과 은 운반하기__

이분탐색

## __문제__

[금과 은 운반하기](https://school.programmers.co.kr/learn/courses/30/lessons/86053)<br><Br>

## __나의 풀이__(오답)
```c++
#include <string>
#include <vector>
#include <map>

using namespace std;

// 목표 금, 목표 은, 금, 은, 최대 운반광물 무게, 편도 이동 시간
long long solution(int a, int b, vector<int> g, vector<int> s, vector<int> w, vector<int> t) {
    long long answer = -1;
    map<float, int, greater<float>> cityIndex;
    int goldTime = 0;
    int silverTime = 0;
    
    // 금 도시 정렬
    for(int i= 0; i < g.size(); i++)
    {
        if(g[i] == 0)continue;
        float temp = g[i]/t[i]*w[i];
        cityIndex.insert(make_pair(temp, i));
    }
    
    // 금 운반
    for(auto mit : cityIndex)
    {
        int curIndex = mit.second;
    }
    
    // 은 도시 정렬
    for(int i= 0; i < s.size(); i++)
    {
        if(s[i] == 0)continue;
        float temp = s[i]/t[i]*w[i];
        cityIndex.insert(make_pair(temp, i));
    }
    
    return answer;
}
```

map을 이용하여 각 도시의 운반률을 key, 도시의 번호를 value로 하여,

key값이 가장 큰 값을 가지는 도시 순서대로 운반계산을 하려고 했습니다.

하지만 이런 방식으로 하니 금을 운반할 때 아무리 운반률이 좋은 도시라도 두 번쨰 운반은 왕복이 됨으로써 그 다음 도시보다 효율이 안 좋을 수 있다는 경우가 제외 되었습니다.

참고한 풀이에서는 이분탐색이라는 알고리즘을 사용합니다.

처음 접하는 알고리즘이기 때문에 어떤 알고리즘인지 알아보겠습니다.
