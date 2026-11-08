# Supabase 설정

LIME의 계정, Discovery 관심 기업, Research 기업 기록을 기기 간에 동기화하기 위한 설정입니다.

## 1. Supabase 프로젝트 만들기

Supabase에서 새 프로젝트를 만들고 프로젝트가 준비될 때까지 기다립니다.

## 2. 데이터베이스 테이블과 권한 만들기

### Discovery 테이블 추가

Research 기업 목록이 이미 열리는 기존 프로젝트라면 Discovery용 SQL 파일 하나만 실행하면 됩니다.

1. Supabase 대시보드에서 앱이 연결된 프로젝트를 엽니다.
2. 왼쪽 메뉴에서 **SQL Editor → New query**를 선택합니다.
3. 이 저장소의 `supabase/setup_discovery.sql` 파일을 열어 전체 내용을 복사해 SQL Editor에 붙여넣습니다.
4. **Run**을 누르고 성공 메시지를 확인합니다.
5. `http://localhost:5173` 앱을 새로고침하고 Discovery의 관심 기업 추가 버튼을 확인합니다.

이 스크립트가 Discovery 테이블과 사용자별 접근 권한, Realtime 설정을 한 번에 만듭니다. 이미 테이블이 있다면 데이터를 지우지 않고 다시 실행할 수 있습니다. Research의 `companies` 테이블도 아직 없다면 먼저 `supabase/migrations/20261107000000_create_companies.sql`을 실행합니다.

Supabase CLI를 사용하는 경우에는 `supabase/migrations/`의 파일을 날짜순으로 적용합니다. SQL Editor에서 `setup_discovery.sql`을 실행했다면 같은 테이블을 만드는 마이그레이션을 중복 실행하지 않습니다.

## 3. 앱 연결 키 설정하기

Supabase 프로젝트의 **Project Settings → API Keys** 또는 **Connect** 창에서 Project URL과 publishable key를 확인합니다.

**Authentication → URL Configuration**에서 Site URL을 `https://lime-umber-nine.vercel.app`으로 설정하고, Redirect URLs에 `https://lime-umber-nine.vercel.app/**`과 `http://localhost:5173/**`을 추가합니다. 뒤의 주소는 로컬 가입 확인을 위해 필요합니다.

로컬 개발은 프로젝트 루트의 `.env.local`에 아래처럼 저장합니다. `.env.local`은 Git에 커밋하지 않습니다.

```env
VITE_SUPABASE_URL=https://your-project.supabase.co
VITE_SUPABASE_PUBLISHABLE_KEY=sb_publishable_...
```

Vercel 배포는 **Project Settings → Environment Variables**에 같은 이름으로 두 값을 추가하고 다시 배포합니다. secret/service_role 키는 브라우저 앱에 넣으면 안 됩니다.

## 4. 계정 만들기와 기존 기록 가져오기

앱에서 이메일과 비밀번호로 계정을 만듭니다. 이메일 확인이 켜져 있으면 확인 링크를 누른 뒤 로그인합니다. 로그인 후 DB에 아직 기업 기록이 없으면 해당 브라우저에 있던 기존 `lime.companies.v1` 기록을 한 번 가져옵니다.

두 기기에서 같은 계정으로 로그인하면 기록이 Supabase에 저장되고, 한 기기에서 변경한 내용은 다른 기기에도 실시간으로 반영됩니다.

## 5. Portfolio 저장 테이블 추가

기존 Supabase 프로젝트에서 Portfolio를 사용하려면 SQL Editor에서
`supabase/setup_portfolio.sql` 전체를 실행합니다. 사용자별 보유 종목과
현금 테이블, RLS 접근 정책, Realtime 구독 설정을 추가합니다. Supabase CLI를
사용하는 환경은 `supabase/migrations/20261110000000_create_portfolio.sql`과
추가 마이그레이션을 날짜순으로 적용합니다. 테이블이 이미 있다면 업데이트된
`setup_portfolio.sql`을 다시 실행해도 보유 데이터를 유지하면서 성장 단계
컬럼을 추가합니다. SQL Editor 방식과 CLI 마이그레이션을 중복 적용하지 않습니다.
