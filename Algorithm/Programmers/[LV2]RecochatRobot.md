2023.05.02

# __[프로그래머스 LV2] 리코쳇 로봇__


## __문제__

[리코쳇 로봇](https://school.programmers.co.kr/learn/courses/30/lessons/169199)<br><Br>

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