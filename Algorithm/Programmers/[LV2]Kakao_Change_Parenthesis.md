
# 괄호 변환

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
        
    if(IsCorrectStr(u))
    {
        v = solution(v);
        answer = u + v;
        return answer;
    }
    else
    {
        // 4. 올바른 문자열이 아닐경우
        string temp;
        temp += '(';
        temp += solution(v);
        temp += ')';

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
    return answer;
}
```