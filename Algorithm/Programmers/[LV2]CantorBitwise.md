2023.05.18

# __[프로그래머스 LV2] 유사 칸토어 비트열__

분할 정복

----

## __문제__

[유사 칸토어 비트열](https://school.programmers.co.kr/learn/courses/30/lessons/148652#qna)<br><Br>

## __나의 풀이__
```c++
#include <string>
#include <vector>
#include <iostream>

using namespace std;

string ConvertCantor(string s)
{
    string temp;
    for(int i = 0; i < s.size(); i++)
    {
        char curNum = s[i];
        if(curNum == '1')
        {
            temp += "11011";
        }
        else if(curNum == '0')
        {
            temp += "00000";
        }
    }
    return temp;
}

int solution(int n, long long l, long long r) {
    // 1 -> 11011 치환
    // 0 -> 00000 치환
    // l <= n <= r, 1의 개수
    int answer = 0;
    string DP[20];
    
    // 0번째 유사 칸토어 비트열 "1"
    // 1번째 유사 칸토어 비트열 == 11011
    // 2번째 유사 칸토어 비트열 == 11011 11011 00000 11011 11011
    DP[0] = '1';
    for(int i = 1 ; i <= n; i++)
    {
        DP[i] = ConvertCantor(DP[i-1]);
    }
    
    // 주어진 범위 내의 1의 개수 구하기
    for(int i = l-1; i < r; i++)
    {
        if(DP[n][i] == '1')
            answer++;
    }
        
    return answer;
}
```

## 규칙1
DP배열의 요소를 구하는 과정에서 

모든 요소를 순회하며 자신의 -1 인덱스의 값을 이용해 자신의 문자열을 구하는 함수를 실행하였습니다.

하지만 1/3의 테스트 케이스에서 시간초과의 결과가 출력되었습니다.<BR><bR>

주어진 범위내의 1의 개수를 구하는 과정은 범위의 값들을 검색하기위해 반드시 필요한 작업이기 때문에 DP의 요소들을 유사 칸토어 함수로 치환하는 과정을 더 효율적으로 변경해야했습니다.

이 과정에서 참고한 풀이에서는 

DP[0] 의 값이 '1' 로 주어졌기 때문에, 아래와 같은 점화식을 세울 수 있었습니다.

___f(n) = f(n-1) + f(n-1) + 5^(n-1) + f(n-1) + f(n-1) ___

만약 위 점화식의 n값이 1 이라면

___f(1) = (f0) + f(0) + 5^(-1) + f(0) + f(0)___

f(0) 은 '1'이라는 값이 주어져있기 때문에 아래와 같은 의미가 됩니다.

___f(1) = '1' + '1' + 0 + '1' + '1'___<Br><Br>


## 규칙2

    0번째 유사 칸토어 비트열 == 1
    1번째 유사 칸토어 비트열 == 11011
    2번째 유사 칸토어 비트열 == 11011 11011 00000 11011 11011

    주어진 0번째 비트열을 제외하고 규칙이 발생합니다.

    주어진 문자열을 5로 나눈값들 중 2번째 인덱스의 값들은 모두 0이고,

    나누어진 문자열을 다시 5로 나누어도 그 중 2번째 인덱스의 값 또한 0이라는 점입니다.

    이를 이용하면 미리 모든 DP의 비트열값을 계산해 저장해 놓지 않고 바로 답을 구할 수 있습니다. 
<BR<bR>

만약 13번째 인덱스의 값이 0인지 1인지 알고 싶다면,

그 값이 5로 나눈 나머지가 2일 경우 가운데 인덱스인 2에 존재한다는 의미이므로 0임을 알 수 있습니다.

그리고 5로 나눈 나머지가 2가 아니라면, 주어진 값을 5로 나눈 몫이 5 미만이 될 때까지 반복하여 전체 문자열에서 중간에 위치한 값인지 알아내 판단합니다.

___이 과정에서 5개씩 구간을 나누어 검사하는 기법이 [슬라이딩 윈도우](#슬라이딩-윈도우-알고리즘-sliding-window-algorithm) 알고리즘 입니다.___

```C++
int index;

while(true)
{
    if(index % 5 == 2)
        cout << "해당 인덱스의 값은 0";

    if(index < 5 && index != 2)
        cout << "해당 인덱스의 값은 1";

    index /= 5;
}

// 함수로 표현
bool isOne(int index)
{
    if(index % 5 == 2)
        return false;
    if(index < 5 && index != 2)
        return true;
    
    index /= 5;
}
```

<br><Br>


## __전체 풀이__(규칙2 참고)

```c++
#include <string>
#include <vector>
#include <iostream>
#include <cmath>

using namespace std;

bool isOne(long index)
{
    if(index % 5 == 2)
    {
        return false;
    }
        
    if(index < 5 && index != 2)
    {
        return true;
    }
        
    return isOne(index /= 5);
}

int solution(int n, long long l, long long r) {
    // 1 -> 11011 치환
    // 0 -> 00000 치환
    // l <= n <= r, 1의 개수
    int answer = 0;
    
    // 원하는 범위 내의 1의 개수 구하기
    for(long i = l-1; i < r; i++)
    {
        if(isOne(i))
        {
            answer++;
        }
    }
    return answer;
}
```

l 과 r의 범위가 int를 벗어나므로 반복문 변수의 데이터형도 long으로 선언해야 합니다.<br><Br>


## __슬라이딩 윈도우 알고리즘 Sliding Window Algorithm__ 

## __투 포인터 알고리즘 Two Pointer Algorithm__ 

<img src="https://github.com/EunYoungP/TIL/assets/80774412/4c3d1f0a-4fbf-4e68-859b-a3753acb1cf4"></img>

[이미지 출처](https://velog.io/@zwon/%EC%8A%AC%EB%9D%BC%EC%9D%B4%EB%94%A9-%EC%9C%88%EB%8F%84%EC%9A%B0Sliding-Window)

