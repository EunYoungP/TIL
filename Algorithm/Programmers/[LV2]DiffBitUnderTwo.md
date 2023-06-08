2023.06.08

# __[프로그래머스 LV2] 2개 이하로 다른 비트__

----

## __문제__

[2개 이하로 다른 비트](https://school.programmers.co.kr/learn/courses/30/lessons/77885#qna)<br><Br>


## __나의 풀이__
```c++
#include <string>
#include <vector>
#include <cmath>
#include <iostream>

using namespace std;

// 숫자를 비트로 변경
string GetBit(long long num)
{
    string temp;
    while(num / 2 != 0)
    {
        temp += to_string(num % 2);
        num /= 2;
    }
    temp += to_string(num % 2);
    return temp;
}

vector<long long> solution(vector<long long> numbers) {
    // x와 비트가 2개이하 다른 수들 중 최소값
    vector<long long> answer;
    
    for(int i = 0; i < numbers.size(); i++)
    {
        // 짝수
        if(numbers[i] % 2 == 0)
            answer.push_back(numbers[i]+1);
        // 홀수
        else
        {
            // 첫번째 0의 인덱스 구하기
            string bit = GetBit(numbers[i]);
            int index = 0;
            while(bit[index] != '0' && index < bit.size())
            {
                index++;
            }

            // 첫 번째 0 이전의 비트가 모두 1일 경우
            if(index == bit.size())
                bit += '1';
            else
                bit[index] = '1';
            
            // 첫 번째 0 이전의 비트를 1로 변경
            for(long long j = index-1; j > index-2; j--)
            {
                bit[j] = '0';
            }
            
            // 이진수를 십진수로 변경
            long long temp = 0;
            for(long long j = 0; j < bit.size(); j++)
            {
                if(bit[j] == '1')
                {
                    temp += pow(2,j);
                }
            }
            answer.push_back(temp);
        }
    }
    return answer;
}
```

numbers의 길이는 int 범위안에 들어가지만,

numbers의 각 요소들은 long long 임을 주의해야합니다.

답 또한 numbers의 각 요소보다 큰 값들을 더하는 것이기 때문에 long long 자료형으로 반환했습니다.