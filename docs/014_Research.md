# Research — 기업 분석

## 목적

Research는 기업별 체크리스트를 통해 사업의 경쟁력과 실행을 기록하는 공간이다. 관심 기업 발굴은 Discovery에서 별도로 관리한다.

## 화면 기능

- 기업을 등록하고 기업명, 티커, 국가, 산업을 확인한다.
- 기업 목록에서 검색, 분석 점수, 체크리스트 평가 현황, 최근 업데이트를 확인한다.
- 기업을 선택하면 상세 화면에서 10개 체크리스트 항목을 각각 0~5점으로 평가한다.
- 분석 점수와 진행률은 체크리스트 점수에서 계산한다.
- 기업 목록의 휴지통 버튼으로 기업 및 해당 분석 기록을 삭제한다. 확인 대화상자에서 취소할 수 있다.
- 삭제는 Supabase에 반영되고 화면 목록에서도 제거된다. 오류가 나면 삭제하지 않고 알림을 표시한다.

## 삭제 동작

삭제 대상은 Research의 `companies` 행이다. 사용자 확인 후 현재 로그인 사용자의 행만 삭제 요청하며, Supabase RLS와 `user_id` 조건으로 다른 사용자의 데이터에 접근하지 못하게 한다. 체크리스트 점수와 기업 정보는 한 행에 보관되므로 함께 삭제된다. 이 동작은 Discovery의 `discovery_companies`에는 영향을 주지 않는다.

## 저장 데이터

기업 정보와 체크리스트 점수는 Supabase `public.companies` 테이블에 저장한다. 로그인 사용자별 데이터 분리와 기기 간 Realtime 동기화를 적용한다. 테이블 구성과 초기 설정은 [Supabase 설정](06_Supabase_설정.md)을 따른다.

## 구현 파일

- 화면: `src/features/research/ResearchView.tsx`
- 삭제 요청과 DB 저장: `src/app/App.tsx`
- 공통 테이블 스타일: `src/styles/global.css`
- 체크리스트 항목: `src/features/dashboard/types/company.ts`
