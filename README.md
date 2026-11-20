# DK 현장관리 웹앱

관리자 + 기사 역할 기반의 Next.js / Supabase 현장관리 프로젝트입니다. 업로드해 주신 DK 로고를 `public/company-logo.jpg`로 적용했습니다.

## 현재 포함된 기능
- 이메일/비밀번호 로그인 (Supabase Auth)
- 관리자/기사 역할 프로필
- 관리자 현장 등록 및 기사 배정
- 기사는 본인 배정 현장 조회
- 작업 상태 변경
- 작업 보고서 작성
- 비공개 사진 저장소 업로드 (JPG/PNG/WEBP, 최대 10MB)
- PostgreSQL RLS 권한 정책 및 기사 현장 수정 제한 트리거
- 반응형 웹 화면

## 필수 외부 계정
실제 인터넷 서비스로 배포하려면 다음 계정과 설정이 필요합니다.
1. Supabase 프로젝트 1개 (데이터베이스, 인증, 사진 저장)
2. Vercel 계정 1개 (웹앱 호스팅)
3. 선택 사항: 회사 도메인

이 프로젝트에는 사용자의 Supabase/Vercel 계정에 접근할 인증 권한이 없으므로, 계정 생성이나 실제 배포 완료를 대신했다고 표시하지 않습니다.

## 1. Supabase 설정
1. Supabase에서 새 프로젝트를 만듭니다.
2. SQL Editor에서 `supabase/schema.sql` 전체를 실행합니다.
3. Authentication > Users에서 관리자와 기사 계정을 생성합니다.
4. 각 사용자의 UUID를 확인하고 SQL Editor에서 프로필을 등록합니다.

```sql
insert into public.profiles (id, full_name, role, company_name)
values
('관리자_AUTH_USER_UUID', '관리자 이름', 'admin', 'DK'),
('기사_AUTH_USER_UUID', '기사 이름', 'worker', 'DK');
```

계정은 실제 사용할 이메일과 안전한 비밀번호로 생성하세요. 비밀번호를 이 프로젝트 파일이나 채팅에 기록하지 마세요.

## 2. 로컬 환경 변수
`.env.example`을 `.env.local`로 복사하고 Supabase 프로젝트의 Project URL 및 anon/public key를 입력합니다.

```env
NEXT_PUBLIC_SUPABASE_URL=https://YOUR_PROJECT.supabase.co
NEXT_PUBLIC_SUPABASE_ANON_KEY=YOUR_SUPABASE_ANON_KEY
```

`service_role` 키는 브라우저에 노출되는 `NEXT_PUBLIC_` 변수로 절대 등록하지 마세요.

## 3. 로컬 실행
Node.js 20 이상에서 실행합니다.

```bash
npm install
npm run dev
```

브라우저에서 `http://localhost:3000` 접속.

## 4. Vercel 배포
1. 프로젝트 폴더를 GitHub 저장소에 올립니다.
2. Vercel에서 해당 저장소를 Import 합니다.
3. Environment Variables에 `NEXT_PUBLIC_SUPABASE_URL`, `NEXT_PUBLIC_SUPABASE_ANON_KEY`를 등록합니다.
4. Deploy를 실행합니다.
5. 배포된 도메인을 Supabase Authentication > URL Configuration의 Site URL 및 Redirect URLs에 등록합니다.

## 보안 및 운영 전 점검
- 관리자/기사 계정으로 각각 로그인 테스트
- 기사가 다른 기사 현장을 조회/수정할 수 없는지 테스트
- 사진 업로드/다운로드 권한 테스트
- 백업, MFA, 비밀번호 정책, 모니터링 설정
- 현재 보고서/사진의 미리보기, 고객 전자서명, 자재 입력 UI, PDF/엑셀 내보내기, 감사로그, 푸시 알림은 추가 개발 항목입니다.

## 브랜드
- 로고: `public/company-logo.jpg`
- 대표 파랑: `app/globals.css`의 `#0879bd`
- 회사명: 프로필의 `company_name` 값 (`DK` 권장)
