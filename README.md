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
회원을 무료('Free')와 프리미엄('Premium')으로 구분하고, 유료 결제 시 구독 기간을 관리합니다.
Users 테이블의 membership 컬럼('Free'/'Premium')으로 등급 구분.
User_Subscription 테이블의 start_date, end_date로 유효기간 체크.
Payments 테이블에 결제 이력을 남기고, auto_renew 값에 따라 스케줄러가 만료일 자동 갱신 처리.
  </details>
  <details>
  <summary><b>2. 음악 재생 및 청취 이력 로그</b></summary>
음악을 재생하면 로그를 남기고, 이를 바탕으로 개인별 '많이 들은 곡'이나 '최근 들은 곡'을 제공합니다.
재생 시 Play_Log에 played_at(시작시간)과 play_duration(청취시간)을 INSERT.
동시에 UserSongStat 테이블의 play_count를 +1 하고 last_played_at을 갱신 (개인화 데이터).
      </details>
        <details>
  <summary><b>3. 커스텀 플레이리스트 (순서 편집 포함)</b></summary>  
          나만의 재생목록을 만들고, 곡의 순서를 자유롭게 바꿉니다. 공개 설정 시 타인도 검색 가능합니다.
Playlists 테이블의 is_public 컬럼으로 공개/비공개 처리.
Playlist_Song 테이블의 display_order 컬럼을 사용하여 사용자가 지정한 곡 순서를 저장 및 정렬.
          </details>
        <details>
  <summary><b>4. 아티스트/곡 상호작용 (좋아요 & 팔로우)</b></summary> 
          좋아하는 아티스트를 팔로우하여 모아보고, 마음에 드는 곡에 좋아요를 누릅니다. (중복 방지 필수)
팔로우: User_Artist 테이블 사용 (User ↔ Artist N:M 관계).
좋아요: User_Song 테이블 사용 (User ↔ Song N:M 관계).
두 테이블 모두 (user_id, 대상_id) 유니크 제약을 통해 중복 클릭 방지.
          </details>
        <details>
  <summary><b>5. 차트 집계 및 순위 변동 제공</b></summary>
일간/주간/월간 단위로 인기 곡 순위를 보여주고, 지난 순위 대비 상승/하락 폭을 알려줍니다.  
          
Play_Log를 집계하여 Charts 테이블(메타정보)과 Chart_Song 테이블(순위정보) 생성.  
Chart_Song의 rank_change 컬럼을 통해 "▲2", "▼1", "New" 등의 UI 표시 구현.
  </details>
        <details>
  <summary><b>6. 멀티 아티스트 참여 정보 제공</b></summary>
 하나의 곡에 메인 가수, 피처링, 작곡가 등 여러 아티스트가 참여한 정보를 정확히 표기합니다.
Song_Artist 테이블의 artist_type 컬럼('Main', 'Feat', 'Composer')을 활용.
곡 상세 조회 시 이 테이블을 조인하여 참여한 모든 아티스트를 역할별로 그룹화하여 응답.
          </details>
        <details>
  <summary><b>7. 앨범 및 수록곡 구조화</b></summary>
앨범을 선택하면 수록된 곡들이 트랙 순서대로 나열되고, 앨범 자체의 발매일과 타입을 보여줍니다.
Songs 테이블의 album_id로 앨범과 연결.
Songs 테이블의 track_number 컬럼을 사용하여 앨범 내 수록곡 정렬 순서 보장.
Songs 테이블의 is_title 컬럼으로 앨범 내 타이틀곡 식별 강조.
  </details>
  
---

### WBS
<details>
  <summary><b>기획</b></summary>
<img width="1426" height="337" alt="11" src="https://github.com/user-attachments/assets/6f6e6eaa-1594-476a-8c6e-2636b110ed65" />
  </details>
  <details>
  <summary><b>설계</b></summary>
<img width="1426" height="578" alt="12" src="https://github.com/user-attachments/assets/b4351aa4-d008-4e7f-958f-2a88031f1d6c" />
  </details>
<details>
  <summary><b>구축</b></summary>
<img width="1426" height="701" alt="13" src="https://github.com/user-attachments/assets/4073f836-17ec-4dfb-8eca-911e2891f956" />
  </details>
  <details>
  <summary><b>테스트</b></summary>
<img width="1426" height="171" alt="14" src="https://github.com/user-attachments/assets/9a0a4236-4bc2-4ae1-b0d0-9b9f5feace8d" />
  </details>
  
---

### 요구사항정의서
  <details>
  <summary><b>1. 회원관리</b></summary>
<img width="1780" height="147" alt="1" src="https://github.com/user-attachments/assets/b857df7b-1628-442c-bc7d-15893d600ec2" />
  </details>
  <details>
  <summary><b>2. 음악서비스</b></summary>
<img width="1778" height="191" alt="2" src="https://github.com/user-attachments/assets/b54d99b3-7a6f-4d2a-bebb-b075bbfab567" />
  </details>
    <details>
  <summary><b>3. 앨범관리</b></summary>
<img width="1780" height="87" alt="3" src="https://github.com/user-attachments/assets/1b073700-8849-4e1a-850c-0e2677e6e0ec" />
    </details>
    <details>
  <summary><b>4. 아티스트관리</b></summary>
<img width="1777" height="98" alt="4" src="https://github.com/user-attachments/assets/83c79a39-23e5-4978-b68b-7be98069011e" />
  </details>
    <details>
  <summary><b>5. 플레이리스트관리</b></summary>
<img width="1778" height="127" alt="5" src="https://github.com/user-attachments/assets/87b82d13-ca1b-4c88-aa1a-362bd6f22a8c" />
    </details>
    <details>
  <summary><b>6. 차트/통계</b></summary>
<img width="1780" height="53" alt="6" src="https://github.com/user-attachments/assets/b4c6bbd1-bf26-4ada-b0c1-6bbea6020ba4" />
  </details>
    <details>
  <summary><b>7. 구독/결제</b></summary>
<img width="1780" height="123" alt="7" src="https://github.com/user-attachments/assets/706d536a-608a-4022-ae62-e6a71d471ec9" />
    </details>
  
---

### ERD
<img width="3150" height="2032" alt="test" src="https://github.com/user-attachments/assets/2ca1286d-ef1a-49a0-a024-2560ea180df2" />

---

### 쿼리

  <details>
  <summary><b>DDL</b></summary>

```sql
-- 1. Users 테이블: 사용자 기본 정보
CREATE TABLE users (
    user_id BIGINT PRIMARY KEY AUTO_INCREMENT COMMENT '사용자 기본 키',
    email VARCHAR(100) UNIQUE NOT NULL COMMENT '이메일 (로그인 ID)',
    password VARCHAR(255) NOT NULL COMMENT '비밀번호 (암호화)',
    nickname VARCHAR(50) UNIQUE NOT NULL COMMENT '닉네임',
    membership ENUM('Free', 'Premium') NOT NULL DEFAULT 'Free' COMMENT '회원 등급: Free, Premium',
    created_at DATETIME NOT NULL DEFAULT CURRENT_TIMESTAMP COMMENT '가입일시',
    is_admin BOOLEAN NOT NULL COMMENT '관리자여부'
) COMMENT '사용자 기본 정보';

-- 2. Artists 테이블: 가수/밴드 정보
CREATE TABLE artists (
    artist_id BIGINT PRIMARY KEY AUTO_INCREMENT COMMENT '아티스트 기본 키',
    name VARCHAR(100) NOT NULL COMMENT '활동명',
    agency VARCHAR(100) COMMENT '기획사',
    debut_date DATE COMMENT '데뷔날짜',
    image_url TEXT COMMENT '이미지 URL'
) COMMENT '가수/밴드 정보';

-- 3. Albums 테이블: 앨범 정보
CREATE TABLE albums (
    album_id BIGINT PRIMARY KEY AUTO_INCREMENT COMMENT '앨범 기본 키',
    title VARCHAR(255) NOT NULL COMMENT '앨범 제목',
    release_date DATE NOT NULL COMMENT '발매일',
    cover_img_url TEXT COMMENT '커버 이미지 URL',
    album_type VARCHAR(20) COMMENT '앨범 타입: Single, EP, Full'
) COMMENT '앨범 정보';

-- 4. Songs 테이블: 개별 음원 트랙 정보
CREATE TABLE songs (
    song_id BIGINT PRIMARY KEY AUTO_INCREMENT COMMENT '곡 기본 키',
    album_id BIGINT NOT NULL COMMENT 'FK: Albums 테이블 참조',
    title VARCHAR(255) NOT NULL COMMENT '곡 제목',
    track_number INT COMMENT '앨범 내 트랙 순서',
    duration INT NOT NULL COMMENT '재생 시간 (초)',
    lyrics TEXT COMMENT '가사 정보',
    file_url TEXT NOT NULL COMMENT '음원 파일 경로',
    is_title BOOLEAN NOT NULL COMMENT '타이틀 곡 여부 (0:false, 1:true)',
    CONSTRAINT fk_song_album FOREIGN KEY (album_id) REFERENCES albums(album_id)
) COMMENT '개별 음원 트랙 정보';

-- 5. Genres 테이블: 장르 분류 마스터
CREATE TABLE genres (
    genre_id BIGINT PRIMARY KEY AUTO_INCREMENT COMMENT '장르 기본 키',
    name VARCHAR(50) UNIQUE NOT NULL COMMENT '장르명'
) COMMENT '장르 분류 마스터';

-- 6. Song_Artist 테이블: 곡 ↔ 아티스트 참여 관계
CREATE TABLE song_Artist (
    id BIGINT PRIMARY KEY AUTO_INCREMENT COMMENT '관계 기본 키',
    song_id BIGINT NOT NULL COMMENT 'FK: Songs 테이블 참조',
    artist_id BIGINT NOT NULL COMMENT 'FK: Artists 테이블 참조',
    artist_type VARCHAR(100) NOT NULL COMMENT '참여 유형: Main, Feat, Composer 등',
    CONSTRAINT fk_sa_song FOREIGN KEY (song_id) REFERENCES songs(song_id),
    CONSTRAINT fk_sa_artist FOREIGN KEY (artist_id) REFERENCES artists(artist_id),
    UNIQUE KEY uix_song_artist (song_id, artist_id) -- 한 곡에 한 아티스트는 한 번만 등록 (역할은 다를 수 있지만, 기본적으로 참여 여부 판단)
) COMMENT '곡 ↔ 아티스트 참여 관계';

-- 7. Song_Genre 테이블: 곡 ↔ 장르 연결 관계
CREATE TABLE song_Genre (
    id BIGINT PRIMARY KEY AUTO_INCREMENT COMMENT '관계 기본 키',
    song_id BIGINT NOT NULL COMMENT 'FK: Songs 테이블 참조',
    genre_id BIGINT NOT NULL COMMENT 'FK: Genres 테이블 참조',
    CONSTRAINT fk_sg_song FOREIGN KEY (song_id) REFERENCES songs(song_id),
    CONSTRAINT fk_sg_genre FOREIGN KEY (genre_id) REFERENCES genres(genre_id),
    UNIQUE KEY uix_song_genre (song_id, genre_id) -- 한 곡에 같은 장르는 한 번만
) COMMENT '곡 ↔ 장르 연결 관계';

-- 8. Playlist_Song 테이블: 플레이리스트 ↔ 곡 수록 관계
CREATE TABLE playlist_Song (
    id BIGINT PRIMARY KEY AUTO_INCREMENT COMMENT '관계 기본 키',
    playlist_id BIGINT NOT NULL COMMENT 'FK: Playlists 테이블 참조',
    song_id BIGINT NOT NULL COMMENT 'FK: Songs 테이블 참조',
    display_order INT NOT NULL COMMENT '플레이리스트 내 곡 순서',
    added_at DATETIME NOT NULL COMMENT '플레이리스트에 곡 추가 시점',
    CONSTRAINT fk_ps_playlist FOREIGN KEY (playlist_id) REFERENCES playlists(playlist_id),
    CONSTRAINT fk_ps_song FOREIGN KEY (song_id) REFERENCES songs(song_id),
    UNIQUE KEY uix_playlist_song (playlist_id, song_id) -- 한 플레이리스트에 동일한 곡 중복 추가 불가 (제약조건)
) COMMENT '플레이리스트 ↔ 곡 수록 관계';

-- 9. User_Artist 테이블: 회원 ↔ 아티스트 팔로우 관계
CREATE TABLE user_Artist (
    id BIGINT PRIMARY KEY AUTO_INCREMENT COMMENT '관계 기본 키',
    user_id BIGINT NOT NULL COMMENT 'FK: Users 테이블 참조',
    artist_id BIGINT NOT NULL COMMENT 'FK: Artists 테이블 참조',
    follow_created_at DATETIME COMMENT '팔로우 시점',
    CONSTRAINT fk_ua_user FOREIGN KEY (user_id) REFERENCES users(user_id),
    CONSTRAINT fk_ua_artist FOREIGN KEY (artist_id) REFERENCES artists(artist_id),
    UNIQUE KEY uix_user_artist (user_id, artist_id) -- 한 회원은 같은 아티스트를 한 번만 팔로우 (제약조건)
) COMMENT '회원 ↔ 아티스트 팔로우 관계';

-- 10. Album_Artist 테이블: 앨범 ↔ 아티스트 참여 (합작 등)
CREATE TABLE album_Artist (
    id BIGINT PRIMARY KEY AUTO_INCREMENT COMMENT '관계 기본 키',
    album_id BIGINT NOT NULL COMMENT 'FK: Albums 테이블 참조',
    artist_id BIGINT NOT NULL COMMENT 'FK: Artists 테이블 참조',
    roles VARCHAR(100) COMMENT '참여 역할: Main, Featuring 등',
    CONSTRAINT fk_aa_album FOREIGN KEY (album_id) REFERENCES albums(album_id),
    CONSTRAINT fk_aa_artist FOREIGN KEY (artist_id) REFERENCES artists(artist_id),
    UNIQUE KEY uix_album_artist (album_id, artist_id, roles) -- 앨범-아티스트-역할 조합 유니크
) COMMENT '앨범 ↔ 아티스트 참여 (합작) 관계';

-- 11. Playlists 테이블: 사용자 플레이리스트
CREATE TABLE playlists (
    playlist_id BIGINT PRIMARY KEY AUTO_INCREMENT COMMENT '플레이리스트 기본 키',
    user_id BIGINT NOT NULL COMMENT 'FK: Users 테이블 참조 (생성자)',
    title VARCHAR(100) NOT NULL COMMENT '플레이리스트 제목',
    is_public BOOLEAN NOT NULL DEFAULT 0 COMMENT '공개 여부 (0:비공개, 1:공개)',
    play_created_at DATETIME NOT NULL COMMENT '생성일시',
    CONSTRAINT fk_playlist_user FOREIGN KEY (user_id) REFERENCES users(user_id)
) COMMENT '사용자 플레이리스트';

-- 12. Subscription 테이블: 구독권 종류 정보
CREATE TABLE subscription (
    sub_id BIGINT PRIMARY KEY AUTO_INCREMENT COMMENT '구독권 기본 키',
    name VARCHAR(50) NOT NULL COMMENT '구독권명',
    price INT NOT NULL COMMENT '가격',
    period_days INT NOT NULL COMMENT '유효 기간 (일)'
) COMMENT '구독권 종류 정보';

-- 13. User_Subscription 테이블: 회원별 구독 현황
CREATE TABLE user_Subscription (
    us_id BIGINT PRIMARY KEY AUTO_INCREMENT COMMENT '구독 현황 기본 키',
    user_id BIGINT NOT NULL COMMENT 'FK: Users 테이블 참조',
    sub_id BIGINT NOT NULL COMMENT 'FK: Subscriptions 테이블 참조',
    start_date DATETIME NOT NULL COMMENT '구독 시작일',
    end_date DATETIME NOT NULL COMMENT '만료일',
    status VARCHAR(20) NOT NULL COMMENT '상태: Active, Expired',
    auto_renew BOOLEAN NOT NULL COMMENT '자동 갱신 여부',
    CONSTRAINT fk_usub_user FOREIGN KEY (user_id) REFERENCES users(user_id),
    CONSTRAINT fk_usub_sub FOREIGN KEY (sub_id) REFERENCES subscription(sub_id)
    -- 한 회원은 동시에 하나의 구독권만 보유 (제약조건 - Active 상태에 대해)
    -- 논리적 제약이지만, 테이블 구조상 UNIQUE KEY (user_id, status='Active')는 DB 제약으로 구현하기 어려움.
    -- 대신, 현재 활성 구독권 조회 시 status='Active' 필터링 사용.
) COMMENT '회원별 구독 현황';

-- 14. Payments 테이블: 결제 내역 기록
CREATE TABLE payments (
    pay_id INT PRIMARY KEY AUTO_INCREMENT COMMENT '결제 기본 키',
    user_id BIGINT NOT NULL COMMENT 'FK: Users 테이블 참조',
    us_id BIGINT COMMENT 'FK: User_Subscription 테이블 참조 (어떤 구독에 대한 결제인지)',
    amount INT NOT NULL COMMENT '결제 금액',
    pay_method VARCHAR(50) COMMENT '결제 수단',
    paid_at DATETIME NOT NULL COMMENT '결제일시',
    is_success VARCHAR(20) NOT NULL COMMENT 'true : 성공, false : 실패',
    CONSTRAINT fk_pay_user FOREIGN KEY (user_id) REFERENCES users(user_id),
    CONSTRAINT fk_pay_usub FOREIGN KEY (us_id) REFERENCES user_Subscription(us_id)
) COMMENT '결제 내역 기록';

-- 15. Play_Log 테이블:  재생 기록 로그
CREATE TABLE play_Log (
    log_id BIGINT PRIMARY KEY AUTO_INCREMENT COMMENT '로그 기본 키',
    user_id BIGINT NOT NULL COMMENT 'FK: Users 테이블 참조',
    song_id BIGINT NOT NULL COMMENT 'FK: Songs 테이블 참조',
    started_at DATETIME NOT NULL COMMENT '재생 시작 시간',
    ended_at DATETIME NOT NULL COMMENT '재생 종료 시간',
    CONSTRAINT fk_log_user FOREIGN KEY (user_id) REFERENCES users(user_id),
    CONSTRAINT fk_log_song FOREIGN KEY (song_id) REFERENCES songs(song_id)
) COMMENT '모든 재생 이벤트 로그';

-- 16. UserSongStat 테이블: 회원별 곡 누적 통계
CREATE TABLE userSongStat (
    user_id BIGINT COMMENT 'PK, FK: Users 테이블 참조',
    song_id BIGINT COMMENT 'PK, FK: Songs 테이블 참조',
    play_count INT NOT NULL DEFAULT 0 COMMENT '누적 재생 횟수',
    last_played_at DATETIME COMMENT '최종 재생 시간',
    is_like BOOLEAN COMMENT '좋아요 여부',
    PRIMARY KEY (user_id, song_id), -- 복합 기본 키
    CONSTRAINT fk_uss_user FOREIGN KEY (user_id) REFERENCES users(user_id),
    CONSTRAINT fk_uss_song FOREIGN KEY (song_id) REFERENCES songs(song_id)
) COMMENT '회원별 곡 누적 통계';

-- 17. Charts 테이블: 일간/주간/월간 차트 정의
CREATE TABLE charts (
    chart_id BIGINT PRIMARY KEY AUTO_INCREMENT COMMENT '차트 기본 키',
    title VARCHAR(255) COMMENT '차트 제목 (예: 2023년 5월 1주차 주간 차트)',
    chart_type VARCHAR(100) NOT NULL COMMENT '차트 유형: Daily, Weekly, Monthly',
    base_time DATETIME NOT NULL COMMENT '집계 기준 일시'
) COMMENT '일간/주간/월간 차트 정의';

-- 18. Chart_Song 테이블: 차트별 곡 순위 정보
CREATE TABLE chart_Song (
    id BIGINT PRIMARY KEY AUTO_INCREMENT COMMENT '관계 기본 키',
    chart_id BIGINT NOT NULL COMMENT 'FK: charts 테이블 참조',
    song_id BIGINT NOT NULL COMMENT 'FK: songs 테이블 참조',
    rank INT NOT NULL COMMENT '순위',
    rank_change INT COMMENT '전주 대비 순위 변동',
    CONSTRAINT fk_cs_chart FOREIGN KEY (chart_id) REFERENCES charts(chart_id),
    CONSTRAINT fk_cs_song FOREIGN KEY (song_id) REFERENCES songs(song_id),
    UNIQUE KEY uix_chart_ranking (chart_id, ranking), -- 한 차트에서 순위는 중복될 수 없음
    UNIQUE KEY uix_chart_song (chart_id, song_id) -- 한 곡은 한 차트에 한 번만 등록
) COMMENT '차트별 곡 순위 정보';
```
  </details>
  
  <details>
  <summary><b>테스트-유저</b></summary>

```sql
    -- 1) 회원가입 (중복검사 및 비밀번호 해시)
DELIMITER //
CREATE PROCEDURE sp_register_user(
    IN p_email VARCHAR(100),
    IN p_password VARCHAR(255),
    IN p_nickname VARCHAR(50)
)
BEGIN
    DECLARE v_cnt INT DEFAULT 0;
    START TRANSACTION;
    -- 이메일 중복 체크
    SELECT COUNT(*) INTO v_cnt FROM users WHERE email = p_email;
    IF v_cnt > 0 THEN
        ROLLBACK;
        SIGNAL SQLSTATE '45000' SET MESSAGE_TEXT = 'Email already exists';
    END IF;
    -- 닉네임 중복 체크
    SELECT COUNT(*) INTO v_cnt FROM users WHERE nickname = p_nickname;
    IF v_cnt > 0 THEN
        ROLLBACK;
        SIGNAL SQLSTATE '45000' SET MESSAGE_TEXT = 'Nickname already exists';
    END IF;
    -- 사용자 추가 (비밀번호 해시 없이 원문 저장)
    INSERT INTO users (email, password, nickname, membership, created_at, is_admin)
    VALUES (p_email, p_password, p_nickname, 'Free', NOW(), 0);
    COMMIT;
END //

    -- 2) 로그인 (비밀번호 검증 후 user_id 반환. 해시관련 로직 삭제, 유효성검증)
DELIMITER //
CREATE PROCEDURE sp_login_user(
    IN p_email VARCHAR(100),
    IN p_password VARCHAR(255),
    OUT o_user_id BIGINT
)
BEGIN
    -- 입력받은 비밀번호(p_password)를 그대로 비교
    -- 데이터가 없으면 o_user_id에 NULL이 할당됨
    SET o_user_id = (SELECT user_id 
                     FROM users 
                     WHERE email = p_email AND password = p_password);
    -- 일치하는 사용자가 없는 경우
    IF o_user_id IS NULL THEN
        SIGNAL SQLSTATE '45000' SET MESSAGE_TEXT = 'Invalid credentials';
    END IF;
END //
DELIMITER ;

-- 3) 비밀번호 변경(해시 로직 삭제)
DELIMITER //
CREATE PROCEDURE sp_update_password(
    IN p_user_id BIGINT,
    IN p_old_password VARCHAR(255),
    IN p_new_password VARCHAR(255)
)
BEGIN
    DECLARE v_cnt INT DEFAULT 0;
    -- 기존 비밀번호가 맞는지 확인 (해시 없이 원문 비교)
    SELECT COUNT(*) INTO v_cnt 
    FROM users 
    WHERE user_id = p_user_id AND password = p_old_password;

    IF v_cnt = 0 THEN
        SIGNAL SQLSTATE '45000' SET MESSAGE_TEXT = 'Old password is incorrect';
    END IF;
    -- 새 비밀번호로 업데이트 (해시 없이 원문 저장)
    UPDATE users 
    SET password = p_new_password 
    WHERE user_id = p_user_id;
END //
DELIMITER ;



-- 5) 구독 적용: User_Subscription 추가 및 Users.membership 업데이트 (간단한 예)
DELIMITER //
CREATE PROCEDURE sp_subscribe_user(
IN p_user_id BIGINT,
IN p_sub_id BIGINT,
IN p_start_date DATETIME,
IN p_period_days INT,
IN p_auto_renew BOOLEAN,
IN p_amount INT,
IN p_pay_method VARCHAR(50)
)
BEGIN
DECLARE v_end_date DATETIME;
DECLARE v_us_id BIGINT;
SET v_end_date = DATE_ADD(p_start_date, INTERVAL p_period_days DAY);
START TRANSACTION;
INSERT INTO user_Subscription (user_id, sub_id, start_date, end_date, status, auto_renew)
VALUES (p_user_id, p_sub_id, p_start_date, v_end_date, 'Active', p_auto_renew);
SET v_us_id = LAST_INSERT_ID();
INSERT INTO payments (user_id, us_id, amount, pay_method, paid_at, is_success)
VALUES (p_user_id, v_us_id, p_amount, p_pay_method, NOW(), 'true');
UPDATE users SET membership = 'Premium' WHERE user_id = p_user_id;
COMMIT;
END//
DELIMITER ;

-- 7) 곡 검색 기능
-- 조회 쿼리 두번 사용하여 사용자는 하나의 검색창에 키워드를 입력했을 때, 
-- 그 키워드가 포함된 곡, 가수, 앨범, 가사, 장르 뿐만 아니라 
-- 공개된 플레이리스트 제목까지 한 번에 볼 수 있게 됨.
DELIMITER //
CREATE PROCEDURE sp_search_all_content(
    IN p_keyword VARCHAR(255)
)
BEGIN
    -- 키워드 앞뒤에 %를 붙여 부분 일치 검색을 준비합니다.
    SET @keyword_pattern = CONCAT('%',p_keyword, '%');

    -- =======================================================
    -- 1. 곡(Song) 및 관련 정보 통합 검색 결과 반환
    -- (곡 제목, 가사, 앨범명, 가수명, 장르명)
    -- =======================================================
    SELECT DISTINCT
        s.song_id,
        s.title AS song_title,
        a.title AS album_title,
        GROUP_CONCAT(DISTINCT art.name SEPARATOR ', ') AS artists_involved,
        GROUP_CONCAT(DISTINCT g.name SEPARATOR ', ') AS genres_involved,
        s.duration,
        s.is_title
    FROM
        songs s
    JOIN
        albums a ON s.album_id = a.album_id
    LEFT JOIN
        song_Artist sa ON s.song_id = sa.song_id
    LEFT JOIN
        artists art ON sa.artist_id = art.artist_id
    LEFT JOIN
        song_Genre sg ON s.song_id = sg.song_id
    LEFT JOIN
        genres g ON sg.genre_id = g.genre_id
    WHERE
        s.title LIKE @keyword_pattern       -- 1. 곡 제목 검색
        OR s.lyrics LIKE @keyword_pattern   -- 2. 가사 검색
        OR a.title LIKE @keyword_pattern    -- 3. 앨범명 검색
        OR art.name LIKE @keyword_pattern   -- 4. 가수명 검색
        OR g.name LIKE @keyword_pattern     -- 5. 장르명 검색
    GROUP BY
        s.song_id, s.title, a.title, s.duration, s.is_title
    ORDER BY
        s.title;

    -- =======================================================
    -- 2. 공개된 플레이리스트 통합 검색 결과 반환
    -- (플레이리스트 제목)
    -- =======================================================
    SELECT
        p.playlist_id,
        p.title AS playlist_title,
        u.nickname AS creator_nickname,
        p.play_created_at,
        COUNT(ps.song_id) AS song_count
    FROM
        playlists p
    JOIN
        users u ON p.user_id = u.user_id
    LEFT JOIN
        playlist_Song ps ON p.playlist_id = ps.playlist_id
    WHERE
        p.is_public = 1                     -- 공개된 플레이리스트만
        AND p.title LIKE @keyword_pattern   -- 플레이리스트 제목 검색
    GROUP BY
        p.playlist_id, p.title, u.nickname, p.play_created_at
    ORDER BY
        p.title;

END//

DELIMITER ;

-- 8) 곡 재생 기능 (곡 재생 기록 로그 및 통계 업데이트)

DELIMITER //

CREATE PROCEDURE sp_play_song(
    IN p_user_id BIGINT,
    IN p_song_id BIGINT,
    IN p_started_at DATETIME,
    IN p_ended_at DATETIME
)
play_song:
BEGIN
    -- 재생 시간을 초 단위로 계산합니다. (간단한 유효성 검사)
    DECLARE v_duration_check INT;

    -- 트랜잭션 시작
    START TRANSACTION;

    -- 1. 유효성 검사 (재생 시간이 0초 이하이면 처리 중단)
    -- 시작 시간(p_started_at)과 종료 시간(p_ended_at) 사이의 간격을 지정된 단위로 반환
    SET v_duration_check = TIMESTAMPDIFF(SECOND, p_started_at, p_ended_at);
    
    IF v_duration_check <= 0 THEN
        ROLLBACK;
        SIGNAL SQLSTATE '45000' SET MESSAGE_TEXT = 'Playback duration must be positive.';
        LEAVE play_song;
    END IF;

    -- 2. play_Log 테이블에 재생 기록 로그 추가 (무조건 INSERT)
    INSERT INTO play_Log (user_id, song_id, started_at, ended_at)
    VALUES (p_user_id, p_song_id, p_started_at, p_ended_at);

    -- 3. userSongStat 테이블 업데이트/삽입 (누적 통계 처리)

    -- 이미 해당 곡을 재생한 기록이 있는지 확인
    INSERT INTO userSongStat (user_id, song_id, play_count, last_played_at, is_like)
    VALUES (p_user_id, p_song_id, 1, p_ended_at, NULL) -- 새 레코드를 추가하려 시도
    ON DUPLICATE KEY UPDATE -- 이미 있다면 (user_id, song_id가 복합 PK이므로) play_count(재생 수) + 1 업데이트 수행
        play_count = play_count + 1,
        last_played_at = p_ended_at;
        
    -- 트랜잭션 완료
    COMMIT;
END//

DELIMITER ;



-- 10) 아티스트 팔로우
DELIMITER //
CREATE PROCEDURE sp_follow_artist(
IN p_user_id BIGINT,
IN p_artist_id BIGINT
)
BEGIN
DECLARE v_exists INT DEFAULT 0;
SELECT COUNT(*) INTO v_exists FROM user_Artist WHERE user_id = p_user_id AND artist_id = p_artist_id;
IF v_exists > 0 THEN
SIGNAL SQLSTATE '45000' SET MESSAGE_TEXT = 'Already following';
END IF;
INSERT INTO user_Artist (user_id, artist_id, follow_created_at) VALUES (p_user_id, p_artist_id, NOW());
END//
DELIMITER ;


-- 11) 아티스트 / 언팔로우
DELIMITER //
CREATE PROCEDURE sp_unfollow_artist(
IN p_user_id BIGINT,
IN p_artist_id BIGINT
)
BEGIN
DELETE FROM user_Artist WHERE user_id = p_user_id AND artist_id = p_artist_id;
END//
DELIMITER ;

-- 12) 플레이리스트 생성
DELIMITER //
CREATE PROCEDURE sp_create_playlist(
IN p_user_id BIGINT,
IN p_title VARCHAR(100),
IN p_is_public BOOLEAN,
OUT o_playlist_id BIGINT
)
BEGIN
INSERT INTO playlists (user_id, title, is_public, play_created_at)
VALUES (p_user_id, p_title, p_is_public, NOW());
SET o_playlist_id = LAST_INSERT_ID();
-- LAST_INSERT_ID()는 현재 DB 세션(connection) 에서 마지막으로 실행한 AUTO_INCREMENT 값(직전 INSERT로 생성된 PK 값)을 반환하는 MySQL 내장 함수
-- OUT o_playlist_id 는 방금 생성된 playlist_id를 돌려주기 위한 반환값
END//
DELIMITER ;

-- 13) 플레이리스트에 곡 추가 (display_order가 NULL이면 끝에 추가)
DELIMITER //
CREATE PROCEDURE sp_add_song_to_playlist(
IN p_playlist_id BIGINT,
IN p_song_id BIGINT,
IN p_display_order INT
)
BEGIN
DECLARE v_max_order INT;
IF p_display_order IS NULL OR p_display_order <= 0 THEN
SELECT IFNULL(MAX(display_order),0) + 1 INTO v_max_order FROM playlist_Song WHERE playlist_id = p_playlist_id;
SET p_display_order = v_max_order;
ELSE
-- 다른 곡들의 order 조정 (간단히 뒤로 밀기)
UPDATE playlist_Song SET display_order = display_order + 1
WHERE playlist_id = p_playlist_id AND display_order >= p_display_order;
END IF;
INSERT INTO playlist_Song (playlist_id, song_id, display_order, added_at)
VALUES (p_playlist_id, p_song_id, p_display_order, NOW());
END//
DELIMITER ;

-- 14) 플레이리스트에서 곡 제거
DELIMITER //
CREATE PROCEDURE sp_remove_song_from_playlist(
IN p_playlist_id BIGINT,
IN p_song_id BIGINT
)   
BEGIN
DELETE FROM playlist_Song WHERE playlist_id = p_playlist_id AND song_id = p_song_id;
END//
DELIMITER ;

-- 15) 플레이리스트 내 곡 순서 변경 (안전하게 트랜잭션으로 처리)
DELIMITER //

CREATE PROCEDURE sp_change_playlist_order(
    IN p_playlist_id BIGINT,
    IN p_song_id BIGINT,
    IN p_new_order INT
)
sp_change_playlist_order: -- <<<<< 프로시저 이름과 동일한 레이블 추가
BEGIN
    -- 1. 변수 선언
    DECLARE v_old_order INT;

    -- 2. 트랜잭션 시작
    START TRANSACTION;

    -- 3. 현재 순서 조회 및 동시성 제어를 위한 락 (FOR UPDATE)
    SELECT display_order 
      INTO v_old_order 
      FROM playlist_Song 
     WHERE playlist_id = p_playlist_id 
       AND song_id = p_song_id 
       FOR UPDATE;

    -- 4. 예외 처리: 플레이리스트에 곡이 없는 경우
    IF v_old_order IS NULL THEN
        ROLLBACK;
        SIGNAL SQLSTATE '45000' SET MESSAGE_TEXT = 'Song not in playlist';
    END IF;

    -- 5. 현재 순서와 같으면 변경 불필요
    IF p_new_order = v_old_order THEN
        COMMIT;
        -- 레이블이 정의되었으므로 LEAVE 사용 가능
        LEAVE sp_change_playlist_order;
    END IF;

    -- 6. 순서 변경 로직 (display_order 값이 클수록 플레이리스트에서 더 뒤쪽 순서)
    IF p_new_order < v_old_order THEN
        -- 끼워넣기 (뒤에서 앞으로 이동): 범위에 있는 항목들의 순서를 +1 증가
        UPDATE playlist_Song 
           SET display_order = display_order + 1
         WHERE playlist_id = p_playlist_id 
           AND display_order >= p_new_order 
           AND display_order < v_old_order;
    ELSE
        -- 뒤쪽순서으로 이동 (앞에서 뒤로 이동): 범위에 있는 항목들의 순서를 -1 감소
        UPDATE playlist_Song 
           SET display_order = display_order - 1
         WHERE playlist_id = p_playlist_id 
           AND display_order <= p_new_order 
           AND display_order > v_old_order;
    END IF;

    -- 7. 대상 곡의 순서를 새로운 순서로 업데이트
    UPDATE playlist_Song 
       SET display_order = p_new_order 
     WHERE playlist_id = p_playlist_id 
       AND song_id = p_song_id;

    -- 8. 트랜잭션 완료
    COMMIT;
END//
DELIMITER ;
```

  </details>

 <details>
  <summary><b>테스트-관리자</b></summary>
   
```sql
