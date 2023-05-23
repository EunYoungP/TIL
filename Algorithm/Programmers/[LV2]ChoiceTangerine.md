2023.05.23

# __[프로그래머스 LV2] 귤 고르기__

----

## __문제__

[귤 고르기](https://school.programmers.co.kr/learn/courses/30/lessons/138476)<br><Br>


## __나의 풀이__
```c++
#include <string>
#include <vector>
#include <map>
#include <algorithm>

using namespace std;

bool cmp(int a, int b)
{
    return a > b;
}

int solution(int k, vector<int> tangerine) {
    int answer = 0;
    map<int, int> m;
    vector<int> numVector;
    
    // map을 사용해서 종류별 개수 구하기
    for(int i = 0; i < tangerine.size(); i++)
    {
        m[tangerine[i]]++;
    }
    
    // 귤의 종류별 개수만 벡터에 저장
    for(auto i : m)
    {
        numVector.push_back(i.second);
    }
    
    // 귤의 개수 벡터를 내림 차순으로 정렬
    sort(numVector.begin(), numVector.end(), cmp);
    
    // 내림차순으로 순회하며 k와 비교
    int temp = 0;
    for(int i = 0; i < numVector.size(); i++)
    {
        answer++;
        int curNum = numVector[i];
        temp+=curNum;
        
        if(temp >= k)
        {
            break;
        }
    }
    
    return answer;
}
```

종류별 개수가 많은 순으로 정렬한 벡터를 순회하며 주어진 k개수와 비교하였습니다.

k에서 주기마다 귤의 개수를 더해 k보다 크거나 같아지는 분기의 answer가 답이됩니다.<br><Br>

문제에서 주어진 tangerine 벡터의 값을 map으로 옮기는 과정은 시간복잡도가 O(N)입니다.

그리고 정리한 map의 second값들만 벡터로 다시 옮기는 과정도 O(N)입니다.

마지막으로 정렬된 벡터를 순회하여 k와 비교하는 과정 또한 O(N)이기 때문에,

시간 복잡도 O(N) 으로 문제를 해결할 수 있습니다.