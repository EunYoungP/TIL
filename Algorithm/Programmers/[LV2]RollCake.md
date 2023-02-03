2023.02.03

# __[프로그래머스 LV2] 롤케이크__

std::set

---- 


## __문제__

<img src="https://user-images.githubusercontent.com/80774412/216596088-800c7493-a8ba-486f-804f-0c4a606c79b3.PNG"></img>

<br><br>

## __나의 풀이__

```c#
#include <string>
#include <vector>
#include <set>

using namespace std;

int solution(vector<int> topping) {
    int answer = 0;
    
    int toppingSize = topping.size();
    set<int> base;
    set<int> compare;
    vector<int> count(10000);
    
    if(topping.size() <= 1)
        return 0;
    
    for(int i = 0; i < toppingSize; i++)
    {
        count[topping[i]]++;
        base.insert(topping[i]);
    }
    
    for(int i = 0; i < toppingSize; i++)
    {
        int compareValue = topping[i];
        compare.insert(compareValue);
        
        if(count[compareValue] >= 1)
            count[compareValue]--;
        if(count[compareValue] == 0)
            base.erase(compareValue);
        
        if(base.size() == compare.size())
            answer++;
    }
    return answer;
}
```
 처음 문제를 풀이할떄는, topping 벡터를 임의의 피봇을 반복문으로 증가시키며 배열 두개로 나누어서 중복을 제거하고 매번 토핑의 개수를 비교해줬습니다.

 위의 방법으로 풀이했더니 시간초과가 발생했습니다.

 이후에는 topping 벡터를 매 반복문마다 순회하며 두부분으로 나눠주지 않는 방법을 찾았습니다.

 topping의 모두 토핑의 개수를 담는 base set 컨테이너와
 topping의 모든 토핑의 종류와 각 종류의 개수를 담은 count 벡터,
 그리고 빈 set 컨테이너인 compare를 생성했습니다.

 그리고 topping의 개수만큼 반복하며 

 한번의 반복에서 topping의 값을 하나 추출하여 
 
 compare 에 추가시켜주고, 
 counter 배열에서 해당 토핑값의 인덱스에 담긴 토핑의 개수를 검사하여
 토핑의 중복을 검사했습니다.

 만약 검사해야할 토핑의 개수가 더이상 1보다 작다면 값을 줄여주고 base 에서도 해당 토핑을 제거했습니다.

 base와 compare를 set로 생성하여 매 반복문에서 일일히 중복된값이 들어가거나 삭제되는지 검사할 필요 없이 구현할 수 있었습니다.

이렇게 구현하면 굳이 topping의 실제값을 가지고 삭제하거나 정렬하지 않고 
비교 검사할 수 있었습니다.

## __MAP__

__개념__

* map 컨테이너는 인덱스로 int가 아닌 다른 자료형을 사용할 수 있는 각 노드가 key와 value의 쌍으로 이루어진 트리입니다.

* 검색, 삽입, 삭제 등의 속도를 빠르게 하기 위해 균형 이진 트리 중의 하나인 __'레드 블랙 트리'__ 로 구현되어 있습니다.

* 그 이유는 key를 기준으로 정렬된 상태이기 때문입니다.

__용도__

* 연관 있는 두 값을 함께 묶어서 관리하면서 검색을 빠르게 하고 싶은 경우에 사용합니다.
<br><br>

## __SET__

__개념__

* key만 존재하는 map, 혹은 정렬된 집합 입니다.

__용도__

* 특정 값에 대해 검색을 빠르게 하고 싶은 경우 사용합니다.
<br><br>