2023.05.28

# __[프로그래머스 LV2] 피로도__

----

## __문제__

[피로도](https://school.programmers.co.kr/learn/courses/30/lessons/87946)<br><Br>


## __나의 풀이__
```c++
#include <string>
#include <vector>
#include <queue>

using namespace std;

struct Dungeon
{
    int minFatigue;
    int consumFatigue;
    
    bool operator< (const Dungeon other)const
    {
        return other.consumFatigue < consumFatigue;
    }
    
Dungeon(int _m, int _c){minFatigue = _m; consumFatigue = _c;}
};

int solution(int k, vector<vector<int>> dungeons) {
    int answer = 0;
    priority_queue<Dungeon> pq;
    for(int i = 0; i < dungeons.size(); i++)
    {
        pq.push(Dungeon(dungeons[i][0], dungeons[i][1]));
    }
    
    Dungeon cur;
    while(!pq.empty())
    {
        cur = pq.top();
        if(k > cur.consumFatigue)
        {
            pq.pop();
            continue;
        }
        else
        {
            if(cur.minFatigue > k)
            {
                pq.pop(cur);
                pq.push(cur);
            }
        }
        pq.pop();
    }

    
    return answer;
}
```

우선순위 큐를 이용하여 사용하여야 하는 피로도를 기준으로 정렬하려고 해보았습니다.

## __나의 풀이2__

```c++
#include <string>
#include <vector>
#include <queue>

using namespace std;

// 탐험할 수 있는 최대 던전 수
int answer = 0;

void dfs(int k, int cnt, vector<vector<int>>& dungeons, vector<bool> visited)
{
    if(answer < cnt)
        answer = cnt;
    
    for(int i = 0; i < dungeons.size(); i++)
    {
        if(!visited[i] && dungeons[i][0] <= k)
        {
            visited[i] = true;
            int cur = k - dungeons[i][1];
            int curCount = cnt+1;
            dfs(cur, curCount, dungeons, visited);
            visited[i] = false;     // 방문체크 원상복귀
        }
    }
}

int solution(int k, vector<vector<int>> dungeons) {

    vector<bool> visited(dungeons.size());
    
    dfs(k, 0, dungeons, visited);
    
    return answer;
}
```

주의해야할 점은 dfs풀이로 모든 분기를 탐색하기위한 경우,

방문검사를 실행할 때 조건을 만족하는 객체를 방문체크해주고 다음 분기로 넘겨준 후,

새로운 분기를 검사하기 전에 다시 false로 만들어줘야한다는 것입니다.<br><Br>

그래야 새로운 모든 분기를 검사할 수 있습니다.

분기 하나 당 O(N)의 시간복잡도를 가지므로 총 시간복잡도 또한 O(N)이 됩니다.