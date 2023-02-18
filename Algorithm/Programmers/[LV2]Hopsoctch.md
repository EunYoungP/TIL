
2023.01.31

[2023.02.18 재풀이](#20220218-재풀이)

# __[프로그래머스 LV2] 땅따먹기__

DFS, DP, 메모이제이션

---- 

## __문제설명__

<img src=""></img><br><br>

# __나의 풀이__

```C++
#include <vector>
#include <algorithm>
using namespace std;

void dfs(vector<vector<int>>& land, int curX, int curY, int compareAnswer,int& answer)
{
    curY = curY+1;
    if(land.size() <= curY)
    {
        answer = max(answer, compareAnswer);
        return;
    }
    
    for(int j = 0; j < 4; j ++)
    {
        if(curX == j)
            continue;

        dfs(land, j, curY, land[curY][j]+compareAnswer, answer);
    }
}

int solution(vector<vector<int>> land)
{
    int Ysize = land.size();
    int answer = 0;
    
    for(int i = 0; i < 4; i++)
    {
        dfs(land, i, 0, land[0][i], answer);
    }
    return answer;
}
```

DFS를 사용하여 모든 경우의 수를 재귀함수로 반복하여 답을 구했습니다.

테스트 컴파일에서 모든 테스트를 통과했지만

제출실행에서 모든 테스트케이스가 런타임 오류가 호출됐습니다.

한번 계산한 결과값을 저장을 하지않고 계속 새로 계산해서 런타임 오류가 발생한걸로 파악했습니다.

이를 해결하기위해 __동적프로그래밍__ 과 __메모이제이션__ 을 사용했하여 수정했습니다.

<br><br>

# __동적 프로그래밍__ 

## __1. Top-down 방식__

```c++
public class FibonacciNumberDP {
    private static Integer[] fArr; 
    public static void main(String[] args) {
        fArr = new Integer[46];
        fArr[0] = 0;
        fArr[1] = 1;
        fArr[2] = 1;
        
        int number = getFibonacciNumber(45);
        System.out.println(number);
    }
    
    public static int getFibonacciNumber(int index) {
        if(fArr[index] != null) {
            return fArr[index];
        }
        
        fArr[index] = getFibonacciNumber(index-2) + getFibonacciNumber(index-1);
        return fArr[index];
    }
}
```

'하향식', 큰 문제를 해결하기 위해 작은 문제를 호출하는 방식입니다.

주로 재귀 호출을 통해 답을 구합니다.

함수 호출 횟수를 줄이기 위해 __메모제이션__ 기법이 사용됩니다.<BR><bR>

## __2. Bottom-UP 방식__

상향식 DP를 사용해 구현한 피보나치 수열
```c++
public class FibonacciNumberDP2 {
    private static Integer[] fArr; 
    public static void main(String[] args) {
        fArr = new Integer[46];
        fArr[0] = 0;
        fArr[1] = 1;
        fArr[2] = 1;
        
        for(int i=3; i<46; i++) {
            fArr[i] = fArr[i-2] + fArr[i-1];
        }
        int result = fArr[45];
        System.out.println(result);
    }
}
```

'상향식', 가장 작은 문제들부터 답을 찾아 가면서 마지막에는 전체 문제의 답을 구하는 방식입니다.

주로 반복문을 이용해 구현합니다.

<Br><br>

# __메모이제이션__

개념

* 동일 계산을 반복해야 할 때, 계산값을 메모리에 저장하여 재사용하는 기법으로, 계산속도를 향상시켜줍니다.
<BR><bR>

# __수정 답안__
```C++
#include <vector>
#include <algorithm>
using namespace std;

int solution(vector<vector<int>> land)
{
    int Ysize = land.size();
    int answer = 0;

    for(int i = 1; i < Ysize; i++)
    {
        land[i][0] += max({land[i-1][1],land[i-1][2],land[i-1][3]});
        land[i][1] += max({land[i-1][0],land[i-1][2],land[i-1][3]});
        land[i][2] += max({land[i-1][0],land[i-1][1],land[i-1][3]});
        land[i][3] += max({land[i-1][0],land[i-1][1],land[i-1][2]});
    }
    answer = max({land[Ysize-1][0],land[Ysize-1][1],land[Ysize-1][2],land[Ysize-1][3]});
    
    return answer;
}
```
첫번째 풀이에서 하나의 노드에서 깊은 검색으로 모든 경우의 수를 검색하여 마지막에 값을 비교하여 답을 구했습니다.

하지만 윗 단계에서 미리 최대값을 비교하여 값을 저장하며 검색한다면

윗 단계의 계산을 다시 반복할 필요가 없어집니다.

이렇게 land의 마지막 열에 각 행이 가질 수 있는 최대값이 저장되고

반복문이 끝나고 마지막 열의 값들중 최대값을 추출하여 답을 구할 수 있습니다.

즉, 반복문을 작은 부분부터 계산하여 값을 저장하는 상향식 DP를 사용했습니다.
<BR><BR>

# __DFS DP 차이점__

__DFS__ 는 그래프 탐색 기법입니다.

__DP__ 는 어떤 문제를 더 작은 문제의 연장선으로 생각하고 큰 문제를 해결하는 방식을 의미합니다.

DP를 사용하려면 문제가 재귀적인 점화식으로 해결이 되는지 판별해야합니다.

재귀적인 점화식의 가장 기본적인 조건은 재귀가 종료되는 __Ground Case__ 가 존재해야 한다는 것입니다.<BR><BR>

# __2022.02.18 재풀이__

```c++
#include <vector>
#include <algorithm>
using namespace std;

void dfs(vector<vector<int>> land, vector<vector<int>>& value, int x, int y)
{
    if(y >= land.size()-1)
        return;
    
    int cur = value[y][x];
    for(int i = 0; i < 4; i++)
    {
        if(i == x)
            continue;
        
        int nValue = cur + land[y+1][i];
        value[y+1][i] = max(value[y+1][i], nValue);
        
        dfs(land, value, i, y+1);
    }
    return;
}

int solution(vector<vector<int> > land)
{
    int answer = 0;
    
    int landX = land[0].size();
    int landY = land.size();
    
    vector<vector<int>> value(landY,vector<int>(landX,0));
    value[0][0] = land[0][0];
    value[0][1] = land[0][1];
    value[0][2] = land[0][2];
    value[0][3] = land[0][3];
    
    dfs(land, value, 0, 0);
    dfs(land, value, 1, 0);
    dfs(land, value, 2, 0);
    dfs(land, value, 3, 0);
    
    answer = max({value[landY-1][0],value[landY-1][1],value[landY-1][2],value[landY-1][3]});
    
    return answer;
}
```
이번 재풀이 때 동적 프로그래밍 풀이 방법보다 DFS 방법으로 풀이해야겠다고 생각하게 되었고, 처음과 같은 풀이로 시간 초과의 결과를 마주했습니다.

이것은 문제풀이 경험의 부족으로 판단되어 DP 와 DFS 문제를 구분하여 풀이는 연습이 필요하다고 느꼈습니다.

DFS 로 풀이 시, 하나의 행에 4개의 노드의 최대 값을 구하는 연산의 수는

X값이 겹치지 않는 이전 행의 3가지 값을 4개의 노드를 반복하며 비교하기 때문에 12 가지가 됩니다.

하지만 DP bottom-up 방식의 풀이로는 하나의 행에 필요한 연산은,

각 노드에 X가 겹치지 않는 이전 행의 모든 값의 비교를 max함수로 한번 연산하기 때문에 행의 노드의 개수인 4 가지가 됩니다.










