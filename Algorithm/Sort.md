# 2022 Sort
# 정렬 <br><br>


## :blush: 책 '실전대비 C 알고리즘 인터뷰' 의 내용을 정리한 문서입니다.<br><br>


 
## :blue_book: 강의소개
:point_right: 
정렬 알고리즘에 대한 내용을 설명합니다.

* 정렬 유형

* 정렬 알고리즘 비교

* 정렬 문제
<br><br>


## __part 1. 정렬 유형__

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


## 1. 버블 정렬

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

## 2. 삽입 정렬 InsertionSort

시간 복잡도는 O(n^2)로 버블 정렬과 같지만, 실제 성능은 조금 더 낫습니다.

e.g. 카드게임 

<Br>


```c++
void InsertionSort(int arr[], int size)
{
    int selected;
    for(int i = 1; i < size; i++)
    {
        // 추가할 원소 설정
        selected = arr[i];
        // 오름차순 비교 정렬 함수 more
        for(int j = i; j > 0 && more(arr[j-1], selected); j--)
        {
            arr[j] = arr[j-1];
        }
        arr[j] = selected;
    }
}
```

내부 반복문의 조건문에 more 함수로 인해 새로 추가되는 원소가 오름차순으로 정렬되는 자리까지 반복되고 종료됩니다.

외부 반복문에서 정렬된 배열의 크기가 1씩 증가합니다.

<br>

__시간 복잡도__

* __최악의 성능 시간 복잡도 : O(n^2)__

* __평균 성능 복잡도 : O(n^2)__

* __최선의 성능 시간 복잡도 : O(n)__

* __공간 복잡도 : O(1)__

<br><br>

## 3. 선택 정렬 SelectionSort

정렬되지 않은 배열에서 가장 큰 값을 찾아 맨 뒤에 놓습니다.

버블, 선택 정렬과 같은 이차 시간 복잡도지만, 비교 횟수가 적어서 더 나은 성능을 보여줍니다.

정렬이 뒤에서부터 이루어집니다.

<br><br>

선택 정렬 구현 코드

```c++
void SelectionSort(int arr[], int size)
{
    // 최댓값의 인덱스
    int max;
    for(int i = 0; i < size -1; i++)
    {
        max = 0;
        for(int j = 1; j <= size-1-i; j++)
        {
            if(arr[j] > arr[max])
            {
                max = j;
            }
        }
        int temp = arr[size -1 -i];
        arr[size-1-i] = arr[max];
        arr[max] = temp;
    }
}
```

* 내부 반복문에서 배열의 가장 큰 값을 찾아 배열의 끝에 배치합니다.

* 뒤에서 부터 정렬하는 코드입니다.<br><br>


내림차순 선택 정렬 알고리즘 구현 코드
```c++
void SelectionSort2(int arr[], int size)
{
    int max;
    for(int i = 0; i < size -1; i++)
    {
        max = 0;
        for(int j = 1; j <= size -1 -i; j++)
        {
            if(arr[j] > arr[max])
            {
                max = j;
            }
        }
        int temp = arr[i];
        arr[i] = arr[max];
        arr[max] = temp;
    }
}
```
<br>

앞 인덱스부터 선택 정렬
```c++
void SelectionSort3(int arr[], int size)
{
    int min;
    for(int i = 0; i < size -1; i++)
    {
        min = 0;
        for(int j = i+1; j < size; j++)
        {
            if( arr[j] < arr[min])
            {
                min = j;
            }
        }
        int temp = arr[i];
        arr[i] = arr[min];
        arr[min] = temp;
    }
}
```


<br><br>


## 4. 병합 정렬 MergeSort
<br>

__개념 & 특징__

각 단계에서 입력을 반으로 나눠 재귀 호출을 합니다.

최종 정렬된 출력으로 결과를 병합합니다.

정렬되지 않은 배열과 같은 크기의 추가 공간이 필요합니다.

* -> 큰 배열의 정렬에 사용하는 것은 지양합니다.

* -> 연결 리스트를 정렬하는 데 적합합니다.

<br>


__병합 정렬 코드 구현__
```c++
// 분할된 배열을 정렬하여 병합하는 기능
void Merge(int *arr, int *tempArray, int lowerIndex, int middleIndex, int upperIndex)
{
    int lowerStart = lowerIndex;
    int lowerStop = middleIndex;
    int upperStart = middleIndex + 1;
    int upperStop = upperIndex;
    int count = lowerIndex;

    while(lowerStart <= lowerStop && upperStart <= upperStop)
    {
        if(arr[lowerStart] < arr[upperStart])
        {
            tempArray[count++] = arr[lowerStart++];
        }
        else
        {
            tempArray[count++] = arr[upperStart++];
        }
    }

    while(lowerStart <= lowerStop)
    {
        tempArray[count++] = arr[lowerStart++];
    }

    while(upperStart <= upperStop)
    {
        tempArray[count++] = arr[upperStart++];
    }

    for(int i = lowerIndex; i < upperIndex; i++)
    {
        arr[i] = tempArray[i];
    }
}

// 배열을 반으로 최대한 나누는 기능
void MergeSortUtil(int *arr, int *tempArry, int lowerIndex, int upperIndex)
{
    if(lowerIndex >= upperIndex)
        return;

    int middleIndex = (lowerIndex + upperIndex) / 2;
    MergeSortUtil(arr, tempArray, lowerIndex, middleIndex);
    MergeSortUtil(arr, tempArray, middleIndex + 1, upperIndex);
    Merge(arr, tempArray, lowerIndex, middleIndex, upperIndex);
}

// 메인 함수
void MergeSort(int *arr, int size)
{
    int tempArray[size];
    MergeSortUtil(arr, tempArray, 0, size);
}
```
* 최소 인덱스와 최대 인덱스의 합을 2로 나눈 중간값을 구합니다.

* 최소값과 중간값, 중간값과 최대값을 넘겨주는 재귀 함수로 더 이상 나눌 수 없을 때까지 반복합니다.

* 두개로 나눠진 배열의 원소를 하나씩 비교해 임시 공간에 추가해줍니다.

* 하나의 분할 배열의 원소들이 모두 추가되었다면 남은 부분 배열의 원소들을 모두 임시 공간에 넣어줍니다.

* 정렬된 임시 공간의 원소들을 본 배열에 복사해줍니다.

<br><Br>

__시간 복잡도__

* 최선의 성능 시간 복잡도 : O(nlongn)

* 평균의 성능 시간 복잡도 : O(nlogn)

* 최악의 성능 시간 복잡도 : O(nlogn)

    각 단계마다 입력을 반으로 나누기 떄문에 -> 2^c(실행수) = n(입력수) : log(2)(n) = c

    각 단계를 병합하는 데 선형 시간이 필요하기 때문에 n번을 곱해 -> nlogn

* 공간 복잡도 : O(n)

    * -> 정렬할 공간과 같은 크기의 추가 공간이 필요합니다.

<br><Br>


## 5. 퀵 정렬 QuickSort
<br>

__개념 & 특징__

재귀 알고리즘으로 각 단계의 __피벗__ 을 선택하여 정렬합니다.

피벗은 랜덤으로 선택할 수도 있고 특정 값을 선택할 수도 있습니다.

매우 적은 공간을 사용합니다.

다른 원소와의 비교만으로 정렬을 수행하는 __비교 정렬__ 속합니다.

문제를 작은 2개의 문제로 분리하고 각각 해결한 다음, 결과를 모아 원래 문제를 해결하는 __분할 정복 알고리즘__ 입니다.

병합 정렬과달리 배열을 __비균등__ 하게 분할합니다.
 <br><Br>

__시간 복잡도__

* 최악의 성능 복잡도 : O(n^2)
* 평균의 성능 복잡도 : O(nlogn)
* 최선의 성능 복잡도 : O(nlogn)
* 공간 복잡도 : O(logn)
    * 필요한 추가 공간이 적습니다.
<br><br>

__퀵 정렬 구현 코드__ (피봇 : 첫번째 인덱스)
```c++
void QuickSortUtil(int arr[], int lower, int upper)
{
    if(upper <= lower)
        return;

    // 피봇을 첫번째 요소로 설정
    int pivot = arr[lower];
    int start = lower;
    int stop = upper;

    while(lower < upper)
    {
        while(arr[lower] <= pivot)
        {
            lower++;
        }

        while(arr[upper] > pivot)
        {
            upper--;
        }

        if(lower < upper)
        {
            swap(arr, upper, lower);
        }
    }

    swap(arr, upper, start);    // upper는 pivot의 위치입니다.
    QuickSortUtil(arr, start, upper-1);
    QuickSotyUtil(arr, upper+1, stop);
}

void QuickSort(int arr[], int size)
{
    QuickSortUtil(arr, 0, size-1);
}
```
<br>
<img src="https://user-images.githubusercontent.com/80774412/209871026-32291832-0f6d-41e4-8c50-a09a886b154a.png" title="QuickSort"></img>

__퀵정렬 구현단계__

* __분할 Divide__ : 입력 배열을 pivot 기준으로 비균등하게 2개의 부분 배열로 분할합니다.

* __정복 Conquer__ : 부분 배열을 정렬합니다. 부분 배열의 크기가 충분히 작지 않으면 __재귀 호출__을 이용해 다시 분할 정복 방법을 적용합니다.

* __결합 Combine__ : 정렬된 부분 배열들을 하나의 배열에 합병합니다.

<br><br>

### __퀵 선택__

퀵 선택 알고리즘은 실제로 전체 배열을 정렬하지 않고, 

정렬된 배열에서 n번째 위치에 있는 원소를 찾는 데 사용합니다.

n번째 원소가 있는 부분배열 영역에만 집중합니다.

퀵 정렬과 비슷하게 작동합니다.

<br>

__퀵 선택 구현 코드__
```c++
void QuickSelectUtil(int arr[], int lower, int upper, int n)
{
    if(upper <= lower)
        return;

    int pivot = arr[lower];
    int start = lower;
    int stop = upper;

    while(lower < upper)
    {
        while(lower < upper && arr[lower] <= pivot)
        {
            lower++;
        }
        while(lower <= upper && arr[upper] > pivot)
        {
            upper--;
        }
        if(lower < upper)
        {
            swap(arr, upper, lower);
        }
    }

    swap(arr, upper, start);    // upper는 pivot의 위치입니다.
    if( n < upper)
    {
        QuickSelectUtil(arr, start, upper-1, n);
    }
    if( n > upper)
    {
        QuickSelectUtil(arr, upper+1, stop, n);
    }
}

int QuickSelect(int *a, int count, int index)
{
    QuickSelectUtil(a, 0, count-1, index-1);
    return a[index-1];
}

int main()
{
    int arr[10] = {4,5,3,2,6,7,1,8,9,10};
    std::cout << "5번째 요소 : " << QuickSelect(arr, sizeof(arr)/sizeof(int), 5);
}
```
<br><Br>

__시간 복잡도__

* 최악의 성능 복잡도 : O(n^2)
* 평균의 성능 복잡도 : O(n)
* 최선의 성능 복잡도 : O(n)
* 공간 복잡도 : O(logn)
    * 필요한 추가 공간이 적습니다.
<br><br>


## 6. 버킷 정렬 BucketSort

가장 간단하고 효율적인 정렬 형태

사전 정의된 데이터 범위에 대한 __엄격한 요구사향__ 이 존재합니다.

* 데이터 집합이 균등한 분포를 가집니다.

* 데이터 집합의 범위를 알고있어야 합니다.

__계수 정렬 CountingSort__ 이라고도 불립니다.

__시간 복잡도 및 분석__

k : 버킷의 수
n : 배열의 총 원소 수

* 최악의 성능 시간복잡도 : O(n + k)
* 평균의 성능 시간복잡도 : O(n + k)
* 최악의 공간 복잡도 : O(k)
* 자료구조 : 배열

```c++
// n-> 데이터 개수
void BucketSort(int array[], int n, int range)
{
    int count[range];

    for(int i = 0; i < range; i++)
    {
        count[i] = 0;
    }

    for(int i = 0; i < n; i++)
    {
        count[array[i]]++;
    }

    int j = 0;
    for(int i = 0; i < range; i++)
    {
        while(count[i] > 0)
        {
            array[j++] = i;
            count[i]--;
        }
    }
}

void main()
{
    int arr[10] = {4,5,3,2,6,7,1,8,9,10};
    BucketSort(arr, sizeof(arr)/sizeof(int), 10);
}
```
__구현 단계__

* 카운트를 저장하는 배열을 만듭니다.
* 카운트 배열의 원소를 0으로 초기화합니다.
* 입력 배열에 해당하는 인덱스를 증가시킵니다.
* 카운트 배열에 저장된 정보를 결과 배열에 저장합니다.

<br><br>

## 일반화된 버킷 정렬

버킷에 들어가는 원소들이 유일하거나 균등하지 않은 경우에 사용합니다.

여러 원소가 존재하는 버킷의 데이터는 삽입 정렬과 같은 간단한 정렬알고리즘으로 정렬합니다.

```c++
void BucketSort(int array[], int n)
{
    List<string> bucket[n];

    // 버킷에 기준되는 키들과 키에 맞는 데이터들을 넣어줍니다.
    for(int i = 0; i < n; i++)
    {
        int index = array[i] * n;
        bucket[index].add(array[i]);
    }

    // 버킷마다 데이터들을 정렬시킵니다.
    for(int i = 0; i < n; i++)
    {
        sort(bucket[i].begin(), bucekt[i].end());
    }

    // 정렬된 카운트 배열의 데이터들을 결과 배열에 순차로 저장합니다.
    int index = 0;
    for(int i = 0; i < n; i++)
    {
        for(int j = 0; j < bucket[i].size(); j++)
        {
            array[index++] = bucket[i][j];
        }
    }
}

void main()
{
    string array[10] = {"Ape", "Apple", "Bus", "Bucket", "Cat", "Zebra"}
    BucketSort(array, sizeof(array)/sizeof(string));
}
```

위 코드는 버킷 정렬을 구현시킨 코드입니다.

현재 데이터들이 정수나 실수로 이루어진 경우에는 키값을 설정할 수 있지만,

문자열로 이루어진 키값들을 다루는건 따로 다루겠습니다.


<br><Br>

## 7. 힙 정렬 Heap Sort 

__시간 복잡도 및 분석__

* 최악의 성능 시간복잡도 : O(nlogn)
* 평균의 성능 시간복잡도 : O(nlogn)
* 최악의 공간 복잡도 : O(1)
* 자료구조 : 배열


<br><br>

## 8. 트리 정렬 Tree Sort 
 
이진 탐색 트리의 중위 순회 inorder traversal 는 정렬 알고리즘으로 볼 수도 있습니다.


__시간 복잡도 및 분석__

* 최악의 성능 시간복잡도 : O(n^2)
* 평균의 성능 시간복잡도 : O(nlogn)
* 최선의 성능 시간복잡도 : O(nlogn)
* 공간 복잡도 : O(n)

<BR><BR>


## 9. 외부 정렬 External Sort

정렬하려는 데이터가 너무 커 메모리 RAM 에 전체 데이터를 올릴 수 없을 때 사용합니다.

데이터 중 덩어리를 선택하여 메모리에서 병합 정렬을 사용해 정렬합니다.

정렬한 데이터 덩어리를 디스크에 다시 저장합니다.

각 덩어리는 자체 __큐__ 를 가지는데, 이 큐에서 정렬된 데이터 덩어리로부터 POP을 통해 데이터를 읽어옵니다.

<BR><BR>

## 10. 안정 정렬 Stable Sort

동일한 키의 두 원소가 정렬하기 전 순서대로 정렬 결과에 유지되는 것을 __안정 정렬 stable sort__ 이라고 합니다.

동일한 키의 원소를 재배열하지 않도록 보장합니다.

<br><br>

## __part 2. 정렬 알고리즘 비교__
<Br>

<img src="https://user-images.githubusercontent.com/80774412/210015541-65f2dcc1-4bdc-47c3-be7e-4fb5c6a49b0f.PNG" title="compare sorting structure"></img><BR><BR>

__퀵 정렬__

안정 정렬이 필요하지 않고, 데이터가 무작위일 때 선호합니다.<BR><BR>

__병합 정렬__

안정 정렬이 필요할 때 사용합니다.

병합 단계에서 많은 복사가 이루어지므로 일반적으로 __퀵정렬보다 느립니다.__

두개의 __정렬된 연결 리스트__ 를 병합하거나 __외부 정렬__ 에서 사용합니다.<BR><BR>

__힙 정렬__

안정 정렬이 필요하지 않습니다.

O(1)의 보조 공간 사용으로 입력이 매우 커도 예측 불가한 메모리 부족을 겪지 않습니다.<BR><BR>




