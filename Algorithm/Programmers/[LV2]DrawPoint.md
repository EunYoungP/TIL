2023.05.20 

# __[프로그래머스 LV2] 점 찍기__

피타고라스의 정의

----

## __문제__

[점 찍기](https://school.programmers.co.kr/learn/courses/30/lessons/140107)<br><Br>




## __나의 풀이__
```c++
#include <string>
#include <vector>
#include <cmath>

using namespace std;

long long solution(int k, int d) {
    long long answer = 0;
    
    // c^2 = a^2 + b^2
    // c = sqrt(a^2 + b^2)
    // b = sqrt(c^2 - a^2)
    for(int i = 0; i <= d; i+=k)
    {
        int y = floor(sqrt(pow(d,2) - pow(i,2)));
        answer += y/k + 1;
    }
    
    return answer;
}
```
<br><Br>

<img src="https://github.com/EunYoungP/TIL/assets/80774412/ac2e0ab4-c323-4dad-8d7d-e25453d7c83a" width = 400, height = 300 title = "피타고라스의 정리"></img>


피타고라스의 정리에 따르면 직삼각형에서 

___밑변^2 + 높이^2 = 빗변^2___ 입니다.<br><Br>

위 이미지는 밑변=a, 높이=b, 빗변=c 라고 할 수 있습니다.

그리고 빗변은 문제에서 원점으로부터의 거리인 d와 같습니다.

___a^2 + b^2 = c^2___ <br><Br>

그렇다면 만약 x가 3이라면 원점으로부터의 거리가 5이하인 모든 y의 좌표를 알 수 있습니다.

___높이^2 = 빗변^2 - 밑변^2___ 과 같기 떄문입니다.

즉 빗변이 d인 높이를 구하여 

정점의 개수를 구하는 것이기때문에 내림 처리를 하면 최대의 높이를 구할 수 있습니다.<br><BR>

그리고 구한 높이는 0~구한높이 모든 범위를 포함하지만, y 또한 k를 곱한 값이기 때문에,

구한 높이 / k 를 해줍니다.

그리고 y가 0 인 경우의 수 1을 더해줍니다.

그렇다면 특정 x 좌표에서 원점으로부터의 거리가 d보다 이하인 모든 y좌표의 개수를 구할 수 있습니다.




