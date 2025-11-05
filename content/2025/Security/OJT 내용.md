## 1. 사업 및 솔루션

- **정적 분석 솔루션 및 서비스**: SMsolutions는 소프트웨어 개발 과정에서 코드 품질과 보안을 확보하기 위한 정적 분석(실행 없이 소스코드 분석) 솔루션을 제공함.
- **오픈소스 분석**: Black Duck과 같은 오픈소스 컴플라이언스 및 취약점 분석 도구도 함께 제공.
- **퍼징 테스트**: Defensics 등 퍼징(비정상 입력으로 취약점 탐지) 솔루션도 취급.
- **센티온**: Coverity와 연동되는 자체 솔루션도 보유.

---

## 2. 시장 배경 및 규제

- **국제 표준 및 규제**: 자동차, 산업 등 민감 분야에서 국제 표준(UN WP.29, UN R155, ISO 21434 등)에 따라 사이버보안 검증이 필수화되고 있음
- **규제 목적**: 보안 취약점 방지 및 안전 확보, 유럽 등 해외 시장 진출을 위한 필수 요건.
- **고객 수요**: 이런 규제 대응을 위해 기업들이 정적 분석, 오픈소스 분석, 퍼징 테스트 등 다양한 보안 솔루션을 도입.

---

## 3. 정적 분석과 Coverity

- **정적 분석의 필요성**: 소프트웨어 결함은 코딩 단계에서 주로 유입되지만, 뒤늦게 발견할수록 수정 비용이 커짐. 정적 분석을 통해 개발 초기에 결함을 발견하고 비용과 리스크를 줄일 수 있음
- **Coverity**:
    - Synopsys의 대표적인 정적 분석(SAST) 솔루션으로, 코드 품질 결함과 보안 취약점을 개발 단계에서 자동으로 탐지
    - 다양한 언어(C, C++, Java 등)와 산업 표준(예: SEI CERT C, MISRA, ISO 26262 등) 지원
    - 서버-클라이언트 구조: CA(Coverity Analysis, 클라이언트)에서 소스코드를 분석해 CC(Coverity Connect, 서버)로 결과를 전송·관리.
    - 빌드 과정 연동: 실제 빌드 명령어를 통해 소스코드를 분석, call graph와 dependencies를 구성해 분기별로 경로를 따라가며 결함을 탐지.
    - 분석 경로마다 순회하며 깊이 있는 분석 제공.
        

---

## 4. 자동차 분야와 보안 규제

- **WP.29/UN R155**: UN 산하 WP.29에서 제정한 자동차 사이버보안 규제. 유럽 등에서 차량 판매 시 사이버보안 검증 필수[1](https://blackberry.qnx.com/en/ultimate-guides/wp-29-vehicle-cybersecurity).
- **ISO 21434**: 자동차 사이버보안 엔지니어링 표준. 제품 개발, 운영, 유지보수 전 과정에서 보안 위험 관리 요구[2](https://www.intertek.com/automotive/cybersecurity/).
- **MISRA**: 자동차 등 임베디드 소프트웨어의 기능 안전을 위한 C/C++ 코딩 규칙. 기능 안전과 보안 모두를 강화[6](https://www.synopsys.com/glossary/what-is-misra.html).
- **CERT C**: 보안 코딩 표준. Coverity는 SEI CERT C, MISRA 등 다양한 외부 표준을 분석 규칙에 포함[5](https://www.electronicspecifier.com/products/design-automation/synopsys-expands-coverity-support-for-programming-languages)[6](https://www.synopsys.com/glossary/what-is-misra.html)

---

## 5. 실습 및 활용

- **실습 자료**: Coverity_수행_가이드, Coverity_사용자_가이드 등 내부 문서를 참고해 실습 가능.
- **실습 절차**: 설치파일과 라이선스 확보 → 소스코드 준비 → 빌드 명령어와 연동해 분석 실습.
- **매뉴얼**: analysis 폴더 내 doc 디렉터리에 상세 매뉴얼 존재, 필요시 참고.

---

## 6. 용어 정리

- **보안약점**: 코딩상의 버그(오류)
- **보안취약점**: 공격자가 악용할 수 있는 보안상의 허점
    

---

## 요약

SMsolutions는 자동차 등 민감 산업 분야의 국제 규제(UN WP.29, ISO 21434 등)에 대응하기 위해 Coverity 기반의 정적 분석, 오픈소스 분석(Black Duck), 퍼징(Defensics) 등 다양한 보안 솔루션을 제공하고 있습니다.  
Coverity는 실제 빌드와 연동해 소스코드의 결함과 보안 취약점을 개발 초기 단계에서 탐지하며, 업계 표준 코딩 규칙(MISRA, CERT C 등)을 준수해 안전하고 신뢰성 높은 소프트웨어 개발을 지원합니다.  
실습은 내부 가이드와 매뉴얼을 참고해, 설치-분석-결과 확인의 순서로 진행할 수 있습니다.