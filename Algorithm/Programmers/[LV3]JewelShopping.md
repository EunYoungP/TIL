2023.04.17

# __[프로그래머스 LV3] kakao 보석 쇼핑__

map

----

## __문제__

[보석 쇼핑](https://school.programmers.co.kr/learn/courses/30/lessons/67258#qna)<br><Br>

## __나의 참고 풀이__
```c++
#include <string>
#include <vector>
#include <map>

using namespace std;

vector<int> solution(vector<string> gems) {
    vector<int> answer(2);
    
    map<string, int> gmaps;
        
    int head = 0;
    int end = 0;
    
    int minHead = 0;
    int minEnd = 0;
    
    for(int i = 0; i < gems.size(); i++)
    {
        gmaps[gems[i]]++;
        
        // 중복되지 않은 종류의 보석이 추가되었을 경우
        if(gmaps[gems[i]] == 1)
        {
            end = i;
            minHead = head;
            minEnd = i;
        }
        // 중복된 종류의 보석이 추가되었을 경우
        else
        {
            end = i;
            for(int j = head; j < i; j++)
            {
                if(gmaps[gems[j]] > 1)
                    gmaps[gems[j]]--;
                else
                {
                    head = j;
                    
                    if(end - head < minEnd - minHead)
                    {
                        minHead = head;
                        minEnd = end;
                    }
                    break;
                }
            }
        }
    }
    answer[0] = minHead + 1;
    answer[1] = minEnd + 1;
    
    return answer;
}
```

보석의 종류를 map을 이용하여 검색을 빠르게 했습니다.

map<string, int> 에서 __string : 보석 종류, int : 해당 보석의 수__ 로 설정하여 정리했습니다.

보석 종류를 모두 가진 최소 범위의 시작점 head와 끝 인덱스 end를 설정했습니다.

주의할 점은 문제에서 주어진 answer 벡터의 크기를 초기화하지 않아 오류가 발생할 수 있고,

보석의 인덱스는 1부터 시작이므로 0부터 검색한 head, end 인덱스에 1을 더해서 답을 구해야합니다.