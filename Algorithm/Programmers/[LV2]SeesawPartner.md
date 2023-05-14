2023.05.13

# __[프로그래머스 LV2] 시소 짝꿍__

MAP, Euclidean Algorithm

----

## __문제__

[시소 짝꿍](https://school.programmers.co.kr/learn/courses/30/lessons/152996#qna)<br><Br>

## __나의 풀이__(풀이 참조)
```c++
#include <string>
#include <vector>
#include <map>

using namespace std;

long long GetDistinct(int number)
{
    long long combNum = 0;
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
    // 유클리드 호제법
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

주의해야 할 점은 weights의 길이가 100000개 까지 주어질 수 있기 때문에

만약 모든 요소가 중복되는 무게라면 GetDistinct함수에서 반환되는 값이 int범위를 벗어나 오류가 발생할 수 있습니다. 

따라서 long long으로 반환해줘야합니다.
<br><Br>

## 최대공약수 GCD Greatest Common Divisor

: 두 자연수의 공통된 약수 중 가장 큰 수<br><Br>

## 최소공배수 LCM Least Common Multiple

: 두 자연수의 공통된 배수 중 가장 작은 수<br><Br>

## __유클리드 호제법 Euclidean Algorithm__

두 자연수를 입력받아 최대공약수를 구하려고 할 경우,

2부터 두 자연수 중 작은 수 까지 모두 나누어보면서 가장 큰 공약수를 구하면 

시간복잡도는 O(N)이 됩니다.

__유클리드 호제법__ 은 시간복잡도를 __O(logN)__ 으로 줄일 수 있습니다.<br><BR>

<img src = "https://github.com/EunYoungP/TIL/assets/80774412/32d683ba-8622-4dd0-b341-7ffa93c9c4c8"></img>

___<유클리드 호제법 수식>___

a>b 일 때,

a와 b의 최대공약수는 a % b 와 b의 최대공약수와 같습니다.

여기서 a % b 를 c라고 한다면,

a와 b의 최대공약수는 b와 c의 최대공약수와 같다고 할 수 있습니다.<br><Br>

그렇다면 b % c를 또한 d라고 한다면

b와 c의 최대공약수는 c와 d의 최대공약수와 같습니다.

이렇게 계속 나눗셈을 진행하다가 나머지가 0이 될 경우의 __나눠지는 수__ 즉 두 입력값 중 더 __작은 수__ 가 최대공약수가 됩니다.<br><Br>

최소공배수는 a와 b의 곱을 a와 b의 최대공약수로 나눈 것과 같다.