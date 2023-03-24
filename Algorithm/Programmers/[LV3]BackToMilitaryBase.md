[2023.03.24](#나의-풀이참고)

# __[프로그래머스 LV3] 부대 복귀__

[프로그래머스_부대복귀](https://school.programmers.co.kr/learn/courses/30/lessons/132266)

BFS/DFS

---- 

<BR>

## __문제__

<img src = "https://user-images.githubusercontent.com/80774412/227523543-896b6315-2f05-4ea0-bcdd-3a9025dfd298.PNG"></img>

<img src = "https://user-images.githubusercontent.com/80774412/227523553-bb58c0b3-28c8-4c3f-90c6-b127dbfc6f17.PNG"></img>
<BR><BR>

## __나의 풀이__(BFS)
```c++
#include <string>
#include <vector>
#include <queue>

using namespace std;
struct Place
{
    int index, dist;
    
    Place(int _index, int _dist){index = _index; dist = _dist;}
};

// 총 지역의 수, 두 지역 왕복 가능 여부, 각 부대원 위치 지역, 강철부대의 지역
vector<int> solution(int n, vector<vector<int>> roads, vector<int> sources, int destination) {
    vector<int> answer(sources.size());
    
    vector<vector<int>> road(n+1, vector<int>(n+1));
    vector<int> dist(n+1,-1);
    //왕복 여부 정리
    for(int i= 0; i < roads.size(); i++)
    {
        road[roads[i][0]].push_back(roads[i][1]);
        road[roads[i][1]].push_back(roads[i][0]);
    }
    
    // 각 부대원의 복귀 최단시간 배열 구하기
    queue<Place> q;
    q.push(Place(destination,0));
    dist[destination] = 0;

    while(!q.empty())
    {
        Place cur = q.front();
        q.pop();

        //이어진 길 존재 검사
        for(int k = 0; k < road[cur.index].size(); k++)
        {
            int next = road[cur.index][k];
            if(dist[next] == -1 || dist[next] > cur.dist + 1)
            {
                q.push(Place(next, cur.dist+1));
                dist[next] = cur.dist + 1;
            }
        }
    }

    for(int i = 0; i < sources.size(); i++)
    {
        answer[i] = dist[sources[i]];
    }
    
    return answer;
}
```
시작점을 베이스 캠프인 destination으로 설정하고

모든 이어진 길들을 검색하며 각 장소의 최단 거리를 저장해줍니다.

마지막에 각 요원들이 배치되어있는 장소의 인덱스들을 검색하며

answer 배열에 넣어주어 반환합니다.<br><Br>

이 풀이로 간단한 테스트들은 통과했지만 시간초과로 반정도는 실패했습니다.


<BR><bR>

## __나의 풀이__(DFS)
```c++
#include <string>
#include <vector>
#include <queue>
#include <iostream>

using namespace std;
struct Place
{
    int index, dist;
    
    Place(int _index, int _dist){index = _index; dist = _dist;}
};

void DFS(int curIndex, int index, vector<vector<int>> road, vector<int>& dist)
{
    if(dist[curIndex] == -1 || dist[curIndex] > index)
        dist[curIndex] = index;

    for(int i = 0; i < road[curIndex].size(); i++)
    {
        DFS(road[curIndex][i], index+1, road, dist);
    }
    return;
}

// 총 지역의 수, 두 지역 왕복 가능 여부, 각 부대원 위치 지역, 강철부대의 지역
vector<int> solution(int n, vector<vector<int>> roads, vector<int> sources, int destination) {
    vector<int> answer(sources.size());
    
    vector<vector<int>> road(n+1,vector<int>(n+1));
    vector<int> dist(n+1, -1);
    
    for(int i= 0; i < roads.size(); i++)
    {
        //cout << roads[i][0] <<" "<< roads[i][1] << endl;
        road[roads[i][0]].push_back(roads[i][1]);
        road[roads[i][1]].push_back(roads[i][0]);
    }
    
    DFS(destination, 0, road, dist);

    for(int i= 0; i < sources.size(); i++)
    {
        answer[i] = dist[sources[i]];
    }
    
    return answer;
}
```