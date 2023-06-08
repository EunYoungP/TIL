2023.06.07

# __[프로그래머스 LV2] k진수에서 소수 개수 구하기__

----

## __문제__

[k진수에서 소수 개수 구하기](https://school.programmers.co.kr/learn/courses/30/lessons/92335#qna)<br><Br>


## __나의 풀이__
```c++
#include <string>
#include <vector>
#include <iostream>
#include <cmath>

using namespace std;

// 소수 판별
bool isPrime(long x)
{
    if(x == 1)
        return false;
    
    for(long i = 2; i <= x/2; i++)
    {
        if(x % i == 0)
            return false;
    }
    return true;
}

// 양의 정수, k진수
int solution(int n, int k) {
    // 소수의 개수
    int answer = 0;
    
    // 1. k진수 변환
    vector<long> s;
    while(n / k != 0)
    {
        s.push_back(n % k);
        n /= k;
    }
    s.push_back(n % k);
    
    // 2. 구해진 소수들을 반대로 정렬하여 문자열 변환
    string change;
    for(int i = s.size()-1; i >= 0; i--)
    {
        change += to_string(s[i]);
    }
    
    // P0P /P0 /0P /P
    // 3. 소수 개수 찾기
    int index = 0;
    vector<string> primes(s.size());
    for(int i = 0; i < change.size(); i++)
    {
        if(change[i] != '0')
        {
            primes[index] += change[i];
            
            if(change[i+1] == '0' || i+1 >= change.size() )
            {
                if(isPrime(stol(primes[index])))
                {
                    answer++;
                }
                index++;
            }
        }
    }
    
    return answer;
}
```
하나의 테스트 케이스에서 오류가 발견되었습니다.

소수를 구하는 방법을 변경해서 시간초과를 해결해야합니다.