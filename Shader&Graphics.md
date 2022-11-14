# 2022 Shader & Graphics
# 유니티 셰이더 & 렌더링 에센스<br><br>


## :blush: 유튜브 레트로 채널의 셰이더&렌더링 강의 정리 문서입니다.<br><br>


## :cat: 강의 유튜브 주소
:point_right: [유튜브 이동](https://youtu.be/4iSJW7YGrjY?list=PLctzObGsrjfyWa2CaxGtxsLD-W5zYC2JJ)<br><br>

 
## :blue_book: 강의소개
:point_right: 
이 강의는 셰이더와 렌더링 프로그래밍을 최고로 쉽고 정확하게 설명합니다. 

* 셰이더란 무엇인가? 

* 렌더링 파이프라인 : 그래픽스 API 초기화와 드로우 콜

* 렌더링 파이프라인 : 정점 조립과 버텍스 셰이더의 변환행렬

* 렌더링 파이프라인 : 래스터라이저와 프래그먼트 셰이더

* 유니티로 셰이더 작성하기


## :page_with_curl: 강의 정리
<br>

__Part 1 셰이더__


* 화면에 색을 칠하는(Shading) 프로그램입니다.

* 그래픽스의 렌더링 파이프라인의 변경가능한 중간과정을 수정하게 만들어주는 프로그램입니다.

* 렌더링 파이프라인에서 프로그래머가 직접 수정할 수 있는 부분 = 셰이더

* 중세 인쇄소로 예를 들어보면,
    1. 판화의 밑그림 그리는 사람 
    2. 색입히는 사람 
    3. 프레스하여 최종 인쇄하는 사람

버텍스 셰이더

* 3D 공간상의 위치를 화면상의 위치로 변환해줍니다.

프래그먼트 셰이더

* 그릴 대상을 이루는 수많은 점들의 색을 채워넣어 최중 출력될 픽셀의 색을 결정합니다.
<br><br>


__Part 2 렌더링 파이프라인 : 그래픽스 API 초기화__<br>

렌더링 파이프라인이란 오브젝트를 화면에 표현하기 위한 일련의 작업들입니다. 렌더링 파이프라인을 구성하기전에 그래픽스 API를 구성해야합니다.<bR>


그래픽스 API 초기화

1. GPU 디바이스 생성
    * GPU의 가상 디바이스를 생성되어 CPU의 명령들을 받습니다.
    
    <br>

2. 커맨드 큐 생성
    * CPU와 GPU는 직접 통신하지 않기 때문에 속도가 빠른 GPU는 CPU의 처리가 지연된만큼 대기하게 되면서 성능을 낭비합니다.
    * CPU는 커맨드 큐에 커맨드 버퍼를 쌓게됩니다.
    * 각각의 커맨드는 GPU에 전달할 데이터, 실행될 명령입니다.
    * 명령들은 GPU가 해당 커맨드를 가져가서 실행할때까지 실행상태가 아닙니다.    
<br>


3. 렌더링 파이프라인 상태 생성

    렌더링 파이프라인의 현재 상태를 표현하며 여러개 생성 가능합니다.
    * 렌더링 파이프라인 상태 구성표
    
    - <img src="https://user-images.githubusercontent.com/80774412/201303092-22bb3b3c-c3be-4669-a1aa-c715a69ad959.PNG" width="40%" height="30%" title="px(픽셀) 크기 설정" alt="rendering_pipeline_state"></img>
    
    * 각각 다른 셰이더를 가지고있는 렌더링 파이프라인을 선택해서 커맨드를 실행할 수 있습니다.

<br>

4. 사용할 데이터 로드
사용할 데이터들을 시스템 메모리에 로드해야합니다.

    * 정점 != 위치
        - 위치는 정점의 속성중 하나입니다.

    * 정점 데이터들은 커맨드를 통해 전달될때는 Stream 형태의 버퍼로 전달됩니다.

    * 정점 조립기의 '정점 서술자'를 통해 정점으로 구조화 됩니다.

<br>

__Part 3 드로우 콜__

렌더링 파이프라인이 시작되기 직전의 단계로, CPU가 GPU에 전달할 명령들(커맨드)를 생성하는 단계입니다.

예를들면 삼각형을 그리기위해 삼각형을 표현하는 데이터들과 삼각형을 그리기위한 명령들의 묶음입니다.

이러한 커맨드 버퍼가 커맨드 큐에 쌓이게 되고 GPU로 전달됩니다.

여기까지는 GPU가 동작하기 전, CPU나 응용 프로그램 상에서 처리되는 작업들입니다.
<br><br>

__Part 4 정점조립__

<img src="https://user-images.githubusercontent.com/80774412/201630899-347a455b-fb84-406e-96d6-55908d418736.PNG" width="70%" height="200" title="px(픽셀) 크기 설정" alt="rendering_pipeline_flow"></img>

스트림 형태의 데이터를 정점 구조체 단위로 조립합니다.

```
struc VertexInput
{
    fixed4 position;
    fixed4 color;
}
```
위 코드처럼, 정점 구조체의 이름과 필드를 정의할 수 있습니다.

<br><br>

__Part 5 버텍스 셰이더__

정점을 입력받아 변환행렬을 거쳐 다른형태의 정점으로 변환합니다.
주로 3D 공간 상의 정점의 위치를 클립 공간으로 옮겨 투영해줍니다.

예를들어, 그림자극에서 다양한 3차원 위치의 객체들이 빛에의해 투영되어 그림자가 한 스크린으로 모이게 되는것과 비슷합니다.

정점을 위치로 옮기기 위해서 [변환행렬](https://www.youtube.com/playlist?list=PLZHQObOWTQDPD3MizzM2xVFitgF8hE_ab)을 사용합니다.

__변환행렬__

모델행렬 -> 뷰행렬 -> 투영행렬이 순서대로 대입됩니다.

* __모델 행렬__

    * <img src="https://user-images.githubusercontent.com/80774412/201645266-08952e2e-23ef-4118-8a15-03ad0a6d55a6.PNG" width="70%" height="200" title="모델 행렬" alt="rendering_pipeline_flow"></img>

    * __오브젝트(모델) 공간__ 
        * 3D 모델의 피벗이 원점
        * 모든 정점의 위치는 피벗으로부터 떨어져있는 정도로 결정

    * 오브젝트 공간의 정점들에 모델행렬을 곱셈연산하여 월드공간으로 옮겨줍니다.

    * 세상의 원점이 모델의 위치 반대방향으로 밀려납니다.
* __뷰 행렬__

    * <img src="https://user-images.githubusercontent.com/80774412/201645397-bfb4972f-db1a-42e8-84fd-880954b91ac1.PNG" width="70%" height="200" title="뷰 행렬" alt="rendering_pipeline_flow"></img>

    * 월드 상의 정점들을 카메라 공간으로 투영합니다.

    * __카메라 공간__
        * 카메라의 위치를 원점으로 삼은 공간입니다.
        * 시야와 원금감이 없습니다.

    * 물체를 보는 카메라위치에 따라서 물체의 위치가 다르게 보입니다.

    * 카메라의 위치, 회전을 변경하는것보다 카메라는 원점에 두고 오브젝트를 상대적으로 배치하는것이 계산이 간결합니다.

    * 좌표계의 중심이 카메라의 위치로 이동하게되고, 위치를 재편성 하게됩니다.
* __투영 행렬__

    * <img src="https://user-images.githubusercontent.com/80774412/201645465-ecdd2835-4a95-4bfe-867d-86cadaf63b16.PNG" width="60%" height="200" title="투영 행렬" alt="rendering_pipeline_flow"></img>

    * <img src="https://user-images.githubusercontent.com/80774412/201644839-5d86ed44-f5e9-4307-bf3d-69ad1f3037aa.PNG" height="200" title="투영 행렬" alt="rendering_pipeline_flow"></img>

    * 카메라 위치 기준의 정점을 카메라 시야(화면/뷰포트) 기준의 뷰포트 공간의 정점으로 바꿔줍니다.

    * __클립 공간__
        * 뷰포트를 직육면체로 압축한 공간입니다.
            * X, Y는(-1,1), Z는 (0, 1)
        * 뷰포트를 기준으로 생성된 공간입니다.
        * 카메라 공간과는 달리 시야와 원근감이 존재합니다.
        * 카메라가 볼 수 없는 공간을 잘라내는 공간이기 때문에 '클립'공간 이라고 불립니다.
        * 클립핑을 통하여 클립 공간 외부의 폴리곤은 __NDC__ 로 변환되는 과정에서 잘려나가고 NDC 공간으로 변경됩니다.
    <br><br>
    * __NDC__ (정규화된 기기 좌표계)
        * 공간의 모든 X, Y, Z의 범위가 (-1 ~ 1 또는 0 ~ 1) 로 정규화된 좌표계입니다.
    <br><br>
    * 클립공간과 NDC 둘다 뷰포트를 (2 * 2 * 1)직육면체로 압축한 공간이라는 점은 같지만, 클립공간은 동차좌표계(4차원)을 통해 3차원을 계산했고, NDC는 클립핑을 통해 의도된 3차원 공간으로 변환된 것입니다.

    * 4차원을 빌려 3차원 계산을 하면 계산이 간단해진다는 장점이 있습니다.

    * __뷰포트__ (frustum)

        * <img src="https://user-images.githubusercontent.com/80774412/201644727-5e2c77b7-1e03-4d5f-a59d-dbcb930eea07.PNG" width="50%" height="200" title="frustum" alt="rendering_pipeline_flow"></img>

        * __near clip plane__ 과 __far clip plane__ 으로 구성됩니다.

        * 카메라가 보는 영역인 시야의 역할을 합니다.

        * 투영 행렬 연산 전에 뷰포트의 외부에 위치하던 정점은, 계산후에도 클립 공간의 외부에 위치하게됩니다.

        * <img src="https://user-images.githubusercontent.com/80774412/201645028-66d4711a-5513-4cf6-b340-ac057e1719b0.PNG" width="%" height="200" title="frustum_원근감" alt="rendering_pipeline_flow"></img>

버텍스 셰이더는 이렇게 오브젝트 공간에 있는 정점들을 클립 공간으로 변환하여 래스터라이저에 전달해줍니다.

<br><br>

__Part 6 래스터라이저__

전달 받은 정점들로부터 삼각형을 구성하고, 그 삼각형을 채우는 프래그먼트를 생성하여 프래그먼트 셰이더에 전달합니다.

* __뷰포트 변환__
    * (2 * 2 * 1)의 정규화된 클립공간의 정점들을 스크린(윈도우 화면)에 맞춘 공간으로 변형합니다.
    * 스크린 공간은 가로, 세로 길이가 스크린의 해상도로 구성됩니다.
    하지만 Z값은 0~1 로 구성됩니다.

* __스캔 변환__
    * 정점으로 이루어진 프리미티브(원시 도형)를 채우는 프래그먼트를 생성합니다.
    * 프래그먼트는 픽셀의 후보입니다. 
    * 프래그먼트를 생성하는 알고리즘은 GPU에 내장되어있어 직접 수행할 수 없습니다.
    * 정점보다 프래그먼트가 더 많이 존재합니다.

* __보간__
    * 래스터라이저는 이 정점들의 속성값(정점 컬러, UV 좌표, 노말)들을 보간하여 프래그먼트의 속성 값들을 결정합니다.

이렇게 생성된 프래그먼트들을 프래그먼트 셰이더로 전달해줍니다.


__Part 7 프래그먼트 셰이더__

래스터라이저로부터 전달받은 프래그먼트에 어떤 색을 채울지 결정합니다. 
전달받은 프래그먼트의 수만큼 셰이더가 실행됩니다.

* 프래그먼트 셰이더의 출력
    * 프레임 버퍼에 쓰여질 컬러값, 깊이값(Z)

        ```
        struct FragmentInput
        {
            fixed3 screenPosition;
            fixed3 color;
        }
        ```
        버텍스 셰이더와 래스터라이저를 거쳐 프래그먼트 셰이더에 입력될 값들의 구조체 또한 직접 선언하고 구성할 수 있습니다.


    * __예시1__
        ```
        fixed4 FragmentShader(FragmentInput input)
        {
            fixed4 outputColor = input.color;
            return outputColor;
        }
        ```
    * __예시1 결과__

        <img src="https://user-images.githubusercontent.com/80774412/201658437-7b43b4a3-7f07-49ac-9aa5-a0a196baa8cc.PNG" width="%" height="200" title="use_inputFragment" alt="rendering_pipeline_flow"></img>

    * __예시2__
        ```
        fixed4 FragmentShader(FragmentInput input)
        {
            fixed4 outputColor = fixed4(1,0,0,1);
            return outputColor;
        }
        ```
    * __예시2 결과__

        <img src="https://user-images.githubusercontent.com/80774412/201658624-007819b9-e9ec-4af5-a7b3-e504cbc06371.PNG" width="%" height="200" title="use_inputFragment" alt="rendering_pipeline_flow"></img>

    * __예시3__

        ```
        fixed4 FragmentShader(FragmentInput input)
        {
            fixed4 outputColor = fixed4(input.screenPosition.x / 400,0,0,1);
            return outputColor;
        }
        ```
    * __예시3 결과__

        <img src="https://user-images.githubusercontent.com/80774412/201658669-5ead0dce-b774-4dfe-a53b-cf42dd1051e4.PNG" width="%" height="200" title="use_inputFragment" alt="rendering_pipeline_flow"></img>


__출력 병합__<br>
알파(A)값과 깊이(Z)값을 통해 각 프래그먼트를 픽셀로서 사용해야될지 직전 버퍼의 컬러값과 혼합하여 사용해야될지 탈락시킬지 결정합니다.

<img src="https://user-images.githubusercontent.com/80774412/201660404-5c304558-c30d-4666-ad19-7183a1043580.PNG" width="%" height="200" title="출력병합_깊이값" alt="rendering_pipeline_flow"></img>

이렇게 컬러버퍼에 전달된 컬러들은 최종화면에 그려지게 됩니다.
<br><br>
