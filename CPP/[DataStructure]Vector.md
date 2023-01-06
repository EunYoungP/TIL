# __Vector__

C++로 구현해본 data structure vector 

## __cpp Vector 구현코드 분석__

```c++
#include <cstddef>
#include <utility>

// 어떤 형식이든 vector에 담을 수 있게 만들어줍니다.
template <typename T>
class  Vector {
private:
  static constexpr size_t DEFAULT_CAP = 32;
  T* arr;
  size_t m_size;
  size_t m_capacity;

public:
// 생성자
    Vector(size_t cap = DEFAULT_CAP) : arr(new T[cap]), m_size(0), m_capacity(cap) {}
    Vector(const Vector& v) : arr(new T[v.m_capacity]), m_size(v.m_size), m_capacity(v.m_capacity) 
    {
        for (size_t i = 0; i < m_size; i++)
        arr[i] = v[i];
    }
    // 복사생성자
    Vector(Vector&& v) : arr(std::move(v.arr)), m_size(std::move(v.m_size)), m_capacity(std::move(v.m_capacity))
    {
        v.arr = nullptr;
        v.m_size = 0;
        v.m_capacity = 0;
    }

    // 소멸자
    ~Vector() { delete[] arr; }

    // 연산자 오버로딩
    Vector& operator=(const Vector& other) 
    {
        if (this != &other) {
        if (m_capacity < other.m_capacity) 
        {
            delete[] arr;
            m_capacity = other.m_capacity;
            arr = new T[m_capacity];
        }
        m_size = other.m_size;
        for (size_t i = 0; i < m_size; i++)
            arr[i] = other.arr[i];
        }
    return *this;
    }

    // 연산자 오버로딩
    Vector& operator=(Vector&& other) 
    {
        std::swap(arr, other.arr);
        std::swap(m_size, other.m_size);
        std::swap(m_capacity, other.m_capacity);
        other.m_size = 0;
        return *this;
    }

  T* begin() const { return arr; }

  T* end() const { return arr + m_size; }

  T& front() { return arr[0]; }
  const T& front() const { return arr[0]; }

  T& back() { return arr[m_size - 1]; }
  const T& back() const { return arr[m_size - 1]; }

  T& operator[](size_t idx) { return arr[idx]; }
  const T& operator[](size_t idx) const { return arr[idx]; }

  void push_back(const T& val) {
    if (m_size >= m_capacity) {
      if (m_capacity < DEFAULT_CAP)
        m_capacity = DEFAULT_CAP;
      else
        m_capacity *= 2;
      T *t_arr = new T[m_capacity];
      for (size_t i = 0; i < m_size; i++)
        t_arr[i] = arr[i];
      delete[] arr;
      arr = t_arr;
    }
    arr[m_size++] = val;
  }
  void push_back(T&& val) {
    if (m_size >= m_capacity) {
      if (m_capacity < DEFAULT_CAP)
        m_capacity = DEFAULT_CAP;
      else
        m_capacity *= 2;
      T *t_arr = new T[m_capacity];
      for (size_t i = 0; i < m_size; i++)
        t_arr[i] = arr[i];
      delete[] arr;
      arr = t_arr;
    }
    arr[m_size++] = std::move(val);
  }

  void pop_back() { m_size = m_size > 0 ? m_size - 1 : 0; }

  void resize(size_t n, T val = T()) {
    T *t_arr = new T[n];
    m_size = m_size < n ? m_size : n;
    m_capacity = n;
    for (size_t i = 0; i < m_size; i++)
      t_arr[i] = arr[i];
    for (size_t i = m_size; i < m_capacity; i++)
      t_arr[i] = val;
    delete[] arr;
    arr = t_arr;
    m_size = n;
  }

  void reserve(size_t n) {
    if (n <= m_capacity)
      return;
    T *t_arr = new T[n];
    for (size_t i = 0; i < m_size; i++)
      t_arr[i] = arr[i];
    delete[] arr;
    arr = t_arr;
    m_capacity = n;
  }

  void swap(Vector& other) {
    std::swap(arr, other.arr);
    std::swap(m_size, other.m_size);
    std::swap(m_capacity, other.m_capacity);
  }

  size_t capacity() const { return m_capacity; }
  size_t size() const { return m_size; }
  bool empty() const { return m_size == 0; }
  void clear() { m_size = 0; }

  bool operator==(const Vector& other) const {
    if (m_size != other.m_size)
      return false;
    for (size_t i = 0; i < m_size; i++)
      if (arr[i] != other[i])
        return false;
    return true;
  }
  bool operator!=(const Vector& other) const { return !(*this == other); }
  bool operator< (const Vector& other) const {
    bool is_all_same = true;
    size_t min_size = m_size < other.m_size ? m_size : other.m_size;
    size_t idx = 0;
    for (; idx < min_size; idx++) {
      if (arr[idx] != other[idx]) {
        is_all_same = false;
        break;
      }
    }

    if (is_all_same) {
      if (m_size < other.m_size)
        return true;
    } else {
      if (arr[idx] < other[idx])
        return true;
    }
    return false;
  }
  bool operator<=(const Vector& other) const { return !(other < *this); }
  bool operator> (const Vector& other) const { return (other < *this); }
  bool operator>=(const Vector& other) const { return !(*this < other); }
};
```
<br><Br>

## __내 코드__

__헤더파일__
```c++
 
#include <string>
 
//-------------------------------------------------------------------------------------------------
// MyVector 가 관리하는 오브젝트
//-------------------------------------------------------------------------------------------------
struct MyObject
{
    int _id;

    MyObject(){}
    MyObject(int id) : _id(id){}
};
 
//-------------------------------------------------------------------------------------------------
// MyVector 클래스.
//-------------------------------------------------------------------------------------------------
class MyVector
{
private: 
    MyObject* myObjects;
    size_t vectorSize;
    size_t vectorCapacity;

    void GrowCapacity();

    void SwapObject(size_t swapL, size_t swapR);

    void Sort(size_t lower, size_t upper);

public:
    class iterator
    {

    private:
        friend class MyVector;
    };
 
public: 
 
    // Constructor.
    MyVector(int capacity);
 
    // Copy constructor.
    MyVector(const MyVector& other);
 
    // Assignment operator.
    MyVector& operator=(const MyVector& other);

    // Destructor.
    ~MyVector();
 
public: 
 
    int GetCapacity() const;
 
    int GetSize() const;
 
    void Add(int id);

    MyObject* FindById(int MyObjectId) const;

    void TrimToSize();

    MyObject& operator[](size_t index);

    std::string ToString() const;

    void RemoveAll(int MyObjectId);

    MyVector* GroupById(int* numGroups);
};
```
<br><Br>

__CPP파일__
```c++
#include "MyVector.h"
using namespace std;
#include <iostream>
#include <sstream>

MyVector::MyVector(int capacity)
{
	myObjects = new MyObject[capacity];
	vectorSize = 0;
	vectorCapacity = capacity;
}

// Copy constructor.
MyVector::MyVector(const MyVector& other)
{
	myObjects = new MyObject[other.vectorSize];
	for (int i = 0; i < other.vectorSize; i++)
	{
		myObjects[i] = other.myObjects[i];
	}
	vectorSize = other.vectorSize;
	vectorCapacity = other.vectorCapacity;
}

// Assignment operator.
MyVector& MyVector::operator=(const MyVector& other)
{
	if (this != &other)
	{
		if (other.vectorCapacity > this->vectorCapacity)
		{
			delete[] myObjects;
			myObjects = new MyObject[other.vectorCapacity];
			vectorCapacity = other.vectorCapacity;
		}

		this->vectorSize = other.vectorSize;
		for (int i = 0; i < other.vectorSize; i++)
		{
			myObjects[i] = other.myObjects[i];
		}
	}
	return *this;
}

// Destructor.
MyVector::~MyVector()
{
	delete[] myObjects;
	myObjects = nullptr;
}

int MyVector::GetCapacity() const
{
	return this->vectorCapacity;
}

int MyVector::GetSize() const
{
	return this->vectorSize;
}

// 지정된 ID로 새 MyObject 인스턴스를 만들고 이 벡터의 끝에 추가합니다.
void MyVector::Add(int id)
{
	if (vectorSize >= vectorCapacity)
	{
		vectorCapacity *= 2;
	}

	MyObject* tempArr = new MyObject[vectorCapacity];
	for (int i = 0; i < vectorSize; i++)
	{
		tempArr[i] = myObjects[i];
	}
	delete[] myObjects;
	myObjects = tempArr;

	MyObject* newObj = new MyObject(id);
	myObjects[vectorSize++] = *newObj;
}

// 지정된 ID를 가진 MyObject 인스턴스의 첫 번째 항목을 반환합니다.
MyObject* MyVector::FindById(int MyObjectId) const
{
	for (int i = 0; i < vectorSize; i++)
	{
		if (myObjects[i]._id == MyObjectId)
			return myObjects + i;
	}
	return nullptr;
}

// 이 벡터의 용량을 현재 크기로 자릅니다.
void MyVector::TrimToSize()
{
	if (vectorSize >= vectorCapacity)
		return;

	MyObject* tempArr = new MyObject[vectorSize];
	for (int i = 0; i < vectorSize; i++)
	{
		tempArr[i] = myObjects[i];
	}

	delete[] myObjects;
	myObjects = tempArr;
	vectorCapacity = vectorSize;
}

// 지정된 인덱스에 있는 MyObject 인스턴스를 반환합니다.
MyObject& MyVector::operator[](size_t index)
{
	return myObjects[index];
}

// 벡터의 문자열 표현을 반환합니다.
std::string MyVector::ToString() const
{
	std::stringstream ss;
	for (int i = 0; i < vectorSize; i++)
	{
		if (i == 0)
		{
			ss << " ";
		}
		ss << myObjects[i]._id;
	}
	return ss.str();
}

// 이 벡터에서 지정된 ID를 가진 모든 MyObject 인스턴스를 제거합니다.
void MyVector::RemoveAll(int MyObjectId)
{
	for (int i = 0; i < vectorSize; i++)
	{
		if (myObjects[i]._id == MyObjectId)
		{
			for (int j = i; j < vectorSize - 1; j++)
			{
				myObjects[j] = myObjects[j + 1];
			}
		}
	}
}

void MyVector::SwapObject(size_t swapL, size_t swapR)
{
	MyObject tempObj = myObjects[swapL];
	myObjects[swapL] = myObjects[swapR];
	myObjects[swapR] = tempObj;
}

void MyVector::Sort(size_t lower, size_t upper)
{
	if (upper <= lower)
		return;

	// quick sort
	size_t start = lower;
	size_t stop = upper;
	size_t pivot = myObjects[lower]._id;

	while (lower < upper)
	{
		while (myObjects[lower]._id <= pivot)
		{
			lower++;
		}
		while (myObjects[upper]._id >= pivot)
		{
			upper--;
		}

		if (lower < upper)
		{
			SwapObject(lower, upper);
		}

		SwapObject(upper, start);
		Sort(start, upper - 1);
		Sort(upper, stop);
	}
}

// 새로 할당된 MyVector 객체 배열을 반환합니다.
// 각각의 요소는 MyObject 구조체의 동일한 "_id" 값을 가집니다.
// 'numGroups'는 out 매개변수이며 해당 값은 다음으로 설정되어야 합니다.
// 반환할 MyVector 배열의 크기입니다.
MyVector* MyVector::GroupById(int* numGroups)
{
	if (myObjects == nullptr)
		return;

	// sort + unique
	Sort(0, vectorSize - 1);

	int count = 0;
	int tempId = myObjects[0]._id;
	for (int i = 1; i < vectorSize; i++)
	{
		if (tempId != myObjects[i]._id)
		{
			count++;
		}
		else
		{

		}
	}
}
```

##
## __정리해야할 개념__

* 복사 생성자
* 연산자 오버로딩
* && 우측값 레퍼런스
* const, define 차이점
* ref, out 매개변수
* explicit
* 이동생성자
* 이동 할당 연산자

---
참고자료

[Vector 구현하기_1](https://lazyren.github.io/devlog/vector.html#%EA%B5%AC%ED%98%84)

[vector 구현하기_2](https://oceancoding.blogspot.com/2020/12/c-custom-stdvector.html)

[Vector 개념](http://www.cplusplus.com/reference/vector/vector/)
