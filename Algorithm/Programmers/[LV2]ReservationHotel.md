2023.05.07

# __[프로그래머스 LV2] 호텔 대실_

우선순위큐

## __문제__

[호텔 대실](https://school.programmers.co.kr/learn/courses/30/lessons/155651#qna)<br><Br>

## __나의 풀이__(정답)
```c++
#include <string>
#include <vector>
#include <algorithm>
#include <queue>

using namespace std;

struct TIME
{
    int start;
    int end;
    
    // string 인자를 int로 변경하여 저장합니다.
    TIME(string _start, string _end)
    {
        start = stoi(_start.substr(0,2))*60 + stoi(_start.substr(3,2));
        end = stoi(_end.substr(0,2))*60 + stoi(_end.substr(3,2));
    }
    
    // 구조체 연산자 오버로딩, 오름차순 정렬
    bool operator<(const TIME other)const
    {
        if(start < other.start)
            return false;
        else if(start == other.start)
        {
            if(end < other.end)
                return false;
        }
        return true;
    }
};

int solution(vector<vector<string>> book_time) {
    int answer = 0;
    // 퇴실 기준 10분 청소
    // 필요한 최소 객실 수
    
    // 예약시간 기준으로 오름차순 정렬합니다.
    priority_queue<TIME> pq;
    vector<TIME> rooms;
    
    // 이중벡터값을 구조체에 할당하여 우선순위큐에 저장합니다.
    for(int i = 0; i < book_time.size(); i++)
    {
        TIME curTime = TIME(book_time[i][0],book_time[i][1]);
        pq.push(curTime);
    }
    
    // 구조체 생성자에 string형태의 인자로 선언되어있기 때문에 형식을 주의해야합니다.
    rooms.push_back(TIME("00:00","00:00"));
    
    while(!pq.empty())
    {
        bool isReserve = false;
        TIME curReservation = pq.top();
        pq.pop();
        
        for(int i = 0; i < rooms.size(); i++)
        {
            if(rooms[i].end + 10 <= curReservation.start)
            {
                rooms[i] = curReservation;
                isReserve = true;
                break;
            }
        }
        if(isReserve == false)
            rooms.push_back(curReservation);
    }
    
    answer = rooms.size();
    return answer;
}
```

우선순위큐를 사용하여 호텔 대실 예약 시각이 담긴 2차원 배열을 정렬했습니다.

구조체 내의 연산자 오버로딩을 사용하여 체크인시간, 체크아웃시간이 작은 순으로 정렬했습니다.<br><Br>

우선순위 큐를 순회하면서 모든 방을 검사하여 예약할 수 있는 방이 있다면 패스하고,

예약할 수 있는 방이 없다면 방을 추가하여

모든 예약시각을 검사한 후 방의 개수를 반환했습니다.

