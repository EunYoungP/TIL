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

직접 계산하기 보다는 피타고라스 정리를 이용한 풀이를 참고하였습니다.

<br><Br>


## __참고 풀이__

```c++
#include <string>
#include <vector>
#include <cmath>

using namespace std;

long long solution(int r1, int r2) {
    long long answer = 0;
    
    // c^2 = a^2 + b^2
    long powr1 = pow(r1, 2);
    long powr2 = pow(r2, 2);
    int side = 0;
    
    for(int i = 0; i < r2; i++)
    {
        long big = sqrt(powr2 - pow(i, 2));
        long small = sqrt(powr1 - pow(i, 2));;
        
        if(sqrt((powr1 - pow(i, 2))) % 1 == 0)
            side++;
        
        // 총 좌표평면의 사분면들의 정점개수를 구합니다.
        answer += (big - small) * 4;
    }
    
    // 각 분면에 x=0, y=0 부분의 정점들을 side에 저장했습니다.
    // 4분면 만큼 곱해주고 겹치는 정점부분을 제거 해줍니다.
    answer += side * 4 - 4;
    return answer;
}
```

우선 피타고라스 정리린 직삼각형에서 빗변 길이의 제곱은 다른 두변의 길이의 제곱의 합과 같다는 정리입니다.

c^2 = a^2 + b^2

현재 주어진 값은 반지름의 길이입니다. 이 반지름의 길이를 빗변의 길이로 설정합니다.

그리고 반지름 길이만큼 x좌표를 순회하면서(e.g. 0, 1, 2 ~) y길이를 알 수 있습니다.

구해진 y 길이보다 작은 정점의 개수를 각 x좌표마다 구해 더해주면 하나의 사분면에 존재하는 정점의 개수를 알 수 있습니다.<br><Br>

<img src="https://user-images.githubusercontent.com/80774412/234095519-4669e4a0-7b21-46f8-b526-a24f83a88f16.PNG" width =450></img>

위 그림에서 초록색의 직각 삼각형을 예로 들면,

x는 3이고 r은 반지름의 길이로 주어졌습니다.

피타고라스 정리에 대입 해보면

r^2 = x^2 + y^2

y^2 = r^2 - x^2

y = sqrt(r^2 - x^2)

이렇게 구해진 y길이에서 정점만을 구하면 x가 3인 좌표에서 가능한 정점의 개수를 알 수 있습니다.

위 과정을 반지름 만큼의 x좌표만큼 반복하면 1사분면의 정점의 개수를 알 수 있습니다.

