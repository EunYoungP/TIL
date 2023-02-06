2023.02.06

# __[프로그래머스 LV2] 가장 큰 수 만들기__


---- 

## __문제__

<img src = "https://user-images.githubusercontent.com/80774412/217002407-57c5c57a-e018-4da3-bd3b-faf1eba35838.PNG"></img>

<br><Br>

## __나의 풀이__

```c++
#include <string>
#include <vector>

using namespace std;

struct MAX
{
    char max;
    int index;
    
    MAX(char _max, int _index){max = _max; index = _index;}
};

string solution(string number, int k) {
    string answer = "";
    int numberSize = number.size();
    int startIndex = -1;
    
    for(int i = 0; i < numberSize - k; i++)
    {
        MAX curMax = MAX(number[startIndex+1], startIndex+1);
        for(int j = startIndex+1; j <= numberSize -(numberSize-k)+ i; j++)
        {
             if(curMax.max < number[j])
             {
                 curMax = MAX(number[j], j);
             }
        }
        startIndex = curMax.index;
        answer += curMax.max;
    }
    
    return answer;
}
```
가장 큰 수와 그 인덱스를 담는 구조체를 만들어서 정보를 담게 했습니다. 생성자로 구조체 선언시에 초기화 해줄 수 있게 했습니다.

우선 구해질 문자열의 길이는 number 문자열의 길이에서 제외할 문자의 길이인 k 만큼을 뺀 값이므로, 구할 문자열의 길이만큼 반복문을 실행시켜줍니다.

그리고 구해질 문자열에서 최대값을 검색하여 반환받으면 되는데,

이 단계에서 검색 범위는 처음에는 0 인덱스부터 검색을 하고 처음 구해진 최대값을 다음 반복문의 시작 인덱스로 정합니다.

그리고 만약 4 길이만큼의 문자열을 구해야한다면, 전체 문자열의 길이에서 최소 4를 뺀만큼의 인덱스의 수가 첫 글자로 올 수 있기 때문에 반복문을 통해 구현했습니다.

