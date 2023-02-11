2023.02.12

# __[프로그래머스 LV2] 택배상자__

stack  queue

---- 

## __문제__

<img src="https://user-images.githubusercontent.com/80774412/218274388-73c0f86d-cffc-4d28-883b-e8d514360999.PNG"></img>

<img src="https://user-images.githubusercontent.com/80774412/218274392-634da967-2f20-465e-9672-9afdbd020d88.PNG"></img>


<br><br>

## __나의 풀이__

```c++
#include <string>
#include <vector>
#include <queue>
#include <stack>
#include <iostream>

using namespace std;

int solution(vector<int> order) {
    int answer = 0;
    
    queue<int> main;
    stack<int> sub;
    
    // 메인 컨테이너 벨트에 오름차순 값 초기화
    for(int i = 1; i <= order.size(); i++)
    {
        main.push(i);
    }
    
    for(int i= 0; i < order.size(); i++)
    {
        // 메인 컨테이너 벨트보다 서브 컨테이너 벨트의 검색을 먼저 해줘야합니다.
        // 서브의 탑에 있는 해당 순서의 상자를 찾지 못하고 메인을 검색하여 다른 상자를 추가하면, 서브의 스택 특성때문에 답이 중간에 위치하게되어 검색하지 못합니다.
        if(!sub.empty() && sub.top() == order[i])
        {
            answer++;
            sub.pop();
            continue;
        }
        
        // 메인 컨테이너 벨트를 검색하는 순서에서는 order 순서에 맞는 값이 존재할때까지 메인 컨테이너에 담긴 박스를 모두 꺼내어 빈 상태가 될때까지 검색을 진행합니다.
        // 매 검색때 일치하는 값을 찾지 못했다면 서브 컨테이너로 박스를 옮깁니다.
        while(!main.empty())
        {
            int curBox = main.front();
            main.pop();
            
            if(curBox == order[i])
            {
                answer++;
                break;
            }
            else
            {
                sub.push(curBox);
            }
            if(main.empty())
                return answer;
        }
    }
    return answer;
}
```
__스택__ 과 __큐__ 자료구조의 특성을 이용해 푸는 문제였습니다.

스택은 __FIFO__ First In First Out 구조로, 먼저 들어간 입력값이 먼저 출력됩니다.

큐는 __LIFO__ Last In First Out 구조로, 나중에 입력된 값이 제일 먼저 출력됩니다.