2023.05.16

# __[프로그래머스 LV2] 테이블 해시 함수__

HashTable, Map

----

## __문제__

[테이블 해시 함수](https://school.programmers.co.kr/learn/courses/30/lessons/147354#qna)<br><Br>

## __나의 풀이__
```c++
#include <string>
#include <vector>
#include <map>

using namespace std;

int column;

//  데이터 정렬
bool comp(vector<int> a, vector<int> b)
{
    if(a[column] == b[column])
    {
        if(a[0] > b[0])
            return true;
    }
    else if(a[column] < b[column])
        return true;
    else if(a[column] > b[column])
        return false;
    return false;
}

int solution(vector<vector<int>> data, int col, int row_begin, int row_end) {
    int answer = 0;
    column = col - 1;
    map<int, int> m;
    
    sort(data.begin(), data.end(), comp);
    vector<int> v;
    
    // 데이터 인ㄷ게스 나머지 합 구하기
    for(int i = row_begin-1; i < row_end; i++)
    {
        vector<int> curData = data[i];
        int total = 0;
        for(int j = 0; j < curData.size(); j++)
        {
            total += curData[j] % i;
        }
        v.push_back(total);
    }
    
    // 각데이터 비트 연산하기
    for(int i = 0; i < v.size(); i++)
    {
        v[i] 
    }
    
    return answer;
}
```