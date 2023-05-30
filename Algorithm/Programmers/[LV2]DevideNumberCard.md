2023.05.24

# __[프로그래머스 LV2] 숫자 카드 나누기__

----

## __문제__

[숫자 카드 나누기](https://school.programmers.co.kr/learn/courses/30/lessons/135807)<br><Br>


## __나의 풀이__
```c++
#include <string>
#include <algorithm>
#include <vector>
#include <iostream>

using namespace std;

// 최대공약수
int GCD(int a , int b)
{
    int c = 0;
    while(b!= 0)
    {
        c = a % b;
        a = b;
        b = c;
    }
    return a;
}

// 최대공약수의 약수들을 구합니다.
void GetDevisor(int num, vector<int>* devisor)
{
    for(int i = 1; i < num/2; i++)
    {
        if(num % i == 0)
        {
            devisor->push_back(i);
            if((num / i) != i)
                devisor->push_back(num / i);
        }
    }
}

int solution(vector<int> arrayA, vector<int> arrayB) {
    
    // 각 배열이 모두 나눠지는 경우,
    // 각 배열이 모두 나눠지지 않는 경우
    // 패스됩니다.
    
    // 각 배열 중 하나만 나눠지는 경우 -> 조건 충족
    // 조건을 충족하는 자연수 중 큰 것부터 검사하기위해 최대공약수를 구합니다.
    
    // 각 배열이 모두 나누어지려면 가장 작은 수보다 작은 값이어야 합니다.
    // 즉 범위는 2 ~ 각 배열의 최소값
    // 각 배열의 최대공약수의 약수면서, 범위안에 들어가는 숫자
    
    // 가장 큰 양의 정수 a를 구합니다.
    int answer = 0;
    int aGCD = arrayA[0];
    int bGCD = arrayB[0];
    
    vector<int> devisorA;
    vector<int> devisorB;
    
    // 주어진 두 배열 내림차순 정렬
    sort(arrayA.begin(), arrayA.end(), greater<int>());
    sort(arrayB.begin(), arrayB.end(), greater<int>());
    
    // 각 배열의 최대공약수 구하기
    for(int i = 1; i < arrayA.size(); i++)
    {
        aGCD = GCD(aGCD, arrayA[i]);
        bGCD = GCD(bGCD, arrayB[i]);
    }
    
    // 각 최대공약수의 공약수들 구하기
    GetDevisor(aGCD, &devisorA);
    GetDevisor(bGCD, &devisorB);
    
    // 공약수들 내림차순 정렬
    sort(devisorA.begin(), devisorA.end(), greater<int>());
    sort(devisorB.begin(), devisorB.end(), greater<int>());
    
    bool isDevide = false;
    int minB = arrayB[arrayB.size()-1];
    int minA = arrayA[arrayA.size()-1];
    
    // 조건을 만족하지 않는 경우
    
    for(int i = devisorA.size()-1; i >= 0; i--)
    {
        isDevide = false;
        int cur = devisorA[i];
        for(int b : arrayB)
        {
            if((b % cur) == 0)
            {
                isDevide = true;
                break;
            }
        }
        if(isDevide == false && answer < cur)
        {
            answer = cur;
        }
    }
    
    for(int i = devisorB.size()-1; i >= 0; i--)
    {
        isDevide = false;
        int cur = devisorB[i];
        for(int a : arrayA)
        {
            if((a % cur) == 0)
            {
                isDevide = true;
                break;
            }
        }
        if(isDevide == false && answer < cur)
        {
            answer = cur;
        }
    }
    return answer;
}
```

최대공약수를 구하여 나머지를 비교하는 방법으로 풀이했습니다.

답으로 쓰려는 값은 무조건 하나의 배열을 모두 나눌 수 있어야 합니다.

각 배열의 따라서 최대공약수를 구해줍니다. 그리고 그 최대공약수의 약수들도 해당 배열을 모두 나눌 수 있는 수들이기 때문에 구해줍니다.<br><Br>

그리고 구해진 배열의 공약수들의 묶음이 나머지 다른 배열의 모든 요소들을 나눌 수 없는 수여야합니다.

최대값을 구해야 하기 때문에 각 숫자카드 배열과 공약수들 배열을 모두 내림차순으로 정렬합니다.

그리고 공약수들을 순회하면서 다른 숫자카드 배열을 순회하며 모두 나눌 수 없는지 검사합니다.<br><BR>

만약 나눌 수 있는 수가 나온다면 다음 공약수를 검색합니다.

조건에 만족하는 공약수가 나온다면 answer과 비교하여 더 크면 answer에 값을 할당해줍니다.<br><Br>

이렇게 두개의 배열을 모두 검사해주면 최대값인 answer이 결과로 도출됩니다.



