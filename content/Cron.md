크론(Cron)은 유닉스 계열 운영 체제에서 사용되는 **시간 기반 작업 스케줄러**로 사용자가 지정한 시간에 명령어 또는 스크립트를 자동으로 실행하도록 예약할 수 있는 도구다. 이건 반복적인 작업을 자동화하는 데 아주 유용하다.

# 어쩌다가 알았는가?

사실 크론의 존재조차 알지못했는데, 거래처에서 요구하는기능을 구현하기위해 sql문이 일정시간간격으로 들어가야하는 작업이 생겼다. 어떻게 해야할지 알아보는데 카페24 리셀러 관리자의 db건드는 권한이 그리 크지않아서 크론을 이용하기로했다.

# 주요 특징

자동화된 작업 스케줄링: 특정 시간, 날짜, 주기적으로 작업을 실행할 수 있다.

1. Crontab 파일: 크론 작업을 설정하는 파일로, 각 줄에 작업 스케줄과 실행할 명령어를 정의한다.
2. 주요 활용 사례:
	-  시스템 유지보수
	-  데이터 백업
	-  로그 정리
	-  주기적 보고서 생성


# Crontab 표현식

크론의 표현식은 정규식 형태로 작성된다. 

| 필드명          | 값의 허용 범위           | 예시                     |
| ------------ | ------------------ | ---------------------- |
| 초 (Seconds)  | 0–59               | `*/5` (5초마다)           |
| 분 (Minutes)  | 0–59               | `15,45` (15분, 45분에 실행) |
| 시 (Hours)    | 0–23               | `0 9` (오전 9시에 실행)      |
| 일 (Day)      | 1–31               | `1-7` (매월 1일부터 7일까지)   |
| 월 (Month)    | 1–12 또는 JAN–DEC    | `JAN,MAR` (1월, 3월에 실행) |
| 요일 (Weekday) | 0–6 또는 SUN–SAT     | `MON-FRI` (월~금 실행)     |
| 연도 (Year)    | 생략 가능 또는 1970–2099 | `2025`                 |

특수문자를 사용하여 복잡한 일정도 설정할 수 있다

- `*`: 모든 값
- `,`: 특정 값들
- `-`: 범위 지정
- `/`: 간격 설정
- `?`: 특정 값 없음
- `L`: 마지막 날 또는 요일
- `W`: 가장 가까운 평일
- `#`: 몇째 주의 특정 요일



# 외부 크론

서버 내에 접근하는게 좀 어렵길래 나는 외부 서비스로 정기적인 내용을 진행할 수 있도록 연동했다
쉽게 말하면, 특정 시간마다 외부 서비스가 내가 지정한 URL(예: `wp-cron.php`)을 호출해서 작업을 자동으로 실행하게 만드는 거다

내가 가지고있는 웹호스팅의 권한 한계때문에 간단히 외부 서비스를 사용했다고 보면된다.

## 뭐가 좋은가? 

1. **서버 리소스를 아껴줌**  
    내부 크론은 보통 웹사이트 방문자에 의해 트리거되는데, 방문자가 없으면 작업이 늦게 실행될 수도 있다. 외부 크론은 이런 문제 없이 정확히 설정한 시간에 실행돼서 서버 부하를 완전 줄여준다.
2. **정확한 실행 시간**  
    외부 크론은 설정한 주기에 맞춰 딱딱 실행돼서 작업 누락이나 지연 걱정을 할 필요가없다
3. **사용하기 쉬움**  
    대부분의 외부 크론 서비스는 직관적인 UI를 제공해서 설정하고 관리하기 편리하다
4. **추가 기능 제공**  
    작업 실패 시 알림 보내기, 재시도 기능, 실행 로그 기록 같은 부가 기능도 지원하는데...(써보지는못했다)

나는 무료로 제공하는 **cron-job.org**을 이용했다.
