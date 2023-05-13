2023.05.13

# __[프로그래머스 LV2] 시소 짝꿍__

MAP

----

## __문제__

[시소 짝꿍](https://school.programmers.co.kr/learn/courses/30/lessons/152996#qna)<br><Br>

## __나의 풀이__(풀이 참조)
```c++
#include <string>
#include <vector>
#include <map>

using namespace std;

int GetDistinct(int number)
{
    int combNum = 0;
    for(int i = 1; i <= number-1; i++)
    {
        combNum += i;
    }
    return combNum;
}

// 최대공약수
int getGCD(int a, int b)
{
    int c;
    while (b != 0)
    {
        c = a % b;
        a = b;
        b = c;
    }
    return a;
}

// 최소공배수
int getLCM(int a, int b)
{
    return a * b / getGCD(a, b);
}

long long solution(vector<int> weights) {
    // 완전한 균형 == 시소 짝꿍
    // 사람 무게 * 거리
    long long answer = 0;
    map<int, int> weightsMap;
    
    // map<무게, 사람수>
    for(int i = 0; i < weights.size(); i++)
    {
        weightsMap[weights[i]]++;
    }
    
    for(auto m : weightsMap)
    {
        // 같은 무게인 사람들의 경우의 수
        answer += GetDistinct(m.second);
        
        for(auto i : weightsMap)
        {
            if(i.first != m.first)
            {
                // 최소공배수 구하기
                int lcm = getLCM(m.first, i.first);
                
                // 최소공배수가 조합중의 한 값과 동일할 경우
                if(lcm == i.first)
                    lcm *= 2;
                
                // 더 작은 조합수로 최소공배수를 나눴을 때,
                // 5미터보다 미만에 위치하는 경우
                if(lcm/min(i.first, m.first) < 5)
                    answer += i.second * m.second;
            }
        }
        weightsMap.erase(m.first);
    }
    return answer;
}
```

최소공배수와, 최대공약수를 이용하여 짝꿍이 되는 여부를 결정하는 방법입니다.

MAP을 사용해서 중복되는 무게를 같은 키를 가진 값으로 저장할 수 있었고,

중복되는 사람들의 짝꿍 조합을 얻는 함수를 통해 중복무게 사람의 짝꿍 경우의 수를 구했습니다.