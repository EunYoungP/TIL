
2023.01.31

# __[프로그래머스 LV2] 최댓값과 최솟값__

std::string
---- 

## __문제__

<img src="https://user-images.githubusercontent.com/80774412/218707294-0de38084-62b6-41de-bea2-84e4d2e5d58f.PNG"></img>

<br><br>

## __나의 풀이__

```c++
#include <string>
#include <vector>
#include <algorithm>

using namespace std;

string solution(string s) {
    string answer = "";
    
    vector<int> temp;
    int curPosition = 0;
    int position;

    while((position =  s.find(" ",curPosition)) != std::string::npos)
    {
        int len = position - curPosition;
        string nstr = s.substr(curPosition, len);
        temp.push_back(stoi(nstr));
        curPosition += len + 1;
    }
    temp.push_back(stoi(s.substr(curPosition)));
    
    sort(temp.begin(), temp.end());
    
    answer += to_string(temp[0]);
    answer += " ";
    answer += to_string(temp[temp.size()-1]);
    
    return answer;
}
```
std::string 의 함수들을 이용하여 문제를 풀이했습니다.

## __string 자르는 방법__

__1. substr__

    string substr(size_t pos = 0, size_t len = npos) const

문자열의 index를 의미하는 __pos__ 와,

pos 로부터 검색할 문자열의 길이를 의미하는 __len__ 을 인자로 받아 string을 반환합니다.

즉, __pos__ 로부터 __len__ 길이 만큼 문자열에서 분리하여 반환해줍니다.

검색을 실시한 문자열은 실제로 값이 분리되지는 않습니다.

- 1-1. __find 와 substr__

        int position;
        while((position =  s.find(" ",curPosition)) != std::string::npos){}
    find 함수를 사용하여 분리자를 검색하여 인덱스를 반환받습니다.

    반환받은 인덱스와 현재 검색기준 인덱스와의 차이를 이용하여 분리된 문자열들을 얻을 수 있습니다.

__2. std::getline__

delim 이 발견될 때까지 is 에서 문자를 추출하고 str 에 저장합니다.

```c++
// 입력스트림 오브젝트, 문자열을 저장할 string 객체, 종결 문자
getline(istream& is, string str, char delim);

// 사용 예시
istringstream iss(s);
string str_buf;
char separator = ',';
while(getline(iss, str_buf, separator))
```

getline 과 istringstream 을 이용하여 구분자로 문자열을 분리할 수 있습니다.

istringstream 으로 문자열을 다른 형식으로 변환하고,

getline 을 사용해서 구분자로 string 객체에 값을 분리하여 넣습니다.

<br><Br>

## __sstream 헤더__

문자열 파싱할때 사용하는 함수들이 대거 포함되어있습니다.

(e.g. istringstream, ostringstream, stringstream)

* __istringstream__

    공백을 기준으로 문자열을 파싱하여 변수의 형식에 맞게 변환해줍니다.

        ```c++
        istringstream iss("doby is 27 free");

            string s1, s2, s3;
            int i1;

            // 공백을 기준으로 문자열을 파싱하고, 변수의 형식에 맞게 변환합니다.
            iss >> s1 >> s2 >> i1 >> s3;
        ```
   
* __ostringstream__

    string 을 조립하거나 특정 수치를 문자열로 변환하기 위해 사용합니다.

    ```c++
    ostringstream oss;
    string s1 = "abc", s2 = "eun";
    int i1 = 7777;
    double d1 = 3.14;

    oss << s1 << "\n" << i1 << "\n" << s2 << "\n" << d1;

    // str() 은 ostringstream 객체에서 조립된 문자열을 꺼내주는 함수입니다.
    cout << oss.str() << endl;
    ```

    int, double 등의 수치값을 간단하게 string 형식으로 바꿔줍니다.

* __stringstream__

    문자열에서 공백과 \n 을 제외한 문자열을 차례대로 빼내줍니다.

    ```c++
    string str = "123 456\nabc\ndef";
    string token;

    stringstream ss(str);
    while(ss >> token) 
    {
        // 123
        // 456
        // abc
        // def
        cout << token << endl;
    }
    ```

