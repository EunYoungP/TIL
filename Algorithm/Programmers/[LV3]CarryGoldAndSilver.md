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

검색이 진행될 때마다 선형탐색보다 비교할 수 없게 빨라집니다.


[참고 자료]("https://velog.io/@kimdukbae/%EC%9D%B4%EB%B6%84-%ED%83%90%EC%83%89-%EC%9D%B4%EC%A7%84-%ED%83%90%EC%83%89-Binary-Search")

<br><Br>

## __이진 탐색을 이용한 풀이__ (풀이 참고)

첫 풀이에서는 금과 은을 운반할 수 있는 가장 빠른 시간을 구하려고 했습니다.

* => 1초 부터 시작하여 탐색을 하면 정답까지 모든 경우의 수를 탐색합니다.

하지만 반대로 시간을 기준으로 모든 금과 은을 운반할 수 있는지 검사한다면 이분탐색을 적용하여 풀이할 수 있습니다.

* => 시간을 정해놓고, 그 시간내에 금과 은을 모두 운반할 수 있는지 탐색합니다.

<br><Br>

이진 탐색은 시작인덱스와 끝인덱스에서 중간값을 구하여 탐색합니다.

* 시작 인덱스인 가장 적은 운반 시간은 1초가 됩니다.

* 끝 인덱스인 가장 많은 운반 시간을 계산해봅시다.

    * 우선 각 트럭은 각 도시에 정차 되어있는 상태로 시작하기 때문에,
    미리 한번 신도시에 금과 은을 운반했다고 가정한 상태에서 시작하기위한 계산을 해줍니다.

    한번에 운반할 수 있는 무게의 금과 은을 옮겨야할 광물의 총 양에서 빼주고,
    현재까지 걸린시간에 한번 운반하는데 걸린 시간을 더해줍니다.

    이 다음부터는 운반하기 위한 시간을 왕복으로 계산합니다.

    수식화하면 아래와 같습니다.
    ```
    필요한 광물의 전체 양(금+은)            = A
    한번에 운반 가능한 양                  = B
    한번 운반하는데 걸리는 시간             = C
    
    필요한 광물을 모두 옮기는데 필요한 시간  = 2C * ((A-B)/B) + C
    ```

    문제에서 제시된 조건을 이용하여 끝값이 될 운반시간에 최악의 경우를 위한 조건은
    금과 은의 운반해야할 양의 최대값은 10^9,
    한번에 운반할 수 있는 양의 최소값은 1,
    한번 운반하는데 걸리는 시간의 최대값은 10^5
    로 설정할 수 있습니다.

    위의 수식에 대입해보면 끝값은
    * (10^5 * 2) * (10^9-1)/1 + 10^5
    * => 10^14 * 2 + 10^5가 됩니다. 
    * => 10^14 * 3
    * 1은 너무 미미한 수치이기 때문에 생략


                
### __코드 구현__

```c++
#include <string>
#include <vector>
#include <algorithm>
#include <iostream>

using namespace std;

bool BinarySearch(int a, int b, vector<int> g, vector<int> s, vector<int> w, vector<int> t, long long mid)
{
    long long totalGold = 0;
    long long totalSilver = 0;
    long long totalMineral = 0;
    
    for(int i = 0; i < g.size(); i++)
    {
        long long roundTime = t[i] * 2;
        
        // 운반 횟수 판정방법 1
         long long moveCount = (mid - t[i]) / roundTime;
         moveCount++;
         // 운반 횟수 판정방법 2
         // long long moveCount = mid / roundTime;
         // if((mid % roundTime) >= t[i])moveCount++;
        long long maxTake = w[i] * moveCount;
        
        totalGold += min((long long)g[i], maxTake);             // 주어진 시간 내 옮길 수 있는 금의 양
        totalSilver += min((long long)s[i], maxTake);           // 주어진 시간 내 옮길 수 있는 은의 양
        totalMineral += min((long long)g[i] + s[i], maxTake);   // 주어진 시간 내 옮길 수 있는 전체 광물의 양
    }
    
    if(totalGold >= a && totalSilver >= b && totalMineral >= a+b)
    {
        return true;
    }
    return false;
}

// 목표 금, 목표 은, 금, 은, 최대 운반광물 무게, 편도 이동 시간
long long solution(int a, int b, vector<int> g, vector<int> s, vector<int> w, vector<int> t) {
    long long start = 0;
    long long end = 10e14 * 3;
    long long answer = end;
    
    while(start <= end)
    {
        long long mid = (start + end) / 2;
        if(BinarySearch(a,b,g,s,w,t,mid) == true)
        {
            answer = min(answer, mid);
            end = mid-1;
        }
        else start = mid+1;
    }
    
    return answer;
}
```

운반횟수 판정방법 1 에서는 미리 맨 처음 도시에서 신도시로 편도로 운반할 수 있는 광물의 개수와 시간을 더해주는 방법입니다.

운반횟수 판정방법 2 에서는 정해진 시간을 왕복 운반 시간으로 나눠 운반 횟수를 구해주고, 그 나머지가 편도 운반시간 이상이면 개수와 시간을 더해줍니다.

이렇게 이진탐색을 이용한 문제풀이를 해보았습니다.