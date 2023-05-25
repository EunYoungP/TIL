2023.05.25

# __[프로그래머스 LV2] 연속 부분 수열 합의 개수__

----

## __문제__

[연속 부분 수열 합의 개수](https://school.programmers.co.kr/learn/courses/30/lessons/131701)<br><Br>


## __나의 풀이__
```c++
#include <string>
#include <vector>
#include <iostream>
#include <set>

using namespace std;

set<int> s;

int solution(vector<int> elements) 
{
    int answer = 0;
    
    ::elements = elements;
    
    int len = 1;
    while(len<=elements.size())
    {
        for(int i=0;i<elements.size();i++)
        {
            int sum = 0;
            for(int j=i;j<i+len;j++)
            {
                if(j>=elements.size())
                     sum += elements[j-elements.size()];
                else sum += elements[j];
            }
            s.insert(sum);
        }
        len++;
    }
    
    answer = s.size();
    
    return answer;
}
```

set은 중복된 값을 가질 수 없는 특성을 사용하여 중복된 숫자의 합을 제거했습니다.



