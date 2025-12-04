![logo](https://github.com/user-attachments/assets/a4ead842-5050-4658-93a6-b1f5d148d154)

---

### TEAM 주토피아
<table>
  <tr align="center">
    <td>김정훈</td>
    <td>최혜수</td>
    <td>조강현</td>
    <td>이은경</td>
  </tr>
  <tr align="center">
    <td><a target="_blank" href="https://github.com/jeonghuny"><img src="https://github.com/user-attachments/assets/e3ac8e79-a981-4a0e-9d55-19c775231a6a" width="130px"><br>@jeonghuny</a></td>
    <td><a target="_blank" href="https://github.com/Hyesu-Choi"><img src="https://github.com/user-attachments/assets/53607573-e9ba-4aab-8ec6-6bd087e2fafd" width="130px"><br>@Hyesu-Choi</a> </td>
    <td><a target="_blank" href="https://github.com/ybrocks"><img src="https://github.com/user-attachments/assets/0b87c805-69be-4fc2-91f9-61ebcd067131" width="130px"><br>@ybrocks</a> </td>
    <td><a target="_blank" href="https://github.com/DLDMSRUD-BIT"><img src="https://github.com/user-attachments/assets/c17e8a30-6f48-47af-aa52-dd2369b3ea86" width="130px"><br>@DLDMSRUD-BIT</a> </td>
  </tr>
</table>

---

### 스택
<p>
<img src="https://img.shields.io/badge/mariaDB-003545?style=for-the-badge&logo=mariaDB&logoColor=white">
<img src="https://img.shields.io/badge/git-F05032?style=for-the-badge&logo=git&logoColor=white">
<img src="https://img.shields.io/badge/github-181717?style=for-the-badge&logo=github&logoColor=white">
<img src="https://img.shields.io/badge/discord-5865F2?style=for-the-badge&logo=discord&logoColor=white">
</p>

---

### 프로젝트 개요
음악 스트리밍 시장에서 개인화 추천과 구독 기반 수익 모델의 안정성이 중요해짐에 따라, 본 프로젝트는 안정적이고 확장 가능한 데이터베이스 설계를 목표로 합니다. 회원 관리, 음악 서비스, 플레이리스트, 차트 등 핵심 기능을 지원하며, 재생 기록을 최소 1년 이상 보관하고 분석할 수 있도록 합니다. 구독 상태와 만료일을 정확히 관리하여 서비스 운영의 효율성을 높입니다

음악 스트리밍 서비스는 개인화 추천과 구독 관리 등 다양한 기능을 안정적으로 지원할 수 있는 데이터베이스가 필요합니다. 기존 DB 구조는 장기 재생 기록 보관과 다대다 관계 처리, 구독 상태 추적에서 한계를 보여 서비스 운영에 제약이 있었습니다. 이에 본 프로젝트는 핵심 비즈니스 로직을 지원하고, 신규 미디어 타입 추가에도 유연하게 대응할 수 있는 설계를 목표로 합니다. 또한 장기적인 데이터 보관과 분석을 통해 사용자 경험을 개선하고 서비스 운영 효율성을 높이고자 합니다.

### 주요 기능

<details>
  <summary><b>1. 회원 등급 및 구독 관리 시스템</b></summary>
  회원을 무료(Free)와 프리미엄(Premium)으로 구분하고, 유료 결제 시 구독 기간을 관리합니다.
DB 매핑:
Users 테이블의 membership 컬럼('Free'/'Premium')으로 등급 구분.
User_Subscription 테이블의 start_date, end_date로 유효기간 체크.
Payments 테이블에 결제 이력을 남기고, auto_renew 값에 따라 스케줄러가 만료일 자동 갱신 처리.
  </details>
  <details>
  <summary><b>2. 음악 재생 및 청취 이력 로그</b></summary>
음악을 재생하면 로그를 남기고, 이를 바탕으로 개인별 '많이 들은 곡'이나 '최근 들은 곡'을 제공합니다.
DB 매핑:
재생 시 Play_Log에 played_at(시작시간)과 play_duration(청취시간)을 INSERT.
동시에 UserSongStat 테이블의 play_count를 +1 하고 last_played_at을 갱신 (개인화 데이터).
      </details>
        <details>
  <summary><b>3. 커스텀 플레이리스트 (순서 편집 포함)</b></summary>  
          나만의 재생목록을 만들고, 곡의 순서를 자유롭게 바꿉니다. 공개 설정 시 타인도 검색 가능합니다.
DB 매핑:
Playlists 테이블의 is_public 컬럼으로 공개/비공개 처리.
Playlist_Song 테이블의 display_order 컬럼을 사용하여 사용자가 지정한 곡 순서를 저장 및 정렬.
          </details>
        <details>
  <summary><b>4. 아티스트/곡 상호작용 (좋아요 & 팔로우)</b></summary> 
          좋아하는 아티스트를 팔로우하여 모아보고, 마음에 드는 곡에 좋아요를 누릅니다. (중복 방지 필수)
DB 매핑:
팔로우: User_Artist 테이블 사용 (User ↔ Artist N:M 관계).
좋아요: User_Song 테이블 사용 (User ↔ Song N:M 관계).
두 테이블 모두 (user_id, 대상_id) 유니크 제약을 통해 중복 클릭 방지.
          </details>
        <details>
  <summary><b>5. 차트 집계 및 순위 변동 제공</b></summary>
일간/주간/월간 단위로 인기 곡 순위를 보여주고, 지난 순위 대비 상승/하락 폭을 알려줍니다.
DB 매핑:
Play_Log를 집계하여 Charts 테이블(메타정보)과 Chart_Song 테이블(순위정보) 생성.
Chart_Song의 rank_change 컬럼을 통해 "▲2", "▼1", "New" 등의 UI 표시 구현.
  </details>
        <details>
  <summary><b>6. 멀티 아티스트 참여 정보 제공</b></summary>
 하나의 곡에 메인 가수, 피처링, 작곡가 등 여러 아티스트가 참여한 정보를 정확히 표기합니다.
DB 매핑:
Song_Artist 테이블의 artist_type 컬럼('Main', 'Feat', 'Composer')을 활용.
곡 상세 조회 시 이 테이블을 조인하여 참여한 모든 아티스트를 역할별로 그룹화하여 응답.
          </details>
        <details>
  <summary><b>7. 앨범 및 수록곡 구조화</b></summary>
앨범을 선택하면 수록된 곡들이 트랙 순서대로 나열되고, 앨범 자체의 발매일과 타입을 보여줍니다.
DB 매핑:
Songs 테이블의 album_id로 앨범과 연결.
Songs 테이블의 track_number 컬럼을 사용하여 앨범 내 수록곡 정렬 순서 보장.
Songs 테이블의 is_title 컬럼으로 앨범 내 타이틀곡 식별 강조.
  </details>
  
---

### WBS

---

### 요구사항정의서

---

### ERD
<img width="2790" height="1972" alt="test" src="https://github.com/user-attachments/assets/bdb16b32-a70f-4074-b8df-40a120e1ab83" />

---

### 쿼리

#### DDL

#### DML

#### 프로시저
