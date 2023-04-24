2023.04.24

# __[프로그래머스 LV2] 두 원 사이의 정수 쌍__

## __문제__

[두 원 사이의 정수 쌍](https://school.programmers.co.kr/learn/courses/30/lessons/181187)<br><Br>

## __나의 풀이__
```c++
#include <iostream>
#include <string>
#include <vector>
#include <cmath>

using namespace std;

int GetVertexNumber(int radius)
{
    return pow((radius * 2 ), 2) + 4;
}

long long solution(int r1, int r2) {
    long long answer = 0;
    
    int big;
    int small;
    // 정수인 점의 개수 = (반지름 * 2)^2 + 4
    if(r1 > r2)
    {
        big = GetVertexNumber(r1);
        small = GetVertexNumber(r2);
    }
    else if(r1 < r2)
    {
        small = GetVertexNumber(r1);
        big = GetVertexNumber(r2);
    }
    else
        return 0;

    answer = big - small;
    return answer;
}
```

낮은 수의 테스트 케이스들만 생각하고 문제를 풀이했더니 

간단한 테스트만 통과하고 높은 입력값의 테스트들은 모두 불통과 되었습니다.

직접 계산하기 보다는 삼각함수를 이용해 풀해야야 될 것 같습니다.
<br><Br>



```c++
```