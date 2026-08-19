# Higher Ground NYC — 관리자 페이지 설정

한 번만 해두면 그 뒤로는 `사이트주소/admin` 에 로그인해서 내용을 바꿀 수 있습니다.
전부 무료 범위 안에서 됩니다. 처음부터 끝까지 30~40분 정도 걸립니다.

순서대로 따라가면 되고, 막히는 단계가 있으면 그 번호를 알려주세요.

---

## 준비물

- 이메일 주소 하나
- 이 폴더 (`index.html`, `content`, `images`, `admin`)

---

## 1단계 — GitHub 계정 만들기

GitHub는 파일이 저장되는 곳입니다. 관리자 화면에서 수정한 내용이 여기에 기록됩니다.

1. https://github.com/signup 접속
2. 이메일, 비밀번호, 사용자 이름 입력
   - 사용자 이름은 나중에 주소에 쓰이니 `highergroundnyc` 처럼 알아보기 쉬운 걸로
3. 이메일 인증 완료
4. 요금제는 **Free** 선택

## 2단계 — 저장소(repository) 만들고 파일 올리기

1. 로그인 후 오른쪽 위 **+** → **New repository**
2. Repository name 칸에 `highergroundnyc` 입력
3. **Public** 선택 (무료 호스팅을 쓰려면 공개여야 합니다)
4. **Create repository** 클릭
5. 다음 화면에서 **uploading an existing file** 링크 클릭
6. 이 폴더 안의 **내용물 전체**를 끌어다 놓기
   - 폴더 자체가 아니라, 안에 있는 `index.html` · `content` · `images` · `admin` 을 함께
7. 아래 **Commit changes** 클릭

## 3단계 — 사이트 올리기 (Netlify)

1. https://app.netlify.com/signup 접속 → **GitHub으로 가입** 선택
2. **Add new site** → **Import an existing project** → **GitHub**
3. 방금 만든 `highergroundnyc` 저장소 선택
4. 빌드 설정은 비워둔 채 **Deploy** 클릭
5. 1분쯤 뒤 `무작위이름.netlify.app` 주소가 생깁니다 — 열어서 사이트가 뜨는지 확인

> 이미 다른 곳에서 호스팅 중이라면 이 단계는 상황에 맞게 바꿔야 합니다.
> 지금 쓰는 호스팅이 어디인지 알려주면 그에 맞게 안내하겠습니다.

## 4단계 — 로그인 장치 만들기 (Cloudflare)

관리자 화면이 GitHub에 접속할 수 있게 해주는 중간 다리입니다.

1. https://dash.cloudflare.com/sign-up 에서 무료 가입
2. 왼쪽 메뉴 **Workers & Pages** → **Create** → **Import a repository**
   - 또는 https://github.com/sveltia/sveltia-cms-auth 저장소의 안내에 있는
     **Deploy to Cloudflare** 버튼을 눌러도 됩니다
3. 배포가 끝나면 `무언가.workers.dev` 주소가 생깁니다 — **복사해두세요**

## 5단계 — GitHub에 로그인 앱 등록

1. GitHub → 오른쪽 위 프로필 → **Settings**
2. 맨 아래 **Developer settings** → **OAuth Apps** → **New OAuth App**
3. 이렇게 입력합니다
   - Application name: `Higher Ground NYC Admin`
   - Homepage URL: 3단계에서 받은 사이트 주소
   - Authorization callback URL: 4단계 주소 뒤에 `/callback` 을 붙인 것
     예) `https://무언가.workers.dev/callback`
4. **Register application**
5. **Client ID** 를 복사하고, **Generate a new client secret** 을 눌러 **Client Secret** 도 복사
   - 시크릿은 이 화면을 벗어나면 다시 볼 수 없으니 바로 메모해두세요

## 6단계 — 열쇠 넣어주기

1. Cloudflare → Workers & Pages → 4단계에서 만든 Worker 클릭
2. **Settings** → **Variables and Secrets**
3. 아래 세 개를 추가합니다
   - `GITHUB_CLIENT_ID` → 5단계의 Client ID
   - `GITHUB_CLIENT_SECRET` → 5단계의 Client Secret
   - `ALLOWED_DOMAINS` → 사이트 주소 (예: `highergroundnyc.com`)
4. 저장 후 **Deploy** 다시 실행

## 7단계 — 설정 파일 두 줄 고치기

1. GitHub 저장소에서 `admin` → `config.yml` 클릭
2. 연필 아이콘(Edit) 클릭
3. 위쪽 두 줄을 실제 값으로 바꿉니다

   ```yaml
   repo: 내GitHub사용자이름/highergroundnyc
   base_url: https://무언가.workers.dev
   ```

4. **Commit changes**

## 8단계 — 들어가보기

`사이트주소/admin` 으로 접속 → **Sign in with GitHub** → 승인.

Artists · Studio Info · News & Events 세 메뉴가 보이면 완료입니다.

---

## 쓰는 법

**아티스트 추가** — Artists → Artist list → 목록 맨 아래 **Add Artist** →
이름, 사진, 소개, 스타일 입력 → 오른쪽 위 **Save**

**순서 바꾸기** — 목록에서 항목 왼쪽의 손잡이를 끌어서 이동

**삭제** — 항목 오른쪽의 메뉴에서 삭제

**타투 작업 사진** — 각 아티스트 안의 **Tattoo photos** 에서 추가.
Style label 은 그 아티스트의 Specialty 이름과 **똑같이** 적어야 갤러리 필터가 맞습니다.

**공지 올리기** — News & Events 에서 추가. 목록이 비어 있으면 사이트에 News 섹션이 아예 안 나옵니다.
지우지 않고 잠시 감추려면 **Show on site** 를 끄면 됩니다.

저장하면 1분 안에 사이트에 반영됩니다.

---

## 주의할 점

- 사진은 올리기 전에 흑백으로 바꾸고 세로 비율(3:4)로 잘라두는 게 좋습니다.
  사이트가 자동으로 흑백 처리하지 않습니다.
- 사진 파일은 한 장에 500KB 아래로 유지하세요. 크면 사이트가 느려집니다.
- `admin` 주소는 검색에 노출되지 않게 막아뒀지만, 주소를 아는 사람은 접근할 수 있습니다.
  다만 GitHub 로그인을 통과해야 실제 수정이 가능합니다.
