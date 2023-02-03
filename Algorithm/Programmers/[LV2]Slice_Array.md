2023.02.02

# __[프로그래머스 LV2] 배열 자르기__

STL

---- 


<img src="https://user-images.githubusercontent.com/80774412/216351693-87310b93-4b01-460f-a934-cb404a6067c1.PNG"></img>


<img src="https://user-images.githubusercontent.com/80774412/216351544-66dffa73-64db-4b73-b8a3-85e49e0eb13f.gif"></img>


## __나의 풀이__

```c#
#include <string>
#include <vector>
#include <iostream>
using namespace std;

vector<int> solution(int n, long long left, long long right) {
    vector<int> answer;
    vector<vector<int>> temp(n,vector<int>(n));
    
    // 1. 2차원 배열에 값 채우기
    for(int i = 0; i < n; i ++)
    {
        for(int j = 0; j < i; j++)
        {
            temp[i][j] = i+1;
            temp[j][i] = i+1;
        }
        temp[i][i] = i+1;
    }
    
    // 2. 2차원배열을 1차원배열로 변환
    for(int i = 0; i < n; i++)
    {
        for(int j = 0; j < n; j++)
        {
            answer.push_back(temp[i][j]);
        }
    }
    
    answer.assign(answer.begin() + left,answer.begin() + right + 1);
    return answer;
}
```

2차원 배열을 만들기위한 벡터속벡터는 미리 초기화 해주지 않으면 'temp[n][m]'이런 형식으로 사용할 수 없습니다.

assign은 배열의 부분을 삭제하고 새로운 빈 배열에 할당해주는 함수입니다.

테스트코드는 통과하지만 최종테스트에서 반 정도되는 테스트 케이스에서 시간 초과에 막혀 코드를 수정했습니다.

## __수정 답안__

```c#
#include <string>
#include <vector>

using namespace std;

vector<int> solution(int n, long long left, long long right) {
    vector<int> answer;
    
    for(long long i = left; i < right+1; i++)
    {
        int divisor = i / n;
        int mod = i % n;
        
        answer.push_back(divisor < mod? mod + 1 : divisor + 1);
    }
    
    return answer;
}
```

2차원 배열에서 행과 열을 비교한 후 더 큰 값에 배열이 0부터인 특성을 바탕으로 1을 더해주면 그 인덱스에 해당하는 칸의 값이 됩니다.

반복문안의 반복자의 값형식을 long long 이 아닌 int로 설정하면 시간 초과 오류가 발생합니다.

left 값이 long long 범위에 있는 값일 수도 있기 때문에 주의해야합니다.


