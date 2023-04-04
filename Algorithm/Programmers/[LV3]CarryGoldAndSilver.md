2023.04.03

# __[프로그래머스 LV3] 금과 은 운반하기__

이분탐색

## __문제__

[금과 은 운반하기](https://school.programmers.co.kr/learn/courses/30/lessons/86053)<br><Br>

## __나의 풀이__(오답)
```c++
#include <string>
#include <vector>
#include <map>

using namespace std;

// 목표 금, 목표 은, 금, 은, 최대 운반광물 무게, 편도 이동 시간
long long solution(int a, int b, vector<int> g, vector<int> s, vector<int> w, vector<int> t) {
    long long answer = -1;
    map<float, int, greater<float>> cityIndex;
    int goldTime = 0;
    int silverTime = 0;
    
    // 금 도시 정렬
    for(int i= 0; i < g.size(); i++)
    {
        if(g[i] == 0)continue;
        float temp = g[i]/t[i]*w[i];
        cityIndex.insert(make_pair(temp, i));
    }
    
    // 금 운반
    for(auto mit : cityIndex)
    {
        int curIndex = mit.second;
    }
    
    // 은 도시 정렬
    for(int i= 0; i < s.size(); i++)
    {
        if(s[i] == 0)continue;
        float temp = s[i]/t[i]*w[i];
        cityIndex.insert(make_pair(temp, i));
    }
    
    return answer;
}
```

map을 이용하여 각 도시의 운반률을 key, 도시의 번호를 value로 하여,

key값이 가장 큰 값을 가지는 도시 순서대로 운반계산을 하려고 했습니다.

하지만 이런 방식으로 하니 금을 운반할 때 아무리 운반률이 좋은 도시라도 두 번쨰 운반은 왕복이 됨으로써 그 다음 도시보다 효율이 안 좋을 수 있다는 경우가 제외 되었습니다.

참고한 풀이에서는 이분탐색이라는 알고리즘을 사용합니다.

처음 접하는 알고리즘이기 때문에 어떤 알고리즘인지 알아보겠습니다.
<br><Br>

## 💡 __이분 탐색 / 이진 탐색 Binary Search__

이론은 알고 있지만 직접 문제에 적용시켜 풀이해보는 것은 처음이기 때문에 다시 한번 개념을 정리하겠습니다.

### __개념__

<img src="https://user-images.githubusercontent.com/80774412/229822048-875590c0-fc15-4013-8ba9-af378f8ca617.gif"></img>

__정렬 되어 있는__ 리스트에서 __탐색 범위를 절반씩__ 좁혀가며 데이터를 탐색하는 방법입니다.

start, end, mid  변수 3개를 사용하여 탐색하는데,

찾으려는 데이터와 중간점 위치에 있는 mid 데이터를 찾습니다.

<br><Br>

###  __이진 탐색 구현__
```c++
#include <vector>
#include <algorithm>
using namespace std;

// 검색할 리스트 인덱스의 범위를 받고,
// 리스트의 해당 인덱스값이 타겟과 일치하면 반환해줍니다.
int BinarySearch(vector<int> array, int startIdx, int endIdx, int target)
{
    int mid = 0;
    int midIdx = 0;
    int start = array[startIdx];
    int end = array[endIdx];

    while(startIdx <= endIdx)
    {
        int midIdx = (startIdx + endIdx) / 2;
        mid = array[midIdx];

        if(mid == target) return midIdx;
        else if(mid < target) start = array[midIdx + 1];
        else if(mid > target) end = midIdx -1;
    }
    return 0;
}

void main()
{
    vector<int> arr = {1, 3, 4, 5, ,6 7,7, 7 8, 8, 2, 4};

    sort(arr.begin(), arr.end());

    int startIndex = 0;
    int endIndex = arr.size()-1;
    int targetIndex = BinarySearch(arr, startIndex, endIndex, 5);
}
```
__시간 복잡도는 O(logN)__ 입니다.

매 반복 단계마다 범위를 반으로 나누어 검색하기 때문입니다.

(e.g. 32, 16, 8, 4, 2, 1 순으로 진행됩니다.)


[참고 자료]("https://velog.io/@kimdukbae/%EC%9D%B4%EB%B6%84-%ED%83%90%EC%83%89-%EC%9D%B4%EC%A7%84-%ED%83%90%EC%83%89-Binary-Search")