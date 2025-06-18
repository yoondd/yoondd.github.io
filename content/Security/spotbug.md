## SpotBugs란?

SpotBugs는 Java 바이트코드를 정적 분석하여 코드 내의 버그 패턴, 잠재적 오류, 보안 취약점 등을 자동으로 탐지해주는 무료 오픈소스 도구다. FindBugs의 공식 후속 프로젝트로, 코드의 정확성, 스타일, 성능, 보안 등 다양한 품질 문제를 사전에 파악할 수 있다.

## 주요 특징

- Eclipse, IntelliJ 등 주요 IDE에서 플러그인 형태로 사용 가능
- Jenkins, SonarQube 등 CI/CD 도구와 연동 가능
- 코드 내 버그를 심각도(Scariest, Scary, Troubling, Of Concern)별로 분류해 시각적으로 제공
- FindSecurityBugs 플러그인 추가 시 보안 취약점 진단 강화

## Eclipse에서 SpotBugs 설치 및 사용법

**설치 방법**

1. Eclipse 실행 후 `Help > Eclipse Marketplace`로 이동
2. 검색창에 "SpotBugs" 입력 후 SpotBugs Eclipse Plugin 설치
3. 설치 후 Eclipse 재시작
    

**FindSecurityBugs 추가(선택)**

- [FindSecurityBugs 공식 사이트](https://find-sec-bugs.github.io/)에서 최신 jar 파일 다운로드
- Eclipse 메뉴에서 `Window > Preferences > Java > SpotBugs`로 이동
- `Plugins and misc. Settings`에서 Add 버튼 클릭 후 jar 파일 추가

**사용 방법**

1. 검사할 프로젝트 또는 파일을 우클릭
2. `SpotBugs > Find Bugs` 선택
3. 분석이 완료되면 하단 또는 별도 View(Bug Explorer, SpotBugs Perspective)에서 결과 확인
    - 결과는 심각도별로 분류되어 표시
    - 각 버그를 클릭하면 해당 소스 코드로 이동 및 상세 설명 확인 가능
        

## 활용 예시 및 장점

- NullPointerException, 잘못된 equals 사용, 리소스 미반환 등 자주 발생하는 오류를 사전에 탐지
- 코드 품질 및 보안 강화, 납품 전 자가 검증 도구로 널리 활용
- 무료이며, KISA 등 국내외 공공기관에서도 권장하는 도구

## 참고 사항

- SpotBugs는 Java 1.8 이상이 필요하며, 메모리 512MB 이상을 권장한다
- FindBugs는 더 이상 유지보수되지 않으므로, SpotBugs 사용이 표준이다

---

각 언어마다  이런것이 있는데 대표적인것만 알아볼까?

| 언어             | 대표 도구                                           | 특징/설명                            |
| -------------- | ----------------------------------------------- | -------------------------------- |
| **Java**       | SpotBugs, PMD, SonarQube                        | 바이트코드 및 소스코드 분석, 다양한 IDE 플러그인 지원 |
| **C/C++**      | Cppcheck, Clang Static Analyzer, Coverity       | 메모리 누수, 포인터 오류 등 C/C++ 특화 분석     |
| **Python**     | Pylint, Pyflakes, Prospector, Bandit, SonarQube | 코드 스타일, 오류, 보안 취약점 등 다양한 검사      |
| **JavaScript** | ESLint, JSHint, SonarQube                       | 문법, 스타일, 잠재적 오류 및 버그 탐지          |
| **C#/.NET**    | SonarQube, Roslyn Analyzers, StyleCop           | 코드 품질, 스타일, 보안 분석 지원             |
| **Go**         | Golint, Staticcheck                             | 코드 스타일, 버그, 비효율적 코드 패턴 탐지        |
| **Ruby**       | RuboCop                                         | 스타일, 코드 품질, 버그 탐지                |
| **Swift**      | SwiftLint                                       | 스타일, 코드 품질, 잠재적 오류               |
| **PHP**        | PHPStan, Psalm                                  | 타입, 코드 품질, 보안 취약점                |

그러니까 우리는 지금 coverity를 쓰고있지만 굳이 자바에서 만든 spotbug내용을 다시 구현할 필요가없잖아. 그래서 cov-analyze에서 한번에 확인할 수 있는거야. 

