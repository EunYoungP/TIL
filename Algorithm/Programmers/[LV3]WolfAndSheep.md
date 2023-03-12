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