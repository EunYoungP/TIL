2023.02.20

# __[프로그래머스 LV2] 기지국 설치__


[프로그래머스](https://school.programmers.co.kr/learn/courses/30/lessons/12979#qna)

---- 

## __문제__

<img src="https://user-images.githubusercontent.com/80774412/220029248-c9165f5e-f698-425c-bddb-866031aff710.PNG"></img>

<img src="https://user-images.githubusercontent.com/80774412/220029267-90e3cd13-89f6-4d0f-9394-0af384bbdbcc.PNG"></img>

<br><br>


## __나의 풀이__(오답)

```c++
#include <iostream>
#include <vector>
#include <algorithm>
using namespace std;

int solution(int n, vector<int> stations, int w)
{
int answer = 0;

    int checkNum = 0;
    
    int curIndex = 0;
    int startIndex = 0;
    int endIndex = 0;
    
    for(int i = 0; i < stations.size(); i++)
    {
        if(curIndex >= stations[i])
        {
            endIndex = stations[i] + w - 1;
            curIndex = endIndex + 1;
            continue;
        }
        // 전파 영향 시작 지점
        startIndex = stations[i] - w - 1;
        // 전파 영향 끝나는 지점
        endIndex = stations[i] + w - 1;
        if(endIndex > n + 1)
          endIndex = n + 1;
        checkNum = startIndex - curIndex;
        curIndex = endIndex + 1;
        
        if((checkNum % (w * 2 + 1)) == 0)
        {
            answer += checkNum / (w * 2 + 1);
        }
        else
        {
            answer += checkNum / (w * 2 + 1) + 1;
        }
    }
    
    if(curIndex <= n)
    {
        checkNum = n - curIndex - 1;
        if((checkNum % (w * 2 + 1)) == 0)
        {
            answer += checkNum / (w * 2 + 1);
        }
        else
        {
            answer += checkNum / (w * 2 + 1) + 1;
        }
    }
    return answer;
```

간단한 테스트는 통과가 되었지만 모든 테스트 케이스가 통과되지 않아 실패했습니다.

구글링을 통해 도움을 얻어서 재풀이 했습니다.

<br><Br>

## __수정 답안__

```c++
#include <iostream>
#include <vector>
#include <algorithm>
using namespace std;

int GetBaseStationNum(int start, int end, int w)
{
    int electricRange = end - start + 1;
    int wRange = w * 2 + 1;
    
    if(electricRange <= 0)return 0;
    
    if((electricRange % wRange) == 0)
    {
        return electricRange / wRange;
    }
    else
    {
        return electricRange / wRange + 1;
    }
}

int solution(int n, vector<int> stations, int w)
{
    int answer = 0;
    
    for(int i = 0; i < stations.size();i++)
    {
        if(i == 0)
        {
            answer += GetBaseStationNum(1, stations[i] - w - 1, w);
        }
        else
        {
            answer += GetBaseStationNum(stations[i-1] + w + 1, stations[i] - w - 1, w);
        }
    }
    
    answer += GetBaseStationNum(stations[stations.size() - 1] + w + 1, n , w);
    return answer;
}
```

오답 풀이와 접근 방식은 동일했으나 코드를 함수로 정리하지 않고 하나의 함수안에서 풀이하다보니 오류가 있었던것 같습니다.

수정 답안에서는 기지국 설치 개수를 계산해주는 기능을 함수로 따로 구현하여 코드를 정리했습니다.

이를 통해 아파트의 넘버가 0 부터 시작하지 않아 생기는 인덱스 혼란을 줄일 수 있었습니다.

기지국의 영향을 받지 않는 연속적인 아파트들의 __시작넘버__ 와 __끝넘버__, 그리고 __전파 범위__ 를 매개변수로 받아 계산합니다.

__전파가 닿는 아파트의 범위__ = w * 2 + 1

__연속된 아파트들의 개수__ = 끝넘버 - 시작넘버 + 1

__설치해야할 기지국의 개수__ 는 조건에 따라 두가지로 나뉩니다.

1. 연속된 아파트의 개수가 전파 범위와 맞아떨어질 경우

    설치해야할 기지국의 개수 = 연속된 아파트의 개수 / 전파가 닿는 아파트의 범위

2. 연속된 아파트의 개수가 전파 범위와 맞아떨어지지 않고 나머지가 생기는 경우

    설치해야할 기지국의 개수 = ( 연속된 아파트의 개수 / 전파가 닿는 아파트의 범위 ) + 1

<br><br>

----

[참고자료](https://yabmoons.tistory.com/635)
