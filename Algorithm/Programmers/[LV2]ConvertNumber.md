2023.05.11

# __[프로그래머스 LV2] 숫자 변환하기__



----

## __문제__

[숫자 변환하기](https://school.programmers.co.kr/learn/courses/30/lessons/154538)<br><Br>

## __나의 풀이__
```c++
#include <string>
#include <vector>

using namespace std;

int solution(int x, int y, int n) {
    int answer = 0;
    vector<int> answerV;
    
    answerV.push_back(x);
    
    int a = 0;
    int b = 0;
    int c = 0;
    while(a < y || b < y || c < y)
    {
        answer++;
        for(int j : answerV)
        {
            a = j + n;
            b = j * 2;
            c = j * 3;

            if(a == y || b == y || c == y)
                return answer;
            
            answerV.push_back(a);
            answerV.push_back(b);
            answerV.push_back(c);
        }
    }
    return -1;
}
```

이대로 실행을 했더니 전체 테스트 케이스에서 반정도의 정답률을 보였습니다.

위 풀이에서는 x를 y로 만드는 과정으로 풀이했지만,

y에서 주어진 연산을 반대로 실행하여 나눗셈의 연산 결과로 나오는 float의 경우를 제외하고 검색하는 편이

더 빠를수 있습니다.

해당 풀이를 y에서 x를 찾는 과정,

bfs/dfs로 변환하여 풀이해봐야 비교할 수 있을것 같습니다.

```c++
#include <string>
#include <vector>
#include <queue>
using namespace std;

int solution(int x, int y, int n)
{
    vector<int> vis(1000001,0);
    queue<int> q;
    q.push(x);
    while (!q.empty())
    {
        int cur = q.front();
         q.pop();

        if(cur == y) 
            return vis[cur];//시작하자마자 도착할수도있다.

        int dx[3] = {cur + n, cur * 2, cur * 3};
        for(int dir=0;dir<3;dir++)
        {
            int nx = dx[dir];
            if(nx > y || vis[nx]!= 0) continue;//범위를 넘어가거나 이미 방문한적 있다면
            vis[nx] = vis[cur] + 1;
            q.push(nx);
        }
    }
    return -1;
}
```

2중 bfs로 풀이하는 방법입니다.