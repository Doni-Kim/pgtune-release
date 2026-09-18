# pgtune — PostgreSQL 실시간 모니터 (무료)

![.NET 11](https://img.shields.io/badge/.NET-11.0-512BD4)
![C# 15](https://img.shields.io/badge/C%23-15-239120)
![Blazor Hybrid](https://img.shields.io/badge/Blazor-Hybrid-5C2D91)
![PostgreSQL 14+](https://img.shields.io/badge/PostgreSQL-14%2B-336791)
![Windows x64](https://img.shields.io/badge/Windows-x64-0078D6)

**[최신 배포본 내려받기](https://github.com/Doni-Kim/pgtune-release/releases/latest)** ·
[English](README.md) · [매뉴얼 (HTML)](pgtune.html)

PostgreSQL 상태를 실시간으로 보는 모니터 툴 pgtune 을 전면 개편했습니다. 부담 없이 쓰시라고 공유드립니다.

## 화면

| 실시간 대시보드 | Top SQL |
|---|---|
| ![대시보드](screenshots/dashboard.png) | ![Top SQL](screenshots/top-sql.png) |

| 락 체인 | History |
|---|---|
| ![락 체인](screenshots/locks.png) | ![History](screenshots/history.png) |

![알림](screenshots/alerts.png)

## 설치·설정

- 압축을 풀고 폴더째 둔 뒤 `pgtune.exe` 를 실행하면 됩니다 (Single File Publishing).
- .NET 설치 불필요 — 런타임이 실행 파일에 포함되어 있습니다.
- 설정은 같은 폴더의 `pgtune.json` 하나만 채우면 됩니다.

## 주요 기능

- 실시간 대시보드: 세션·성능·대기 이벤트·추이 그래프
- Top SQL, 락 체인, 인덱스 진단, VACUUM/XID, 복제 상태
- 임계값 알림(접속 포화·데드락·복제 지연 등)
- **History**: `L` 로 로컬 SQLite 에 모니터링 데이터를 쌓고, `H` 로 지난 흐름을 되짚습니다.
  - 구간은 1시간 / 6시간 / 24시간 / 1주 / 1개월 / 전체 중에서 고릅니다.
  - 지표는 TPS · 캐시 적중률 · Active · Waiting · 접속 수 · Idle in tx · 롤백 · 데드락 · 임시파일 · BufBackend 열 가지입니다.
  - 차트에 마우스를 올리면 그 시각의 값이 나오고, 옅은 띠로 그 구간의 최소~최대를 같이 보여 줍니다.
  - 오래된 기록은 자동으로 지웁니다(기본 지표 30일·세션 7일, `pgtune.json` 에서 조정).
- Excel 저장: ClosedXML 기반이라 Excel 이 없어도 xlsx 파일이 저장되고, 설치돼 있으면 저장 후 자동으로 열립니다.
- `F1` 을 누르면 단축키 도움말이 나옵니다.

자세한 사용법은 첨부한 `pgtune.html` 문서를 참고해 주세요. 사용상 제한 없습니다.

## 제약사항

- UI 가 Web 기반(Blazor Hybrid)이라 Windows 전용입니다. Linux·macOS 에서는 단독 실행되지 않습니다.
- 코드는 ConfuserEx 로 난독화(무료 툴이라 강력한 수준은 아닙니다).

## 화면이 안 뜬다면 (WebView2)

창은 뜨는데 내용이 백지라면 WebView2 런타임이 없는 경우입니다.

- **Windows 11**: OS 에 기본 내장이라 항상 있습니다.
- **Windows 10**: 2021년 이후 Windows Update 로 대부분 자동 배포됐지만, 업데이트를 오래 안 한 PC 나 LTSC 같은 특수 에디션에는 없을 수 있습니다.
- **Windows Server (2016/2019/2022)**: 기본 미포함인 경우가 많아 별도 설치가 필요할 수 있습니다.

없으면 Microsoft 의 "에버그린 독립형 설치 프로그램"(`MicrosoftEdgeWebView2RuntimeInstallerX64.exe`)을 설치하시면 됩니다.
→ https://developer.microsoft.com/microsoft-edge/webview2/

## 문제가 생기면

오류가 나면 실행 파일 옆에 `pgtune.log` 가 생깁니다.

- **버그 · 질문**: [Issues](https://github.com/Doni-Kim/pgtune-release/issues) 에 남겨 주세요.
  로그 파일은 거기에 올리지 마세요 — 비밀번호는 없지만 서버 주소와 SQL 문장이 들어 있을 수 있습니다.
- **로그 파일**이나 공개하기 어려운 내용은 메일로 보내 주세요: **doniikim@gmail.com**

## 기술 스택

- .NET 11.0 (x64), C# 14, Blazor Hybrid
- 패키지: Npgsql · Microsoft.Data.Sqlite · ClosedXML · Microsoft.Web.WebView2 · Microsoft.AspNetCore.Components.WebView.WindowsForms
- 실행 파일에 묶인 위 오픈소스들의 저작권 고지 · 라이선스 전문: [THIRD-PARTY-NOTICES.txt](THIRD-PARTY-NOTICES.txt) (zip 안에도 들어 있습니다)

## pgtune.json 예시

```json
{
  "databases": [
    {
      "userId": "postgres",
      "password": "postgres",
      "server": "127.0.0.1",
      "port": "5432",
      "database": ""
    }
  ],
  "interval": 4
}
```

- `password` 는 평문으로 적으면 첫 실행 때 자동 암호화됩니다.
- `database` 를 비워 두면 접속한 뒤 DB 를 고르는 화면이 뜹니다.
- `alerts`, `topSql`, `logRetention` 같은 절은 적지 않아도 됩니다. 프로그램을 닫을 때 기본값으로 채워 넣어 주니, 그 뒤에 보고 고치시면 됩니다.

## 모니터링 전용 계정

superuser 가 아니어도 됩니다. 아래 권한이면 모든 화면이 superuser 와 같게 보입니다.

```sql
CREATE ROLE pgtune_monitor LOGIN PASSWORD '...';
GRANT pg_monitor TO pgtune_monitor;          -- 다른 사용자 세션 · 쿼리 · 크기 · 설정
GRANT pg_signal_backend TO pgtune_monitor;   -- Ctrl+K(세션 취소)가 필요할 때만
```

`pg_monitor` 가 없으면 PostgreSQL 이 다른 사용자 세션을 가립니다. pgtune 은 빈 화면처럼 보이지 않게
세션 목록 · Connections · Locks 위에 "N sessions of other users are hidden — grant pg_monitor …" 로 알려 줍니다.

## 사용 조건

업무든 개인이든 자유롭게 쓰셔도 됩니다. 실행 파일 재배포와 리버스 엔지니어링은 삼가 주세요.
소스는 공개하지 않습니다.

## 연락처

DBMS Works — **doniikim@gmail.com**

Oracle → PostgreSQL / MySQL 마이그레이션, DB 성능 튜닝 문의도 받습니다.
