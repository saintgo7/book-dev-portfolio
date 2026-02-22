---
title: "부록"
order: 11
tags: [appendix, tech-stack, tools, reference, troubleshooting]
---

# 부록

---

## A. 기술 스택 인덱스

### 언어별 프로젝트 목록

**Go**
- `02_ERP/saas-erpmes` — K-ERP SaaS 플랫폼
- `01_ABADA/saas-iso-cert` — ISO 인증 관리

**Python**
- `03_TRAVEL/app-passport-reserve` — 여권 OCR 시스템
- `01_ABADA/saas-hosting` — AI 호스팅 자동화
- `04_AI_BOT/bot-claude-code` — 텔레그램 봇
- `04_AI_BOT/crewbot-core` — CrewAI 에이전트
- `07_WEB/product-jichim` — 제품 관리

**TypeScript / Node.js**
- `01_ABADA/saas-crew-mgmt` — WKU Software Crew
- `03_TRAVEL/k-guide-platform` — K-Guide 플랫폼
- `08_DEVTOOLS/claude-hook-logger` — 훅 로거
- `08_DEVTOOLS/tui-tool` — CLI 도구

**React Native / Expo**
- `01_ABADA/saas-orak` — ORAK 게임

**Laravel / PHP**
- `02_ERP/web-erpmes-v2` — SmartFactory ErpMes

---

## B. 인프라 & DevOps 패턴

### Docker Compose 표준 구성

```yaml
# docker-compose.yml 기본 패턴
version: '3.8'

services:
  app:
    build: .
    ports:
      - "3000:3000"
    environment:
      - NODE_ENV=production
      - DATABASE_URL=${DATABASE_URL}
    depends_on:
      db:
        condition: service_healthy
    restart: unless-stopped

  db:
    image: postgres:16-alpine
    volumes:
      - postgres_data:/var/lib/postgresql/data
    environment:
      POSTGRES_DB: ${DB_NAME}
      POSTGRES_USER: ${DB_USER}
      POSTGRES_PASSWORD: ${DB_PASSWORD}
    healthcheck:
      test: ["CMD-SHELL", "pg_isready -U ${DB_USER}"]
      interval: 10s
      timeout: 5s
      retries: 5

  redis:
    image: redis:7-alpine
    volumes:
      - redis_data:/data
    restart: unless-stopped

volumes:
  postgres_data:
  redis_data:
```

---

## C. 공통 트러블슈팅

### PostgreSQL 연결 문제

```bash
# 연결 테스트
psql -h localhost -U postgres -d mydb

# pg_isready로 상태 확인
pg_isready -h localhost -p 5432

# Docker 내부 연결 확인
docker exec -it postgres psql -U postgres
```

### Docker 빌드 캐시 초기화

```bash
# 모든 캐시 삭제
docker builder prune -af

# 특정 이미지 재빌드 (캐시 없이)
docker build --no-cache -t myapp .
```

### Node.js 메모리 오류

```bash
# 메모리 증가
NODE_OPTIONS="--max-old-space-size=4096" npm run build

# 메모리 사용량 모니터링
node --inspect app.js
```

---

## D. 개발 환경 설정

### zsh + Oh My Zsh 설정

```bash
# 필수 플러그인
plugins=(git docker kubectl node python golang)

# 유용한 alias
alias gst='git status'
alias gcm='git commit -m'
alias dcu='docker compose up -d'
alias dcd='docker compose down'
alias dlog='docker compose logs -f'
```

### Git 전역 설정

```bash
git config --global user.name "Your Name"
git config --global user.email "your@email.com"
git config --global core.editor "code --wait"
git config --global init.defaultBranch main
git config --global pull.rebase false
```

---

## E. 프로젝트 통계 요약

| 카테고리 | 프로젝트 수 | 주요 기술 |
|----------|------------|----------|
| ABADA SaaS | 15 | TypeScript, Python, Go |
| ERP/MES | 6 | Go, React, Laravel |
| 여행/관광 | 6 | NestJS, Next.js, Python |
| AI/Bot | 5 | Python, CrewAI, Claude |
| 이메일 | 2 | Node.js |
| 헬스케어 | 2 | Node.js, Supabase |
| 웹 | 6 | Node.js, Next.js |
| 개발도구 | 8 | Node.js, Python |
| 학습/실험 | 17 | Go, Rust, Flutter |
| 기타/아카이브 | 9 | 다양 |
| **합계** | **76+** | **다양** |

---

## F. 용어 사전

| 용어 | 설명 |
|------|------|
| SaaS | Software as a Service — 구독 기반 소프트웨어 서비스 |
| ERP | Enterprise Resource Planning — 기업 자원 관리 |
| MES | Manufacturing Execution System — 제조 실행 시스템 |
| BaaS | Backend as a Service — 백엔드 서비스 플랫폼 |
| OCR | Optical Character Recognition — 광학 문자 인식 |
| MRZ | Machine Readable Zone — 여권 기계 판독 영역 |
| TUI | Terminal User Interface — 터미널 사용자 인터페이스 |
| MVP | Minimum Viable Product — 최소 기능 제품 |
| CI/CD | Continuous Integration/Deployment — 지속적 통합/배포 |
| POI | Point of Interest — 관심 지점 (관광지 등) |
| LLM | Large Language Model — 대규모 언어 모델 |

---

*이 책의 모든 프로젝트 소스 코드는 각 GitHub 저장소에서 확인할 수 있습니다.*

**작성 완료**: 2026-02-22
**총 챕터**: 11개 (서문 + 10개 본문 + 부록)
**총 프로젝트**: 76개+
