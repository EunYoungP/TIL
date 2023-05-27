2023.05.27

# __[프로그래머스 LV2] 혼자 놀기의 달인__

----

## __문제__

[혼자 놀기의 달인](https://school.programmers.co.kr/learn/courses/30/lessons/131130)<br><Br>


## __나의 풀이__
```c++
#include <string>
#include <vector>
#include <queue>
#include <algorithm>
#include <iostream>

using namespace std;

int solution(vector<int> cards) {
    // 1번 상자 그룹에 속한 상자 수 * 2번 상자 그룹에 속한 상자 수
    int answer = 0;
    
    vector<bool> visited(cards.size());
    vector<int> group(cards.size(), 0);
    queue<int> q;
    
    q.push(1);
    int gIndex = 0;
    while(!q.empty())
    {
        int curIndex = q.front() - 1;
        q.pop();
        int curValue = cards[curIndex];
        
        while(visited[curIndex] == false)
        {
            visited[curIndex] = true;
            group[gIndex] += 1;
            curValue = cards[curIndex-1];
            curIndex = cards[curIndex-1];
        }
        
        for(int i = 0; i <visited.size(); i++)
        {
            if(visited[i] == false)
            {
                cout << gIndex << " " << i+1 << endl;
                q.push(i+1);
                break;
            }
        }
        gIndex++;
    }
    
    sort(group.begin(), group.end(), greater<int>());
    
    if(group[0] == cards.size())
        return 0;
    else
        answer = group[0] * group[1];
    
    return answer;
}
```

큐를 사용하여 마치 bfs와 같은 풀이로 문제를 풀었습니다.

만약 모든 상자가 한번의 작업에 연결되어있다면 마지막 하나의 작업에 사용된 상자의 개수를 나타내는 group의 요소를 비교할 때,

요소들을 내림차순으로 정렬하여 가장 큰 첫번째 요소가 카드의 개수와 같다면 0으로 반환되도록 예외처리 했습니다.



