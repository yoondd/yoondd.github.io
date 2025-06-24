## Coverity 분석 결과를 보는 관리자를 위한 3시간 C/C++ 핵심 강의 스크립트

Coverity로 정적 분석한 코드의 결과를 해석하고 관리하는 관리자라면, 단순한 문법보다는 "어떤 코드 패턴이 Coverity에서 문제로 잡히는지", "대표적인 결함 유형과 그 의미", "C/C++의 주요 결함 포인트"에 집중해야 합니다. 아래는 그런 목적에 맞춘 3시간 강의용 스크립트입니다.

---

## 1. 오리엔테이션 및 환경설정 (10분)

- "오늘은 Coverity로 분석된 C/C++ 코드의 결함을 효과적으로 이해하고 관리하는 데 필요한 핵심 개념을 빠르게 익혀보겠습니다."
- "VS Code 등 코드 뷰어를 활용해 실제 코드를 보며 설명할 예정입니다."

---

## 2. C/C++ 코드의 주요 결함 유형과 Coverity의 역할 (30분)

- "Coverity는 정적 분석 도구로, 코드를 실행하지 않고 소스코드를 스캔해 잠재적 결함을 탐지합니다."
- "C/C++에서 Coverity가 주로 잡아내는 결함 유형은 다음과 같습니다."
    - **메모리 누수(leak):** 할당한 메모리를 해제하지 않음
    - **널 포인터 역참조(NULL pointer dereference):** 널 포인터를 잘못 사용
    - **버퍼 오버플로우(buffer overflow):** 배열/버퍼 경계 초과 접근
    - **초기화되지 않은 값 사용(uninitialized value):** 선언만 하고 값을 할당하지 않은 변수 사용
    - **자원 누수(resource leak):** 파일, 소켓 등 시스템 자원을 해제하지 않음
    - **반환값 미확인(unchecked return value):** 함수의 반환값(특히 오류 코드)을 무시
    - **동시성 문제(concurrency):** 멀티스레드 환경에서의 데이터 경쟁 등
- "Coverity는 각 결함에 대해 코드 경로와 함께 상세한 설명을 제공합니다. 관리자는 결함의 심각도와 우선순위를 파악해 개발팀에 전달하는 역할을 하게 됩니다."[1](https://scan.coverity.com/)[2](https://cds.cern.ch/record/2243763/files/ATL-SOFT-PROC-2017-032.pdf)[3](https://cppscripts.com/cpp-coverity)[4](https://www.it-cisq.org/wp-content/uploads/sites/6/2022/09/Coverity-Static-Analysis.pdf)
    

---

## 3. 결함 예시와 실제 코드 리뷰 (60분)

**(1) 메모리 누수 예시**

```c
char* data = (char*)malloc(100); strcpy(data, "test"); // free(data); // 누락 시 메모리 누수 발생
```
- "free(data);가 없으면 Coverity가 메모리 누수(leak)로 표시합니다."

**(2) 널 포인터 역참조 예시**

```c
void printName(char* name) { printf("%s\n", name); // name이 NULL일 경우 문제 발생 }
```
- "name이 NULL일 때 Coverity가 null pointer dereference로 경고합니다."

**(3) 버퍼 오버플로우 예시**

```c
char buf[10];
strcpy(buf, "12345678910"); // buf 크기 초과
```
- "buf의 크기를 넘는 데이터 복사 시 Coverity가 buffer overflow로 탐지합니다."


**(4) 반환값 미확인 예시**

```c
FILE* fp = fopen("file.txt", "r"); // fp가 NULL일 때 체크하지 않으면 Coverity가 unchecked return value로 경고
```

**(5) 동적 메모리 관리 실수**

```c
int* arr = (int*)malloc(sizeof(int)*10); // arr 사용 후 free(arr); 누락 시 leak
```

**(6) C++ 객체지향 결함**

```cpp
class Base { public: virtual void foo() {} };
class Derived : public Base { ~Derived() {} };
// Base 포인터로 delete 시 소멸자가 virtual이 아니면 메모리 누수 가능
```

- "Coverity는 이런 클래스 계층 구조의 소멸자 문제도 잡아냅니다."[2](https://cds.cern.ch/record/2243763/files/ATL-SOFT-PROC-2017-032.pdf)[3](https://cppscripts.com/cpp-coverity)[4](https://www.it-cisq.org/wp-content/uploads/sites/6/2022/09/Coverity-Static-Analysis.pdf)
    

---

## 4. Coverity 결과 해석 및 관리 포인트 (30분)

- "Coverity 리포트는 결함의 종류, 심각도, 발생 위치, 코드 경로를 제공합니다."
- "결함의 우선순위는 심각도(critical, high, medium, low)로 분류됩니다."
- "관리자는 다음을 중점적으로 체크해야 합니다."
    - 반복적으로 등장하는 결함 유형(예: 메모리 누수, 반환값 미확인)
    - 결함이 발생한 코드의 맥락(테스트 코드 vs. 실제 서비스 코드)
    - false positive(오탐)와 진짜 결함 구분
    - 결함 추적 및 개선 현황 관리(이슈 트래킹, 담당자 지정 등)
- "Coverity 웹 인터페이스에서 결함별 상세 경로와 설명을 확인할 수 있습니다."[1](https://scan.coverity.com/)[2](https://cds.cern.ch/record/2243763/files/ATL-SOFT-PROC-2017-032.pdf)[3](https://cppscripts.com/cpp-coverity)[4](https://www.it-cisq.org/wp-content/uploads/sites/6/2022/09/Coverity-Static-Analysis.pdf)
    

---

## 5. 실전: Coverity 리포트 해석 실습 (30분)

- "실제 Coverity 리포트 샘플을 보며, 결함의 의미와 우선순위, 개발팀에 전달 시 주의할 점을 함께 살펴봅니다."    
- "각 결함의 발생 경로(코드 흐름)를 따라가며, 왜 문제가 되는지, 어떻게 수정해야 하는지 토론합니다."
- "의심스러운 결함(오탐)과 실제 결함을 구분하는 방법도 소개합니다."

---

## 6. Q&A 및 마무리 (20분)

- "오늘 배운 결함 유형과 Coverity 분석 결과 해석법을 바탕으로, 앞으로 코드 품질 관리에 더 효과적으로 기여할 수 있습니다."
- "추가로 궁금한 점이나 실무에서 겪는 이슈를 자유롭게 질문해 주세요."
    

---

## 요약: 관리자에게 꼭 필요한 포인트

- Coverity가 잡아내는 대표 결함(메모리/자원 누수, null pointer, buffer overflow, unchecked return 등)과 그 의미를 이해할 것
- 결함의 심각도와 우선순위, 코드 맥락을 고려해 개발팀에 전달할 것
- 반복적 결함, 오탐, 개선 현황을 체계적으로 관리할 것
    

