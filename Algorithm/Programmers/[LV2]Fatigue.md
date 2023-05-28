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