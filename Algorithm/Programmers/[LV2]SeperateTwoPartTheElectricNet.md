2023.06.10

# __[프로그래머스 LV2] 전력망 두개로 나누기__

----

## __문제__

[전력망 두개로 나누기](https://school.programmers.co.kr/learn/courses/30/lessons/86971)<br><Br>


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