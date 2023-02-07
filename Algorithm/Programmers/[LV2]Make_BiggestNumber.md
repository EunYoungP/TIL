2023.02.06

# __[프로그래머스 LV2] 가장 큰 수 만들기__


---- 

## __문제__

<img src = "https://user-images.githubusercontent.com/80774412/217002407-57c5c57a-e018-4da3-bd3b-faf1eba35838.PNG"></img>

<br><Br>

## __나의 풀이__(힌트 정답)

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

이 문제에서는 정답 문자열의 순서가 시존 문자열의 순서와 다르면 안된다는점을 고려해야합니다.

만약 4 길이만큼의 문자열을 구해야한다면, 전체 문자열의 길이에서 최소 4를 뺀만큼의 인덱스의 수가 첫 문자로 와야지만

기존 순서를 위배하지 않고 나머지 문자열이 채워질 수 있는 인덱스만큼 남아있기 때문에

반복문을 순회하면서 1을 의미하는 i를 더하면서 검색할 수 있는 인덱스를 늘려갔습니다.

중요한 점은 첫 번째 문자를 구하고 그 다음 반복문 부터 curMax를 MAX('0',0) 으로 설정하면

만약 검색할 문자열들이 모두 '0'인 경우에는 인덱스가 반영되지 않아 오류가 나기때문에 

반드시 검색해야할 인덱스와 문자를 대입해주고 반복문을 시작해야합니다.


