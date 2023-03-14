2023.03.14

# __[프로그래머스 LV3] 다단계 칫솔 판매__

[프로그래머스](https://school.programmers.co.kr/learn/courses/30/lessons/77486?language=cpp)

map

---- 

<BR>

## __문제__

<img src = "https://user-images.githubusercontent.com/80774412/224870931-c4546dd8-ec92-4fa5-80cf-472b348c985a.PNG"></img>

<img src = "https://user-images.githubusercontent.com/80774412/224870939-3c2728d5-a71c-44c6-a515-cbc7b97086db.PNG"></img>

<img src = "https://user-images.githubusercontent.com/80774412/224870946-c4360691-bac7-466c-802e-e5aefe1c39f4.PNG"></img>
<BR><BR>


## __나의 풀이__(정답)
```c++
#include <string>
#include <vector>
#include <algorithm>
#include <map>

using namespace std;

map<string, string> connection;
map<string, int> moneyCon;

// 판매원 이름과 이익금을 받아 부모에 10%를 부여해줍니다.
void UpdateSales(string name, int money)
{
    if(name == "none") return;
    
    int tenPercent = money / 10;
    int remainder = money - tenPercent;
    moneyCon[name] += remainder;
    if(tenPercent < 1)return;
    
    UpdateSales(connection[name], tenPercent);
}
// 각 판매원 이름, 판매원을 조직에 참여시킨 판매원, 판매량 집계 데이터(판매원), 판매량 집계 데이터(판매 수량)
vector<int> solution(vector<string> enroll, vector<string> referral, vector<string> seller, vector<int> amount) {
    vector<int> answer;

    // 10원 단위에서 절사
    // 10% < 1원, 이득 분배 하지않음
    
    // 벡터에 들어있는 부모-자식 관계 정보들을 이름을 키로 만든 map 컨테이너에 넣어줍니다.
    for(int i = 0; i < enroll.size(); i++)
    {
        if(enroll[i] == "-")connection[enroll[i]] = "none";
        connection[enroll[i]] = referral[i];
    }
    
    // 칫솔을 판매한 사람의 이익금을 계산하여 분배합니다.
    for(int i = 0; i < seller.size(); i++)
    {
        UpdateSales(seller[i], amount[i] * 100);
    }
    
    // map 컨테이너에 정렬된 판매원이름-이익금 데이터를 answer 벡터에 enroll 판매원 이름 순으로 넣어줍니다.
    for(int i = 0; i < enroll.size(); i++)
    {
        answer.push_back(moneyCon[enroll[i]]);
    }
    
    return answer;
}
```

여러 벡터에 나눠져 있는 데이터를 map을 사용해서 이름을 키로 검색할 수 있게 했습니다.

위 코드에서는 map 컨테이너에 데이터를 넣었지만, 키가 정렬되지 않아도 되는 문제이기 때문에

unordered_map 컨테이너에 데이터를 넣는것도 좋을것 같습니다.