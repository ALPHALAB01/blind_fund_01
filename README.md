# 투자수익 시뮬레이터 배포 가이드

## 저장소 구조 (Cloudflare Pages 루트)

```
/
├── index.html      투자자 열람용 (확정 조건은 고정 표시)
├── admin.html      관리자 전용 시뮬레이터 + 배포값 생성
├── config.json     ★ 확정 조건 원본 — 이 파일만 고치면 됨
├── _headers        config.json 캐시 해제 / admin 색인 차단
└── robots.txt      admin 크롤링 차단
```

## 값을 바꾸는 절차

1. `admin.html`을 열고 조합 설정을 조정한다.
2. 5번 카드 **투자자 페이지 배포값**에서 값을 확인한다.
   - `자동 반영` 체크 시 위 시뮬레이션 값이 그대로 들어온다.
   - 체크를 풀면 세 값을 직접 입력할 수 있다.
3. **config.json 내려받기**를 눌러 파일을 받는다.
4. GitHub 저장소 루트의 `config.json`을 덮어쓰고 커밋한다.
5. Cloudflare Pages가 1~2분 내 자동 재배포한다.
6. 투자자 페이지를 새로고침해 값이 반영됐는지 확인한다.

`index.html`은 수정할 필요가 없다.

## config.json 항목

| 키 | 의미 | 기본값 |
|---|---|---|
| `cbRate` | CB 연 이자율 (%) | 10 |
| `cbYears` | 이자 지급 기간 (년) | 2 |
| `divRate` | 연 배당률 (%) | 30 |
| `fundYears` | 조합 존속기간 (년) | 5 |
| `mgmtRate` | 고정보수 (연, 소수) | 0.02 |
| `hurdleRate` | 허들레이트 (연, 소수) | 0.1 |
| `carryRate` | 허들 성과보수율 (소수) | 0.3 |
| `updated` | 투자자 화면 기준일 | "2026-08-13" |

`config.json`을 읽지 못하면 `index.html` 안의 기본값(위 표와 동일)으로 표시된다.

## admin 페이지 접근 제한

`robots.txt`와 `noindex`는 검색 노출만 막을 뿐, 주소를 아는 사람은 접속할 수 있다.
접근 자체를 막으려면 둘 중 하나를 쓴다.

- **Cloudflare Access (권장)**: Zero Trust → Access → Applications에서 `도메인/admin.html`을
  Self-hosted 앱으로 등록하고, 정책을 허용 이메일 목록 + One-time PIN으로 설정한다. 무료 플랜 50명까지.
- **아예 배포하지 않기**: `admin.html`을 저장소에서 빼고 로컬에서만 연다. config.json 생성에는 지장이 없다.
