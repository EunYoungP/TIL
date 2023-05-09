2023.05.09

# __[프로그래머스 LV2] 뒤에 있는 큰 수 찾기__


----

## __문제__

[뒤에 있는 큰 수 찾기](https://school.programmers.co.kr/learn/courses/30/lessons/154539)<br><Br>

## __나의 풀이__(일부 시간 초과)
```c++
#include <string>
#include <vector>
#include <algorithm>

using namespace std;

bool isAfterBig(int a, int b)
{
    return a < b;
}

vector<int> solution(vector<int> numbers) {
    // 각 원소의 뒷큰수 배열 구하기
    vector<int> answer;
    bool isFind = false;
    
    for(int i = 0; i < numbers.size(); i++)
    {
        isFind = false;
        int curNum = numbers[i];
        for(int j= i; j < numbers.size(); j++)
        {
            if(numbers[j] > curNum)
            {
                isFind = true;
                answer.push_back(numbers[j]);
                break;
            }
        }
        if(!isFind)
            answer.push_back(-1);
    }
    
    return answer;
}
```

테스트 정확도 케이스에서 모두 통과가 되었지만,

조건에서 numbers의 길이가 최대 1000000 까지 입력 가능하기 때문에

이중 반복문으로 구성한 코드는 

___시간복잡도O(N^2)___ == ___O(1000000^2) 가 되어 시간 초과가 일어난것 같습니다.

각 원소를 기준으로 뒷큰수를 찾아야하기 때문에 필수로 필요한 반복문을 하나로 줄여 풀이하는 방법을 찾아야 합니다.

