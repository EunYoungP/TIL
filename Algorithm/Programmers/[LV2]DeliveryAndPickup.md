2023.05.15

# __[프로그래머스 LV2] 택배 배달과 수거하기__

----

## __문제__

[택배 배달과 수거하기](https://school.programmers.co.kr/learn/courses/30/lessons/150369#qna)<br><Br>

## __나의 풀이__(풀이 참조)
```c++
#include <string>
#include <vector>
#include <stack>

using namespace std;

// 적재 가능한 최대 개수, 집의 개수, 배달 목록, 수거 목록
long long solution(int cap, int n, vector<int> deliveries, vector<int> pickups) {
    long long answer = 0;
    stack<int> D, P;
    
    for(int d : deliveries)    
        D.push(d);
    
    for(int p : pickups)    
        P.push(p);
    
    while(!D.empty() && D.top() == 0)
        D.pop();
    
    while(!P.empty() && P.top() == 0)
        P.pop();
    
    while(!D.empty() || !P.empty())
    {
        answer += max(D.size() * 2, P.size() * 2);
        
        int deliv = 0;
        while(!D.empty() && deliv <= cap)
        {
            if(deliv + D.top() <= cap)
            {
                deliv += D.top();
                D.pop();
            }
            else
            {
                D.top() -= (cap - deliv);
                break;
            }
        }
        
        int pickup = 0;
        while(!P.empty() && pickup <= cap)
        {
            if(pickup + P.top() <= cap)
            {
                pickup +=  P.top();
                P.pop();
            }
            else
            {
                P.top() -= (cap - pickup);
                break;
            }
        }
    }
    return answer;
}
```

위 문제의 포인트는 물건을 배달하거나 수거할 경우 모두 

마지막 집에서부터 검사를 한다는 점입니다.

뒤에서부터 입력값을 입/출력 할 수 있는 스택을 이용할 수 있다는 점을 연상할 수 있습니다.<br><BR>

문제에서 택배 배달과 수거가 동시에 이루어지지 않습니다.

그렇다면 두 부분을 따로 연산할 수 있습니다.

따라서 배달과 수거 목록을 스택에 옮기고, 각 스택을 순회하며 검사합니다.<br><Br>

두 스택 중 하나의 스택이라도 값이 남아있다면 트럭은 배달 또는 수거를 하러 이동해야 합니다.

그렇다면 각 반복주기마다 두 개의 스택 중 더 큰 스택의 사이즈가 그 주기의 트럭이 이동해야할 범위입니다. 

이것을 각 주기마다 답에 더해주면 최종적으로 최단 이동 거리를 구할 수 있습니다.<br><BR>

## __다른 풀이__

```c++

#include <string>
#include <vector>
#include <stack>

using namespace std;

// 적재 가능한 최대 개수, 집의 개수, 배달 목록, 수거 목록
long long solution(int cap, int n, vector<int> deliveries, vector<int> pickups) {
    long long answer = 0;
    int deliv = 0;
    int pickup = 0;
    
    for(int i = deliveries.size()-1; i >= 0; i--)
    {
        if(deliveries[i] != 0 || pickups[i] != 0)
        {
            int count = 0;
            while(deliv < deliveries[i] || pickup < pickups[i])
            {
                count++;
                deliv += cap;
                pickup += cap;
            }
            deliv -= deliveries[i];
            pickup -= pickups[i];

            answer = answer + (long long)((i+1) * count * 2);
        }
    }
    return answer;
}
```

첫 번째 풀이에서는 각 스택을 여러번 순회하며 스택이 빌 때까지 반복하는 과정으로 풀이했습니다.

위의 풀이는 집의 수만큼만 반복문을 순회합니다.

그리고 해당 인덱스의 집에 배달 또는 수거가 모두 0인 상태가 되기위한 반복 횟수를 구해 바로 답에 대입합니다.

해당 인덱스는 집으로까지의 거리가 되고, 반복 횟수는 그 집으로의 이동 횟수가 됩니다. 이것을 두배해주면 트럭이 집에 배달과 수거를 완료하기 위해 왕복한 이동 거리를 알 수 있게 됩니다.

중요한 점은 연산 후 남은 배달 혹은 수거 가능 개수를 다음 반복문으로 이월시켜줍니다.<br><Br>

만약 5개의 배달 가능 횟수가 남은 상태로 다음 집의 택배해야할 물건의 개수가 2개라면,

그 집의 순회 횟수는 0이 될 수 있습니다.<Br><Br>