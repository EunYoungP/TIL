22.02.01

# __DOTween__

__DOTween__ 이란 유니티 오브젝트의 애니메이션, 속성값 변경 시 기존 유니티에서 제공하는 애니메이션 기능이나 스크립트 작성 대신 함수 몇개로 쉽고 다양한 모션을 줄 수 있는 API Application Programming Interface 입니다.

[DOTween Documentation](http://dotween.demigiant.com/)<br><br>

## __프로젝트 적용__

제작한 게임의 기존 UI들을 유니티 애니메이션 기능을 이용한 구현에서 DOTween을 이용한 방법으로 구현하여 업그레이드 시켰습니다.

```c#
    using DG.Tweening;

    // 퀘스트창 활성화될시 적용될 시퀀스
    Sequence showSequence;
    // 퀘스트창 비활성화시 적용될 시퀀스
    Sequence hideSequence;
    // 퀘스트창이 위아래로 반복하여 움직이는 애니메이션을 표현하는 트위너
    Tweener floatTweener;

    private void BouncePanel()
    {
        floatTweener = questPanel.transform.DOMoveY(10, 1).SetRelative().SetLoops(-1, LoopType.Yoyo).SetEase(Ease.InOutQuad);

        showSequence = DOTween.Sequence().SetAutoKill(false).Pause()
            .Join(questPanel.transform.DOScale(0, 1).From().SetEase(animationCurve))
            .AppendCallback(() => floatTweener.Play());

        hideSequence = DOTween.Sequence().SetAutoKill(false).Pause()
            .Join(questPanel.transform.DOScale(0, 1).SetEase(Ease.InBack))
            .OnComplete(() => isActive = false )
            .AppendCallback(() => floatTweener.Pause())
            .AppendCallback(()=> gameObject.SetActive(isActive));
    }
```
퀘스트가 완료되었을때 보여지는 창이 활성화되고 비활성화 될때의 애니메이션을 두트윈을 사용해서 구성했습니다.<br><br>

## __오류 수정__

선언해둔 트윈을 사용하기 위해서 퀘스트창이 Open 되고 Close 되는 시점의 코드에서 만약 퀘스트창을 여는 시점이라면,

```c#
showSequence.Play();
hideSequence.pause();
```

이런 형식으로 시퀀스를 제어해주는 함수를 사용했는데,

첫번째 사용 이후부터 쿠세트창이 다시 열리거나 닫히지 않는 오류가 발생했습니다.

이것은 퀘트창을 다시 사용할 때 시퀀스의 제어 함수를 Restart() 가 아닌 Play() 로 호출해서 생긴 오류였습니다.

트윈을 한번 Play 하고 난 후에는 Restart로 처음부터 다시 실행시켜주거나

Rewind() 함수로 처음으로 되돌려준 뒤에 Play로 다시 실행시켜야 합니다.

__퀘스트창을 두번째로 open 실행한 시점__

```c#
showSequence.Restart();
hideSequence.pause();
```
```c#
showSequence.Rewind().Play();
hideSequence.pause();
```
<br><br>

## __floatTweener 구성__

* __DOMoveY__ 

    대상의 위치를 지정된 값으로 이동하고 선택한 Y축만 트위닝합니다.

* __SetRelartive()__ 

    해당 트윈을 상대적으로 적용합니다. Y 축의 이동을 월드좌표의 10이 아닌, 트윈이 적용되는 객체로부터 Y축으로 10만큼 이동합니다.

* __SetLoops(int 루프횟수, LoopType loopType = LoopType.Restart)__ 

    트윈에 대한 반복 옵션을 설정합니다.

    루프횟수를 -1 로 설정하면 무한반복합니다.

    - __LoopType__

        <img src = "https://user-images.githubusercontent.com/80774412/216150544-19c2a174-8f5e-4750-9241-8e00cfd6175d.gif"></img>

* __SetEase(Ease/AnimationCurve/EaseFunction)__

    트윈의 Ease를 설정합니다.

    트위너 대신 시퀀스에 적용하면 애니메이션 타임라인인 것처럼 이즈가 전체 시퀀스에 적용됩니다.

    InOutQuad 타입을 사용하여 퀘스트창이 위->아래, 아래->위 로 움직일때 시작과 끝부분에서 움직이는 속도가 느리도록 설정했습니다.

    - __EaseType__

        <img src = "https://user-images.githubusercontent.com/80774412/216151068-27ecde7a-cbbb-4b0a-b43c-30444a83e0fc.gif"></img>

        <img src = "https://user-images.githubusercontent.com/80774412/216152977-6514a97a-35f5-4f26-85ad-7ca6a36089cb.PNG"></img>

<br><br>

## __showSequence 구성__

* __DOTween.Sequence()__

    시퀀스를 할당해줍니다.

* __SetAutoKill(false)__

    기본적으로 트윈은 완료 시 자동으로 종료되지만, 매개 변수로 사용하려는 경우 이 함수를 사용하여 변경할 수 있습니다.

    퀘스트창이 계속해서 활성화/비활성화 될것이기 때문에 트윈을 종료시키지않고 메모리에 남깁니다.

* __Pause()__

    트윈을 일시정지 시킵니다.

* __.Join(questPanel.transform.DOScale(0, 1).From().SetEase(animationCurve))__

    트위너를 Join으로 시퀀스에 추가시켰습니다.

    퀘스트창의 스케일값을 DOScale 함수로 제어했습니다.

    본래라면 스케일값 0 으로 1초동안 변경되는 트위너지만 바로뒤에 From 함수로 인해 0부터 본래 스케일값으로 변경되는 트위너가 되었습니다.

    그리고 animationCurve 라는 직접 설정한 Ease값을 부여하여 움직임을 구현했습니다.

* __.AppendCallback(() => floatTweener.Play())__

    AppendCallback 함수로 마지막으로 추가된 트윈 뒤에 처음에 만들었던 floatTweener 라는 트위너를 실행시키도록 했습니다.

    floatTweener는 y값을 10만큼 계속 위아래로 변경시키는 트위너입니다.

    showSequence를 사용해 스케일을 0 -> 1로 바꿔주는 트윈이 끝나면, 퀘스트창이 떠있는 동안 y값의 변화를 이용해 둥둥 떠있는 느낌을 주기위함입니다.
<br><Br>

## __hideSequence 구성__

시퀀스의 기본 설정은 showSequence와 동일하게 설정하였습니다.

From함수를 제거하여 DOScale 함수가 정상적으로 1 -> 0으로 변경되게 하였고,

OnComplete 함수를 통해 트윈이 끝나면 isActive 변수에 false값을 부여해 주고

AppendCallback에 둥둥떠다니는 효과를 주는 floatTweener를 Puase시키고, 객체를 비활성화 시켜줍니다.

<br><br>

## __시퀀스 사용__

```c#
hideSequence.Pause();
showSequence.Play();
```
시퀀스를 제어해주는 함수들로 퀘스트창이 활성화/비활성화되는 시점에서 시퀀스를 제어해줍니다.

<br><br>

## __개념__

* __Tweener__: 값과 애니메이션들의 조절을 가능하게 합니다.

* __Sequence__: 특별한 트윈으로, 값을 조정하는 대신에 다른 트윈들을 제어하고 그룹으로 애니메이션화합니다.

* __Tween__: 트위너와 시퀀스를 모두 나타내는 단어입니다.

* __NestedTween__: 시퀀스 내에 포함된 트윈입니다.
<br><br>


## __트위너 만들기__

__트위너__ 는 DOTween의 일개미입니다. 속성, 필드를 가져와 목표값을 향해 애니메이션화합니다.

__트위닝 가능한 값의 유형__

float, double, int, uint, long, ulong, vector2, vector3, vector4, Quarternion, Rect, RectOffset, Color, string 등을 트위닝할 수 있습니다.<br><br>

###  __A. The generic way__
```c#
// getter: 속성값을 트윈으로 반환하는 대리자입니다.
// setter: 속성값을 트윈으로 설정하는 대리자입니다.
// to: 도달할 최종 목표값입니다.
// duration: 트윈의 기간입니다.
DOTween.To(getter, setter, to, float duration);

// Vector3 타입의 myVector를 3,4,8 값으로 1초동안 트윈합니다.
DOTween.To(() => myVector, x => myVector = x, new Vector3(3, 4, 8), 1);

// float 타입의 myFloat의 값을 46 으로 3초동안 트윈합니다.
DOTween.To(() => myFloat, x => myFloat = x, 46, 3);
```
<Br>

### __B. The shorcuts way__

DOTween에는 __Transform, Rigidbody, Material__ 같은 일부 유니티 개체에 대한 지름길이 포함되어 있습니다. 

```c#
transform.DOMove(new Vector(2,3,4), 1);
rigidbody.DOMove(new Vector3(2,3,4),1);
material.DOColor(Color.green, 1);
```

## __FROM__

* 각 지름길 참조 방법에서는 FROM 대체 버전도 존재합니다.

* FROM은 즉시 대상을 지정된 값으로 보낸 다음 이전 값으로 트위닝합니다.


__형식__

```
From(bool isRelative = false);
```

__FROM 대체 버전__

    ```c#
    transform.DOMove(new Vector(2,3,4), 1).From();
    rigidbody.DOMove(new Vector3(2,3,4),1).From();
    material.DOColor(Color.green, 1).From(true);
    ```
    FROM 은 트윈 특정 옵션을 제외한 다른 설정 전에 연결해야합니다.
<br><br>


## __시퀀스 특징__

* 시퀀스는 __다른 Tweener, 시퀀스__ 를 그룹으로 애니메이션에 적용합니다.

* 시퀀스는 __다른 시퀀스__ 내에 포함될 수 있습니다.

* __Insert__ 메서드를 사용해 트윈(Sequence, Tweener)을 겹칠 수 있습니다.

* 여러 Sequence에서 동일한 트윈을 재사용할 수 없습니다.<br><br>

## __시퀀스 만들기__

1. 새 시퀀스를 가져와 참조로 사용하고 저장합니다.

```c#
Sequence mySequence = DOTween.Sequence();
```

2. 시퀀스에 트윈, 간격 및 콜백을 추가합니다.

* 메서드는 시퀀스가 시작되기 전에 적용해야합니다.

* 중첩된 TWeener/Sequence 는 Sequence에 추가하기 전에 완전히 생성되어야 합니다.

```c#
// 시퀀스의 끝에 트윈을 추가합니다.
mySequence.Append(transform.DOMoveX(45,1));
// 시퀀스의 끝에 지정된 콜백을 추가합니다.
mySequence.AppendCallback(MyCallback);
// 시퀀스의 끝에 지정된 간격을 추가합니다.
mySequence.AppendInterval(interval);
// 주어진 시간 위치에 트윈을 삽입합니다.(트윈을 겹칠 수 있게 해줍니다.)
mySequence.Insert(1, transform.DOMoveX(45,1));
// 주어진 시간 위치에 지정된 콜백을 삽입합니다.
mySequence.InsertCallback(1, MyCallback);
// 마지막 트윈 또는 콜백의 동일한 시간 위치에 지정된 트윈을 삽입합니다.
mySequence.Append(trnasform.MoveX(23,1));
mySequence.Join(transform.DORotate(new Vector3(0,180,1), 1));
// 시퀀스 시작 부분에 지정된 트윈을 추가하고, 내용을 제 시간에 앞으로 밀어냅니다.
mySequence.Prepend(transform.DOMoveX(34,1));
// 시퀀스 시작 부분에 지정된 콜백을 추가합니다.
mySequence.PrependCallback(MyCallback);
```

시퀀스는 체인을 사용하여 구현할 수 있습니다.

```c#
Sequence mySequence = DOTween.Sequence();
mySequence.Append(transform.DOMove(34,2))
    .Append(transform.DORotate(new Vector3(0,180,0),2))
    .PrependInterval(1)
    .Insert(0, trasnform.DOScale(new Vector3(3,3,3), mySequence.Duration()));
```
<br><br>

## __Controlling a Tween__

* __A. 정적 메서드 및 필터를 이용__

    DOTween에는 트윈을 제어할 수 있는 많은 정적 메서드가 포함되어 있습니다.

    각각 모든 트윈에 적용되는 '전체버전', 트윈의 id나 대상으로 필터링할 수 있는 버전으로 제공됩니다.

    ```
    DOTween.PauseAll();

    DOTween.Pause("questPanel");

    DOTween.Pause(someTransform);
    ```

* __B. 트윈에서 직접 제어__

    정적 메서드 대신 트윈의 참조에서 동일한 메서드를 호출할 수 있습니다.

    ```
    myTween.Pause();
    ```

## __Control methods__

* Complete / CompleteAll(bool withCallbacks)

    트윈을 끝 위치로 보냅니다.(무한 루프가 있는 트윈에는 효과 없음)

* Flip / FlipAll()

    트윈의 방향을 뒤집습니다.

* Goto / GotoAll(float to, bool andPlay = false)

    지정된 시간의 위치로 트윈을 이동시킵니다.

* Kill / KillAll(bool complete = false, params object[] idsOrTargetsToExclude)

    트윈을 종료시킵니다.

    트윈은 완료되면 자동으로 종료되지만 더 이상 필요하지 않은 트윈을 더 빨리 종료할 수 있습니다.

    - __complete__ TURE일 경우 트윈을 죽이기 전에 즉시 완료시킵니다.

    - __idsOrTargetsToExclude__ KillAll을 실행할 경우, 작업에서 제외할 대상 또는 id 입니다.

* Pause / PauseAll()

    트윈을 일시정지 시킵니다.

* Play / PlayAll()

    트윈을 재생시킵니다.

* Restart / RestartAll(bool includeDelay = true, float changeDelayTo = -1)

    트윈을 다시 시작합니다.

    - __includeDelay__ TURE일 경우 최종 트윈 지연을 포함합니다.

    - __changeDelayTo__ 지정된 값만큼 지연으로 설정합니다.

* Rewind / RewindAll(bool includeDelay = true)

    트윈을 되감기하고 일시정지합니다.

    - __includeDelay__ TURE일 경우 최종 트윈 지연을 포함합니다.

* SomoothRewind / SmoothRewindAll()

    트윈을 부드럽게 되감기합니다.

    재생 방향이 반전됩니다.

* TogglePause / TogglePauseAll()

    일시정지된 경우 재생시키고, 재생 중인 경우 일시정지합니다.

<br><br>

----

참고자료

[DOTween 문서](http://dotween.demigiant.com/)

[사사로운 블로그](https://m.blog.naver.com/hana100494/221320177107)