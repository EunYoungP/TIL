
2023.01

# __[프로그래머스 LV2] 수식 최대화__

완전 탐색

---- 

## __문제설명__

<img src="https://user-images.githubusercontent.com/80774412/213696657-80cb6ccd-2792-4d12-8023-dd2586dca460.PNG"></img>

<img src="https://user-images.githubusercontent.com/80774412/213696668-ddbcd418-943f-4d1f-b150-c3f2fe9a95fd.PNG"></img>

<img src="https://user-images.githubusercontent.com/80774412/213696673-474df28e-e1b5-4cd0-8c75-e163a8cb6fc0.PNG"></img>

__완전 탐색__ 이란, 가능한 경우의 수를 모두 조사하는 방법입니다.

__장점__ : 무조건 답을 찾을 수 있습니다.

__단점__ : 답을 찾는데 시간이 오래걸립니다.

=> 탐색할 요소가 적을 경우 적합합니다.

위 문제에서는 주어진 연산자 '*', '-', '+' 의 순서에 따른 경우의 수를 완전 탐색합니다.

'*', '-', '+'
'*', '+', '-'
'-', '*', '+'
'-', '+', '*'
'+', '-', '*'
'+', '*', '-'

3! = 3 x 2 = 6

즉, 6가지 경우에 계산되는 모든 경우를 탐색합니다.


__풀이__
```c++
#include <string>
#include <vector>
#include <algorithm>

using namespace std;

// 연산자에 따른 피연산자들의 계산값을 반환합니다.
long long calculator(long long a, long long b, char op)
{
    if( op == '*')
        return a * b;
    else if(op == '+')
        return a + b;
    else if(op == '-')
        return a - b;
}

long long solution(string expression) {
    long long answer = 0;
    vector<long long> operands;
    vector<char> operators;
    
    string temp = "";
    for(int i = 0; i < expression.size(); i++)
    {
        if(expression[i] >= '0' && expression[i] <= '9')
            temp += expression[i];
        else
        {
            operators.push_back(expression[i]);
            operands.push_back(stoll(temp));
            temp = "";
        }
    }
    operands.push_back(stoll(temp));
    
    vector<int> perm = {0,1,2};
    string op= "+-*";
    
    do
    {
        // 여기서 새로운 벡터를 선언하여 값을 복사하는 이유는,
        // 한번 do 구문이 실행되고 while 조건에 따라 다시 반복문이 시작될 때
        // 값들이 다시 원래 피연산자, 연산자가 채워진 상태여야하기 때문입니다.
        vector<long long> temp_operands = operands;
        vector<char> temp_operators = operators;
       for(int i = 0; i < perm.size(); i++)     
       {
           for(int j = 0; j < temp_operators.size();)
           {
               if(temp_operators[j] == op[perm[i]])
               {
                   long long resul = calculator(temp_operands[j], temp_operands[j+1], temp_operators[j]);
                   
                   temp_operands.erase(temp_operands.begin() + j);
                   temp_operands.erase(temp_operands.begin() + j);
                   temp_operands.insert(temp_operands.begin() + j, resul);
                   
                   temp_operators.erase(temp_operators.begin() + j);
               }
               else
                   j++;
           }
        }
        answer = max(answer, abs(temp_operands[0]));
        // next_permutation 은 순열의 오름차순 다음값을 리턴해줍니다.
    }while(next_permutation(perm.begin(), perm.end()));
    
    return answer;
}
```

처음에는 do문 안에서 피연산자와, 연산자를 새로운 벡터로 복사하지 않아,

두번째 do문이 실행될때 이미 연산자는 모두 삭제되어 없고

피연산자는 첫번째 구문에서 계산된 반환값만 존재하기 떄문에 오류를 겪었습니다.

연산자 벡터 사이즈 출력 디버깅을 통해 오류를 알아내어 수정했습니다.