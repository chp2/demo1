# Poker-Mate 튜토리얼 (Tutorial)

이 튜토리얼은 Poker-Mate 백엔드 서버를 로컬 환경에 설치하고, 실행하여 직접 API 요청을 보내보는 과정을 단계별로 안내합니다.

## 1. 사전 준비 (Prerequisites)
시작하기 전에 다음 도구들이 설치되어 있어야 합니다.
*   **Java 17** 이상 (`java -version` 으로 확인)
*   **Docker & Docker Compose** (MySQL과 Redis 실행용)

## 2. 설치 및 설정 (Installation)

### 저장소 복제 (Clone)
```bash
git clone https://github.com/YOUR_GITHUB_ID/poker-mate-backend.git
cd poker-mate-backend
```

### 환경 변수 설정 (Environment)
프로젝트 루트에 `.env` 파일을 생성하거나 `application.yml` 설정을 확인하여 OpenAI API 키 등을 입력해야 합니다. (데모 실행 시에는 H2/Embedded 모드가 활성화되어 있다면 생략 가능할 수 있습니다.)

## 3. 서버 실행 (Run Server)

### 인프라 실행 (MySQL, Redis)
도커를 이용하여 필요한 데이터베이스와 캐시 서버를 띄웁니다.
```bash
docker-compose up -d
```

### 애플리케이션 실행
Maven 래퍼(Wrapper)를 사용하여 서버를 시작합니다.
```bash
./mvnw spring-boot:run
```
서버가 정상적으로 떴다면 `http://localhost:8080` 에서 대기 상태가 됩니다.

## 4. 첫 요청 보내기 (Making Request)

이 서버는 **SSE (Server-Sent Events)** 프로토콜을 사용하므로, 일반적인 `curl` 요청 시 스트리밍 응답을 확인할 수 있습니다.

### 채팅 요청 예시 (Chat Stream)
```bash
curl -N -X POST http://localhost:8080/api/chat/stream \
     -H "Content-Type: application/json" \
     -d '{
           "userId": "user123",
           "message": "지금 내 패가 AA인데, 상대가 올인을 했어. 어떻게 할까?"
         }'
```
*   **`-N` 옵션:** 버퍼링 없이 즉시 응답을 출력합니다 (SSE 필수).
*   실행 결과: AI의 답변이 `data: ...` 형식으로 한 글자씩 톡톡 튀어나오는 것을 볼 수 있습니다.

## 5. 페르소나 변경하기 (Changing Persona)

상황에 따라 AI의 성격을 바꿔서 요청해볼 수 있습니다.

### 전략가 모드 (Strategist)
```bash
curl -N -X POST http://localhost:8080/api/chat/stream \
     -H "Content-Type: application/json" \
     -d '{
           "userId": "user123",
           "persona": "strategist",
           "message": "확률적으로 이길 가능성이 얼마나 돼?"
         }'
```

### 위로 모드 (Companion)
```bash
curl -N -X POST http://localhost:8080/api/chat/stream \
     -H "Content-Type: application/json" \
     -d '{
           "userId": "user123",
           "persona": "companion",
           "message": "아... 풀하우스한테 졌어. 너무 화가 나."
         }'
```

이제 여러분만의 포커 파트너와 대화를 시작해보세요!
