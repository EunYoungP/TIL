2023.05.26

# __[프로그래머스 LV2] 할인 행사__

----

## __문제__

[할인 행사](https://school.programmers.co.kr/learn/courses/30/lessons/135807)<br><Br>


## __나의 풀이__
```c++
#include <string>
#include <vector>
#include <map>
#include <iostream>

using namespace std;

int solution(vector<string> want, vector<int> number, vector<string> discount) {
    int answer = 0;
    map<string, int> m;
    bool isFind = true;
    
    //  치킨, 사과, 사과, 바나나, 쌀, 사과, 돼지고기, 바나나, 돼지고기, 쌀, 냄비, 바나나, 사과, 바나나
    for(int i = 0; i <= discount.size()-10; i++)
    {
        isFind = true;
        int count = 0;
        
         m.clear();
        while(count < 10)
        {
            m[discount[i+count]]++;
            count++;
        }
        
        for(int j = 0;j < want.size(); j++)
        {
            if(m.find(want[i])==m.end())
            {
                isFind = false;
                break;
            }
            
            int curNumber = m[want[i]];
            
            if(number[i] > curNumber)
            {
                isFind = false;
                break;
            }
        }
        if(isFind == true)
            answer++;
    }
    return answer;
}
```

위 풀이로 간단한 테스트들은 통과가 되었지만 전체 테스트에서 거의 모든 케이스가 sagmentation Fault 가 발생했습니다.

단순히 문제에 따라서 풀이했습니다.

1. discount 리스트를 0인덱스부터 순회합니다.

    1-1. 위 인덱스를 시작으로 10개씩 map에 할당하여, 물품의 종류에 따른 개수를 구합니다.

2. 사고싶은 리스트를 순회하면서 map에 모두 존재하는지 검색합니다.

3. 사고싶은 물품의 종류의 개수 또한 검사합니다.

4. 맵을 전부 비우고 반복합니다.<br><Br>

