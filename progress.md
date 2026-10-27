# 진행 상황 (progress)

> 새 대화는 이 파일부터 읽습니다. 세션을 마무리할 때마다 갱신합니다. 작업 지침은 [CLAUDE.md](CLAUDE.md)에 있습니다.

## 현재 상태 (2026-10-27 기준)

- **현재 단계**: 기획 완료, 구현 전. 다음은 **S0 준비** ([WORKPLAN.md](WORKPLAN.md))
- **기준 문서**: [PLAN.md](PLAN.md) v3, [WORKPLAN.md](WORKPLAN.md)
- **다음 할 일 (S0)**
  1. `victoria-tech.com`을 브라우저로 열어 색상·글꼴·헤더·로고 확인 → 디자인 기준표 작성
  2. 임시 파일 삭제: `package.json`(`cool-tesla-speedtest`, express/cors), `package-lock.json`, `node_modules` (git에는 올리지 않았음)
  3. S1 프로젝트 골격 착수
- **사용자에게 받을 것**
  - 홈페이지 `package.json` (Next.js 버전, 스타일 방식)
  - 로고 파일
  - 기존 홈페이지가 `/speed` 경로를 쓰는지 여부
  - EC2 웹 서버 종류 (Nginx 등)
  - 없으면 기본안(최신 안정 Next.js, CSS Modules, 임시 색상)으로 진행

## 확정된 결정

| 항목 | 결정 |
| :--- | :--- |
| 측정 | `@cloudflare/speedtest` 1.15.x + `speed.cloudflare.com`. 자체 측정 엔진·서버 없음 |
| 비용 | 추가 비용 0원. 유료 API·TURN·지도 타일 사용 안 함 |
| 패킷 손실 | 측정 안 함 (TURN 유료). AIM 점수 대신 자체 품질 평가 규칙, "참고용" 표기 |
| 연결 정보 | `speed.cloudflare.com/meta` (비공식, Referer 필요) + `/cdn-cgi/trace` 폴백 |
| 프레임워크 | Next.js App Router + TypeScript, `output: 'export'` 정적 빌드 |
| 차트·지도 | 직접 만든 SVG 컴포넌트, 한국 SVG 지도 |
| 배포 | 기존 EC2 웹 서버가 정적 파일 제공, 주소 `victoria-tech.com/speed` (권장안) |
| 측정 순서 | 다운로드 100kB/1MB/10MB/25MB, 업로드 100kB/1MB/10MB, 최대 약 220MB |

## 확인된 사실 (2026-10-27)
- `speed.cloudflare.com/__down`, `/__up`: CORS `*` 허용, 업로드 사전 요청(OPTIONS) 허용
- `/meta`: Referer 있으면 200 + JSON(IP, ASN, 통신사명, 좌표, colo), 없으면 403
- Cloudflare TURN: 단독 사용 시 GB당 $0.05 → 비용 0원 원칙으로 제외
- 대안 측정 서비스: M-Lab NDT7(무료, 측정 데이터 공개 동의 필요) — 장기 대안으로만 기록

## 작업 일지

### 2026-10-27
- **한 일**
  - 초기 계획(PLAN.md v1: 자체 Node 서버 + 자체 엔진) 검토
  - v2: 기존 홈페이지가 Next.js on EC2임을 확인, `@cloudflare/speedtest` 사용으로 전환
  - v3: 비용 0원 원칙 적용 (패킷 손실·TURN·서버 API 제외, 정적 사이트)
  - WORKPLAN.md 작성: S0~S9 단계, 단계별 목표·기대 결과·화면 검증 체크리스트
  - CLAUDE.md(자동 지침), progress.md 작성, GitHub 저장소 연결
- **결정**: 위 "확정된 결정" 표 참고
- **남은 일**: S0부터 구현 시작
