2023.04.04

# __Binary Search 이진 탐색__

[이진 탐색 관련 문제](https://school.programmers.co.kr/learn/courses/30/lessons/86053)

---- 

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

- 이진수는 0 과 1 로 이루어져 있어