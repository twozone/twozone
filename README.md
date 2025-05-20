# twozone
2025학년도 모바일프로그래밍 2021(이공이일)조

프로젝트 개요 : 커뮤니티 – 사람을 찾습니다. 정보제공 통제 권한 정리 필요

팀 : 2021(이공이일, twone)

팀장 : 위세영

팀원 : 윤이준 이영민 최우진

(프로젝트 수정)

제목 : GameCalendar(임시)

👥2021(이공이일)조 

📌역할

위세영 : 조장, 자료조사

최우진 : 메인 개발

이영민 : 서브 개발

윤이준 : 자료 조사, 테스트

📌목표

다양한 게임을 즐기는 게이머들이 게임마다 수행해야 하는 일정이 다르기 때문에 게임마다 일정을
제대로 수행할 수 있도록 도와주는 애플리케이션
 
📌기능

1. **게임별 일정 관리**
📌기능 설명 요약 :
1. 유저가 각 게임을 추가할 수 있고, 각 게임마다 여러개의 일정을 등록할 수 있어야 함

📌세부기능
1. 게임 추가: 게임 이름, 아이콘 이미지 선택 가능
2. 일정 추가: 일정 이름, 시작일/종료일, 시간, 메모
3. 게임별 일정 보기: 특정 게임의 모든 일정 리스트/캘린더로 보기
      
2. **알람 및 푸시 알림**
📌기능 설명 요약 :
1. 일정 시간이 다가오면 사용자에게 푸시 알림 보내기, 유저가 원하는 시간 전에 알림받도록 설정 가능 

📌세부기능
🔔알림 설정 옵션
1. 알림 시간: 시작 10분 전, 30분 전, 1시간 전 등 선택
2. 알림 방법: 시스템 알림 + 진동/소리

3. **반복 일정 기능**
📌기능 설명 요약 :
- 반복 유형 선택 -> 매일 / 매주 / 특정 요일 반복
- 자동 알림 설정 -> 반복 일정에도 알람 자동 설정

📌세부기능 
🔁반복 설정 옵션
- 매일 / 매주 / 특정 요일(월, 수, 금 등)
- 시작일/종료일 설정 가능

4. **게임 일정 데이터 공유**
📌기능 설명 요약 :
1. 친구/길드원에게 내 일정을 공유        
2. 공개 일정: 특정 게임의 공식/공통 일정 추가 가능  
3. 공유 링크 또는 QR로 일정 전달           
4. 일정 수신 후 저장 및 알림 등록 가능

📌세부기능
📤 공유 방법
1. JSON 형태로 일정 내보내기
2. Intent 공유, QR 생성, Firebase 연동 중 선택

📥 수신 방법
1. JSON 붙여넣기 or QR 스캔 → 파싱하여 일정으로 추가

5. **게임 이벤트 & 업데이트 캘린더**
📌기능 설명 요약 :
1. 최신 게임 이벤트 자동 표시	서버에서 받아오는 이벤트 정보로 캘린더에 자동 반영
2. 업데이트/패치 일정 확인	정기 점검, 대형 업데이트 일정 등을 리스트 또는 캘린더에 표시

📌세부기능
🌐 데이터 소스
1. 앱에 포함된 정적 JSON 파일 (assets)
2. 또는 Firebase/REST API에서 동적 로딩

📌OpenWorldGame 개발 계획

1. 프로젝트 세팅
언어 : JAVA
build.gradle에 Room, Recycleview, 기타 필요한 라이브러리 추가

dependencies {
    implementation "androidx.room:room-runtime:2.6.1"
    annotationProcessor "androidx.room:room-compiler:2.6.1"
    implementation 'androidx.recyclerview:recyclerview:1.3.2'
    implementation 'androidx.lifecycle:lifecycle-viewmodel:2.7.0'
    implementation 'androidx.lifecycle:lifecycle-livedata:2.7.0'
}

2. 데이터 설계
model 패키지 - Game.java, Schedule.java
database 패키지 - Schedule.java, Appdatabase.java

3. Room 데이터베이스 초기화
MyApplication.java 또는 DatabaseClient.java 생성해서 Room 데이터베이스 싱글톤으로 초기화

4. 게임 목록 화면 만들기
Mainactivity.java
게임 리스트를 보여주는 RecycleView
스타레일, 원신, 젠레스 존 제로, 명조 리스트 추가
클릭하면 GameDetailActivity로 이동

5. 게임별 일정 화면 만들기
GameDetailActivity.java
해당 게임에 등록된 일정 리스트 보여주기 (RecyclerView)
일정 추가 버튼 (+) 만들기 → ScheduleAddActivity로 이동

6. 일정 추가 화면 만들기
ScheduleAddActivity.java
일정 이름, 날짜, 시간, 메모, 반복 설정 입력받기
저장 버튼 누르면 Room DB에 추가
저장 후 알람 설정 (AlarmManager 사용해서)

7. 알림 기능 구현
저장된 일정 시간 기준으로 AlarmManager에 등록
시간이 되면 AlarmReceiver가 Notification 띄움

8. 일정 반복 기능 구현
매일/매주/특정 요일 설정에 따라 Alarm 재등록

9. 일정 공유 기능
공유 버튼 누르면 Intent로 링크 또는 QR코드 생성
상대방은 링크 클릭시 일정 저장 가능

요약
1	프로젝트 생성, 라이브러리 추가 -> build.gradle
2	데이터 설계 (모델, DAO) ->Game.java, Schedule.java, ScheduleDao.java
3	Room DB 초기화 -> AppDatabase.java
4	게임 목록 UI 작성	-> MainActivity.java
5	게임별 일정 화면 작성 -> GameDetailActivity.java
6	일정 추가 화면 작성 -> ScheduleAddActivity.java
7	알림 기능 연결 -> AlarmHelper.java, AlarmReceiver.java
8	반복 일정 기능 -> ScheduleAddActivity.java 수정
9	일정 공유 기능 -> 공유 기능 코드 추가

📌설계 문서 작성

1. 소개
1-1 목적
이 문서는 GameAlarm 시스템의 소프트웨어 아키텍처를 다각도로 설명하며, 설계에 있어 중요한 결정을 문서화하고 팀원들이 이해할 수 있도록 하는 것을 목표로 한다.

1-2 범위
이 문서는 GameAlarm 앱의 주요 기능, 데이터 저장 방식(JSON), 알람 처리 로직, 반복 일정 및 공유 기능 등 전체 시스템 구조에 영향을 주는 내용을 다룬다.

1-3 용어 정의
GameAlarm: 유저가 게임별 일정을 관리할 수 있는 안드로이드 앱
JSON: 일정 데이터 저장을 위한 구조화된 문자열 포맷
SharedPreferences: Android의 기본 로컬 저장소

1-4 참고 자료
Android 개발자 문서
Gson 라이브러리 문서
Firebase 클라우드 메시징 (향후 확장 고려)

1-5 문서 구성
이 문서는 5가지 아키텍처 뷰(유스케이스, 논리, 프로세스, 배포, 구현)에 따라 내용을 설명하며, JSON 기반의 데이터 흐름 및 저장 구조를 포함한다.

2. 아키텍처 표현 방식
논리 뷰: Game, Schedule, DataManager 클래스 중심 구조
유스케이스 뷰: 게임 등록, 일정 등록, 알림 설정, 반복 설정, 일정 공유
프로세스 뷰: 알람 등록 및 Notification 발송 흐름
배포 뷰: 안드로이드 단말기 로컬 저장소 기반
구현 뷰: 액티비티 중심 Java 기반 앱 구조

3. 아키텍처 목표 및 제약 사항
3-1 목표
유저가 여러 게임의 일정을 쉽게 관리
반복 알림, 공유 기능 제공
직관적인 UI와 가벼운 구조

3-2 제약 사항
로컬 저장소만 사용
Room DB가 아닌 JSON + SharedPreferences 사용
Java 기반 구현

4. 유스케이스 뷰
게임 추가 : 게임 이름과 아이콘 선택 후 저장
일정 추가 : 일정 제목, 날짜, 시간, 메모 입력 후 저장
반복 일정 설정 : 매일, 매주, 특정 요일 반복 선택
일정 공유 : JSON 또는 QR을 통한 내보내기 / 가져오기

4-1 유스케이스 실현
일정 저장 → SharedPreferences에 JSON 저장
공유 기능 → Gson으로 변환 → Intent 또는 QR코드 생성

5. 논리 뷰
5-1 개요
Game, Schedule 클래스 기반으로 설계
Datamanager는 JSON 변환 및 저장 처리 담당

5.2 설계 패키지 상세
5-2-1 Game 클래스
public class Game {
    public int id;
    public String name;
    public String iconUri;
}

5-2-2 Schedule 클래스
public class Schedule {
    public int id;
    public int gameId;
    public String title;
    public String date;
    public String time;
    public String memo;
    public boolean isRepeating;
    public String repeatType;
    public List<String> repeatDays;
}

5-2-3 DataManager 클래스
JSON 저장/불러오기 처리
SharedPreferences에 "games와 "schedule키로 저장

6. 프로세스 뷰
반복 일정은 앱 시작 시 재등록 처리

7. 배포 뷰
물리노드 : 안드로이드 스마트폰
저장소 위치 : SharedPreferences
네트워크 : 로컬 기반

8. 구현 뷰
8-1 개요
Activity 위주로 구성된 단일 모듈 구조
주요 구성요소 : UI 레이어, 데이터 레이어

8-2 레이어 구성
UI 레이어 : MainActivity, GameDetailActivity, ScheduleAddActivity
데이터 레이어 : Game, Schedule, DataManager

9. 데이터 뷰
형태 : JSON(문자열 형태로 저장)
저장소 : SharedPreferences
처리 도구 : Gson 라이브러리 사용

예시코드
[
  {
    "id": 1,
    "gameId": 2,
    "title": "일일 퀘스트",
    "date": "2025-05-21",
    "time": "10:00",
    "isRepeating": true,
    "repeatType": "weekly",
    "repeatDays": ["월", "수", "금"]
  }
]

10. 크기 및 성능
소규모 유저 데이터에 최적화
성능 이슈 없이 수백 건의 일정 처리 가능
RecyclerView 사용으로 UI 렌더링 최적화

11. 품질 속성
품질 속성
유지보수성: 간단한 JSON 구조로 유지보수 용이
확장성: 서버 연동 및 Firebase 기능 향후 추가 가능
안정성: Android 시스템 API 사용으로 안정성 확보
경량성: 로컬 저장소만 활용해 빠른 실행

메인화면(MainActivity)
![image](https://github.com/user-attachments/assets/bc9379ab-cd88-4347-b53f-36fe434015eb)

일정 추가 화면
![image](https://github.com/user-attachments/assets/d699f6ae-8aec-4a1a-9151-891e65a3b7f8)

캘린더 화면
![image](https://github.com/user-attachments/assets/e0b986b5-095c-495f-a6bd-3821db85cf4d)








