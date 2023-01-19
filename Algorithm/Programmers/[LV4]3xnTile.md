2023.01.20

# __[프로그래머스 LV4] 3 x n 타일링__

동적프로그래밍 

---- 

<img src="https://user-images.githubusercontent.com/80774412/213525557-d4245d36-70dc-4dc9-9bf1-9a90802af8a9.PNG"></img>

동적프로그래밍의 __분할정복 기법__ 으로 점화식을 알아내야합니다.

작은 문제를 풀어 재계산하지 않고 값을 저장해두는것입니다.

f(2) = 3<br>
f(4) = 11

현재 주어진 데이터는 위와같습니다.

f(3)이 존재하지 않는 이유는, 직사각형의 가로길이가 홀수인 경우는 타일을 가득채워 배치할 수 없기떄문입니다.

두가지 경우의 수로는 규칙을 알아내기 어려우니 그림으로 다음 값을 구해보겠습니다.

<img src="https://user-images.githubusercontent.com/80774412/213527485-c0b45cab-0c6e-4d0a-96ed-d6c957900781.jpg" width = 400 height=500></img>

이상이란, 타일을 겹쳐서 배치했을 경우 특수하게 나오는 경우의 수를 의미합니다. 

예를들어 f(4)에서 이상이란, 직사각형의 가로길이의 가운데 2타일이, 가로모양으로 눕혀져서 배치되는 경우 입니다.

f(n) = f(n-2) * f(2) + f(n-4) * 2 ``` + f(2) * 2 + 2

위 그림을 점화식으로 바꿨습니다.

```c++
#include <string>
#include <vector>
 
#define MODULER 1000000007
using namespace std;
 
int solution(int n) 
{
    // 직사각형의 가로길이는 짝수일때만 성립됩니다.
    if(n % 2 == 1) return 0;
    long long DP[5010] = { 0, };
    DP[0] = 1;
    DP[2] = 3;
    for(int i = 4; i <= n ; i = i + 2)
    {
        DP[i] = DP[i - 2] * 3;
        for(int j = i - 4; j >= 0 ; j = j - 2)
        {
            DP[i] = DP[i] + DP[j] * 2;
        }
        DP[i] = DP[i] % MODULER;
    }
   return DP[n];
}
```


