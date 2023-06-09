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

소수를 구하는 방법을 변경해서 시간초과를 해결해야합니다.<br><Br>

## __오류 해결__

원래 사용했던 소수 구하는 방법은 소수 검사할 숫자를 나누기 2한 값까지 검사하는 방식이었습니다.

수정한 방법은 소수 검사할 대상을 제곱근한 수까지만 검사하는 방식입니다.<br><Br>

만약 100 이 소수인지 검사한다면 앞선방법으로는 50번의 검사가 이뤄져야합니다.

하지만 제곱근을 이용한다면 10번의 검사만으로 가능합니다.

```c++
// x의 소수여부 반환 함수
bool isDecimal(long x)
{
    if(x == 1)
        return false;
    
    for(long i = 2; i <= sqrt(x); i++)
    {
        if(x % i == 0)
            return false;
    }
    return true;
}
```
