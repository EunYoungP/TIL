# 2022 Sort
# 정렬 <br><br>


## :blush: 책 '실전대비 C 알고리즘 인터뷰' 의 일부 내용을 정리한 문서입니다.<br><br>


 
## :blue_book: 강의소개
:point_right: 
정렬 알고리즘에 대한 내용을 설명합니다.

* 정렬 유형

* 정렬 알고리즘 비교

* 정렬 문제
<br><br>


## part 1. 정렬 유형

정렬은 배열의 원소를 오름차순 또는 내림차순으로 배치하는 과정입니다.

다양한 정렬 알고리즘의 값은 비교함수 중 하나로 비교됩니다. <br><br>

__비교함수__

```c++
void less(int value1, int value2)
{
    return value1 < value2;
}
```
less 함수를 사용하면 내림차순으로 정렬합니다.


```c++
void more(int value1, int value2)
{
    return value1 > value2;
}
```
more 함수를 사용하면 오름차순으로 정렬합니다.<br><Br>

__정렬 유형__

* 내부 정렬

    모든 원소를 한 번에 메모리로 읽어 들여 정렬을 수행합니다.

    * 선택 정렬

    * 삽입 정렬

    * 버블 정렬

    * 퀵 정렬

    * 병합 정렬

* 외부 정렬

    데이터 크기가 너무 커, 전체 데이터를 여러 덩어리로 나누어 정렬합니다.

    * 병합 정렬
<br><Br>


## 버블 정렬

인접한 값의 각 쌍을 비교합니다.

__특징__

* 가장 느린 알고리즘

* 작은 데이터에 사용하며 구현하기 쉽습니다.

* 첫 번째 패스(전체 데이터를 일거나 쓰는 것)가 끝나면 가장 큰 값이 가장 오른쪽에 오며, 배열을 완전히 정렬하는데 n번의 패스를 수행합니다.

```c++
void BubbleSort(int arr[], int size)
{
    for(int i = 0; i < size - 1; i++)
    {
        for(int j = 0; j < size - i - 1; j++)
        {
            if(arr[j] > arr[j+1])
            {
                int temp = arr[j];
                arr[j] = arr[j+1];
                arr[j+1] = temp;
            }
        }
    }
}
```
오름차순으로 버블정렬을 실행했습니다.<br><br>

__최악의 성능 시간 복잡도__ : __O(n^2)__

__평균 성능 시간 복잡도__ : __O(n^2)__

__공간 복잡도__ : __O(1)__ 

- temp로 선언된 하나의 임시 변수만이 필요
<br><Br>

## 향상된 버블 정렬

__거의 정렬되어 있는 배열__ 에 향상된 버블 정렬을 사용하면 이 알고리즘의 최선의 성능이 향상됩니다.

이 경우 패스는 단 한 번이면 되고 __최선의 성능 복잡도__ 는 __O(n)__ 이 됩니다.


```c++
void BubbleSort2(int arr[], int size)
{
    int swapped = 1;

    for(int i = 0; i < size -1 && swapped; i++)
    {
        swapped = 0;
        for(int j = 0; j < size -i -1; j++)
        {
            if( arr[j] > arr[j+1])
            {
                int temp = arr[j];
                arr[j] = arr[j+1];
                arr[j+1] = temp;

                swapped = 1;
            }
        }
    }
}
```
하나의 패스에서 더 이상 교환할 것이 없다면 정렬이 완료된 것입니다.

swapped 가 1 로 변환되는 부분은 교환이 실행되는 분기 속에 포함되어 있습니다.

따라서 swapped가 다음 패스로 넘어가는 외부 반복문의 i가 증가 될때까지 0 이라면,

한 패스동안 교환이 실행되지 않았으므로 정렬이 완료되었음을 알 수 있습니다.<br><br>


__최선의 성능 시간 복잡도 : O(n)__

* 패스가 단 한 번이면 되기 때문에 내부 반복문이 한 번만 실행되고 종료됩니다.

__최악의 성능 시간 복잡도 : O(n^2)__

__평균 성능 시간 복잡도 : O(n^2)__

__공간 복잡도 : O(1)__

__배열이 거의 정렬된 경우 시간 복잡도 : O(n)__

<br><br>

## 삽입 정렬 InsertionSort

시간 복잡도는 O(n^2)로 버블 정렬과 같지만, 실제 성능은 조금 더 낫습니다.

e.g. 카드게임 

<Br>


```c++
void InsertionSort(int arr[], int size)
{
    int seelcted;
    for(int i = 1; i < size; i++)
    {
        // 추가할 원소 설정
        selected = arr[i];
        // 오름차순 비교 정렬 함수 more
        for(int j = i; j > 0 && more(arr[j-1], selected); j--)
        {
            
        }
    }
}
```