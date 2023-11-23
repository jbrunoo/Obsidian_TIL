
**데이터 저장** :
- F : Cloud Firestore(NoSQL 문서 데이터베이스)와 이전의 Realtime Database를 제공
- S : PostgreSQL을 데이터베이스로 사용합니다. SQL 기반의 인터페이스를 제공하며 Firebase의 Realtime Database와 유사한 실시간 데이터 업데이트를 지원

**인증:**
- F : 이메일/비밀번호, Google, Facebook 등 다양한 인증 제공 업체를 지원. 익명 로그인 및 사용자 정의 인증과 같은 기능도 제공.
- S : Google, GitHub, GitLab과 같은 인기 있는 제공 업체를 지원하는 인증을 제공. 사용자 정의 JWT 인증도 지원 + Kakao 있는 것 봤음.

**실시간 기능:**
- Firebase: Firestore 및 Realtime Database에는 내장된 실시간 기능이 있습니다. 이를 통해 데이터가 변경될 때 실시간으로 업데이트를 수신할 수 있습니다.
- Supabase: Firebase와 유사하게 Supabase는 PostgreSQL 기반의 시스템을 통해 실시간 업데이트를 지원합니다.

**함수/서버리스:**
- F : Firebase Cloud Functions를 사용하여 Firebase 기능 및 HTTPS 요청에서 이벤트에 응답하도록 백엔드 코드를 실행할 수 있음.
- S : Supabase는 JavaScript로 작성된 서버리스 함수를 지원하며 이를 사용하여 백엔드 로직을 처리할 수 있음.

**파일 저장:**
- F : 이미지 및 비디오와 같은 사용자 생성 콘텐츠를 저장하고 제공하기 위한 Cloud Storage를 제공.
- S : Supabase Storage API를 통해 파일 저장을 지원.

**UI 구성 요소:**
- F : Firebase에는 인증 플로우 및 기타 일반적인 기능을 구축하는 데 사용할 수 있는 UI 구성 요소 및 라이브러리가 포함되어 있음.
- Supabase: Supabase는 주로 백엔드에 중점을 둔다는 점에서 UI 구성 요소를 직접 만들 수 있도록 개발자에게 API를 제공.

**가격:**
   - F : Firebase는 사용에 따라 지불하는 요금 모델을 가지고 있으며 무료 티어가 제공. 비용은 사용량에 따라 다르며 다른 서비스에 대한 가격이 다를 수 있음.
   - S : Supabase는 오픈 소스 코어를 제공하며 원하는 경우 자체 호스팅할 수 있음. 또한 사용에 기반한 요금 체계를 가진 클라우드 호스팅 서비스를 제공.

**오픈 소스:**   
 - F : Firebase는 Google이 소유한 프로프리어터리 플랫폼.
 - S : Supabase는 오픈 소스이며 원하는 경우 자체 호스팅할 수 있음.

