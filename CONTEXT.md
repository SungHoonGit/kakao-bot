# Project Context (AI-readable)

## Overview

KakaoTalk 메시지 전송 API 래퍼 + Flask 웹훅 챗봇 서버.

## Repo Structure

```
C:\Users\KIM\git\kakao-bot\               ← git root
├── CONTEXT.md                             ← 이 파일
├── README.md
├── .gitignore
├── requirements.txt
├── run.sh                                 ← 실행 스크립트
│
├── kakao_api.py                           ← KakaoAPI 클래스 (핵심 모듈)
│   ├── get_auth_url()                     ← OAuth 인증 URL 생성
│   ├── exchange_code()                    ← code → access_token 교환
│   ├── get_friends()                      ← 친구 목록 조회
│   ├── send_to_me()                       ← 나에게 메시지 전송
│   ├── send_to_friend()                   ← 친구에게 메시지 전송
│   └── send_job_alert()                   ← job-scraper 연동용 (feed 템플릿)
│
├── bot_server.py                          ← Flask 웹훅 서버 (챗봇)
│
└── scripts/
    ├── get_token.py                       ← OAuth 토큰 발급 도우미
    └── send_test.py                       ← 메시지 전송 테스트
```

## Related Repos

| Repo | Path | Purpose |
|------|------|---------|
| job-scraper | `C:\Users\KIM\git\job-scraper` | 채용공고 수집 + 알림 |
| kakao-bot | `C:\Users\KIM\git\kakao-bot` | KakaoTalk 챗봇 서버 |

## kakao-bot vs job-scraper/kakao_notifier.py

| Feature | kakao-bot | job-scraper notifier |
|---------|-----------|---------------------|
| 나에게 보내기 | ✅ `send_to_me()` | ✅ `KakaoNotifier.send()` |
| 친구에게 보내기 | ✅ `send_to_friend()` | ❌ |
| 친구 목록 조회 | ✅ `get_friends()` | ❌ |
| Flask 웹훅 서버 | ✅ `bot_server.py` | ❌ |
| Token 관리 | ✅ `get_token.py` | ✅ `scripts/get_kakao_token.py` |
| job-scraper 연동 | ✅ `send_job_alert()` | ✅ 직접 내장됨 |

## 실행 흐름

```
사용자 메시지 → KakaoTalk → cloudflared → bot_server.py (Flask)
                                               │
                                               ├─ kakao_api.py → 검색/알림
                                               └─ job-scraper 결과 조회 (history.md)
```

## Config

```json
{
  "kakao": {
    "rest_api_key": "...",
    "access_token": "...",
    "refresh_token": "..."
  }
}
```
