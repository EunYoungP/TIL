2023.06.06

# __[프로그래머스 LV2] 모음사전__

----

## __문제__

[모음사전](https://school.programmers.co.kr/learn/courses/30/lessons/84512)<br><Br>


## __나의 풀이__
```c++
#include <string>
#include <vector>
#include <map>

using namespace std;

// 모든 경우의 수를 구해서 인덱스를 구한다.
// 단어가 입력되면 계산해서 인덱스를 구한다.

int solution(string word) {
    // 사전에서의 순서
    int answer = 0;
    
    // 모음의 종류
    char dic[5] = {'A','E','I','O','U'};
    // 각 자리의 모음이 한 번 바뀌기 위해 필요한 인덱스
    int diff[5] = {781, 156, 31, 6, 1};
    
    // <모음, 모음순서 인덱스>
    map<char, int> m;
    for(int i = 0; i < 5; i++)
    {
        m[dic[i]] = i;
    }

    for(int i = word.size()-1; i >= 0; i--)
    {
        word[i] == 'A'? answer += 1 :answer += m[word[i]] * diff[i] + 1;
    }
    
    return answer;
}
```
## 예시

    A        1
    AA       2
    AAA      3
    AAAA     4
    AAAAA    5
    AAAAE    6
    AAAAI    7
    AAAAO    8
    AAAAU    9
    AAAE     10
    AAAEA    11
    AAAI     16

 5까지는 단순한 패턴이므로 이해가 쉽습니다.

 하지만 AAAE와 AAAEA는 다른 순번이라는것을 알아야합니다.<BR><bR>

 AAAAA는 5, AAAAE는 6 인걸로 보아 5번째 모음이 다음 모음으로 넘어갈때 1씩 증가한다는 것을 알 수 있습니다.

 그렇다면 AAAA에서 AAAE 로 넘어갈 경우에는 증가되는 정도를 생각해봅니다.

 네 번째 순서의 모음이 하나 바뀌기 위해서는 다섯 번째 모음이 모드 한번씩 바뀌어야 합니다. 즉 모든 모음의 숫자만큼 더해집니다.

AAAA    4
AAAAA   5
AAAAE   6
AAAAI   7
AAAAO   8
AAAAU   9
AAAE    10

 모음의 종류가 5가지이므로 5가 증가됩니다. 그리고 다섯 번째 자리의 모음이 사라지고 네 번째 자리의 모음이 증가되는 순서가 추가로 증가됩니다. +1

 즉, 이전 자리가 바뀌기 위한 경우의 수 * 모음의 종류 + 1<BR><br>

 그렇다면 세 번째 자리의 모음이 다음 인덱스의 모음으로 바뀌기 위해서는

 (네 번째 자리의 모음이 한번 바뀌기 위한 경우의 수 * 5)  + 1

= (다섯 번째 자리의 모음이 한번 바뀌기 위한 경우의 수* 5 + 1) * 5 + 1

= (1 * 5 + 1) * 5 + 1 = 6 * 5 + 1 = 31<BR><br>

세 번째 자리의 모음이 한번 바뀌기 위해서는 31만큼의 인덱스가 추가됩니다.
