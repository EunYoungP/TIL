2023.03.11

# __[프로그래머스 LV3] 단어변환__

[프로그래머스](https://school.programmers.co.kr/learn/courses/30/lessons/43163)

DFS/BFS

---- 

<BR>

## __문제__

<img src = "https://user-images.githubusercontent.com/80774412/224491091-32db1115-6c32-4106-b2a7-0d2f09e77330.PNG"></img>
<BR><BR>


## __나의 풀이__(오답)
```c++
#include <string>
#include <vector>
#include <queue>
#include <algorithm>
#include <iostream>

using namespace std;

void dfs(vector<string> words, string begin, string target, vector<bool> visited, int dist, int& answer)
{
    if(dist >= words.size()-1)
        return;
    
    if(begin == target)
    {
        answer = min(answer, dist);
        return;
    }

    for(int i = 0; i < words.size(); i++)
    {
        if(visited[i])
            continue;
    
        string compStr = words[i];
        int count = 0;

        for(int j = 0;j < compStr.size(); j++)
        {
            if(begin[j] == compStr[j])
                count++;
        }

        if(count == compStr.size()-1)
        {
            visited[i] = true;
            dfs(words, compStr, target, visited, dist+1, answer);
        }
    }
}

int solution(string begin, string target, vector<string> words) {
    // int answer = 100; 나올 수 있는 최대값을 50 이기 떄문에 더 큰 100을 대입해줍니다.
    int answer = 0;
    int size = words.size();
    vector<bool> visited(words.size());

    if(find(words.begin(), words.end(), target) == words.end())
    {
        return 0;
    }

    dfs(words, begin, target, visited, 0, answer);
    return answer;
}
```

매번 answer 변수에 대입되는 최소값을 답으로 책정하기 때문에 

맨 처음 asnwer 값을 나올 수 있는 최대값보다 큰 값으로 초기화해야 합니다.

위 코드에서는 0 으로 초기화해서 어떤 값이 나오든 0 이 최소값이 되었기 때문에 풀이되지 않았습니다.