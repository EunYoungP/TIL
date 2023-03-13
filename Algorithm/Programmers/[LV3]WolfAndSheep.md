2023.03.12

# __[프로그래머스 LV3] 양과늑대__

[프로그래머스](https://school.programmers.co.kr/learn/courses/30/lessons/92343)

DFS/BFS

---- 

<BR>

## __문제__

<img src = "https://user-images.githubusercontent.com/80774412/224552843-ca8e943d-8a7b-471d-b970-6cdd068124f2.PNG"></img>

<img src = "https://user-images.githubusercontent.com/80774412/224552848-284ec3e3-82fe-4643-aff1-7016ec7b4650.PNG"></img>

<img src = "https://user-images.githubusercontent.com/80774412/224552851-5316b65c-bf17-43f0-924f-c6a056cbf8e0.PNG"></img>
<BR><BR>


## __나의 풀이__(오답)
```c++
#include <string>
#include <vector>
#include <algorithm>

using namespace std;

void DFS(vector<int> info, vector<vector<int>> edges,int index,int num, int& answer)
{
    if(index == edges.size()-1)
    {
        answer = max(answer, num);
        return;
    }
    
    for(int i = 0; i < edges[index].size(); i++)
    {
        // 양이라면
        if(info[deges[index][i]] == 0)
        {
            num += 1;
        }
        // 늑대라면
        else
        {
            num -= 1;
            if(num <= 0)
                num = 0;
        }
        visited[deges[index][i]] = true;
        DFS();
    }
}

int solution(vector<int> info, vector<vector<int>> edges) {
    int answer = 0;
    
    vector<bool> visited(edges.size());
    visited[0] = true;
    
    return answer;
}
```

이진 트리 구조의 특징을 이용하여 DFS 에 적용시키는 부분이 어려웠습니다.

왼쪽 트리의 루트에서 오른쪽 트리 루트로 갈 때 방문했었던 노드들을 또 방문할 수 있기 때문에 visited 벡터의 사용을 어떤식으로든 바꿔야했습니다.

DFS 함수에서 당므으로 검색할 노드의 조건은,

현재 노드에서 이동할 수 있는 노드들을 담은 nextNode 벡터에 담겨있는 노드들로 한정됩니다.<br><Br>

## 나의 풀이(풀이 참고)
```c++
#include <string>
#include <vector>
#include <algorithm>

using namespace std;

void DFS(vector<int> info, vector<int> nextNodes, int curIndex,int wolf, int sheep, vector<vector<int>> sortEdges, int& answer)
{
    int curAnimal = info[curIndex];
    if(curAnimal)wolf++;
    else sheep++;
    
    // 해당 루트에서 양의 수가 최대값인지 비교하고 answer 참조변수에 담습니다.
    answer = max(answer, sheep);
    
    // 늑대의 수가 양의 수와 같거나 커질 경우 모든 양은 잡아먹힙니다.
    if(wolf >= sheep)return;
    
    // 현재 인덱스와 이어진 다음 노드들을 순회하며 검색합니다.
    for(int i = 0; i < nextNodes.size(); i++)
    {
        // 이어질 수 있는 노드들 중 새로 선택된 노드를 삭제합니다.
        vector<int> next = nextNodes;
        next.erase(next.begin()+i);
        
        // 새로 선택된 노드와 연결된 다음 노드들을 추가합니다.
        for(int j = 0; j < sortEdges[nextNodes[i]].size(); j++)
        {
            next.push_back(sortEdges[nextNodes[i]][j]);
        }
        // 노드정보, 선택가능노드들, 현재노드, 늑대 수, 양 수, 정렬된 연결정보, 답을 채울 변수
        DFS(info, next, nextNodes[i], wolf, sheep, sortEdges, answer);
    }
}

int solution(vector<int> info, vector<vector<int>> edges) {
    int answer = 0;
    
    vector<vector<int>> sortEdges(info.size());
    // 이진 트리를 각 노드에서 연결된 노드들을 담은 벡터로 정렬합니다.
    for(int i = 0; i < edges.size(); i++)
    {
        sortEdges[edges[i][0]].push_back(edges[i][1]);
    }
    
    vector<int>  nextNodes;
    // 0번째 노드에서 갈 수 있는 노드들을 담은 벡터입니다.
    for(int i = 0 ; i < sortEdges[0].size(); i++)
    {
        nextNodes.push_back(sortEdges[0][i]);
    }

    DFS(info, nextNodes, 0, 0, 0, sortEdges, answer);
    
    return answer;
}
```

이런 방법 외에도 비트마스크 방법이 있어 참고하여 풀이 방법을 알아봤습니다.

