# 나의 실천 목록 (학생용 To-Do 웹앱, Firebase 버전)

이름과 비밀번호로 들어와 실천할 일을 적고, 실천 날짜와 부모님 확인을 체크하는 웹페이지입니다.
데이터는 Firebase에 저장되어 **어느 기기에서든 같은 이름·비밀번호로 접속하면 같은 목록**이 보입니다.

## 1. Firebase 설정 (기존 word-kingdom 프로젝트 재사용)
1. Firebase 콘솔 → 프로젝트 `word-kingdom` → **빌드 → Authentication → 시작하기**
   → **로그인 방법** 탭에서 **이메일/비밀번호**를 사용 설정합니다. (익명 로그인 등 기존 설정은 그대로 두세요.)
2. **Firestore Database → 규칙** 탭을 엽니다.
   **기존 규칙은 지우지 말고**, `match /databases/{database}/documents { ... }` 안쪽에 아래를 **추가**합니다.
   ```
   match /todoUsers/{uid} {
     allow read, write: if request.auth != null && request.auth.uid == uid;
   }
   ```
   → **게시**를 누릅니다.
3. **프로젝트 설정(톱니바퀴) → 내 앱**에서 웹 앱 설정값(`firebaseConfig`)을 복사해
   `index.html`의 `firebaseConfig` 부분에 붙여넣습니다. (word-kingdom 게임에 쓴 값과 같습니다.)
4. **Authentication → Settings → 승인된 도메인**에 `내아이디.github.io` 가 들어 있는지 확인합니다.
   (이미 word-kingdom을 같은 주소로 배포했다면 추가돼 있습니다.)

## 2. GitHub Pages로 배포
```bash
git init
git add index.html README.md
git commit -m "학생용 실천 목록 웹앱"
git branch -M main
git remote add origin https://github.com/내아이디/student-todo.git
git push -u origin main
```
저장소 **Settings → Pages → Branch: main / (root)** 선택 후 Save →
`https://내아이디.github.io/student-todo/` 주소를 학생들에게 알려 주세요.

## 사용 방법
- 처음: 이름 + 비밀번호(6글자 이상) 입력 후 **처음 시작하기**
- 다음부터: 같은 이름 + 비밀번호로 **들어가기**
- 「실천했어요」 체크 → 날짜 자동 기록 → 「부모님 확인함」 체크 가능
- 모든 변경은 자동 저장됩니다.

## 알아둘 점
- 학생 이름이 Firebase(구글 서버)에 저장되므로 실명 대신 별명·번호 사용을 권장합니다.
- 비밀번호를 잊으면 Firebase 콘솔 Authentication → 사용자에서 해당 계정을 삭제한 뒤 새로 시작하게 해야 합니다.
  (학생 계정은 이름 대신 해시값으로 표시됩니다. 학생이 몇 명 안 되면 삭제 후 재가입이 가장 간단합니다.)
- 같은 이름을 쓰는 학생끼리는 계정이 충돌하니 이름이 겹치지 않게 해 주세요.
