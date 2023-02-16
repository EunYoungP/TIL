2023.02.16

# __[프로그래머스 LV2] JadenCase 문자열 만들기__

std::string

[프로그래머스](https://school.programmers.co.kr/learn/courses/30/lessons/12951)

---- 

## __문제__

<img src="https://user-images.githubusercontent.com/80774412/219243508-f9ecdbdb-1266-4b0f-bf0b-2751ae2af249.PNG"></img>

<br><br>


## __나의 풀이__(정답)

```c++
#include <string>
#include <vector>
#include <algorithm>
#include <cctype>

using namespace std;

string solution(string s) {
    string answer = "";
    
    vector<string> words;
    string seperator = " ";
    
    int curIndex = 0;
    int findIndex = 0;
    while((findIndex = s.find(seperator, curIndex)) != std::string::npos )
    {
        int len = findIndex - curIndex;
        string str = s.substr(curIndex, len);
        words.push_back(str);
        curIndex = curIndex + len  +1;
    }
    words.push_back(s.substr(curIndex));
    
    for(int i = 0; i < words.size(); i++)
    {
        string word = words[i];
        for(int j = 0; j < word.size(); j++)
        {
            if(j == 0)
            {
                if(word[j] > '9' || word[j] < '0')
                {
                    answer += toupper(word[j]);
                }
                else
                    answer += word[j];
            }
            else
                answer += tolower(word[j]);
        }
        answer += " ";
    }
    answer = answer.substr(0, answer.size()-1);
    return answer;
}
```

문자의 대문자 소문자 변환을 이용하여 풀이하였습니다.

<br><br>


## __대문자 소문자 변환__

* __toupper, tolower__

__toupper__ 함수를 통해 대문자로 변환하고,

__tolower__ 함수를 통해 소문자로 변환할 수 있습니다.

두개의 함수는 모두 __cctpye__ 헤더파일에 포함되어있습니다.

    int toupper(int c);
    int tolower(int c);

매개변수는 int 타입이지만 문자를 입력하면 아스키 코드표에 기반한 10진수로 변환되어 들어갑니다.

65 == 'A'

아스키 코드표를 이용하여 

'a' -> 'A'

97 -> 65

즉 32를 더하거나 뺌으로써 문자를 변환합니다.

__아스키 코드표__

American Standard Code for Information Interchange

<img src="https://user-images.githubusercontent.com/80774412/219270316-104a3671-ba15-47be-8aac-84612cc222ee.pn"></img>
<br><br>

__유니 코드__

문자가 많은 국가에서는 아스키 코드의 수로는 표현이 제한적이기때문에 등장한 국제 표준 코드입니다.







