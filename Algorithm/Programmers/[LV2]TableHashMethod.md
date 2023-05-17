2023.05.16

# __[프로그래머스 LV2] 테이블 해시 함수__

----

## __문제__

[테이블 해시 함수](https://school.programmers.co.kr/learn/courses/30/lessons/147354#qna)<br><Br>

## __나의 풀이__
```c++
#include <string>
#include <vector>
#include <algorithm>
#include <iostream>

using namespace std;

int column;

//  데이터 정렬
bool comp(vector<int> a, vector<int> b)
{
    // 기본키 기준 내림차순
    if(a[column] == b[column])
    {
        if(a[0] > b[0])
            return true;
    }
    // col번째 컬럼 기준 오름차순
    else if(a[column] < b[column])
        return true;
    else if(a[column] > b[column])
        return false;
    return false;
}

// 1. 해시 함수는 col, row_bergin, row_end 을 입력
int solution(vector<vector<int>> data, int col, int row_begin, int row_end) {
    int answer = 0;
    column = col - 1;
    
    // 2. 테이블 튜플 정렬
    sort(data.begin(), data.end(), comp);
    vector<int> v;
    
    // 3. 각 컬럼의 값을 i로 나눈 나머지들의 합 정의
    for(int i = row_begin-1; i < row_end; i++)
    {
        vector<int> curData = data[i];
        int total = 0;
        for(int j = 0; j < curData.size(); j++)
        {
            total += curData[j] % (i+1);
        }
        v.push_back(total);
    }
    
    // 각 데이터 XOR 비트 누적 연산
    for(int i = 0; i < v.size(); i++)
    {
        answer ^= v[i]; 
    }
    
    return answer;
}
```



