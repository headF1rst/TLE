## 2주차 이진탐색 문제풀이

- PPAP
- 그렇고 그런 사이
- 난로
- 정육점
- 수열의 점수

### 1. PPAP

**문제**

[PPAP](https://www.acmicpc.net/problem/16120)

**접근방법**

주어진 문자열에서 `PPAP`를 `P`로 바꿔나갈 때 결국 문자열은 P의 연속으로 남아있게 된다.

예제 1,2를 예로들어 접근해 보자면 P`PPAP`AP는 P`P`AP가 되고 이는 다시 P가 된다.

반면 `PPAP`APP의 경우 `P`APP가 되고 이는 P로 변환되지 못하기 때문에 P의 연속된 문자열이 아니므로 NP를 출력하게 된다.

이를 활용하여 문자열의 문자들을 스택에 넣어가며 조건에 맞춰 스택 원소의 삭제를 반복하여 스택에 P 문자열만 남도록 하였다.

이때 주의해야 할것은 P는 PPAP 문자열이며 PP, PPP, PPPP는 PPAP문자열이 되지 못한다는 것이다. 때문에 이러한 반례를 다음과 같이 처리해 주었다.

``` cpp
if(str.size()==1 && str[0] == 'P')
    {
        cout<<"PPAP";
        return 0;
    }
    if(str.size() <= 3 || !(str.compare("PPPP")))
    {
        cout<<"NP";
        return 0;
    }
```

**구현**

``` cpp
#include<iostream>
#include<cstring>
#include<stack>
using namespace std;

int main(void)
{
    ios::sync_with_stdio(false); cin.tie(0);
    stack<char> st;
    string str;
    cin>>str;
    
    if(str.size()==1 && str[0] == 'P')
    {
        cout<<"PPAP";
        return 0;
    }
    if(str.size() <= 3 || !(str.compare("PPPP")))
    {
        cout<<"NP";
        return 0;
    }

    for(int i=0; i<str.size(); i++)
    {
        if(st.empty()) 
        {
            if(str[i] == 'A')
            {
                cout<<"NP";
                return 0;
            }
            st.push(str[i]);
            continue;
        }

        if(str[i] == 'P')
        {
            if(st.top() == 'P') st.push(str[i]);
            else
            {
                if(st.top()=='A' && st.size()>=3)
                {
                    for(int j=0; j<3; j++) st.pop();
                    st.push(str[i]);
                }
            }
        }
        else
        {
            if(st.top()=='A' || st.size()<2)
            {
                cout<<"NP";
                return 0;
            }
            else st.push(str[i]);
        }
    }
    if(st.top() == 'A') cout<<"NP";
    else cout<<"PPAP";
    return 0;
}
```