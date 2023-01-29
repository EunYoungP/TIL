2023.01.29

# __[프로그래머스 LV2] 압축__


---- 


## __문제__
<img src = "https://user-images.githubusercontent.com/80774412/215331142-27d386f0-8a4f-44cc-9f49-fa0171f45078.PNG"></img>

<img src = "https://user-images.githubusercontent.com/80774412/215331144-d88d754b-c984-4c1c-8934-21e0ea35464a.PNG"></img><br><Br>


## __풀이__
```c++
#include <string>
#include <vector>
#include <map>

using namespace std;

bool CheckInDic(map<int, string>& myDic, string str, int& answer)
{
    map<int, string>::iterator it;
    for(it = myDic.begin(); it != myDic.end(); it++)
    {
        if(it->second.compare(str) == 0)
        {
            answer = it->first;
            return true;
        }
    }
    return false;
}

vector<int> solution(string msg) {
    vector<int> answer;
    map<int, string> myDic;

     // 유니코드 65~90 = A~Z
     // 반복문 밖으로 선언하여 값이 유지되게 했습니다. 
    int index;
    for(index = 1; index <= 26; index++)
    {
        myDic[index] = index+64;
    }
    
    string compareStr;
    int containIndex;
    for(int i = 0; i < msg.size(); i++)
    {
        compareStr = msg[i];
        bool isInDic = CheckInDic(myDic, compareStr, containIndex);
        
        while(isInDic)
        {
           i++;
           compareStr += msg[i];
           isInDic = CheckInDic(myDic, compareStr, containIndex);
            
            if(!isInDic)
            {
                myDic[index++] = compareStr;
                i--;
                answer.push_back(containIndex);
            }
        }
    }
    return answer;
}
```
문자열을 검색하는 사전을 __map__ 으로 구현했습니다.

유니코드 65~90 까지 대문자인점을 이용해 순차대로 map에 추가하고,

index를 유지하여 다음 추가되는 문자열의 키값으로 사용했습니다.<br><br>


__STL::map__
 
각 노드가 key, value 쌍으로 이루어진 트리입니다.

검색, 삽입, 삭제가 O(logn)인 __레드블랙트리__ 로 구성되어 있습니다.

자료를 저장할때 key값의 오름차순으로 자동 정렬합니다.

* 검색 map.find(검색키값) != map.end()

* 삽입 map.insert 
    - 값이 중복되면 삽입되지 않습니다.

* 삭제  map.erase(map.begin() + 1);             특정 위치 요소 삭제
        map.erase(key값);                       키값을 기준으로 특정 요소 삭제
        map.erase(map.begin(), map.end());      모든 요소 삭제
        map.clear();                            모든 요소 삭제

__make_pair__

* pair 클래스
    
    두 객체를 하나의 객체로 취급할 수 있게 묶어주는 클래스입니다.

    STL에서 '쌍'을 표현할때 자주 사용됩니다.

    utility 헤더에 존재합니다.

template<class T1, class T2> 
pair<T1,T2> make_pair(T1 x, T2 y)
{
    return (pair<T1,T2>(x, y));
}

make_pair 는 x와 y가 들어간 pair를 만들어줍니다.