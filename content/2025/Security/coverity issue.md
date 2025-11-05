
| Checker 분류​                        | 설명​                                              | 발생 가능한 검사기 고장/오류​                                   |
| ---------------------------------- | ------------------------------------------------ | --------------------------------------------------- |
| Resource leaks (메모리 누수)​           | 리소스(메모리)를 정상적으로 할당/해제하지 못해서 발생하는 리소스(메모리) 누수 현상​ | 성능 저하, 시스템 중단, 서비스 중단, 또는 리소스(메모리) 획득 실패에 따른 기능 장애​ |
| Performance inefficiencies​        | C++ 언어에서 Copy 대신 Move를 통한 성능 향상과 리소스의 효율적 사용​    | 성능 저하 및 리소스 낭비​                                     |
| Exceptional resource leaks​        | 리소스(메모리)를 정상적으로 할당/해제하지 못해서 발생하는 리소스(메모리) 누수 현상​ | 성능 저하, 시스템 중단, 서비스 중단, 또는 리소스(메모리) 획득 실패에 따른 기능 장애​ |
| Null pointer dereferences​         | NULL 포인터 역참조​                                    | Exception Error 발생 가능성​                             |
| Incorrect expression​              | 수식 오류 (0으로 나누기, 복사&붙여넣기 오류, 개발자의 타이핑 실수 등)​      | Exception Error 발생 가능성​                             |
| Concurrent data access violations​ | Lock과 Unlock 없이 Data에 대한 접근 위반​                  | Race Condition에 의한 DeadLock 가능성​                    |
| Control flow issues​               | Dead Code 등 결코 실행되지 않는 코드​                       | 프로그램 비효율성 및 SW 버그 발생 원인 가능성​                        |
| Code maintainability issues​       | 사용하지 않는 변수 등​                                    | 개발자 오타 등 단순한 실수이지만, 다양한 기능 오류 가능성​                  |
| Uninitialized members​             | 초기화하지 않은 클래스 멤버 등​                               | 초기화하지 않은 변수 접근에 따른 비정상 동작​                          |
| Memory - illegal accesses​         | 메모리 해제 이후 사용, 초기화 하지 않은 변수 사용 등​                 | 메모리에 비정상적인 접근 등을 통한 예상할 수 없는 비정상 동작​                |
| Error handling issues​             | return 값이나 파라미터값이 예상 범위를 벗어나는 상황에서 예외처리 부족​      | 예상 범위를 벗어나는 상황에서 비정상 동작​                            |
| Uninitialized variables​           | 초기화하지 않은 변수 및 Return 값이 없는 함수 등​                 | Exception Error 발생 가능성​                             |
| Program hangs​                     | 무한 루프 등으로 인한 프로그램 Hang​                          | 프로그램 Hang으로 인한 시스템 중단​                              |
| Memory - corruptions​              | 할당된 범위 밖의 메모리 접근​                                | 비정상 동작 가능성​                                         |
| Integer handling issues​           | 할당된 범위 밖의 메모리 접근​                                | 비정상 동작 가능성​                                         |
| Data race undermines locking​      | 공유 데이터 등에 대한 수정/보호 등 미비​                         | 비정상 동작 가능성                                          |