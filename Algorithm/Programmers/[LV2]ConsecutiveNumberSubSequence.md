2023.05.25

# __[프로그래머스 LV2] 연속 부분 수열 합의 개수__

----

## __문제__

[연속 부분 수열 합의 개수](https://school.programmers.co.kr/learn/courses/30/lessons/131701)<br><Br>


## __나의 풀이__
```c++
#include <string>
#include <vector>
#include <set>
#include <algorithm>
#include <iostream>

using namespace std;

int solution(vector<int> elements) {
    int answer = 0;
    int len = elements.size();
    set<int> s;
    vector<int> copyVector(elements);
    // 같은 배열을 뒤에 복사하여 붙이기
    copy(copyVector.begin(), copyVector.end(), back_inserter(elements));
    
    int num = 0;
    // 연속된 원소개수의 종류
    for(int i = 0; i < len; i++)
    {
        num++;
        // 요소의 인덱스
        for(int j= 0; j < len && num <= len; j++)
        {
            int sum = 0;
            int count = 0;
            // 부분 수열의 개수
            while(count < num)
            {
                sum += elements[j+count];
                count++;
          
            }
            s.insert(sum);
        }
    }
    
    answer = s.size();
    return answer;
}
```

set은 중복된 값을 가질 수 없는 특성을 사용하여 중복된 숫자의 합을 제거했습니다.

원소의 개수가 N인 벡터의 부분 수열이 가질 수 있는 개수의 종류는 N개입니다.

3개의 요소가 있다면 1개씩인 요소, 2개씩인 요소들의 합, 3개씩인 요소들의 합이 존재합니다.<BR><BR>

따라서 각 개수를 분기로 나눠줍니다.

그리고 모든 요소를 순회하면서 N개만큼 더해준 값을 SET에 삽입하여 중복값을 없애줍니다.<BR><bR>

어떻게 보면 투포인터 알고리즘의 방식을 사용했다고 볼 수 있습니다.

변수로 선언하지 않고 반복문 변수로 시작, 끝 인덱스를 지정하여 원하는 요소개수만큼 더해준것과 같습니다.<br><Br>

만약 원소를 3개씩 더한값을 set에 넣어주고싶다면,

인덱스가 [0, 1, 2], [1, 2, 3], [2, 3, 4], [3, 4, 5] ~ 이런 식으로 설정되어야합니다.

여기서 첫번째 인덱스를 반복문으로 설정해준겁니다.

```c++
// i값이 각 인덱스 묶음의 첫번째 값
for(int i = 0; i < len; i++)
```

그리고 while문을 이용하여 원하는 개수만큼 시작 인덱스에서 더해줍니다.

```c++
// 원하는 원소의 개수
int num = 3;
// 헌재 더해준 원소의 개수
int count = 0;
while(count < num)
{
    sum += elements[i+count];
    count++;
}
```

이렇게 투 포인터 알고리즘을 활용해 풀이했습니다.



