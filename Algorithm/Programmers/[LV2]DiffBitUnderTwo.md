2023.06.08

# __[프로그래머스 LV2] 2개 이하로 다른 비트__

----

## __문제__

[2개 이하로 다른 비트](https://school.programmers.co.kr/learn/courses/30/lessons/77885#qna)<br><Br>


## __나의 풀이__
```c++
#include <string>
#include <vector>

using namespace std;

string GetBit(int num)
{
    string temp;
    while(num % 2 != 0)
    {
        temp += num % 2;
        num /= 2;
    }
    temp += num % 2;
    return temp;
}

vector<long long> solution(vector<long long> numbers) {
    // x와 비트가 2개이하 다른 수들 중 최소값
    vector<long long> answer;
    
    
    for(int i = 0; i < numbers.size(); i++)
    {
        if(numbers[i] % 2 == 0)
            answer.push_back(numbers[i]+1);
        else
        {
            string bit = GetBit(numbers[i]);
            int index = 0;
            while(bit[index] != 0 && index < bit.size())
            {
                index++;
            }
            bit[index+1] = 1;

        }
    }
    return answer;
}