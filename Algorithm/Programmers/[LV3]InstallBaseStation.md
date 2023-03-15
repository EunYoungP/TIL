[2023.02.20](#나의-풀이오답)

[2023.03.15](#재풀이)

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


## __재풀이__(오답)
```c++
#include <iostream>
#include <vector>
#include <iostream>
using namespace std;

// 최소 설치 기지국 개수 구하기
int solution(int n, vector<int> stations, int w)
{
    int answer = 0;

    vector<bool> reached(stations.size());
    
    // 기본 설치 기지국 전파 닿는 아파트 검사
    for(int i = 0; i < stations.size(); i++)
    {
        int curStationIndex = stations[i] - 1;

        reached[curStationIndex] = true;
        for(int j = 1; j <= w; j++)
        {
            int plusIndex = curStationIndex + j;
            int minusIndex = curStationIndex - j;

            if(plusIndex >= n) plusIndex = n-1;
            if(minusIndex < 0) minusIndex = 0;

            reached[plusIndex] = true;
            reached[minusIndex] = true;
        }
    }
    
    for(int i = 0; i < n; i++)
    {
        if(reached[i] == false)
        {
            int index = i + w;
            while(reached[index] == true)
            {
                index--;
            }
            answer++;
            
            // 기지국 전파 전달
            reached[index] = true;
            for(int j = 1; j <= w; j++)
            {
                int plusIndex = index + j;
                int minusIndex = index - j;
                
                if(plusIndex >= n) plusIndex = n-1;
                if(minusIndex < 0) minusIndex = 0;
                
                reached[plusIndex] = true;
                reached[minusIndex] = true;
            }
        }
    }

    return answer;
}
```


__결과__

<img src="https://user-images.githubusercontent.com/80774412/225310841-b2d0facf-bb3f-48ed-8f91-8bbf6feff192.PNG"></img>

효율성 테스트에서 모두 코어 덤프 오류가 나고 정확성 테스트도 모두 통과는 되지 않았습니다...

아파트의 수인 N만큼 반복문으로 검사를 진행해서 시간 복잡도가 너무 커진 이유떄문인것 같습니다.

[첫 풀이](#수정-답안) 때 처럼 기지국의 전파가 닿지 않는 범위를 계산해서,

전파가 닿지않는 범위와 기지국 전파의 범위를 계산하여 설치할 기지국의 개수만을 구하는 방법이 시간 복잡도 면에서 유리할 것 같습니다.

위 풀이에서는 모든 아파트를 검사하며 전파가 닿는 상태인지 검사했고 또한 그 분기 속에서 어떤 자리에 설치가 가능한지 검사하는 반복문이 또 들어갔기 때문에 만일 아파트 수가 최대인 20000개라면 최소 200000번의 검사를 진행해야 합니다.<BR><bR>

----

[참고자료](https://yabmoons.tistory.com/635)
