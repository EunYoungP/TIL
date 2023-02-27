2023.02.27

# __[프로그래머스 LV3]풍선 터트리기__

[프로그래머스](https://school.programmers.co.kr/learn/courses/30/lessons/68646)

---- 

## __문제__

<img src="https://user-images.githubusercontent.com/80774412/221412525-8144698f-70e4-42cd-89f0-b92dfabd76aa.PNG"></img>

<img src="https://user-images.githubusercontent.com/80774412/221412528-57e934bf-d374-4ac3-b25d-158be250aed9.PNG"></img>

<br><br>


## __나의 풀이 1__(오답)

```c++
#include <string>
#include <vector>
#include <algorithm>

using namespace std;

int solution(vector<int> a) {
    int answer = 2;
    
    int ASIZE = a.size();
    
    int curNum;
    for(int i = 1; i < ASIZE-1; i++)
    {
        curNum = a[i];
        int leftMin = *min_element(a.begin(), a.begin() + i - 1);
        int rightMin = *min_element(a.begin() + i + 1, a.end());
        
        // 왼쪽, 오른쪽 모두 비교값보다 작다면 마지막까지 남을 수 없습니다.
        if(leftMin < curNum && rightMin < curNum)
            continue;
        else
            answer++;
    }
    return answer;
}
```

우선 나열된 숫자들을 인접한 숫자끼리만 비교할 수 있다는 특징을 가지고 있습니다.

만약 k 번째에 존재하는 숫자가 마지막까지 남을 수 있는지 확인하고 싶다고 가정한다면,

0 ~ (k-1) 인덱스 중에서 가장 작은 수와 (k+1) ~ 마지막인덱스 중에서 가장 작은 수를 구해야 합니다.

규칙에 따르면 인접한 두 수 중 큰수를 제거할 수 있고, 작은 수를 제거하는 것은 단 한번만 가능합니다.

원하는 원소를 비교하기 전 왼쪽 오른쪽 가장 작은 수를 구하는 비교는 큰 수만을 제거하여 가장 작은 수를 구하는 것입니다.

그렇게 __왼쪽 인덱스 중 최소값__, __검색 값__, __오른쪽 인덱스 중 최소값__ 세가지 수를 비교하여 제거 할 수 있는 방법은

큰 값 삭제하기(제한 없음) 과 작은 값 삭제하기(1번 한정) 이 됩니다.

즉, 왼쪽 최소값과 오른쪽 최소값이 모두 검색을 원하는 값보다 작다면 검색을 원하는 값은 한 번은 작은값 삭제하기를 사용하여 남을 수 있지만,

두번째 비교에서 결국 큰 값 삭제하기 밖에 사용할 수 없어 마지막까지 남을 수 없습니다. <br><Br>

하지만 해당 방법으로는 모든 정확도 테스트를 통과할 수 없었습니다.<br><br>

## __나의 풀이 2__(참고 정답)

```c++
#include <vector>
using namespace std;

int solution(vector<int> a) {
    int answer = 0;
    int len = a.size();
    vector<int> left(len);
    vector<int> right(len);

    int MIN = a[0];
    for (int i = 0; i < len; i++) {
        if (MIN > a[i]) MIN = a[i];
        left[i] = MIN;
    }

    MIN = a[len - 1];
    for (int i = len - 1; i >= 0; i--) {
        if (MIN > a[i]) MIN = a[i];
        right[i] = MIN;
    }

    for (int i = 0; i < len; i++) {
        if (a[i] <= left[i] || a[i] <= right[i]) answer++;
    }
    return answer;
}
```