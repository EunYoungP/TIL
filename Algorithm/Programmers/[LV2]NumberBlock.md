2023.02.04

# __[프로그래머스 LV2] 숫자블록__


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

각 블럭의 자리에는 1000000보다 작으면서, 블럭 인덱스의 약수 중 가장 큰 값이 들어갑니다.

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
        if (num == 1){
            answer.push_back(0);
            continue;
        }
        bool found = false;
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