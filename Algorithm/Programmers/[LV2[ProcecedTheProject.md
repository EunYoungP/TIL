2023.04.26

# __[프로그래머스 LV2] 과제 진행하기__


## __문제__

[과제 진행하기](https://school.programmers.co.kr/learn/courses/30/lessons/176962)<br><Br>

## __나의 풀이__
```c++
#include <string>
#include <vector>
#include <stack>
#include <algorithm>
#include <queue>

using namespace std;

// plans의 값들을 할당할 구조체입니다.
struct Node
{
    string name;
    int time;
    int runTime;

    // 시간은 모두 int로 형변환해 할당하는 생성자입니다.
    Node(string _name, string _time, string _runTime)
    {
        name = _name;
        time = stoi(_time.substr(0,2))*60 + stoi(_time.substr(3,2)); 
        runTime = stoi(_runTime);
    }

    // int 형 인자는 그대로 전달됩니다.
    Node(string _name, int _time, int _runTime)
    {
        name = _name;
        time = _time;
        runTime = _runTime;
    }

    // 시작 시간을 기준으로 오름차순
    bool operator< (const Node& other) const
    {
        return time > other.time;
    }
};

// 두개의 문자열 시간 비교
bool compare(vector<string> a, vector<string> b)
{
    int atime = stoi(a[1].substr(0,2)) * 60 + stoi(a[1].substr(3,2));
    int btime = stoi(b[1].substr(0,2)) * 60 + stoi(b[1].substr(3,2));
    
    return atime < btime;
}

bool CompareTime(string a, string b)
{
    int aTime = stoi(a.substr(0,2))*60 + stoi(a.substr(3,2));
    int bTime = stoi(b.substr(0,2))*60 + stoi(b.substr(3,2));
    
    return a > b;
}

// 시간표현 a에 b만큼의 시간을 더한 문자열 표현값 반환
string GetTime(string a, string b)
{
    int time = stoi(a.substr(0,2));
    int min = stoi(a.substr(3,2));
    
    time += (min + stoi(b))/60;
    min += (min + stoi(b)) % 60;
           
    string temp = to_string(time) + ":" + to_string(min) ;
    return temp;
}

// 남은 시간 구하기
int GetRemainingTime(string a, string b)
{
    int atime = stoi(a.substr(0,2)) * 60 + stoi(a.substr(3,2));
    int btime = stoi(b.substr(0,2)) * 60 + stoi(b.substr(3,2));
    
    return atime - btime;
}

vector<string> solution(vector<vector<string>> plans) {
    vector<string> answer;
    
    priority_queue<Node> pq;
    stack<Node> waitStack;

    for(int i = 0; i < plans.size(); i++)
    {
        pq.push(Node(plans[i][0], plans[i][1], plans[i][2]));
    }

    int curTime = 0;
    while(!pq.empty())
    {
        Node curNode = pq.top();
        //  미룬 과제가 있는 경우
        if(!waitStack.empty())
        {
            Node waitNode = waitStack.top();
            waitStack.pop();
            // 현재 과제의 시작시간이 미룬과제의 끝나는 시간보다 같거나 작다면 
            if(curNode.time > curTime)
            {
               pq.push(Node(waitNode.name, curTime, waitNode.runTime));
               continue;
            }
            else
            {
                waitStack.push(waitNode);
            }

        }
        // 미룬 과제가 없는 경우
        pq.pop();
        curTime = curNode.time + curNode.runTime;

        Node nextNode = pq.top();

        if(curTime <=  nextNode.time)
        {
            answer.push_back(curNode.name);
        }
        // 중복되는 과목이 있는 경우
        else
        {
            waitStack.push(Node(curNode.name, curNode.time, curNode.runTime - (nextNode.time - curNode.time)));
            curTime = nextNode.time;
        }
    }

    while(!waitStack.empty())
    {
        Node waitNode = waitStack.top();
        waitStack.pop();
        answer.push_back(waitNode.name);
    }
    return answer;
}
```

미룬 과제들은 나중에 담긴 순으로 다시 불러와야 하기 때문에 스택을 사용해서 담았습니다.

그리고 과제들을 담아둔 벡터는 랜덤으로 담겨있기 때문에 시작시간을 기준으로 정렬했습니다.<br><br>

## __참고 풀이__

```c++
struct Node
{
    string Name;
    int Time;
    int PlayTime;

    Node(string a, string b, string c)
    {
        Name = a;
        int Hour = (CharToInt(b[0]) * 10 + CharToInt(b[1])) * 60;
        Time = Hour + (CharToInt(b[3]) * 10 + CharToInt(b[4]));
        PlayTime = stoi(c);
    }

    Node(string a, int b, int c)
    {
        Name = a;
        Time = b;
        PlayTime = c;
    }

    int CharToInt(char c)
    {
        return c - '0';
    }

    bool operator< (const Node& Other) const
    {
        return Time > Other.Time;
    }
};

// 메인
vector<string> solution(vector<vector<string>> plans) {
    vector<string> answer;

    priority_queue<Node> pq;
    stack<Node> st;

    for(int i = 0; i < plans.size(); i++)
        pq.push(Node(plans[i][0], plans[i][1], plans[i][2]));

    int CurrentTime = 0;
    while(!pq.empty())
    {
        Node node = pq.top();
        if(!st.empty()) /* 미뤄둔 과제가 있는 경우 */
        {
            Node STnode = st.top();
            st.pop();
            if(node.Time > CurrentTime)
            {
                pq.push(Node(STnode.Name, CurrentTime, STnode.PlayTime));
                continue; // 스택에 있는 과제를 pq에 옮겨 담고 재연산
            }
            else
                st.push(Node(STnode.Name, CurrentTime, STnode.PlayTime)); // CurrentTime을 다시 셋업 해주기 위해 일부러 pop을하고 push를 한다.
        }

        pq.pop();        
        Node NextNode = pq.top();    
        CurrentTime = node.Time + node.PlayTime;

        if(CurrentTime <= NextNode.Time)
            answer.push_back(node.Name);           
        else
        {
            CurrentTime = NextNode.Time;
            st.push(Node(node.Name, node.Time, node.PlayTime - (NextNode.Time - node.Time)));
        }
    }

    while(!st.empty())
    {
        Node STnode = st.top();
        st.pop();
        answer.push_back(STnode.Name);
    }

    return answer;
}
```

참고 풀이에서는 우선순위 큐를 이용하여 문제를 해결했습니다.

우선순위 큐의 연산자 오버라이딩을 통해서 과제 시작 시간 오름차순으로 정렬이 가능합니다.

