# Normal Map( Normal Mapping)

## __목차__

* Normal Map 이란?

* Normal mapping vs Parallax mapping vs Displacement mapping<br><br>

## __Normal Map이란__

<img src="https://user-images.githubusercontent.com/80774412/213118274-3306cbbd-4520-40bd-85d5-698e58c7ef25.PNG"></img>

고품질의 그래픽을 뽑기위해 normal map을 사용합니다.

노멀맵은 폴리곤을 아끼기 위한것입니다.

CG나 영화에서는 다르게, 게임같은 실시간 렌더링에서는 비용을 아끼면서 그래픽 효율을 높여줍니다.

normal = 면의 방향


<img src="https://user-images.githubusercontent.com/80774412/213118919-7ba00be1-1e12-45e6-9f94-fd4be971d58c.PNG" width =600></img>

<img src="https://user-images.githubusercontent.com/80774412/213118958-416ebda0-0824-4ed5-b21f-b9ea9adaa078.PNG" width =600></img>


면적이 1인 빛을 직각으로 벽에 쏠때와, 45도로 벽에 쏠때를 비교하면 후자가 빛의 강도가 약해집니다.

이것은 삼각함수 cos으로 설명할 수 있습니다.

<img src="https://user-images.githubusercontent.com/80774412/213118983-1146d0f4-8c42-42aa-8b00-20fdad89ead5.PNG"></img>

<img src="https://user-images.githubusercontent.com/80774412/213119009-0401c2d4-be24-4a8b-b7b1-6974e40983e0.PNG"></img>

<img src="https://user-images.githubusercontent.com/80774412/213119034-1acb8b6a-7d20-46f9-9055-d6257fe7d9d7.PNG"></img>

<img src="https://user-images.githubusercontent.com/80774412/213119079-1c741c90-5247-4df7-9094-57218b88ea2b.PNG"></img>

빛은 마이너스 개념은 존재하지 않기때문에 마이너스는 0으로 치환됩니다.

<img src="https://user-images.githubusercontent.com/80774412/213119109-9a240650-50c9-41eb-b0c3-7813fffdb08e.PNG"></img>

환경광 등의 기타 요소에 의해 뒷면이 밝아지기도합니다.

<img src="https://user-images.githubusercontent.com/80774412/213119126-13bf8281-777c-408f-bac2-0ed82f0425da.PNG" width =600></img>


노말정보는 메쉬의 버텍스마다 저장이 되어있습니다.

왼쪽 사진이 각이 진것은 버텍스가 두개가 겹쳐있기 때문입니다. 이것을 __'하드엣지'__ 라고합니다.

<img src="https://user-images.githubusercontent.com/80774412/213119159-9a5c5573-219b-47be-bcbe-617f5c408817.PNG"></img>


검정선은 실제 면이고, 초록선은 정점과 면의 정점들이 보간된 노멀 정보들을 라이팅한것입니다.

<img src="https://user-images.githubusercontent.com/80774412/213119185-d8ad5910-161b-45d2-981d-1de25718d556.PNG"></img>

텍스처에 추가적인 노멀 정보를 저장해놓습니다. 아래 화살표들이 편차들을 보여줍니다.

월드기준이아닌 면기준에서의 차이를 저장합니다.

지브러시, 블랜더, 포토샵 등에서 노멀맵을 만들 수 있습니다.

유니티, 언리얼, 고닷 엔진 들에서 노멀맵의 기본개념, 매커니즘 모두 같습니다.
<br><br>


## __Normal mapping vs Parallax mapping vs Displacement mapping__ <br><br>

<img src="https://user-images.githubusercontent.com/80774412/213119249-09e54d4e-bfb3-4865-8a17-43f1f1691676.PNG"></img>


__Normap Map__

노멀이란 면의 방향이기 떄문에 빛의 연산이 필요한 요소라고 볼 수 있습니다.
<br><Br>

__Parallax mapping (Height map)__

<img src="https://user-images.githubusercontent.com/80774412/213119215-3536ca14-b3f4-46ca-8752-b9ea8aefab9e.PNG"></img>


음영과 높이갚을 저장합니다.


<img src="https://user-images.githubusercontent.com/80774412/213119270-c81df2c3-0a5b-4c8f-8bab-b85367e6e87c.PNG"></img>


노멀맵에 비해 가성비가 좋지않습니다. 
<br><Br>

__Displacement Mapping__

실제 버텍스를 조작하기 때문에 현실적으로 표현할 수 있습니다.

'Tessellation 버텍스의 개수를 늘려서 보여주는 기술'과 함께 사용할때 더욱 의미가 있습니다.

모바일에서는 Tessellation을 사용하기에는 한계가 있기 때문에 노멀맵이 더욱 상용화 되어있습니다.
<br><Br>

## Map  

노멀맵은 알베도같은 시각정보가 아닌 데이터입니다.

### 'Why is blue?'

<img src="https://user-images.githubusercontent.com/80774412/213119297-a1a1dc62-994b-44fd-af91-6095e52dd20b.PNG"></img>



평면입장에서의 상대적인 정보를 저장합니다.

vector로 이루어진 노멀맵은 

x : -1 ~ 1 => 0 ~ 1 (R)
y : -1 ~ 1 => 0 ~ 1 (G)
z : -1 ~ 1 => 0 ~ 1 (B)

x, y와 달리 z는 -1 값이 저장될 일이 없습니다.

항상 양수로 표현됩니다.
 
라이팅을 처리하기위한 데이터기 때문에 뒷면은 배제되어 앞면의 데이터인 양수로만 표현됩니다.

여기서 Z값이 RGB 중 B, Blue 값이기 때문에 대부분의 노멀맵은 파란색을 띕니다.
<br><br>


## 노멀맵을 특수하게 처리

<img src="https://user-images.githubusercontent.com/80774412/213119317-7b585238-194d-43f4-8b11-4731dfadaea9.PNG"></img>

[감마가 어디감마](https://www.youtube.com/watch?v=Xwlm5V-bnBc)

ASTC도 노멀맵으로 저장하면 퀄리티가 좋아집니다.

이유는 노멀맵은 길이가 '1'이기 때문입니다.

노멀맵은 x, y값만 저장하면 z값은 셰이더 연산으로 구할 수 있습니다.

[추가 자료 : 수포자를위한 게임수학](https://www.youtube.com/watch?v=jmh67nvbcrM&t=20s)
<br><br>

## Tangent Space Transform

<img src="https://user-images.githubusercontent.com/80774412/213119338-bb2523df-61ad-4601-a101-061750509818.PNG"></img>

<img src="https://user-images.githubusercontent.com/80774412/213119384-1e689fa4-c0a1-4535-ac87-9044ccef7f83.PNG"></img>



노멀맵을 저장하려면 공간변환이 필요합니다.

구체에 노멀맵을 씌우려고 가정한다면, 맵을 어느 방향으로 씌울지 기준이 필요합니다.

<img src="https://user-images.githubusercontent.com/80774412/213119418-b9b5a52f-d563-4aba-bbca-d7c7658cb521.PNG"></img>

UV좌표를 따라서 축을 하나 만듭니다. 이것은 __Tangent 벡터__ 입니다.

노멀맵은 오브젝트 기준이 아닌 버텍스단위로 공간이 따로 존재합니다. 이것을 __Tangent space__ 라고 합니다.

<img src="https://user-images.githubusercontent.com/80774412/213119441-635e041a-eba9-4479-975d-89e4f2a52066.PNG"></img>

<img src="https://user-images.githubusercontent.com/80774412/213119463-40c54644-d935-423f-a1d1-755c80d7f64a.PNG"></img>

Unity 의 transform도 모두 행렬로 표현됩니다.

즉, 행렬은 공간변화를 저장할 수 있는 데이터입니다.

유니티는 열기준(Column major) 행렬을 사용합니다.

현재 노멀축과 탄젠트축만 보이기 떄문에 외적을 이용합니다.

<img src="https://user-images.githubusercontent.com/80774412/213119483-8d68d40a-e7dc-40d2-872b-08089a2a8729.PNG"></img>

<img src="https://user-images.githubusercontent.com/80774412/213119514-54805a68-0efb-4f9e-bc0b-7e1f3534cf14.PNG"></img>

이때 나오는 축이 __BiTangent/BiNormal__ 입니다.

이렇게 세개의 축이 만들어져 공간이 만들어집니다.<br><br>

## Shader Graph Node

<img src="https://user-images.githubusercontent.com/80774412/213119539-506bc2b8-9740-4850-bc24-c2c7fef74b89.PNG"></img>

<img src="https://user-images.githubusercontent.com/80774412/213119579-c1cf4500-ade4-4b7d-a6d6-04cbcea51e3a.PNG"></img>


LitShader는 노멀맵 슬롯에 넣어 유니티에서 계산을 해주지만,

직접 구현하는 경우에는 로우메이저, 컬럼메이저 여부를 잘 확인해야합니다.




## URP Shader

예전에는 노멀맵이 범프맵이라고 불렸기때문에 코드상에서는 아직 그 부분이 남아있습니다.

<img src="https://user-images.githubusercontent.com/80774412/213119614-a21a64f9-7965-4c7f-8def-4b6ea5e39182.PNG"></img>

<img src="https://user-images.githubusercontent.com/80774412/213119638-ed42f61e-a6da-4d36-9a49-bab0cc84f048.PNG"></img>

<img src="https://user-images.githubusercontent.com/80774412/213119666-405edff9-bc05-432e-bb39-a3fe981174c1.PNG"></img>

<img src="https://user-images.githubusercontent.com/80774412/213119685-7c203e29-ab54-4ea8-9e9e-e053204e9355.PNG"></img>


