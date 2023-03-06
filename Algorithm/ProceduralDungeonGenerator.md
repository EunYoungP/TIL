23.03.05

# __절차적 맵 생성기 Procedural Dungeon Generator__

프로젝트 '달빛조각사' 에 절차적 랜덤 맵 생성기를 이용한 던전 맵을 추가하기 위해 정리한 문서입니다.<br><br>

- [Part 1. Generator Rooms](#part-1-generator-rooms)
- [Part 2. Separate Rooms](#part-2-separate-rooms)
- [Part 3. Main Rooms](#part-3-main-rooms)
- [Part 4. Delaunay Triangulation + Graph](#part-4-delaunay-triangulation--graph)
- [Part 5. Minimun Spanning Tree](#part-5-minimun-spanning-tree)
- [Part 6. Hallways](#part-6-hallways)
- [END](#end)
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

많은 방의 메쉬들이 하나의 공간속에 존재하고, 방들은 어떻게든 겹치지 않아야 합니다.

TKdev 는 분리 조향 동작을 사용했습니다. 그러나 이것보다 쉽게 물리 엔진만을 사용하는 방법이 있습니다.

모든 방들을 추가한 후, 간단하게 각 방의 위치에 단단한 물리 바디를 매치 시키고,

모든 물리 바디들이 동작을 멈출때까지 시뮬레이션을 실행시킵니다.

아래 gif 에서 시뮬레이션을 정상적으로 실행시키고 있지만, 레벨 사이에서 이 작업을 실행하면 물리들의 시뮬레이션을 더 빠르게 진행할 수 있습니다.

<img src="https://user-images.githubusercontent.com/80774412/222975250-0cbc7a8c-4b21-4c89-82b1-6b93e803fd8d.gif"></img>

물리 본체를 타일 그리드에 연결하지 않아도 방의 위치를 설정할 때 __roundm__ 호출로 랩핑하면,

타일 격자를 존중하면서 서로 겹치지 않는 방들을 얻을 수 있습니다.

아래 gif 에서 물리 바디들을 나타내는 파란 라인들이 상황안에서 이런 과정들을 보여줍니다.

<img src="https://user-images.githubusercontent.com/80774412/222976278-71ee811d-618d-4101-836a-a981b350b355.gif"></img>

파란색 아웃라인들은 항상 조금씩 어긋나 있고, 방들의 위치는 항상 반올림됩니다.<br><Br>

방을 생성하길 원할 때 한가지 이슈를 마주할 수 있는데,

수평적인지 수직적인지 정확하지 않을 경우 입니다.

예를 들어, 작업 중인 게임을 생각할 때:

<img src="https://user-images.githubusercontent.com/80774412/222976304-af60c73a-1e72-4a06-b083-e9ed98d166db.gif"></img>

전투가 매우 수평적이라면 생성되길 원하는 대부분의 방들이 높이 보다 폭이 큰 것을 원할 것입니다.

아래 gif 처럼 처음 방들을 스폰할 때 원이 아닌 얇은 스트립 내부에 방을 생성함으로써 던전 자체의 너비와 높이 비율이 적절해집니다.

<img src="https://user-images.githubusercontent.com/80774412/222979960-68dd4016-c1d5-4a82-b224-242e361673fd.gif"></img>

스트립 내부에 랜덤으로 스폰하기 위해서 __getRandomPointInCircle__ 함수를 타원 속에 포인트들을 스폰하는 것으로 변경합니다.

```swift
// 타원의 높이, 넓이를 인수로 받아 반올림한 값을 반환하는 함수입니다.
function getRandomPointInEllipse(ellipse_width, ellipse_height)
    local t = 2 * math.pi * math.random()
    local u = math.random() + math.random()
    local r = nil
    
    if u > 1 then r = 2 - u 
    else r = u
    end

    return roundm(ellipse_width * r * math.cos(t)/2, tile_size),
           roundm(ellipse_height * r * math.sin(t)/2, tile_size),
```

위 gif 의 ellipse_width = 400 이고 ellipse_hieght = 20 입니다.

<br><br>

## __Part 3. Main Rooms__

다음 단계는 간단하게 어떤 방들이 메인/허브 방들일지 아닐지 결정합니다.

여기서 TVdev 의 접근은 매우 견고합니다. 너비/높이 임계값보다 높은 방들을 선택하기만 하면 됩니다.

아래 gif의 경우 사용된 임계값은 __1.25 * 평균__ 이므로, 만약 __너비 평균__ 과 __높이 평균__ 들이 24 라면 30 보다 큰 너비와 높이의 방들이 선택될 것입니다.

<img src="https://user-images.githubusercontent.com/80774412/222981058-523e7fe4-1f84-4212-bb41-76f4af23559d.gif"></img>

<br><br>

## __Part 4. Delaunay Triangulation + Graph__

이제 선택한 방들의 중간점들을 가져와 Delaunay 절차에 전달합니다.

이 절차는 스스로 구현하거나 누군가 공유한 성공 소스코드를 찾을 수 있습니다.

[Yonaba](https://github.com/Yonaba/delaunay) 에서 이미 이것을 구현했습니다.

아래 인터페이스에서 볼수 있듯이, 정점들을 전달받아 삼각형들을 만들어줍니다.

<img src="https://user-images.githubusercontent.com/80774412/222981389-f329208b-e3d4-4762-bc2c-501e5d74ac26.gif"></img>

삼각형이 있으면 그래프를 생성할 수 있습니다.

그래프 자료구조/라이브러리 를 가지고 있는 경우 이 절차는 매우 간단합니다.

이것을 미리 가지고 있지 않은 경우, 방 객체/구조물에 고유한 ID를 가지고 있어 복사할 필요 없이 그래프에 ID를 추가할 수 있으므로 유용합니다.

<br><br>

## __Part 5. Minimun Spanning Tree__

그 후 __최소 스패닝 트리__ 를 그래프로부터 생성합니다.
 
전 단계와 같이 직접 구현하거나  사용하는 언어로 누군가 구현한 정보를 찾을 수 있습니다.

<img src="https://user-images.githubusercontent.com/80774412/222983248-75dc9000-fa2c-4996-95ec-0365be19a2a6.gif"></img>

최소 스패닝 트리는 던전의 모든 메인 룸을 도달할 수 있게 보장하지만 이전처럼 모두 연결되어있지 않도록 합니다.

우리는 대부분 완전히 연결되어 있거나 도달할수 없는 섬이 존재하는 던전을 원하지 않기 때문에 최소 스패닝 트리는 유용합니다.

하지만 던전이 오직 하나의 선형적 경로를 가지는 것 또한 원하지 않기 때문에

몇개의 간선들을 Delaunay graph 에서 다시 추가해줍니다:

<img src="https://user-images.githubusercontent.com/80774412/222983655-7a0c5c56-9c4a-4f9a-b7ba-bcfa04baf8b8.gif"></img>

이 과정은 몇개의 경로들과 루프들을 더 추가하게 해주고 던전을 더욱 흥미롭게 만들어줍니다.

TKdev 는 15% 의 간선들을 뒤에 추가해줬고, 위 gif는 8-10% 의 결과입니다.

원하는 던전의 최종 경롤연결 형태에 따라 달라질 수 있습니다.

<br><br>

## __Part 6. Hallways__

마지막으로 던전에 복도를 추가해줍니다.

그래프의 각 노드를 통과하고 연결된 각 노드의 사이에 선을 생성합니다.

만약 노드들이 수평적으로 가깝다면 (y 위치가 비슷하다면) 수평적인 선을 생성합니다.

만약 노드들이 수직적으로 가깝다면 수직적 선을 생성합니다.

만약 노드들이 수평적 또는 수직적으로 모두 가깝지 않다면 L 형태의 두개의 라인을 생성합니다.<BR><bR>

가까운 정도의 평균 기준에 대한 예시로는,

두 노드의 위치 사이의 중간점을 계산하고 해당 중간점의 x 또는 y 값이 노드의 경계 안에 있는지 확인합니다.

만약 그렇다면 그 중간점의 위치에서 선을 만듭니다.

존재하지 않는다면 소스의 중간점에서 타겟의 중간점까지 하나의 축에서만 두개의 선을 만듭니다.

<img src="https://user-images.githubusercontent.com/80774412/223135240-2b8e919b-9fc3-4312-a008-a2558d326022.png"></img>

위 사진에서 예시들을 볼 수 있습니다.

노드 62 와 47 은 서로의 사이에 수평적인 라인을 가지고 있습니다.

노드 60 과 125 는 수직적인 라인을 가지고 있고 

노드 118 과 119 는 L 형태의 라인을 가지고 있습니다.

선을 생성할 때 내가 만들고 있는 유일한 라인이 아니라는 점을 유의해야 합니다.
<BR><bR>

이후 각 라인과 충돌하는 메인/허브 방이 아닌 방들을 검색합니다.

검색된 방들은 복도의 골격 역할을 하는 구조가 됩니다.

<img src="https://user-images.githubusercontent.com/80774412/223136691-11e33a4f-869d-4c33-8b3f-d2a147e5ffb2.png"></img>

처음에 설정한 방의 균일성과 크기에 따라 다른 모양의 던전을 만들게 됩니다.

복도가 더 균일하고 덜 이상하게 보이도록 하려면 낮은 표준 편차를 목표로 해야합니다.

또한 방이 너무 비좁지 않도록 몇 가지 점검을 해야 합니다.
<BR><bR>

마지막 단계에서는 누락된 복도를 구성하기 위해 1개의 타일 크기의 그리드 셀을 추가하면 됩니다.

실제로 구조나 너무 멋진 것은 필요하지 않습니다.

타일 크기에 따라 각 라인을 살펴보고 일부 목록에 그리드 둥근 위치를 추가할 수 있습니다. 

예를 들면 1개가 아닌 3개의 라인이 생성되는 곳인 경우입니다.

<img src="https://user-images.githubusercontent.com/80774412/223139361-0125b216-e3ac-4779-8cab-70f321ff9348.png"></img>

<br><br>

## __END__

전체 절차에서 반환한 데이터 구조는 다음과 같습니다.


방 목록
- 방(고유 ID, x/y 위치, 너비/높이)

그래프
- 각 노드가 방 ID를 가리키고 가장자리가 타일의 방 사이의 거리를 갖습니다.

그리드
- 메인/허브 방, 복도 방, 복도 셀 을 가리킬 수 있습니다.

이를 이용하여 문, 적, 항목, 보스가 있어야 할 방 등을 배치할 위치를 파악할 수 있습니다.

----

참고자료

- [Procedural Dungeon Generator](https://github.com/a327ex/blog/issues/7)

- [TinyKeepDev](https://www.reddit.com/r/gamedev/comments/1dlwc4/procedural_dungeon_generation_algorithm_explained/)