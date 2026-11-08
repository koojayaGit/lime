# Dashboard — 연구 현황

## 목적

등록한 Research 기업을 한눈에 확인하고 최근 분석으로 돌아가는 시작 화면이다. 기업 분석과 체크리스트 평가는 Research에서 이어간다.

## 화면 구성과 동작

- 환영 영역에서 장기 투자 연구소의 방향을 보여준다.
- 등록된 Research 기업 수를 요약한다.
- 기업명과 티커로 등록 기업을 검색한다.
- 기업 목록에서 산업, 체크리스트 점수와 진행 현황, 최근 업데이트 날짜를 확인한다.
- 기업 행을 선택하면 Research 상세 화면으로 이동한다.
- 기업 추가 버튼은 Research 기업을 등록한다.
- 최근 업데이트 영역에서 수정한 기업을 다시 열 수 있다.
- LIME 원칙 카드로 장기 기업 분석 관점을 상기시킨다.

평균 분석 점수와 최근 연구 활동 요약 카드는 현재 Dashboard에서 표시하지 않는다. 최근 업데이트 목록은 유지한다.

## 데이터 범위

Dashboard는 Supabase의 Research `companies` 데이터를 읽어 표시한다. Discovery 관심 기업은 이 목록에 포함하지 않는다. Dashboard 검색어 상태는 Research 검색 상태와 분리한다.

## 구현 파일

- 화면: `src/features/dashboard/components/DashboardView.tsx`
- 화면 연결 및 Research 이동: `src/app/App.tsx`
- 화면 전용 스타일: `src/styles/dashboard.css`
