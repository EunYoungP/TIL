# 2022 Shader & Graphics

# 따라하면서 배우는 유니티5 셰이더와 이펙트 입문<br><br>


## :blush: 책 '따라하면서 배우는 유니티5 셰이더와 이펙트 입문' 의 내용을 정리한 문서입니다.


## :blue_book: 강의소개
:point_right: 
이 책은 유니티5를 이용한 차세대 게임용 셰이더 제작과 이펙트 구현을 위한 내용입니다. 


* 표준 셰이더의 이해 

* 커스텀 셰이더 만들기

* 조명과 발광 표면 만들기

* 코드와 셰이더를 이용한 표면 애니메이션

* 투명 표면과 효과

* 스페큘러와 메탈릭 표면

* 생체 표면을 위한 셰이더

* 커스텀 파티클 셰이더: 연기, 증기, 액체

* 모바일을 위한 셰이더 최적화


## :page_with_curl: 강의 정리
<br>

__Part 1 표준 셰이더의 이해__


<br><br>

__Part 2 커스텀 셰이더 만들기__

- Create
    - Shader
        - Standard Surface Shader
<br><br>


셰이더 파일을 생성하여 아래 내용을 작성합니다.

```
// 코드를 셰이더로 정의
// 프로젝트 내의 셰이더 리스트에서 셰이더를 선택하는 방법 지정
// 폴더 이름/셰이더 이름
Shader "PACKT/Moon"
{
	// 머티리얼에서 조정 가능한 슬라이더, 디퓨즈 텍스처, 정의된 색상 등을 포함합니다.
	Properties
	{
		// 코드 시작행에 있는 프로터피 이름은 일반적으로 밑줄을 접수다ㅗ 붙입니다.
		// 이 프로퍼티의 이름이 셰이더 본체에서 사용됩니다.
		_Color("Color", Color) = (1,1,1,1)
	}

	// 셰이더의 주 본체를 포함합니다.
	// 입력 색상, 텍스처 등의 값을 모두 처리합니다.
	// 모든 셰이더에 하나 이상 포함됩니다.
	// 주 기능은 다른 셰이더 정의르르 분리하는 것입니다. 이를 통해 셰이더가 여러 장치에서 작동하게 만들어줍니다.(셰이더 호환성)
	SubShader
	{
		// subShader 는 최소 요건으로 Pass 하나를 포함합니다.
		Pass
		{
			Color[_Color]
		}
	}
}
```

그리고 셰이더의 Color 프로퍼티를 진한 빨간색으로 바꿔주면,

<img src="https://user-images.githubusercontent.com/80774412/201837013-d5f98a02-9f8b-4101-a820-69a134dd4664.PNG"></img>

위 셰이더의 결과로 그림자나 하이라이트가 없는 균일한 색상 표면이 나옵니다.

이러한 셰이더를 무조명 셰이더(unlit shader)라고 하며 성능 면에서 비용이 아주 적게 듭니다.
<br><br>

__셰이더에 텍스처 입히기__

```
Properties
{
    _Color("Color", Color) = (1,1,1,1)
    _MainTex ("Albedo(RGB)", 2D) = "white" {}
}
```

```
Pass
{
    Color[_Color]
    SetTexture[_MainTex] 
    {
        Combine Primary * Texture
    }
}
```
_MainTex 행을 프로퍼티를 추가해줍니다.

프로퍼티 이름을 지정해 텍스처를 설정합니다. 원래(또는 Primary) 내용과 곱해서 적용합니다.<br><br>

_MainTex는 유니티가 주 텍스처 맵에 사용하는 표준 이름입니다.

일반적으로 셰이더의 [알베도](https://docs.unity3d.com/kr/530/Manual/StandardShaderMaterialParameterAlbedoColor.html) 컴포넌트로 사용됩니다.

* 텍스처 맵의 종류

    <img src="https://user-images.githubusercontent.com/80774412/201841442-731f51f6-5ef7-485e-a65d-fce05bbaa619.PNG"></img>

<br><br>

### __셰이더 씬 조명에 반응하게 만들기__

Cg를 사용하여 기존의 패스를 대체해봅시다.

```
SubShader
{
    CGPROGRAM
    #pragma surface surf Lambert

    struct Input
    {
        float2 uv_MainTex;
    };

    sampler2D _MainTex;
    float4 _Color;

    void surf(Input IN, inout SurfaceOutput o)
    {
        o.Albedo = tex2D(_MainTex, IN.uv_MainTex).rgb * _Color.rgb;
    }

    ENDCG
}
```
Cg 코드는 항상 CGPROGRAM 과 EDNCG 사이에 구현됩니다.
<BR><bR>

__CGPROGRAM__

* 이 코드는 프로퍼티를 정의하고 처리하는 Cg블록을 시작합니다.

__#pragma surface surf Lambert__

* 셰이더 컴파일 지시자(shader compoilation derective)

* 항상 여는 Cg 태그 다음에 위치합니다. 

* 여기서는 음영을 지원하는 Lambert 모델을 사용했습니다.

* 셰이더 실행 시 컴파일될 셰이더 함수와 조명 모델을 지정합니다. 

__struct Input__

* 텍스처를 모델 표면에 올바르게 적용하기 위한 UV 데이터를 담는 구조체입니다.

* float 값 두 개로 구성된 float2 데이터입니다.

__sampler2D _MainTex;__

__float4 _Color;__

* 프로퍼티를 셰이더에서 계산할 수 있는 Cg 변수로 지정합니다.

* _MainTex 는 텍스처이며, _Color는 R, G, B, A 채널 값으로 구성되는 float4 변수입니다.

__ENDCG__

* Cg블록을 끝냅니다.

<BR><bR>

### __헬멧 셰이더 생성__

* create
    * Shader
        * Standard Surface Shader

기존 셰이더와 구분하기 위해 자동으로 Custom/"에셋이름" 으로 작성됩니다.

기존 코드를 반투명하게 편집해보겠습니다.

subShader 블록에서는 다양한 셰이더 특성을 정의하며, 투명도를 지정하는 알파 채널이나 다른 프로퍼티를 지정할 수 있습니다.

변경전

    ```
    Tags { "RenderType"="Opaque" }

    #pragma surface surf Standard fullforwardshadows
    ```

변경후

    ```
    Tags { "RenderType"="Transparent" }

    #pragma surface surf Standard fullforwardshadows alpha
    ```

위처럼 코드를 변경했습니다.
<br><Br><br>

### __헬멧 내표면 만들기__

유니티 셰이더는 성능상의 이유로 기본적으로 메시의 외면만 렌더링하며,

내면은 자동으로 제거됩니다.

LOD 200 행 뒤에 아래처럼 코드를 추가해줍니다.

```
LOD 200
    Cull Off
```
<BR><bR>


### __외면과 내면 분리__

셰이더에서 헬멧의 두꼐를 가짜로 표현하게 만들면 더 현실적이게 보이게 할 수 있습니다.

Properties 블록에 아래 코드를 추가합니다.
```
_Thickness ("Thickness", Range(-1,1)) = 0.5
```

Cull Off를 아래와 같이 변경합니다.
```
Cull Back
```

ENDCG 다음 행에 아래 코드를 추가합니다.

첫 번째 셰이더와 사용할 추가 셰이더로 내면의 모양을 정의하는 역할입니다.
```
Cull Front

CGPROGRAM
#pragma surface surf Standard fullforwardshadows alpha vertex:vert

struct Input
{
    float2 uv_MainTex;
};

float _Thickness;
void vert(inout appdata_full v)
{
    v.vertex.xyz += v.normal * _Thickness;
}

sampler2D _MainTex;
half _Glossiness;
half _Metallic;
fixed4 _Color;

void surf(Input IN, inout SurfaceOutputStandard o)
{
    fixed4 c = tex2D(_MainTex, IN.uv_MainTex) * _Color;
    o.Albedo = c.rgb;
    o.Metallic = _Metallic;
    o.Smoothness = _Glossiness;
    o.Alpha = c.a;
}
ENDCG

```
 
__코드 해석__

* __Cull Front__

    두 셰이더가 서로 겹치지 않도록 전면을 제거합니다.

    다른 방법으로는 셰이더를 서로 겹치게 해 블렌딩하는 등의 방법이 있습니다.


* __#pragma surface surf Standard fullforwardshadows alpha vertex:vert__

    지시자에 vertex: vert 를 추가하여 셰이더가 적용되는 버텍스에 접근할 수 있게 해줍니다.

* __float _Thickness;__
    위쪽에서 이미 정의된 프로퍼티 이름을 이용해 지정된 값만큼 버텍스의 위치를 조정합니다.

<br><Br>


## __행성의 대기 개선__

행성 대기의 가스 층은 주변부에서 연무처럼 보이는 현상을 일으킵니다. 효과를 구현해봅시다.

* Create
    * Shader
        * Standard Surface Shader

```
Shader "PACKT/Planet_falloff"
{
    Properties
    {
        _Color ("Color", Color) = (1,1,1,1)
        _MainTex ("Albedo (RGB)", 2D) = "white" {}
        _Thickness ("Thickness", Range(-1,1)) = 0.5
        _AtmosColor ("Atmosphere Color", Color) = (1,1,1,1)
    }
    SubShader
    {
        Tags { "RenderType"="Opaque" }
        LOD 200

        CGPROGRAM
        // Physically based Standard lighting model, and enable shadows on all light types
        #pragma surface surf Standard fullforwardshadows

        // Use shader model 3.0 target, to get nicer looking lighting
        #pragma target 3.0

        sampler2D _MainTex;

        struct Input
        {
            float2 uv_MainTex;
        };

        fixed4 _Color;

        // Add instancing support for this shader. You need to check 'Enable Instancing' on materials that use the shader.
        // See https://docs.unity3d.com/Manual/GPUInstancing.html for more information about instancing.
        // #pragma instancing_options assumeuniformscaling
        UNITY_INSTANCING_BUFFER_START(Props)
            // put more per-instance properties here
        UNITY_INSTANCING_BUFFER_END(Props)

        void surf (Input IN, inout SurfaceOutputStandard o)
        {
            // Albedo comes from a texture tinted by color
            fixed4 c = tex2D (_MainTex, IN.uv_MainTex) * _Color;
            o.Albedo = c.rgb;
            // Metallic and smoothness come from slider variables
            o.Alpha = c.a;
        }
        ENDCG

        // 대기 효과를 시뮬레이션하기 위해 행성 구체 바깥쪽의 내면을 렌더링합니다.
        // 두 번째 패스를 정의하며, 버텍스 함수를 이용하여 지오메트리를 돌출시킵니다.
        // 정반사나 평활도 특징을 사용하진 않지만, 대기를 나타낼 보조 색상을 정의합니다.
        // 또한 이 색상의 알파 채널을 이용해 대기의 불투명도를 설정합니다.

        Cull Front

        CGPROGRAM
        #pragma surface surf Standard fullforwardshadows alpha vertex:vert

        struct Input 
        {
            float2 uv_MainTex;
        };

        float _Thickness;
        void vert(inout appdata_full v)
        {
            v.vertex.xyz += v.normal * _Thickness;
        }

        fixed4 _AtmosColor;
        void surf(Input IN, inout SurfaceOutputStandard o)
        {
            o.Albedo = _AtmosColor.rgb;
            o.Alpha = _AtmosColor.a;
        }
        ENDCG
    }
    FallBack "Diffuse"
}
```

___Thickness__ 로 대기의 깊이를 지정합니다.

___AtmosColor__ 는 실제 행성 표면과 구분할 수 있게 차별화 색상 지정에 사용됩니다.


<img src ="https://user-images.githubusercontent.com/80774412/201922343-04a367ac-c120-45cf-8d23-c7489c8b01e0.PNG"></img>

행성의 셰이더 표면에 파란빛 연무효과가 생성된것을 확인할 수 있습니다.

<br><br>







