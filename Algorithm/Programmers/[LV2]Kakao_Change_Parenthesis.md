
# 괄호 변환

## __문제__

<img src="https://user-images.githubusercontent.com/80774412/215276113-4ad8bccd-e76f-43de-b9f2-47af7d5cb760.PNG"></img>

<br>

__오답__
```c++
#include <string>
#include <vector>

using namespace std;

bool IsCorrectStr(string str)
{
    char strLeft = '(';
    char strRight = ')';
    string u;
    string v;
    int judge = 0;
    // 3. 올바른 문자열 판별
    for(int i = 0; i < str.size(); i++)
    {
        if(u[i] == strLeft)judge++;
        if(u[i] == strRight)judge--;
        
        if(judge < 0)
        {
            return false;
        }
    }
    return true;
}

string solution(string p) {
    string answer = "";
    
    char strLeft = '(';
    char strRight = ')';
    
    string u;
    string v;
    
    bool isCorrect = true;
    
    // 1. 빈 문자열 반환
    if(p == "") return "";

    int left = 0;
    int right = 0;
    // 2. u, v 나누기
    for(int i = 0; i < p.size(); i++)
    {
        if(p[i] == strLeft)left++;
        else if(p[i] == strRight)right++;

        if(left == right)
        {
            u = p.substr(0, i+1);
            v = p.substr(i+1, p.size());
            break;
        }
    }
    
    // 3. 올바른 문자열인지 판별
    if(IsCorrectStr(u))
    {
        // 3-1. 올바른 문자열일 경우
        v = solution(v);
        answer = u + v;
        return answer;
    }
    else
    {
        // 4. 올바른 문자열이 아닐경우
        string temp;
        // 4-1. 빈 문자열에 ( 붙이기
        temp += '(';
        // 4-2. 문자열 v 1단계부터 재귀
        temp += solution(v);
        // 4-3. ) 붙이기
        temp += ')';

        // 4-4. u의 첫번째, 마지막 문자 제거하고 나머지 문자열 괄호 방향 뒤집어서 붙이기
        u.erase(u.begin());
        u.erase(u.end()-1);
        for(int i = 0; i < u.size(); i++)
        {
            if(u[i] == strLeft)
                u[i] = strRight;
            else
                u[i] = strLeft;
        }
        temp += u;
        answer += temp;
    }
    // 4-5. 생성된 문자열 반환
    return answer;
}
```
1, 2, 3 단계를 모두 함수로 추출해서 재귀함수로 만들었습니다.

하지만 그렇게되면 u, v값이 저장되지 않고 함수의 반환값을 오류여부를 알려주는 bool값으로 설정할지,

u의값이 옳은 문자열이 아닐때까지 판별한 후의 u + v의 문자열 string값을 반환할지 판단하기 까다로웠고

수정하는 과정에서 시간이 끝났습니다.

__수정 답안__
```c++
#include <string>
#include <vector>

using namespace std;

bool IsCorrectStr(string str)
{
    char strLeft = '(';
    char strRight = ')';
    
    int judge = 0;
    // 3. 올바른 문자열 판별
    for(int i = 0; i < str.size(); i++)
    {
        if(str[i] == strLeft)judge++;
        if(str[i] == strRight)judge--;
        
        if(judge < 0)
            return false;
    }
    return true;
}

string solution(string p) {
    string answer = "";
    
    char strLeft = '(';
    char strRight = ')';
    
    string u;
    string v;
    
    // 1. 빈 문자열 반환
    if(p == "") return "";

    int left = 0;
    int right = 0;

    // 2. u, v 나누기
    for(int i = 0; i < p.size(); i++)
    {
        if(p[i] == strLeft)left++;
        else if(p[i] == strRight)right++;

        if(left == right)
        {
            u = p.substr(0, i+1);
            v = p.substr(i+1);
            break;
        }
    }
    
    // 3. u가 올바른 문자열인지 판별
    if(IsCorrectStr(u))
    {
        // 3-1. 올바른 문자열일 경우
        v = solution(v);
        answer = u + v;
        return answer;
    }
    else
    {
        // 4. 올바른 문자열이 아닐경우
        string temp;
        // 4-1. 빈 문자열에 ( 붙이기
        temp += '(';
        // 4-2. 문자열 v 1단계부터 재귀
        temp += solution(v);
        // 4-3. ) 붙이기
        temp += ')';

        // 4-4. u의 첫번째, 마지막 문자 제거하고 나머지 문자열 괄호 방향 뒤집어서 붙이기
        u.erase(u.begin());
        u.erase(u.end()-1);
        for(int i = 0; i < u.size(); i++)
        {
            if(u[i] == strLeft)
                u[i] = strRight;
            else
                u[i] = strLeft;
        }
        temp += u;
        answer += temp;
    }
    // 4-5. 생성된 문자열 반환
    return answer;
}
```
문자열이 올바른 문자열인지 판별하는 부분만 함수로 추출하여 구현했습니다.

1, 2, 3 단계는 solution 함수 자체를 재귀로 호출했습니다.

재귀함수 호출이 익숙하지는 않지만 표기된 절차를 그대로 코드로 만들면 통과되는 문제였습니다.

재귀의 이해도를 테스트하려는 문제였던것 같습니다.



