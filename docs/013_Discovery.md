# Discovery — 관심 기업 발굴

## 목적

발견한 회사를 Research에 바로 합치지 않고 별도의 관심목록에서 관리한다. Discovery에서 기업의 기본 정보와 발굴 이유, 검토 상태, 성장 단계, 재무 지표를 정리한 뒤 필요할 때 Research에 별도로 등록한다.

## 화면 기능

- 기업명, 티커, 국가, 시장, 산업, 발굴 이유를 등록하고 수정한다.
- 국내 상장 종목은 티커 입력란에 6자리 종목코드를 입력한다(예: 삼성전자 `005930`). 앞자리 0을 보존하기 위해 텍스트로 입력한다. 종목코드가 없는 비상장 기업은 비워둘 수 있다.
- 관심 기업을 삭제할 수 있다. 삭제 전 브라우저 확인을 거친다.
- 검토 상태를 `관심`, `검토 중`, `Research 이동`, `보류`로 관리한다. `Research 이동`은 상태 표시이며 Research에 자동 복사하지 않는다.
- 관심 기업을 이름·티커·시장·산업으로 검색하고 성장 단계로 필터링한다. 목록은 최근 수정 순으로 표시한다.
- 성장 단계는 현재 판단을 사용자가 선택한다. 주가 등락으로 자동 변경하지 않는다.
- 등록일, 관심을 가진 이유, 기본 지표를 기업 카드에서 확인한다.

## 성장 단계

| 값 | 화면 표시 | 판단 기준 |
|---|---|---|
| `seed` | 씨앗주 | 혁신 기술이나 아이디어가 있고 사업화 또는 시장 검증이 초기 단계 |
| `sprout` | 새싹주 | 시장 검증이 시작되고 고객 또는 매출이 발생·증가 |
| `growth` | 성장주 | 매출과 시장 점유율이 본격적으로 확대 |
| `pillar` | 기둥주 | 산업 내 경쟁우위와 안정적인 사업 기반·현금흐름을 보유 |
| `fruit` | 열매주 | 성숙한 사업을 바탕으로 안정적인 수익과 배당·주주환원을 기대 |

성장 분류는 [앱의 정의](01_앱의_정의.md)의 장기 성장투자 기준을 따른다. 현재는 단계 변경 사유와 과거 이력을 별도 저장하지 않고 가장 최근 판단만 보관한다.

## 기본 지표

| 지표 | 동작 | 저장 방식 |
|---|---|---|
| 현재가 | 미국은 Finnhub, 국내는 네이버 금융 종목 기본정보에서 조회. 카드가 열린 동안 10분 간격으로 갱신 | 표시 통화와 제공자 기준 시각을 함께 표시. DB에는 저장하지 않음 |
| 매수 목표가 | 사용자 입력 | `target_price` |
| 시가총액 | 관심 기업 등록 때 자동 조회하고 카드에서 다시 갱신 가능 | `market_cap` |
| PER | 관심 기업 등록 때 자동 조회하고 카드에서 다시 갱신 가능 | `pe_ratio` |
| 배당률 | 자동 조회, 백분율로 표시 | `dividend_yield` |
| 연간 주당 배당금 | 자동 조회 | `annual_dividend` |
| EPS | 자동 조회 | `eps` |

기본 지표는 관심 기업 카드에 표시한다. 미국 종목의 현재가와 펀더멘털 지표는 Finnhub, 국내 종목의 펀더멘털 지표는 네이버 금융에서 조회하고, 매수 목표가만 사용자가 직접 입력한다. 신규 미국·국내 관심 기업 등록 후 지표를 조회한다. 기존 목록도 다섯 개 펀더멘털 지표가 모두 비어 있는 미국·국내 종목이면 Discovery 화면을 열 때 한 번 자동 조회한다. 기존 값 일부가 있는 종목은 자동 덮어쓰지 않으며, 각 카드의 **기본 지표 갱신** 버튼으로 다시 요청할 수 있다. 여러 종목의 자동 조회는 호출량을 고려해 순차 처리한다. 시가총액·PER·배당률·연간 주당 배당금·EPS는 Supabase에도 저장한다. 지표가 없거나 제공자에 연결할 수 없으면 대시(-)를 유지한다.

### 무료 지표 조회

미국 펀더멘털과 현재가에는 Finnhub API 키가 필요하다. 프로젝트 루트의 `.env.local`에 `FINNHUB_API_KEY=발급받은_키`를 추가하고 Vite 개발 서버를 다시 시작한다. Vercel에서는 Project Settings → Environment Variables에 같은 이름으로 저장하고 재배포한다. API 키는 `VITE_` 접두어를 사용하지 않으며 서버에서만 읽는다. 국내 펀더멘털은 종목코드 6자리로 네이버 금융 모바일 종목 정보에서 시가총액·PER·배당수익률·주당배당금·EPS를 읽고, 현재가는 네이버 금융 종목 기본정보의 가격과 거래 시각을 사용한다. 국내 조회에는 별도 API 키가 필요하지 않으며, 서버 경로는 Supabase 로그인 세션을 확인한다. 네이버 금융의 해당 경로는 공개 웹 앱이 사용하는 비공식 인터페이스라 변경될 수 있으며, 장중 가격과 장 마감 후 마지막 가격이 반환될 수 있다. 조회에 실패하면 카드에서 다시 시도한다. 참고: [Finnhub API 문서](https://finnhub.io/docs/api/company-basic-financials), [Finnhub 요금제](https://finnhub.io/pricing).

티커가 없는 기업도 등록할 수 있으며, 현재가는 `조회 불가`로 표시된다. 티커가 있지만 시세 요청이 실패한 경우에도 관심 기업 정보와 매수 목표가는 계속 이용할 수 있다.

## 저장 및 동기화

- Discovery 데이터는 Research의 `companies`와 분리된 `public.discovery_companies` 테이블에 저장한다.
- `user_id`와 RLS 정책으로 사용자별 데이터를 분리한다.
- 사용자 본인의 행만 조회·추가·수정·삭제할 수 있다.
- 변경 내용은 Supabase Realtime으로 다른 로그인 기기에 반영한다.
- Discovery 데이터 구조는 `src/features/discovery/types.ts`, 화면은 `src/features/discovery/DiscoveryView.tsx`, DB 변환은 `src/lib/supabase.ts`에서 관리한다.

### Supabase 준비

Research 기업 목록이 이미 열리는 환경에서는 `supabase/setup_discovery.sql` 한 파일을 실행해 Discovery 테이블을 추가한다. Supabase 대시보드의 **SQL Editor → New query**에 파일 전체를 붙여넣고 **Run**을 누른 다음 앱을 새로고침하면 된다. 이 스크립트가 테이블, 사용자별 접근 정책, Realtime을 설정하며 기존 Discovery 테이블이 있으면 데이터를 보존하고 재실행할 수 있다.

환경 구성과 적용 순서는 [Supabase 설정](06_Supabase_설정.md)을 따른다. Supabase CLI 환경은 `supabase/migrations/`의 순차 마이그레이션을 사용한다.

Discovery 테이블이 아직 없는 환경에서는 SQL 오류를 사용자에게 노출하지 않는다. 빈 목록 대신 저장 공간 준비 안내와 비활성화된 추가 버튼을 표시한다. 마이그레이션 적용 후 새로고침하면 등록을 사용할 수 있다. 테이블은 있지만 행이 없는 경우에는 일반 빈 상태와 등록 버튼을 표시한다.

## 구현 파일

- 화면: `src/features/discovery/DiscoveryView.tsx`
- 타입: `src/features/discovery/types.ts`
- DB 행 변환: `src/lib/supabase.ts`
- 화면 연결, CRUD, 실시간 구독: `src/app/App.tsx`
- 스타일: `src/styles/discovery.css`
- Finnhub 지표 조회 프록시: `api/discovery-metrics.ts` (Vercel), `vite.config.ts` (로컬 개발)
- Finnhub 응답 변환: `server/discoveryMetrics.ts`
- SQL Editor 초기 설정: `supabase/setup_discovery.sql`
- DB 마이그레이션: `supabase/migrations/20261109000000_create_discovery_companies.sql`
