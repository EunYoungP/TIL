2023.05.19

# __[프로그래머스 LV2] 디펜스 게임__

우선순위큐

----

## __문제__

[디펜스 게임](https://school.programmers.co.kr/learn/courses/30/lessons/142085#qna)<br><Br>

## __나의 풀이__
```c++
#include <string>
#include <vector>
#include <algorithm>
#include <queue>
#include <iostream>

using namespace std;

bool cmp(int a, int b)
{
    return a > b;
}

struct Army
{
    int num;
    Army(int _a){num = _a;}
    
    // 오름차순으로 우선순위 큐에 담긴값들을 정렬합니다.
    bool operator< (const Army& other)const
    {
        return num > other.num;
    }
};

int solution(int n, int k, vector<int> enemy) {
    int answer = 0;

    // 1. 우선순위 큐에 순서대로 값을 오름차순으로 대입합니다.
    priority_queue<Army> pq;
    int total = 0;

    for(int i = 0; i < enemy.size(); i++)
    {
        pq.push(Army(enemy[i]));
        
        // 2. 무적권의 개수만큼의 적목록을 채워줍니다.
        if(i >= k)
        {
            // 3. 그 이후 입력되는 목록들을 하나씩 출력하여 값을 누적합니다.
            total += pq.top().num;
            pq.pop();

            // 4. 병사의 수와 누적된 값을 비교하여 라운드 진행을 제어합니다.
            if(total > n)
                return answer;
        }
        // 라운드 진행 수를 누적시킵니다.
        answer++;
    }
    
    return answer;
}
```

처음에는 단순히 enemy 벡터를 오름차순으로 정렬하여

첫 번째 값부터 누적된 값을 병사의 수와 비교하여 라운드의 수를 구해봤습니다.

하지만 라운드의 순서를 무시하면 안되는 문제이기 때문에 enemy 목록에서 하나씩 값을 꺼내어 비교해야했습니다.<br><BR>

그렇다면 미리 모든 라운드목록을 정렬해놓지 않고, 

순서대로 비교 목록에 입력된 값들을 정렬하는 방식을 사용하기 위해 우선순위큐를 사용했습니다.

우선순위큐를 Army 구조체의 연산자 오버로딩을 이용한 오름차순 정렬 상태롱 만들었습니다.

무적권은 전부 사용해야 더 많은 라운드를 진행할 수 있기 때문에

무적권의 개수만큼 우선순위큐에 값을 삽입하고 그 후 입력된 우선순위큐의 top()값들을 누적 덧셈하여 현재 병사수와 비교했습니다.<Br><Br>