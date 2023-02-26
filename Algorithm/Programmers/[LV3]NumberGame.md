2023.02.26

# __[프로그래머스 LV3]숫자게임__

그리디

[프로그래머스](https://school.programmers.co.kr/learn/courses/30/lessons/12987#qna)

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
#include <iostream>

using namespace std;

int solution(vector<int> A, vector<int> B) {
    int answer = 0;
    
    // B 팀의 남아있는 자연수 중
    // A 팀원보다 높은 수들 중 가장 작은 수
    
    int ASIZE = A.size();
    int BSIZE = B.size();
    
    sort(B.begin(), B.end());
    
    for(int i = 0; i < ASIZE; i++)
    {
        int pivot = A[i];
        
        for(int j = 0; j < BSIZE; j++)
        {
            if(B[j] > A[i])
            {
                B[j] = 0;
                answer++;
                break;
            }
        }
    }
    
    return answer;
}
```
이 풀이는 모든 테스트를 통과했지만, 효율성 테스트를 통과하지 못했습니다.

이중 for 문으로 최대 시간 복잡도가 O(N^2) 이 될 수 있기 때문에 O(N) 이나 O(logN) 으로 줄여야했습니다.<BR><bR>

## __나의 풀이 2__(오답)

```c++
#include <string>
#include <vector>
#include <algorithm>
#include <iostream>

using namespace std;

int solution(vector<int> A, vector<int> B) {
    int answer = 0;
    
    // B 팀의 남아있는 자연수 중
    // A 팀원보다 높은 수들 중 가장 작은 수
    
    int ASIZE = A.size();
    int BSIZE = B.size();
    
    sort(A.begin(), A.end());
    sort(B.begin(), B.end());
    
    int pointer = 0;
    for(int i = 0; i < ASIZE; i++)
    {
        pointer++;
        int pivot = A[i];
            
        while(B[pointer] <= pivot && pointer < BSIZE)
        {
            pointer++;
        }
        
        if(pointer >= BSIZE)
            break;
        
        answer++;
    }
    return answer;
}
```

두 번째 풀이는 포인터를 사용해서 B 벡터를 순회하는 방법에서 FOR 문으로 모든 원소를 순회하지 않고, 포인터를 이용하여 B원소를 검색하는데에 O(1) 의 시간 복잡도를 가질 수 있게 했습니다.

이 방법으로 한번의 for 문을 순회할 수 있게 되어 O(N)의 시간 복잡도를 가졌으나

정확도 테스트 중 한개의 테스트케이스가 통과되지않아 구글링으로 풀이 방법을 참조했습니다.


## __나의 풀이 3__(정답)

```c++
#include <string>
#include <vector>
#include <algorithm>
#include <iostream>
#include <queue>

using namespace std;

int solution(vector<int> A, vector<int> B) {
    int answer = 0;
    
    // B 팀의 남아있는 자연수 중
    // A 팀원보다 높은 수들 중 가장 작은 수
    
    int ASIZE = A.size();
    int BSIZE = B.size();
    
    queue<int> aq;
    queue<int> bq;
    
    // 미리 입력 벡터를 정렬해야만 비교 시 조건을 만족하지 않는 요소를 제거하고 검색할 수 있습니다.
    sort(A.begin(), A.end());
    sort(B.begin(), B.end());
    
    // 모든 원소를 큐에 삽입
    for(int a : A)aq.push(a);
    for(int b : B)bq.push(b);

    // 하나의 큐의 원소가 없다면,
    // 모든 원소가 비교된 상태입니다.
    while(!aq.empty() && !bq.empty())
    {
        int a = aq.front();
        int b = bq.front();
        
        if( b > a)
        {
            aq.pop();
            bq.pop();
            answer++;
            continue;
        }
        else if( b <= a)
        {
            bq.pop();
        }
    }
    return answer;
}
```

구글링으로 참조한 방법은, 큐를 사용한 방법입니다.

포인터를 사용하여 B 벡터의 원소를 한번씩만 검색할 수 있게 한것처럼,

큐를 이용하여 검색을 마친 원소는 POP으로 제거하여 중복 검색을 막았습니다.
