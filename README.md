# Tutor Matching Platform - Backend

튜터와 튜티를 매칭하고  
예약 → 결제 → 수업 확정까지 이어지는 흐름을 설계한  
구간 기반 예약 시스템 중심의 백엔드 프로젝트입니다.

팀 프로젝트에서 백엔드 핵심 로직을 담당했으며,  
인증 구조 설계와 예약/결제 상태 일관성 확보에 집중했습니다.

---

## 🧑‍💻 My Role

- JwtAuthenticationFilter 직접 구현 및 인증 흐름 설계
- SecurityContext 기반 Stateless 인증 구조 적용
- 구간 기반 예약 충돌 서버 재검증 로직 구현
- 과거 시간 예약 차단 로직 설계
- Toss 결제 검증 후 booking 상태 변경 트랜잭션 처리
- 튜터 마이페이지 관련 백엔드 API 설계 및 구현
- MyBatis 기반 데이터 매핑 및 쿼리 설계

---

## 🛠 Tech Stack

**Backend**
- Java 23
- Spring Boot
- Spring Security
- MyBatis
- JWT / OAuth2

**Database**
- MySQL

**External API**
- Toss Payments API
- OpenAI API

---

## ⚙️ Architecture

Client  
→ JwtAuthenticationFilter  
→ Controller  
→ Service  
→ Mapper (MyBatis)  
→ MySQL  

External  
→ Toss Payments API  
→ OpenAI API  

Stateless 인증 구조를 기반으로 요청 단위 인증을 처리하며,  
비즈니스 로직은 Service 계층에서 관리하도록 설계했습니다.

---

## 🔥 Key Technical Decisions

### 1️⃣ 예약 충돌 서버 재검증

FullCalendar 기반 드래그 수정 시  
클라이언트 검증만으로는 시간 겹침 우회 가능성이 존재했습니다.

→ 서버에서 기존 예약 구간과 요청 구간을 비교하여  
겹침 여부를 판단하는 검증 로직을 구현했습니다.

이를 통해 예약 데이터의 무결성을 확보했습니다.

---

### 2️⃣ JWT 인증 흐름 설계

- Filter 단계에서 토큰 검증
- Authentication 객체 생성
- SecurityContextHolder에 저장
- 요청 종료 시 자동 제거

세션을 사용하지 않는 Stateless 인증 구조를 적용하여  
DB 조회를 최소화했습니다.

---

### 3️⃣ 결제-예약 상태 일관성 확보

결제 성공 후 booking 상태 변경 과정에서  
예외 발생 시 데이터 불일치 가능성이 존재했습니다.

→ @Transactional 적용  
→ 결제 검증과 상태 변경을 하나의 트랜잭션으로 묶어 처리  

이를 통해 결제-예약 상태 정합성을 보장했습니다.

---

## 📊 Core Domain Tables

- users
- tutor_profile
- booking
- payment
- review

예약과 결제 상태 흐름을 고려하여  
booking–payment 관계를 설계했습니다.
