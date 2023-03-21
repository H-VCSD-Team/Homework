# C++ 1일차

**namespace**

```cpp
#include <iostream>

using namespace std;

int main() {
	std::cout << "HELLO WORLD" << std::endl;

	return 0;
}
```

**buffer**

cpu는 매우 빠르기 때문에 키보드 input을 기다릴수 없어 임시 저장소인 buffer에 키보드 input을 저장하고 나중에 cpu는 buffer에서 사용된다

**endl**

출력 버퍼를 비워주고 ‘\n’을 출력해주는 함수

# 디버깅 관련

------

**Trace(F10)**

코드 한줄한줄 실행하면서 어캐 진행되는지 나옴

**break point(F9)**

break point 지정하고 디버깅하면 break point 지정한 곳에서 멈춤

**조사식(디버그실행할때)**

창 → 조사식

**c++ 구조체**

1. 변수 생성하고 바로
2. 구조체에 struct안붙여도 됨 ( C언어랑 다른점 )
3. 선택적 초기화 불가능

**C++ string**

C++ string class는 뒤에 ‘\0’ NULL 문자 안들어간다