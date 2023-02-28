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

하지만 해당 방법으로는 모든 정확도 테스트를 통과할 수 없었고 시간 초과가 일어나는 문제가 있었습니다.<br><br>

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

본래 코드에서 시간이 초과되는 주된 원인은 왼쪽 부분, 오른쪽 부분에서 최소값을 매 반복 분기마다 min_element 로 모든 원소를 검사하여 진행했기 때문이라고 생각했습니다.

따라서 미리 어떠한 인덱스에 대한 왼쪽, 오른쪽 부분에대한 최소값을 알 수 있는 방법을 찾아 구현했습니다.

미리 만든 int 형 벡터 두개는 왼쪽 인덱스에서 최소값을 판단하는 벡터와 오른쪽 부분에서 최소값을 판단하게 하는 벡터입니다.

반복문 한번을 통해서 O(N)으로 벡터 하나는 왼쪽부터 비교하여 더 작은 값을 입력하고, 입력된 최소값을 비교값으로 설정하는 과정을 반복했고 오른쪽도 같은 방식으로 끝인덱스부터 첫 인덱스까지 최소값을 각 벡터의 인덱스에 입력했습니다.

이렇게 하면 어떠한 원소가 마지막까지 남아있는지 검색할 때 해당 인덱스를 가지고 left 벡터와 right 벡터에 입력된 각각의 값을 통해 O(1)로 최소값을 알 수 있습니다.
<br><Br>

## __다른 사람 풀이__

```c++
#include <string>
#include <vector>
#include <stack>

using namespace std;
int solution(vector<int> a) {
    int answer = a.size();
    stack<int> stack;
    for(int comp : a){
        while(!stack.empty() && comp < stack.top()){
            stack.pop();
            if(!stack.empty())
                answer--;
        }
        stack.push(comp);
    }
    return answer;
}
```

스택을 사용하여 풀이하는 방법입니다.

마지막까지 남길 수 없는 경우를 체크하여 답에서 빼나가는 방식으로 구합니다.