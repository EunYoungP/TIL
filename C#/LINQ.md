23.03.08

# __LINQ__

## __목차__


<BR><bR>

## __LINQ 표준 연산자__

LINQ 는 .NET 언어에서는 C# 과 VM 에서만 사용 가능합니다.

마이크로소프트에서는 LINQ 쿼리식이 실행될 수 있도록 CLR 을 개선했고, C# 과 VM 컴파일러를 업그레이드 했습니다.

컴파일러가 쿼리식을 CLR 이 이해할 수 있는 코드로 번역을 해줍니다.<BR><bR>


__LINQ 쿼리식 -> 호출 메소드__


_LINQ 예시_
```c#
var profiles = from profile in arrProfiles
                where profile.Hiehgt > 175
                orderby profile.Hieght
                select new {
                    Name = profile.Name, Inheight = prifile.Height * 0.393
                };
```

_C# 컴파일러의 번역_
```C#
var profiles = arrProfiles
                .Where( profile => profile.Height > 175)
                .OrderBy( profile=> profile.Hieght)
                .Select( profile => new {
                    Name = profile.Name,
                    Inheight = profile.Height * 0.393
                });
```
_where -> Where()_

_orderby -> OrderBy()_

_select -> Select()_

위 처럼 연산자가 메소드로 바뀌었고,

from 절의 매개변수인 profile 은 각 메소드에 입력되는 람다식의 매개변수로 변환되었습니다.

메소드들은 System.Linq 네임스페이스에 정의되어 있습니다.

|    종류     | 메소드명 | 설명 | C# 쿼리식 문법 |
|   :---:   | ---| :--- | --- |
| 정렬 | OrderBy | 오름차순으로 정렬합니다. | orderby|
|  | OrderByDescending |내림차순으로 정렬합니다. | orderby ---/ descending|
|  | ThenBy | 오름차순으로 2차 정렬합니다. | orderby ---, --- |
|  | ThenByDescending | 내림차순으로 2차 정렬합니다.| orderby ---, --- descending |
|  | Reverse |컬렉션 요소의 순서를 거꾸로 뒤집습니다. | |
| 집합  |Distinct |중복을 제거합니다. ||
|       |Except |두 컬렉션 사이의 차집합을 반환합니다.| |
|       |Intersect|두 컬렉션 사이의 교집합을 반환합니다. | |
|       |Union |두 컬렉션 사이의 합집합을 반환합니다. | |
| 필터링 |OfType|메소드의 형색 매개변수로 형식 변환이 가능한 값들만 추출합니다. | |
|       |Where |필터링 할 조건 함수를 통과하는 값들만 반환합니다.| where|
| 수량 연산 |All |임의의 조건을 모든 요소가 만족시키는지 검사하고 true/false 로 반환합니다. | |
|            |Any |임의의 조건을 만족하는 요소가 하나라고 있는지 검사하고 true/false 로 반환합니다. | |
|            |Contains |임의의 요소를 포함하고 있는지 검사하고 true/false 를 반환합니다. | |
| 데이터 추출 | Select|값을 추출하여 시퀀스를 만듭니다. | select|
|            | SelectMany|여러 개의 데이터 원본으로부터 값을 추출하여 하나의 시퀀스를 만듭니다. 여러개의 from 절을 사용합니다. | |
|  데이터 분할 |Skip |시퀀스에서 지정한 위치까지 요소들을 건너뜁니다. | |
|  |SkipWhile | 조건 함수를 만족하는 요소들을 건너뜁니다. | |
|  |Take |시퀀스에서 지정한 요소까지 요소들을 취합니다. | |
|  |TakeWhile | 조건 함수를 만족하는 요소들을 취합니다. | |
|데이터 결합  |Join |공통 특성을 가진 서로 다른 두개의 데이터 소스의 객체를 연결합니다. 공동 특성을 키로 삼아, 키가 일치하는 두 객체를 쌍으로 추출합니다. |join --- in --- on --- eqauls |
|            |GroupJoin |Join 연산자와 같은 일을 하되, 결과를 그룹으로 만들어 넣습니다. | join --- in --- on --- eqauls --- into ---|
|그룹화 |GroupBy|공통된 특성을 가지는 요소들을 각 그룹으로 묶습니다. |group --- by / group --- by --- into --- |
|            |ToLookup |키 선택 함수를 이용해 골라낸 요소들을 Lookup 형식의 객체에 삽입합니다. (하나의 키에 여러 개의 객체를 대응시킬 때 사용합니다.) | |
|생성 |DefaultEmpty |빈 컬렉션을 기본값이 할당된 싱글턴 컬렉션으로 바꿉니다. | |
|            |Empty |비어 있는 컬렉션을 반환합니다. | |
|            |Range |일정 범위의 숫자 시퀀스를 담고 있는 컬렉션을 생성합니다. | |
|            |Repeat |같은 값이 반복되는 컬렉션을 생성합니다. | |
|동등 여부 평가|SequenceEqual |두 시퀀스가 서로 일치하는지 검사합니다. | |
|요소 접근  |ElementAt |컬렉션으로부터 임의의 인덱스에 존재하는 요소를 반환합니다. | |
|           |ElementAtOrDefault |컬렉션으로 부터 임의의 인덱스에 존재하는 요소를 반환하되, 인덱스가 컬렉션의 범위를 벗어난다면 기본값을 반환합니다. | |
|           |First |컬렉션의 첫 번째 요소를 반환합니다. 매개변수로 조건식이 온다면 조건식을 만족시키는 첫 번째 요소를 반환합니다. | |
|           |FirstOrDefault |First 연산자와 같은 기능을 하되, 반환할 값이 없으면 기본값을 반환합니다. | |
|           |Last |컬렉션의 마지막 인덱스인 요소를 반환합니다. 조건식이 매개변수로 입력된다면 조건식을 만족하는 마지막 요소를 반환합니다. | |
|           |LastOrDefault |Last 연산자와 같은 기능을 하되, 반환할 값이 없는 경우 기본값을 반환합니다. | |
|           |Single |컬렉션의 유일한 요소를 반환합니다. 조건식이 매개변수로 입력될 경우 조건식을 만족하는 유일한 값을 반환합니다. | |
|           |SingleOrDefault |Single 연산자와 같은 기능을 하되, 반환할 값이 없거나 유일한 값이 아닌 경우 주어진 기본값을 반환합니다. | |
|형식 변환  |AsEnumerable |매개 변수를 IEnumerable 혀익으로 변환하여 반환합니다. | |
|           |AsQueryable |IEnumerable 객체를 IQueryable 형식으로 변환합니다. | |
|           |Cast |컬렉션의 요소들을 특정 형식으로 변환합니다. |범위 변수를 선언할 때 명시적으로 형식을 지정하면 됩니다.(e.g. from _Profile_ profile in arrProfiles) |
|           |OfType |특정 형식으로 변환할 수 있는 값만을 걸러냅니다. | |
|           |ToArray |컬렉션을 배열로 변환합니다. 강제로 쿼리를 실행합니다. | |
|           |ToDictionary |키 선택 함수에 근거해 컬렉션의 요소를 Dictionary 에 삽입합니다. 강제로 쿼리를 실행합니다. | |
|           |ToList |컬렉션을 List<(Of <(T)>)> 형식으로 변환합니다. 강제로 쿼리를 실행합니다. | |
|           |ToLookup |키 선택 함수에 근거해 컬렉션의 요소를 Lookup 에 삽입합니다. | |
|연결  |Concat | 두 시퀀스를 하나의 시퀀스로 연결합니다.| |
|집계  |Aggregate |컬렉션의 각 값에 대해 사용자가 정의한 집계 연산을 수행합니다. | |
|  |Average |컬렉션의 각 값에 대한 평균을 계산합니다.| |
|  |Count |컬렉션에서 조건에 부합하는 요소의 개수를 셉니다. | |
|  |LongCount |Count 와 같은 기능을 하되, 매우 큰 컬렉션을 대상으로 합니다. | |
|  |Max |컬렉션에서 가장 큰 값을 반환합니다. | |
|  |Min |컬렉션에서 가장 작은 값을 반환합니다. | |
|  |Sum |컬렉션 내 값의 합을 계산합니다. | |

표준 LINQ 연산 메소드와 C# 의 쿼리식에서 지원하는 것을 비교해 볼 수 있습니다.

위 메소드와 LINQ 쿼리식을 함께 사용하는 방법도 있습니다.

```C#
var profiles = from profile in arrProfiles
                where profile.Height < 180
                select profile;
```
위 예제에서 profiles 은 180 미만의 데이터만 갖고 있는 IEnumerable<Profile> 형식의 컬렉션이기 때문에 LINQ 를 사용할 수 있습니다.

위 LINQ 결과를 이용한 평균을 구해보겠습니다.
<BR><bR>

__Average 메소드__
```c#
double Average = profiles.Average(profile => profile.Height);
```
위 두개의 코드를 한 문장으로 묶을 수도 있습니다.<BR><bR>

```C#
double Average = (from profile in arrProfiles
                    where profile.Height < 180
                    select profile).Average(profile => profile.Height);
```
<br><Br>


프로필 중 키를 175 이상과 175 미만의 그룹으로 나누고,

각 그룹에서 키가 가장 큰 사람, 가장 작은 사람의 수를 뽑는 코드를 작성해보겠습니다.

```C#
var profilesStat = from profile in arrProfiles
                group profile by profile.Height < 175 into s
                select new
                {
                    Group = s.Key == true ? "175미만" : "175이상",
                    Max = s.Max(profile => profile.Height),
                    Min = s.Min(profile => profile.Hieght),
                    Average = s.Average(profile => profile.Height);
                };
```

groupby 문에서 arrProfiles를 175 미만의 그룹 s와 아닌 그룹으로 나눕니다.

그리고 select 문에서 두개의 그룹을 profilesStat 시퀀스로 반환합니다.

