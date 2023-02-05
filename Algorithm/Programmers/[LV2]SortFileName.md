2023.02.05

# __[프로그래머스 LV2] 파일명 정렬__



---- 

## __문제__

<img src="https://user-images.githubusercontent.com/80774412/216820207-62cd71b8-9a78-4b67-83cc-00f32f1f72ab.PNG"></img>

<img src="https://user-images.githubusercontent.com/80774412/216820210-bd248bdd-cb9d-4469-b79e-75bda7d1ad8c.PNG"></img>

<img src="https://user-images.githubusercontent.com/80774412/216820212-27c6e116-56af-498e-9eb1-7085cee4d355.PNG"></img>


## __나의 풀이__(오답)

```c++
#include <string>
#include <vector>
#include <algorithm>

using namespace std;

bool compare(vector<string> a, vector<string> b)
{
    if(a[0] == b[0])
    {
        if(a[1] == b[1])
        {
            return a[2] < b[2];
        }
        return a[1] < b[1];
    }
    else
        return a[0] < b[0];
}

vector<string> solution(vector<string> files) {
    vector<string> answer;
    
    int filesCount = files.size();
    vector<vector<string>> seperator(filesCount);
    
    bool isNum;
    string curStr;
    int startNum;
    int startTail;
    
    for(int i = 0; i < files.size(); i++)
    {
        isNum = false;
        curStr = files[i];
        
        for(int j = 0; j < curStr.size(); j++)
        {
            if(!isNum)
                if(curStr[j] >= 48 && curStr[j] <= 57)
                {
                    isNum = true;
                    startNum = j;
                }
                    
            if(isNum)
                if(!(curStr[j] >= 48 && curStr[j] <= 57))
                {
                    startTail = j;
                    break;
                }
        }
        seperator[i].push_back(curStr.substr(0, startNum));
        seperator[i].push_back(curStr.substr(startNum, startTail-startNum+1));
        seperator[i].push_back(curStr.substr(startTail+1));
    }
    
    stable_sort(seperator.begin(), seperator.end(), compare);
    
    for(int i = 0; i < seperator.size(); i++)
    {
        string sortVector = seperator[i][0] +seperator[i][1] + seperator[i][2];
        answer.push_back(sortVector);
    }
    return answer;
}
```

일단 문제를 그대로 코드로 구현해보려고 시도했습니다.

반복문을 이용해서 유니코드를 이용하여 파일을 '문자(head), 숫자(number), 나머지(tail)' 세 부분으로 나누고,

모든 부분의 정렬 순위가 같다면 원래 입력받은 값의 순서를 유지하기 위해서 stable_sort를 사용했습니다.

compare 함수를 이용해서 정렬조건을 지정하려고 했으나

시간 초과로 구현에 실패했습니다.

<br><Br>

compare 함수에서 파일명들을 정렬하려면 비교대상들을 같은 유형으로 만들어야합니다.

세 부분으로 나눈 파일명 중 haed 부분은 대/소문자 구분이 없으므로 대문자나 소문자로 통일하여 변환해줍니다.

number 부분은 '012' '12' 같은 경우 비교 가능하게 하기 위해 12로 통일해줍니다.

tail 부분 또한 문자열을 head와 같은 형식으로 통일해줍니다.

```c++
#include <string>
#include <vector>
#include <algorithm>
#include <iostream>

using namespace std;

bool compare(vector<string> a, vector<string> b)
{
    if(a[0] == b[0])
    {
        if(stoi(a[1]) == stoi(b[1]))
        {
            return a[2] < b[2];
        }
        else
            return stoi(a[1]) < stoi(b[1]);
    }
    else
        return a[0] < b[0];
}

vector<string> solution(vector<string> files) {
    vector<string> answer;
    
    int filesCount = files.size();
    vector<vector<string>> seperator(filesCount);
    
    bool isNum;
    int startNum;
    int startTail;
    
    for(int i = 0; i < files.size(); i++)
    {
        // 1. 소문자나 대문자로 형변환
        string curStr;
        for(int j = 0; j < files[i].size(); j++)
        {
            curStr += tolower(files[i][j]);
        }
            
        isNum = false;
        for(int j = 0; j < curStr.size(); j++)
        {
            if(!isNum)
                if(curStr[j] >= 48 && curStr[j] <= 57)
                {
                    isNum = true;
                    startNum = j;
                }
                    
            if(isNum)
                if(!(curStr[j] >= 48 && curStr[j] <= 57))
                {
                    startTail = j;
                    break;
                }
        }
        string head = curStr.substr(0, startNum);
        seperator[i].push_back(head);
        
        string number = curStr.substr(startNum, startTail-startNum);
        seperator[i].push_back(number);
        
        string tail = curStr.substr(startTail);
        seperator[i].push_back(tail);
    }
    
    stable_sort(seperator.begin(), seperator.end(), compare);
    
    for(int i = 0; i < seperator.size(); i++)
    {
        string sortVector = seperator[i][0] +seperator[i][1] + seperator[i][2];
        answer.push_back(sortVector);
    }
    return answer;
}
```

통일한 형식을 비교시에만 유지하고 본래 값을 찾을 수 있도록 인덱스 값을 구조체로 만들어 가지고 있게 해줘야 할 것 같습니다.
