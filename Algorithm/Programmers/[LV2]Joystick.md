2023.02.17

[2023.02.19 재풀이](#재풀이)

# __[프로그래머스 LV2] 조이스틱__

그리디

[프로그래머스](https://school.programmers.co.kr/learn/courses/30/lessons/42860?language=cpp)

---- 

## __문제__

<img src="https://user-images.githubusercontent.com/80774412/219649319-64b578fb-4c87-4567-a71d-be228280a925.PNG"></img>

<br><br>


## __나의 풀이__(수정 답안)

```c++
#include <string>
#include <vector>
#include <algorithm>

using namespace std;

// 각 인덱스에서 알파벳에 도달하기 위한 최소 비용을 구하는 함수입니다.
int GetMinimun(char c)
{
    int asc = c - 'A';
    int desc = 91 - c;
    
    return min(asc, desc);
}

int solution(string name) {
    int answer = 0;
    int nameSize = name.size();
    
    int minimunRoot = nameSize - 1;
    for(int i = 0; i < nameSize; i++)
    {
        if(name[i] != 'A')answer += GetMinimun(name[i]);
        
        int nextIndex = i + 1;
        while(name[nextIndex] == 'A' && nextIndex < nameSize)
        {
            nextIndex++;
        }
        
        // 원점 -> i -> 원점 -> nextIndex
        int a = i;
        // 원점 -> nextIndex -> 원점 -> i
        int b = nameSize - nextIndex;
        // 조이스틱 좌우 움직임 최소값
        minimunRoot = min(minimunRoot, a + b + min(a, b));
    }
    answer += minimunRoot;
    return answer;
}
```
<br><br>



# __재풀이__

```c++
#include <string>
#include <vector>

using namespace std;

int solution(string name) {
    int answer = 0;
    int nameSize = name.size();
    int minimum = nameSize-1;
    
    for(int i = 0; i < nameSize; i++)
    {
        if(name[i] != 'A')
        {
            if(name[i] - 'A' < 14)
            {
                answer += name[i] - 'A';
            }
            else
            {
                answer += 'Z' - name[i] + 1;
            }
        }
        
        int nextIndex = i + 1;
        while(name[nextIndex] == 'A' && nextIndex < nameSize)
        {
            nextIndex += 1;
        }
        
        // min(2a+b,2b+a) = a+b+min(a+b)
        // a = i
        // b = nameSize - ind;
        minimum = min(minimum,i + nameSize - nextIndex + min(i, nameSize - nextIndex));
    }
    answer += minimum;
    return answer;
}
```
minimum 변수는 가장 최단으로 'A' 가 아닌 다른 문자까지의 이동하는 거리입니다.

그러므로 초기화 값은 모든 값을 거쳐 이동하는 nameSize -1 이 됩니다.


<br><br>

-----
[참고 자료](https://4z7l.github.io/2021/03/12/algorithms-prg-42860.html)