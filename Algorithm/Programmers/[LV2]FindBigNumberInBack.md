2023.05.10

# __[프로그래머스 LV2] 뒤에 있는 큰 수 찾기__

stack

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

각 원소를 기준으로 뒷큰수를 찾아야하기 때문에 필수로 필요한 반복문을 하나로 줄여 풀이하는 방법을 찾아야 합니다.<bR><bR>


## __수정 코드__
```C++
#include <string>
#include <vector>
#include <stack>

using namespace std;

vector<int> solution(vector<int> numbers) {
    vector<int> answer(numbers.size());
    stack<int> stk;
    
    for(int i=numbers.size()-1;i>=0;i--)
    {
        while(1)
        {
            if(stk.empty())
            {
                answer[i] = -1;
                break;
            }
            if(stk.top()>numbers[i])
            {
                answer[i] = stk.top();
                break;
            }
            stk.pop();
        }
        stk.push(numbers[i]);
    }
    return answer;
}
```

스택을 이용하여 2중 포문을 줄여 시간복잡도를 줄일 수 있었습니다.

각 for 반복마다 스택에 값을 채우고,

while문에서 조건 검사를 해 스택에 존재하는 요소들만 검사하고 부합하지 않으면 pop()해주어 

검사해야할 요소들을 줄였습니다.

이중 for문을 사용하면 시간복잡도가 O(nlogn) 이고,

스택을 사용하면 하나의 for문에 while문에서 n만큼의 연산을 필요로 하기때문에 O(2n)의 시간복잡도를 가집니다.<br><Br>

입력의 최대수가 1000000 이기 떄문에 시간복잡도에 대입해보면

___O(nlogn) = 1000000 * 5___
___O(2n) = 1000000 * 2___



