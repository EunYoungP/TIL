2023.04.28

# __[프로그래머스 LV2] 광물 캐기__


## __문제__

[광물 캐기](https://school.programmers.co.kr/learn/courses/30/lessons/172927)<br><Br>

## __나의 풀이__
```c++
#include <string>
#include <vector>
#include <queue>
#include <algorithm>
#include <iostream>

using namespace std;

struct SepMineral
{
    int diamond = 0;
    int iron = 0;
    int stone = 0;
    
    SepMineral(vector<string> mineralV)
    {
        for(int i = 0; i < mineralV.size(); i++)
        {
            if(mineralV[i] == "diamond")
                diamond++;
            else if(mineralV[i] == "iron")
                iron++;
            else if(mineralV[i] == "stone")
                stone++;
        }
    }
    
    bool operator< (const SepMineral& other)const 
    {
        if(diamond > other.diamond)
            return true;
        else if(iron > other.iron)
            return true;
        else if(stone > other.stone)
            return true;
        return false;
    }
    
    int GetFatigueLevel(int pick)
    {
        int fatigue = 0;
        if(pick == 0)
        {
            fatigue += diamond * 1;
            fatigue += iron * 1;
            fatigue += stone * 1;
        }
        else if(pick == 1)
        {
            fatigue += diamond * 5;
            fatigue += iron * 1;
            fatigue += stone * 1;
        }
        else if(pick ==2)
        {
            fatigue += diamond * 25;
            fatigue += iron * 5;
            fatigue += stone * 1;
        }
        cout << pick  <<" "<< fatigue << endl;
        return fatigue;
    }
};

// 곡괭이의 개수[dia, iron stone], 광물들의 순서(5<=광물개수<=50)
int solution(vector<int> picks, vector<string> minerals) {
    int answer = 0;
    priority_queue<SepMineral> pq;
    queue<int> q;
    
    // 0:dia, 1:iron, 2:stone
    for(int i = 0; i < picks.size(); i++)
    {
        int cur = picks[i];
        for(int j = 0; j < cur; j++)
        {
            q.push(i);
        }
    }
    // 최소의 피로도 구하기
    
    // 광물 순서를 정렬합니다.
    // 광물을 다섯개씩 나눠서 다이아몬드, 철, 돌 순으로 많은 순으로 정렬합니다.
    int temp;
    if((minerals.size()) % 5 != 0)
        temp = 1;
    
    for(int i= 1; i <= minerals.size()/5 + temp; i++)
    {
        int start = (i-1) * 5;
        int end = i * 5;
        if(end > minerals.size())
        {
            end = minerals.size();
        }
        
        vector<string> sliceVec = vector<string>(minerals.begin()+start, minerals.begin()+end);
        pq.push(sliceVec);
    }
    
    // 정렬된 광물 묶음을 곡괭이의 0, 1, 2 인덱스 순서대로 피로도를 계산합니다.
    while(!pq.empty())
    {
        int curPick = q.front();
        q.pop();
        SepMineral curMinerals = pq.top();
        pq.pop();
        
        answer += curMinerals.GetFatigueLevel(curPick);
    }
    
    return answer;
}
```

광물 벡터를 5개씩 붙어진 부분배열로 구조체에 할당하여 우선순위 큐에 담았습니다.

구조체의 연산자를 오버로딩하여 다이아, 철, 돌이 내림차순으로 정렬되게 만들었습니다.

그리고 구조체 내 함수로 곡괭이 종류에 따라서 각 광물들의 점수가 어떻게 계산되어야할지 다르게 구현하여 5개씩 묶여진 광물의 점수를 계산했습니다.

<img src ="https://user-images.githubusercontent.com/80774412/235181942-c877f519-0462-484d-8829-39d59c753e5d.PNG"></img>

