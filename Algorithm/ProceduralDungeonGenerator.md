23.03.05

# __절차적 맵 생성기 Procedural Dungeon Generator__

프로젝트 '달빛조각사' 에 절차적 랜덤 맵 생성기를 이용한 던전 맵을 추가하기 위해 정리한 문서입니다.<br><br>

- [Part 1. Generator Rooms](#part-1-generator-rooms)
- [Part 2. Separate Rooms](#part-2-separate-rooms)
- [Part 3. Main Rooms](#part-3-main-rooms)
- [Part 4. Delauney Triangulation + Graph](#part-4-delauney-triangulation--graph)
- [Part 5. Minimun Spanning Tree](#part-5-minimun-spanning-tree)
- [Part 6. Hallways](#part-6-hallways)
<br><br>


## __개념__

<img src="https://user-images.githubusercontent.com/80774412/222937475-a637a68d-1f97-4882-9801-a7c600fafc27.gif"></img>

게임에서 던전같은 맵을 랜덤으로 생성할 수 있게 해주는 알고리즘입니다.


## __Part 1. Generator Rooms__

첫 번째로 특정 넓이와 높이를 가진 방들을 원 안의 랜덤하게 위치시켜야 합니다.

게임 플레이에 있어서 방의 크기를 위한 더 많은 파라미터를 설정하는 것이 좋곘지만, 원문 알고리즘에서는 생성될 방의 크기를 평범하게 묘사했습니다.

방의 넓이/길이의 평균과 표준 편차의 비율을 다르게 선정하면 던전은 다른 모습으로 생성됩니다.

이 과정을 위해 필요한 함수 중 하나 __getRandomPointInCircle__ :
```swift
function getRandomPointInCircle(radius)
{
    // 2 * pi * random
    local t = 2 * math.pi * math.random();
    local u = math.random() + math.random();
    local r = nil;
    
    if u > 1 then r = 2 - u 
    else r = u
    end

    return radius *  r * math.cos(t), radius * r * math.sin(t) end
}
```
관련된 자료는 [여기](https://stackoverflow.com/questions/5837572/generate-a-random-point-within-a-circle-uniformly)에서 확인할 수 있습니다.<br><br>

그 결과로 이런 동작들을 실행할 수 있습니다:

<img src="https://user-images.githubusercontent.com/80774412/222945956-a96f0070-244b-4efc-b001-9082a2ad16bb.gif"></img>

고려해야 할 중요한 점은(적어도 개념적으로) 타일 격자를 다루기 때문에 모든 것을 같은 격자에 맞춰야 한다는 것입니다.

위 gif 안의 타일 사이는 4 픽셀이고, 이것은 모든 방의 위치와 사이즈들은 4의 배수임을 의미합니다.

이를 위해서 숫자를 타일 사이즈로 반올림 해주는 함수로 위치와 넓이/높이 할당을 래핑합니다.

```swift
// m의 배수를 반환해 주는 함수
function roundm(n, m)   
    // floor : 정수 반환 함수
    return math.floor(((n + m - 1) / m)) * m
end

// getRandomPointInCircle 함수로 값을 바꿔 반환받을 수 있습니다:
function getRandomPointInCircle(radius)
    ```
    return roundm(radius * r * math.cos(t), tile_size), random(radius * math.sin(t), tile_size)
end
```
<br><Br>

## __Part 2. Separate Rooms__



<br><br>

## __Part 3. Main Rooms__



<br><br>

## __Part 4. Delauney Triangulation + Graph__



<br><br>

## __Part 5. Minimun Spanning Tree__



<br><br>

## __Part 6. Hallways__



<br><br>

----

참고자료

- [Procedural Dungeon Generator](https://github.com/a327ex/blog/issues/7)

- [TinyKeepDev](https://www.reddit.com/r/gamedev/comments/1dlwc4/procedural_dungeon_generation_algorithm_explained/)