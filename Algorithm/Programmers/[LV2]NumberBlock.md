2023.02.04

# __[프로그래머스 LV2] 숫자블록__

divisor

---- 

## __문제__

<img src="https://user-images.githubusercontent.com/80774412/216771318-7b2d8e79-119a-4297-9dba-38466eaa2dd4.PNG"></img>

<img src="https://user-images.githubusercontent.com/80774412/216771316-d4d5b99c-7c1f-4989-8693-7d9106340685.PNG"></img>

<br><br>

## __나의 풀이(오답)__

```c++
#include <string>
#include <vector>
#include <map>
#include <algorithm>
#include <iostream>

using namespace std;

vector<int> solution(long long begin, long long end) {
    vector<int> answer;
    map<int, int> temp;
    
    for(int i = end/2; i > 0; i--)
    {
        int num = 2;
        int blockNum = i * num;
        while(blockNum <= end)
        {
            if(temp.find(blockNum) == temp.end())
            {
                temp[blockNum] = i;
            }
            num++;
            blockNum = i * num;
        }
    }
    
    for(int i = begin; i <= end; i++)
    {
        if(i == 0)
            answer.push_back(0);
        else
            answer.push_back(temp[i]);
    }
    return answer;
}
```

<img src="https://user-images.githubusercontent.com/80774412/216771980-44919943-932f-457d-bcbf-161e83688ecb.PNG"></img>

효율성 테스트를 통과하지 못해 오답처리되었습니다.

도로에 들어갈 블럭의 값이 1000000보다 작아야한다는 조건이 없었기때문에 실패가 나왔습니다.

조건을 추가해 준 후에는 시간 초과로 통과되지 않았습니다.

각 블럭의 자리에는 1000000보다 작으면서, 도로의 인덱스의 약수 중 가장 큰 값이 들어갑니다.

약수를 구하는 방법으로 문제를 다시 풀어보았습니다.<Br><br>

## __두번째 풀이__

```c++
#include <string>
#include <vector>
#include <cmath>

using namespace std;

vector<int> solution(long long begin, long long end) {
    vector<int> answer;

    for(int num = begin; num <= end; ++num){
        // 1. 인덱스 1의 도로에는 항상 0이 들어갑니다.
        if (num == 1){
            answer.push_back(0);
            continue;
        }
        bool found = false;

        // 2. 10000000보다 작으면서, 가장 큰 약수를 찾아 벡터에 넣어줍니다.
        for (int i = 2; i <= sqrt(num); ++i){
            if (num % i == 0 && num / i <= 10000000){
                answer.push_back(num / i);
                found = true;
                break;
            }
        }
        if (!found) 
            answer.push_back(1);
    }
    return answer;
}
```
자신을 제외한 도로의 번호에 들어갈 블럭의 가장큰 약수를 구하는 방법은

가장 큰 약수의 짝을 구해 나누는 방법입니다.

예를 들어 100번째 도로에 들어갈 수 있는 가장 큰 약수는 50인데,

가장 큰 약수는 50의 짝인 2를 구하면 100을 2로 나눔으로써 가장 큰 약수를 구할 수 있습니다.

작은 수부터 검색하는것이 금방 약수의 짝을 구할 가능성이 높기 때문에 작은 수부터 검색하는것이 효율적입니다.

자신을 제외해야하기 떄문에 자신의 짝은 1을 제외하고 2부터 검색합니다.

약수의 짝은 대상값의 제곱근보다 작기 때문에 squr 함수를 이용해 제곱근까지만 검색을 해줍니다.

