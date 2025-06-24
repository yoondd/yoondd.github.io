프로그램이 데이터를 버퍼에 저장할 때, 버퍼가 가득 차서 넘치게 되어 개발자가 지정한 부분의 바깥쪽에 덮어 씌워져버리는 취약점, 버그, 이를 이용한 공격 방법을 말한다.

넘쳐난 데이터는 원래 데이터를 밀어내거나 이상한 곳에 저장이 되는 것이 아니라 덮어 씌우는게 핵심이다. 여기서 덮어씌워진 메모리에는 다른 데이터가 이미 포함되어 있을 수도 있고, 이 때문에 메모리 접근 오류, 프로그램 종료, 시스템 보안 취약점 등이 발생할 수 있다.

버퍼 오버플로우는 보통 데이터를 저장하는 과정에서 그 데이터를 저장할 메모리 위치가 유효한지를 검사하지 않아 발생한다. 이러한 경우 데이터가 담긴 위치 근처에 있는 값이 손상되고, 그 손상이 프로그램 실행에 영향을 미칠 수도 있다. 특히 악의적인 공격으로 인해 프로그램에 취약점이 발생할 수 있다.

흔하게 버퍼오버플로우와 관련되는 프로그래밍 언어는 C와 C++로, 어떤 영역의 메모리에서도 내장된 데이터 접근 또는 덮어쓰기 보호 기능을 제공하지 않으며 어떤 배열에 기록되는 데이터가 그 배열의 범위 안에 포함되는지 자동으로 검사하지 않는다. 

## 발생 원인

프로그램은 수많은 함수로 구성되어있는데, 프포그램을 실행하여 함수가 호출될 때, 지역변수와 복귀주소(Return Address)가 스택이라고 하는 논리 데이터 구조에 저장이 된다. 복귀주소가 저장이 되는 이유는 함수가 종료될 때 OS가 함수를 호출한 프로그램에게 제어권을 넘겨야하는데, 이 복귀주소가 없으면 함수가 종료된 후에 어떤 명령어를 수행해야할지 알 수 없기 때문이다. 버퍼 오버플로우는 이 복귀주소가 함수가 사용하는 지역변수의 데이터에 의해 침범당할 때 발생하게 된다. 개발자 실수로인해 지역변수가 할당된 크기보다 더 큰 크기로 입력되면, 입력 데이터에 의해 복귀주소가 변경되어버린다. 즉, 함수가 종료될 때 전혀 관계없는 복귀주소를 실행시키게 되어 실행 프로그램이 비정상적으로 종료되게 된다는 말이다. 악의적인 공격자가 이러한 취약점을 알고서 데이터의 길이와 내용을 적절히 조정하여 버퍼 오버플로우를 일으켜서 특정 코드를 실행시키게 하는 해킹 공격 기법을 버퍼 오버플로우 공격이라고 한다.


## 예제 소스코드 보기

```c
#include <stdio.h>
#include <string.h>

int main(int argc, char *argv[])
{
  char buffer[16];
  if(argc < 2)
    return -1;
  strcpy(buffer, argv[1]);
}
```
]
위 코드는 실행시 인수로 받은 문자열을 버퍼에 옮기고 버퍼를 출력하는 간단한  c언어 코드다. 



## 버퍼 오버플로우 대응 및 예방 방법

## 1. 안전한 함수 사용 및 경계 검사

- **strcpy, strcat, gets** 등 "경계 검사 없는 함수" 대신,
    - **strncpy, strncat, fgets** 등 "길이 제한이 있는 함수" 사용  
        → 예: `strncpy(buffer, input, sizeof(buffer)-1); buffer[sizeof(buffer)-1] = '\0';`
        
- **입력값 길이 검증**: 외부 입력을 받을 때 반드시 최대 길이를 확인하고, 초과 입력은 잘라내거나 거부
    

## 2. 코드 레벨 보안 습관

- **버퍼 크기 상수화**: 매직넘버 대신 `#define`이나 `const`로 버퍼 크기를 명확히 선언
- **모든 입력값 검증**: 사용자, 네트워크, 파일 등 모든 외부 입력에 대해 유효성 검사
- **코드 리뷰와 정적 분석 도구 활용**: Coverity, AddressSanitizer, fuzzing 등으로 취약점 사전 탐지
    

## 3. 운영체제 및 컴파일러 보안 기능 활용

- **스택 카나리(Stack Canary)**: 스택에 특수값을 두고, 함수 반환 전 값이 바뀌었는지 검사해 오버플로우 탐지
- **ASLR(Address Space Layout Randomization)**: 메모리 주소를 무작위로 배치해 공격자가 위치를 예측하기 어렵게 함
- **DEP(Data Execution Prevention)**: 데이터 영역(버퍼 등)에서 코드 실행을 차단
- **컴파일러 보호 옵션**: `-fstack-protector`, `-D_FORTIFY_SOURCE=2` 등 컴파일러 플래그 사용
    

## 4. 메모리 안전 언어 도입(가능할 경우)

- **Rust, Go, Java** 등 메모리 안전성을 보장하는 언어로 신규 개발 또는 점진적 전환
    - 현실적으로 기존 C/C++ 코드는 어렵지만, 신규 코드나 중요 모듈부터 적용 권장
        

## 5. 패치와 업데이트

- **OS/라이브러리/서버 소프트웨어 최신 보안 패치 적용**: 알려진 취약점이 악용되지 않도록 주기적 업데이트
    

---

## 추가로 중요한 내용

- **권한 상승(Privilege Escalation)**: 버퍼 오버플로우로 공격자가 관리자 권한을 획득할 수 있음[5](https://www.portnox.com/cybersecurity-101/what-is-a-buffer-overflow/)
- **서비스 거부(DoS)**: 프로그램이 비정상 종료되어 서비스 중단 가능[5](https://www.portnox.com/cybersecurity-101/what-is-a-buffer-overflow/)
- **데이터 손상/유출**: 인접 메모리의 중요 데이터(비밀번호, 인증 토큰 등) 노출 위험[5](https://www.portnox.com/cybersecurity-101/what-is-a-buffer-overflow/)[6](https://codasip.com/2023/10/20/buffer-bound-vulnerabilities-and-their-dangers/)
- **보안 기능 우회**: ASLR, DEP 등도 우회될 수 있으므로 다중 방어 필요[5](https://www.portnox.com/cybersecurity-101/what-is-a-buffer-overflow/)
- **Heap Overflow, Stack Overflow**: 버퍼 오버플로우는 스택뿐 아니라 힙 영역에서도 발생할 수 있음[7](https://en.wikipedia.org/wiki/Buffer_overflow)[6](https://codasip.com/2023/10/20/buffer-bound-vulnerabilities-and-their-dangers/)
- **정기적 보안 교육**: 개발자 대상 보안 교육과 코드 리뷰 문화 정착[1](https://purplesec.us/learn/prevent-buffer-overflow-attack/)
    

---

## 대응/예방 요약표

|예방 방법|설명|
|---|---|
|안전한 함수 사용|strncpy, strncat, fgets 등 사용|
|입력값 길이 검증|모든 외부 입력의 최대 길이 체크|
|코드 리뷰/정적 분석|Coverity, AddressSanitizer, fuzzing 등 활용|
|스택 카나리/ASLR/DEP|OS 및 컴파일러 보안 기능 활성화|
|메모리 안전 언어|Rust, Go 등 신규 개발 시 점진적 도입|
|정기적 패치|OS/라이브러리/서버 소프트웨어 최신 상태 유지|
|보안 교육|개발자 대상 정기적 보안 교육|

---

## 결론

- 버퍼 오버플로우는 C/C++에서 매우 위험한 취약점이지만, 안전한 함수 사용, 입력 검증, 보안 기능 활용, 정적 분석 등으로 충분히 예방할 수 있습니다.
- 개발과 운영 전 과정에서 다층적 방어와 지속적인 보안 관리가 중요합니다[1](https://purplesec.us/learn/prevent-buffer-overflow-attack/)[3](https://www.tencentcloud.com/techpedia/103607)[4](https://www.cisa.gov/resources-tools/resources/secure-design-alert-eliminating-buffer-overflow-vulnerabilities)[2](https://www.code-intelligence.com/blog/buffer-overflows-complete-guide).




## 버퍼 오버플로우 위험 함수와 권장 함수

|위험(비권장) 함수|권장(안전) 함수|설명|
|---|---|---|
|strcpy()|strncpy()|복사할 최대 길이를 지정할 수 있음|
|strcat()|strncat()|붙일 최대 길이를 지정할 수 있음|
|gets()|fgets()|입력받을 최대 길이를 지정할 수 있음|
|sprintf()|snprintf()|출력할 최대 길이를 지정할 수 있음|

- 위 위험 함수들은 **버퍼의 크기를 넘는 입력을 막아주지 않기 때문에** 버퍼 오버플로우의 주요 원인이 됩니다[1](https://www.acunetix.com/blog/web-security-zone/what-is-buffer-overflow/)[2](https://dwheeler.com/secure-programs/Secure-Programs-HOWTO/dangers-c.html)[3](https://www.codecademy.com/learn/secure-coding-practices-in-c/modules/secure-coding-practices-c/cheatsheet).
- 반면, 권장 함수들은 **최대 길이(버퍼 크기)를 명시적으로 지정**할 수 있어 오버플로우를 예방할 수 있습니다.
    

---

## 추가로 주의할 함수 및 상황

- **scanf(), fscanf(), sscanf() 등**: %s 포맷을 쓸 때 입력 길이 제한이 없으면 오버플로우 위험[2](https://dwheeler.com/secure-programs/Secure-Programs-HOWTO/dangers-c.html).
    - 예: `scanf("%s", buffer);` 대신 `scanf("%15s", buffer);`처럼 최대 길이 지정 필요.
        
- **memcpy(), memmove()**: 길이 인자를 잘못 계산하면 오버플로우 발생 가능. 항상 버퍼 크기와 일치하는지 확인 필요
- **vsprintf(), vsnprintf()**: sprintf와 동일한 원리. vsnprintf 사용 권장[2](https://dwheeler.com/secure-programs/Secure-Programs-HOWTO/dangers-c.html).
- **realpath(), getwd() 등**: 버퍼 크기를 반드시 확인하고 충분한 크기의 버퍼를 전달해야 함[2](https://dwheeler.com/secure-programs/Secure-Programs-HOWTO/dangers-c.html).

---

## 실전 대응 팁

- **길이 제한이 없는 함수는 무조건 피한다**: 위 표의 위험 함수들은 코드 리뷰에서 발견 즉시 수정 대상.
- **모든 입력·복사 함수에 버퍼 크기 체크**: 권장 함수도 인자에 버퍼 크기를 잘못 넣으면 무용지물이므로 항상 주의.
- **정적 분석 도구 활용**: Coverity, Black Duck 등으로 위험 함수 사용 여부를 자동 탐지
- **코드 리뷰와 보안 교육**: 팀 전체가 위험 함수 사용 금지 원칙을 공유하고, 정기적으로 점검

---

## 요약

- strcpy, strcat, gets, sprintf, scanf(길이 제한 없는 %s) 등은 버퍼 오버플로우의 주요 원인이므로 사용을 피해야 합니다[1](https://www.acunetix.com/blog/web-security-zone/what-is-buffer-overflow/)[2](https://dwheeler.com/secure-programs/Secure-Programs-HOWTO/dangers-c.html)[3](https://www.codecademy.com/learn/secure-coding-practices-in-c/modules/secure-coding-practices-c/cheatsheet).
- strncpy, strncat, fgets, snprintf 등 **길이 제한이 있는 안전 함수**로 대체해야 하며, 항상 버퍼 크기와 일치하는지 확인해야 합니다[3](https://www.codecademy.com/learn/secure-coding-practices-in-c/modules/secure-coding-practices-c/cheatsheet).
- memcpy, memmove 등도 길이 인자 실수로 오버플로우가 날 수 있으니 주의가 필요합니다[4](https://stackoverflow.com/questions/167165/what-c-c-functions-are-most-often-used-incorrectly-and-can-lead-to-buffer-over).

이 원칙만 잘 지켜도 버퍼 오버플로우 위험을 크게 줄일 수 있습니다!