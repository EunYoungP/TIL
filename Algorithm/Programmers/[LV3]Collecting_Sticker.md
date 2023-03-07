[2023.03.07](#나의-풀이풀이-참조)

# __[프로그래머스 LV3] 스티커 모으기__

[프로그래머스](https://programmers.co.kr/skill_checks/468819)

DP

---- 
<BR>

## 문제설명

<img src="https://user-images.githubusercontent.com/80774412/223453352-092de74b-e20a-4028-979d-01e525495b13.PNG" title="collectingSticker1"></img>

<img src="https://user-images.githubusercontent.com/80774412/223453363-4898e69e-c27b-4c24-9c85-2d746dae5050.PNG" title="collectingSticker2"></img>

<br><Br>

## 나의 풀이(풀이 참조)

```c++
#include <vector>
#include <algorithm>
using namespace std;

// DP문제
int solution(vector<int> sticker)
{
    int answer =0;
    
    int stickerSize = sticker.size();
    vector<int> dp(stickerSize);
    
    if(stickerSize == 1)
        return sticker[0];
    
    // 1. 첫 번째 스티커를 뜯은 경우
    dp[0] = sticker[0];
    dp[1] = dp[0];
    
    int temp;
    for(int i = 2; i < stickerSize -1; i++)
    {
        dp[i] = max(dp[i-1], dp[i-2] + sticker[i]);
    }
    
    dp[stickerSize - 1] = dp[stickerSize - 2];
    temp = *max_element(dp.begin(), dp.end());     
    
    // 2. 첫 번째 스티커를 뜯지 않은 경우
    dp[0] = 0;
    
    for(int i = 1; i < stickerSize; i++)
    {
        dp[i] = max(dp[i-1], dp[i-2] + sticker[i]);
    }
    
    answer = *max_element(dp.begin(), dp.end());
    answer = max(answer, temp);
    
    return answer;
}
```

위 문제는 __DP 다이나믹 프로그래밍__ 을 사용하여 풀이해야하는 문제였습니다.

최대값을 구하기 위해 주어진 스티커의 벡터의 첫 번째 인덱스에서부터 최대값을 DP 벡터에 저장하여 bottom-up 형식으로 풀이했습니다.<br><Br>


## __점화식 : dp[i] = max(dp[i-1],dp[i-2] + sticker[i]);__

i 번째까지의 최대값을 dp[i] 라고 가정할 때,

dp[i]를 구하기 위해서 두 가지 유형을 생각할 수 있습니다.

1. i 번째 스티커를 뜯는 경우

    i 번째 스티커를 뜯는 경우 i - 1 번째 스티커는 뜯지 않은 상태여야 하기 때문에 dp[i-2] 에 현재 뜯는 스티커인 sticker[i] 를 더해준 값이 최적해가 됩니다.

2. i 번째 스티커를 뜯지 않는 경우

    i 번째 스티커를 뜯지 않는 경우 아무런 값도 더해지지 않기 때문에 최적해는 dp[i-1] 가 됩니다.

이 두가지 경우를 비교하여 더 큰 값을 dp[i]의 최적해로 저장합니다.<br><Br>

또한 dp를 처음 시작하는 시점에서 두 가지 갈래길로 나뉠 수 있습니다.

피보나치 수열도 0, 1 번째 값이 주어지듯이 dp 또한 dp[0], dp[1]의 값을 부여해야하는데,

이 부분에서 첫 번째 값을 뜯을지, 뜯지 않을지로 나뉩니다.

1. 첫 번째 스티커를 뜯는 경우

    첫 번째 스티커를 뜯는 경우 우선 dp[0]은 첫 번째 스티커인 sticker[0] 이 저장됩니다.

    그리고 1 번째 스티커는 뜯을 수 없기 때문에 dp[0]값에 아무런 값도 더해지지 않고 dp[1] 또한 dp[0]의 값을 가지게 됩니다.

    또한 첫 번쨰 값을 뜯으면 마지막 값을 뜯을 수 없게 되기 떄문에 마지막 인덱스인 dp[stickerSize -1] 또한 dp[stickerSize -2] 값이 그대로 저장됩니다.

2. 첫 번째 스티커를 뜯지 않는 경우

    첫 번째 스티커를 뜯지 않으면 첫 번째 값을 아무런 값도 저장되지 않은 0 으로 저장됩니다.

    그리고 dp[1] 은 스티커의 첫번째 값인 sticker[1] 이 저장됩니다.

    마지막 인덱스도 뜯을 수 있기 떄문에 점화식을 마지막 인덱스까지 적용시켜줍니다.

이렇게 두가지 갈래길의 최적해를 각각 구해 비교해주면, 더큰 값이 답이 됩니다.