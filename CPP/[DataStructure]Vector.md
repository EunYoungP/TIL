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
public:
    class Iterator
    {
    private:

        friend class MyVector;
        MyObject* object;
        size_t index;

    public:

        Iterator(MyObject* obj = nullptr, size_t idx = 0) :object(obj), index(idx) {}

        ~Iterator() {}

        Iterator& operator++() { ++index; return *this; }

        Iterator& operator--() { --index; return *this; }

        Iterator operator+(size_t s) { return Iterator(&object[index], index + s); }

        Iterator operator-(size_t s) { return Iterator(&object[index], index - s); }

        bool operator==(const Iterator& it) const noexcept { return &object[index] == &it.object[index]; }

        bool operator!=(const Iterator& it) const noexcept { return &object[index] != &it.object[index]; }

        MyObject& operator*() const { return object[index]; }

        MyObject* operator->() const { return &object[index]; }
    };

private: // 구현에 필요한 멤버 추가 함수/변수들을 자유롭게 아래에 정의 합니다.

    MyObject* myObjects;
    size_t vectorSize;
    size_t vectorCapacity;

    MyObject& Front();

    MyObject& Back();

    Iterator Begin();

    Iterator End();

    Iterator Erase(Iterator& it);

    Iterator Erase(Iterator& start, Iterator& end);

    Iterator Insert(Iterator& it, MyObject& obj);

    bool Empty();
    void Clear();

    // 저장공간을 늘리는 함수입니다.
    void GrowCapacity(size_t growCapacity);

    // Object 배열의 값을 교환하는 함수입니다.
    void SwapObject(size_t swapL, size_t swapR);

    // MyObject 배열을 퀵정렬하는 함수입니다.
    void SortObejct(size_t lower, size_t upper);

public: // 생성자, 복사생성자, 할당연산자, 소멸자를 .cpp 파일에 구현합니다.
 
    MyVector();

    // Constructor.
    MyVector(int capacity);

    // Copy constructor.
    MyVector(const MyVector& other);
 
    // Assignment operator.
    MyVector& operator=(const MyVector& other);

    // Destructor.
    ~MyVector();
 
public: // 아래 기능 함수들을 .cpp 파일에 구현합니다.
 
    // Returns current capacity of this vector.
    int GetCapacity() const;
 
    // Returns current size of this vector.
    int GetSize() const;
 
    // Creates a new MyObject instance with the given ID, and appends it to the end of this vector.
    // 지정된 ID로 새 MyObject 인스턴스를 만들고 이 벡터의 끝에 추가합니다.
    void Add(int id);
 
    // Returns the first occurrence of MyObject instance with the given ID.
    // 지정된 ID를 가진 MyObject 인스턴스의 첫 번째 항목을 반환합니다.
    // Returns nullptr if not found.
    MyObject* FindById(int MyObjectId) const;
 
    // Trims the capacity of this vector to current size.
    // 이 벡터의 용량을 현재 크기로 자릅니다.
    void TrimToSize();
 
    // Returns the MyObject instance at the specified index.
    // 지정된 인덱스에 있는 MyObject 인스턴스를 반환합니다.
    MyObject& operator[](size_t index);
 
    // Returns string representation of the vector.
    // 벡터의 문자열 표현을 반환합니다.
    std::string ToString() const;
 
    // Remove all MyObject instances with the given ID in this vector.
    // 이 벡터에서 지정된 ID를 가진 모든 MyObject 인스턴스를 제거합니다.
    void RemoveAll(int MyObjectId);
 
    // Returns a newly allocated array of MyVector objects,
    // each of whose elements have the same "_id" value of the MyObject struct.
    // The 'numGroups' is an out parameter, and its value should be set to
    // the size of the MyVector array to be returned.
    // 새로 할당된 MyVector 객체 배열을 반환합니다.
    // 각각의 요소는 MyObject 구조체의 동일한 "_id" 값을 가집니다.
    // 'numGroups'는 out 매개변수이며 해당 값은 다음으로 설정되어야 합니다.
    // 반환할 MyVector 배열의 크기입니다.
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

MyObject& MyVector::Front()
{
	return myObjects[0];
}

MyObject& MyVector::Back()
{
	return myObjects[vectorSize - 1];
}

MyVector::Iterator MyVector::Begin()
{
	return Iterator(myObjects);
}

MyVector::Iterator MyVector::End()
{
	return Iterator(myObjects, vectorSize);
}

MyVector::Iterator MyVector::Erase(Iterator& it)
{
	for (int i = it.index; i < vectorSize-1; i++)
	{
		myObjects[i] = myObjects[i + 1];
	}
	--vectorSize;
}

MyVector::Iterator MyVector::Erase(Iterator& start, Iterator& end)
{
	int count = 0;
	for (int i = end.index; i < vectorSize; i++)
	{
		count++;
		myObjects[start.index++] = myObjects[i];
	}
	vectorSize -= count;
}

MyVector::Iterator MyVector::Insert(Iterator& it, MyObject& obj)
{
	if (vectorCapacity <= vectorSize)
	{
		vectorCapacity *= 2;
	}

	MyObject* newObjects = new MyObject[vectorCapacity];

	for (int i = 0; i < it.index; i++)
	{
		newObjects[i] = myObjects[i];
	}
	newObjects[it.index] = obj;
	for (int i = it.index; i < vectorSize; i++)
	{
		newObjects[i+1] = myObjects[i];
	}

	delete[] myObjects;
	myObjects = newObjects;

	return Iterator(myObjects, it.index);
}

bool MyVector::Empty()
{
	return vectorSize == 0;
}
void MyVector::Clear()
{
	vectorSize = 0;
}

void MyVector::GrowCapacity(size_t growCapacity)
{
	if (growCapacity <= vectorCapacity)
		return;

	vectorCapacity = growCapacity;
	MyObject* tempObjects = new MyObject[vectorCapacity];
	myObjects = tempObjects;
}

MyVector:: MyVector()
{
}

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

MyObject* MyVector::FindById(int MyObjectId) const
{
	for (int i = 0; i < vectorSize; i++)
	{
		if (myObjects[i]._id == MyObjectId)
			return myObjects + i;
	}
	return nullptr;
}

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

MyObject& MyVector::operator[](size_t index)
{
	return myObjects[index];
}

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
			vectorSize--;
		}
	}
}

void MyVector::SwapObject(size_t swapL, size_t swapR)
{
	MyObject tempObj = myObjects[swapL];
	myObjects[swapL] = myObjects[swapR];
	myObjects[swapR] = tempObj;
}

void MyVector::SortObejct(size_t lower, size_t upper)
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
		SortObejct(start, upper - 1);
		SortObejct(upper, stop);
	}
}

MyVector* MyVector::GroupById(int* numGroups)
{
	if (myObjects == nullptr)
		return;

	// 1. 벡터안의 오브젝트 구조체를 id 값에 따라 오름차순으로 정렬합니다.
	SortObejct(0, vectorSize - 1);

	// 2. id 값을 기준으로 그룹화하여 임시 배열에 담아줍니다.
	int objCount = 0;
	int vectorCount = 1;
	int tempId = myObjects[0]._id;
	MyVector* tempVectors = new MyVector[vectorSize];

	for (int i = 0; i < vectorSize; i++)
	{
		objCount++;
		if (tempId != myObjects[i]._id)
		{
			MyVector myVector = MyVector(objCount);
			for (int j = 0; j < objCount; j++)
			{
				myVector.Add(tempId);
			}
			tempVectors[i] = myVector;

			tempId = myObjects[i]._id;
			vectorCount++;
			objCount = 0;
		}
	}

	// 3. 임시 배열의 값들을 리턴 배열에 복사해줍니다.
	MyVector* myVectors = new MyVector[vectorCount];
	for (int i = 0; i < vectorCount; i++)
	{
		myVectors[i] = MyVector(tempVectors[i]);
	}
	*numGroups = vectorCount;
	return myVectors;
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
