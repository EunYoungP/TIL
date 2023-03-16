2023.03.15

# 메모리 최적화를 위한 에셋 관리

Unity Korea 23년 3월 알쓸유잡

[UnityKorea 유튜브](https://www.youtube.com/watch?v=1e2mSCS7o1A)

----

[UI 퍼포먼스 최적화 팁]

## __목차__


- Unity UI 구조

- 

<BR><bR>

<img src="https://user-images.githubusercontent.com/80774412/225233572-f873759f-8883-44d6-938b-f20636ad1c72.PNG"></img>

## __UGUI의 병목__

Unity 도 GPU 바운드가 생길 수 이쓴ㄴ데 플레이트 라고합니다.

너무 중첩되어 그려져서 오버드로우가 발생하는 등의 경우입니다.

UI를 만들다 보면 GPU 보다는 CPU 병목인 경우가 많습니다.

e.g. 배치, 드로우콜, 데이터가 동적이다 보니 재구축 하는데 드는 시간<BR><bR>

사실 UI는 GPU가 렌더링 하는 것이기 때문에 버텍스가 필요합니다.
<BR><bR>


## __Unity UI 구조__

슈퍼카를 유니티라고 생각한다면 엔진은 C++ 이고

조향장치 등은 C# 으로 만들어져 있습니다.

C++ 영역은 오픈 소스는 아니고 C# 영역은 오픈 소스로 공개되어 있습니다.

이 코드를 보며 알아보겠습니다.<BR><bR>

_참고로 규모가 큰 게임 회사들은 협약이 되어있어 유니티의 C++ 코드도 볼 수 있습니다._ <BR><bR>


UI 역시 메시로 이루어져 있습니다. 즉 많은 사각형 평면들의 조합이라고 할 수 있습니다.

메쉬는 데이터의 조합입니다.

<img src="https://user-images.githubusercontent.com/80774412/225234389-28b64b39-7d79-4ada-8f69-c39e295b3341.PNG"></img>

점 -> 선 -> 삼각형 -> 메쉬

<img src="https://user-images.githubusercontent.com/80774412/225234404-764801e8-91c9-4636-9bff-e440cedf3ba9.PNG"></img>

이렇게 만들어진 삼각형으로 3D 오브젝트도 만들고, UI도 만들 수 있습니다.

<img src="https://user-images.githubusercontent.com/80774412/225235398-47d85682-b30d-47b6-8a74-01e4a660e1e9.PNG"></img>

UI 는 동적으로 메쉬가 계속 변동됩니다. 즉 매 프레임 마다 CPU가 연산하여 GPU 메모리로 넘겨주는 과정을 반복하여 연산 비용이 발생합니다.

따라서 대부분 경우 UI에서의 병목은 CPU 병목 인 경우가 많습니다.<BR><bR>

## __Graphic(.cs)__

기본 제공 클래스입니다.

Image 클래스는 MaskableGraphic 을 상속 받고, MaskabpleGraphic 은 Graphic 클래스를 상속받습니다.

Image : MaskableGraphic : Graphic

렌더링 주체는 캔버스지만, 그래픽적 요소를 다루기위한 것들은 Graphic 클래스에 모여있습니다.<br><br>

<img src="https://user-images.githubusercontent.com/80774412/225236578-0fb0bc2d-dcf4-4761-8395-a226423fcce8.PNG"></img>

위 이미지는 Vertex Buffer와 Index Buffer 부분을 코드로 보여줍니다.<br><Br>

## __Canvas(.cpp)__

이미지가 그래픽적 요소를 담고있다면 렌더링 하는것은 Canvas 클래스입니다.

VertextBUffer, IndexBuffer 모두 캔버스가 가지고 있습니다. 캔버스 단위로 배칭을 하게됩니다.

캔버스 하위에 있는 이미지가 바뀌면 캔버스가 가지고 있는 VertexBuffer를 변경해야 합니다.

캔버스에 자식 캔버스를 가지고 있을 수 있기 떄문에 재귀적으로 RanderOverlays 를 호출합니다.

아니라면 DrawRaMesh 함수에서 버텍스버퍼와 인덱스버퍼를 넘겨줍니다.

즉 중요한 점은 캔버스가 렌더링의 주체라는 것입니다.<bR><bR>

Q. 캔버스의 개수가 많아지면? 

완벽한 정답은 없지만, 규모가 제한적인 씬이라면 실시간 동적 라이팅을 줄이고 스태틱 배칭, 오클루전 컬링, 라이팅 맵 등을 쓴다 라는 등의 답이 있지만

캔버스를 나누는것을 권장하는것은 동적인 캔버스와 정적인 캔버스를 나누라는 것입니다.

하지만 이것 또한 갱신비용, 드로우콜 비용 때에 따라 다르게 구현해야 합니다.<BR><bR>

## __Nested Canvas__

캔버스 구성에서 하위에 캔버스를 소유 가능한 기법

배경 프레임에 버튼이 있다면, 버튼은 동적으로 변합니다.

그렇다면 배경과 버튼에 캔버스로 나눌 수 있는데, 

둘 다 부모 캔버스로 하기보다는 NestedCanvas 기법을 이용하여 구현하기를 권장합니다.<br><Br>


## __Dirty flag__

UI는 지속적인 갱신이 이슈이기 떄문에 Dirty flag 구조를 많이 사용합니다.

image.png

<br><Br>

## __RectTransform(.cpp)__
 
Transform 이 있어 계층 구조가 가능합니다.

RectTrnasform::Transform

유니티의 게임 오브젝트에는 Transform을 기본적으로 가지고있습니다.

UI는 그대신 RectTransform이 기본으로 가지고있는데 이는 RectTrasfrom이 Transform을 상속받고 있기 때문입니다.

계층 구조는 연산이 많이 필요한 구조입니다.

따라서 계층 구조가 깊을수록 연산이 오래 걸립니다.<Br><Br>

또한 Re-parenting 비용이 발생합니다.

계층 변경을 한다는 것으로 보면되는데, 부모가 바뀌는것을 말합니다.

아래 세가지 이벤트가 실행됩니다.

계층 구조는 탐색을 효율적으로 하기위해 메모리를 어느정도 연결시켜 줍니다.

<img src="https://user-images.githubusercontent.com/80774412/225241605-4377953f-4dca-4066-afb7-6288c6f22953.PNG"></img>

<br><Br>

## __Rebuild__

컴포넌트의 레이아웃, 메쉬를 기반으로 dirty 체크가 되어있는 부분을 다시 계산을 해줍니다.

레이아웃 요소가 아닌 그래픽적 요소만 바뀐다면 버텍스적 요소만 바뀌면 됩니다.

또는 스프라이트가 바뀌었지만 크기는 같고 이미지만 바뀐 경우라면, 버텍스를 건드리지 않아도 됩니다.

페이드 인-아웃 기능을 위한 화면을 덮는 이미지는 알파값이 0 이여도 비용이 발생합니다.

셰이더, 머티리얼에 따라 배칭이 캔버스에서 나뉠수 있습니다. 즉 배칭 요소를 캔버스가 하나라도 신경을 써야 합니다.

<img src="https://user-images.githubusercontent.com/80774412/225242676-ae67af6f-a72a-4765-aef5-170ecacccfdc.PNG"></img>

<br><Br>

## __Batch bhilding (Canvas)__

캔버스도 메쉬고 배칭을 처리합니다.

캔버스가 dirty로 설정이 안되어있고, 한번 배칭 데이터를 만들면 CPU연산이 다시 필요없이 GPU에서만 연산이 일어나면 됩니다.

_이 이유에서 동적요소들과 정적요소들의 캔버스를 나누는것을 권장하는 이유입니다._

데스크탑 / 랩탑 에 따라 용도가 무엇이냐에 따라 타켓 피시에서 프로파일링하는것이 중요합니다.
<BR><bR>

## __BATCHING__

### __배칭 기준__

- 동일한 캔버스

- 같은 머티리얼, 스프라이트 에셋

- 동일한 Z깊이의 RectTransform

    _z깊이가 다르면 배칭이 꺠집니다._

- 동일한 마스크 적용
<br><Br>


