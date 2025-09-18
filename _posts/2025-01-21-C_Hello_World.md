---
layout: single
title:  "[C++ 여정 - 01] Hello World"
categories:
  - Coding
tags:
  - C++
classes: wide
---

# Visual Studio로 설치 진행한 배경 이야기

C++을 시작하려 개발 환경 세팅 작업을 했다. 이미 vscode가 깔려있기에 ‘vscode에서 세팅하면 되겠지!’라는 안일한 생각으로 시작했지만.. 이것은 곧 나의 자만임을 깨달았다…
별도의 컴파일러 설치 및 세팅이며.. 환경 변수 설정 등등 참으로 많은 단계들을 열심히 거쳐 보았건만.. 겨우 세팅하고 실행한 결과, 빌드 오류가 등장해버렸다.

내가 아는 회사 동료 중 C++ 전문가에게 문의를 드린 결과...

"비쥬얼 스튜디오 설치하심이.."

라고 조언주신대로 진행하기로 결정했다. ㅎㅎㅎㅎ

# Hello World

참고 강의 영상: https://www.youtube.com/playlist?list=PLgqG2uj21HgkcfVtlr5rPekQl5VWJEnIB

**Hello World를 실행하는 코드**

```
#include <iostream>

using namespace std;

int main() {
    cout << "Hello World" << endl;
    return 0;
}
```

자. 이제 위 코드를 뜯어보자.

```
#include <iostream>
```
- 전처리 지시자
- Python에서 `import`같은 역할


```
using namespace std;
```
- `;`: 종결자 역할
- `std`나 아래에 있는 `cout`같은 함수들이 `iostream`안에 정의되어 있다.
  - 위 코드를 입력하지 않으면 `cout`앞에 `std::` 를 일일이 붙였어야 했다. 그걸 덜어내기 위해 사전에 정의하는 것이다.

```
int main() {
  return 0;
}
```
- 코드를 실행하려면 반드시 `main` 함수가 있어야 한다.
- `return 0;` : 일반적으로 쓰이는 실행 코드의 종결 역할
  - 이 코드가 가지는 의미를 찾아보았다:
  > `return 0;`이란 것은 현재 운영체제에게 해당 '코드 0'을 전달한다는 의미이다. 운영체제의 쉘에서는 0이 True값이므로, 아무 에러 없이 함수를 종료했다는 의미를 신호로 전달하는 것이다. 만약 함수 실행 중 에러 코드가 있다면 '코드 xx' 등 다른 수가 전달될 것이고, 운영체제는 이를 에러로 간주할 것이다.
  현대 C / C++에서는 프로그램이 잘 종료되었다는 것은 운영체제에게 암시적으로 전달한다. 즉 `return 0;` 을 생략하여도 문제는 없다. 그러나 프로그램의 안정성을 위해서 return 0;을 계속 써는 것이 하나의 관례처럼 이어지고 있는 것이다. (C의 경우 C99 표준부터, C++의 경우 모든 버전에서 암시적 종료가 가능하다.)
  출처: https://0xffffffff.tistory.com/65

```
cout << "Hello World" << endl;
  //cout: 출력
  //endl: next line의 역할
  //'<<': 데이터의 방향.
    // 위같은 경우엔 cout에다가 'Hello World"를 출력해줘 라는 뜻
```
- `cout`: 출력
- `endl`: next line 역할
  - `endl`과 `\n`의 차이:
    - `endl`:  `flush`라는 출력 버퍼를 지워주는 함수가 실행된다. 자동적으로 버퍼가 비워진다. 그래서 \n보다 속도가 느리다.
    - `\n`: 버퍼를 비우지 않는다.
    - 잠깐! 여기서 '버퍼'란?
    ![](/assets/images/image 6.png)
    출처: 내가 다른 곳에서 작성한 글 중 일부
- `<<`: 데이터 방향을 나타낸다.
  - 위 코드의 경우 '`cout`에다가 'Hello World'를 출력해줘'라는 뜻

# 실행 결과 모습
![](/assets/images/image 7.png)
