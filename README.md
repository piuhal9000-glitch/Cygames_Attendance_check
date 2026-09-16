# 싸이뮤직 방문 인증 웹앱

인증 장소 **싸이뮤직**(대전광역시 유성구 대학로155번길 29)의 중심 좌표에서 250m 이내이고 GPS 정확도가 100m 이내일 때 Firestore의 `checkins` 컬렉션에 방문 기록을 저장합니다.

저장 항목은 다음과 같습니다.

- `name`: 이름 또는 닉네임
- `place`: 인증 장소 이름 (`싸이뮤직`)
- `checkedAt`: Firestore 서버 인증시간
- `distanceMeters`: 인증 지점까지의 거리
- `accuracyMeters`: GPS 정확도
- `dateKey`: 한국 시간 기준 날짜
- `uid`: Firebase 익명 사용자 식별자

실제 위도·경도는 DB에 저장하지 않습니다.

## Firebase 연결

1. [Firebase 콘솔](https://console.firebase.google.com/)에서 프로젝트를 만들고 웹 앱을 등록합니다.
2. **Authentication → Sign-in method**에서 `Google` 로그인을 사용 설정합니다. 익명 로그인은 더 이상 사용하지 않습니다. 이 앱은 이메일이 확인된 `@korea.ac.kr` Google 계정만 허용합니다.
3. **Firestore Database**를 생성합니다. 리전은 운영 환경에 맞게 선택하세요.
4. `index.html`에는 Firebase 프로젝트 `cygame-a7769`의 `firebaseConfig`가 적용되어 있습니다.
5. Firestore의 **규칙** 탭에서 `firestore.rules` 내용을 붙여 넣고 게시합니다.
6. `index.html`을 GitHub Pages, Firebase Hosting, Netlify, Vercel 등 HTTPS 호스팅에 올립니다. Firebase Authentication의 **승인된 도메인**에도 실제 배포 도메인을 추가합니다.

Firebase 웹 설정의 `apiKey`는 브라우저에서 사용하는 식별 설정값입니다. 보호는 키를 숨기는 방식이 아니라 Authentication, Firestore 보안 규칙, 필요하면 App Check로 구성해야 합니다.

## 운영 확인

- 휴대폰에서 HTTPS 주소를 열고 위치 권한을 허용합니다.
- 인증 장소 반경 안에서 이름을 입력하고 방문 인증을 누릅니다.
- Firebase 콘솔의 **Firestore Database → Data → checkins**에서 공용 기록을 확인합니다.
- 문서 ID가 `Google UID_날짜` 형식이어서 같은 학교 Google 계정은 브라우저를 바꿔도 하루 한 번만 기록됩니다.
- 개인 Gmail 및 다른 도메인의 Google 계정은 화면과 Firestore 보안 규칙 양쪽에서 차단됩니다.

## 꼭 알아둘 점

이 앱은 동아리용 간단 인증에 맞춘 클라이언트 방식입니다. 브라우저를 변경해도 같은 Google 계정의 중복은 차단되지만, 다른 Google 계정을 사용하면 다시 인증할 수 있습니다. 시험·근태처럼 강한 부정 방지가 필요하면 승인 회원 목록, 서버 측 검증, App Check, 현장 QR을 추가하세요.

좌표는 공개 지도 검색 결과의 **36.3621354, 127.3505065**를 사용했습니다. 현장 운영 전에 실제 건물 입구에서 한 번 테스트하고 필요하면 `index.html`의 `CHECKIN.latitude`, `CHECKIN.longitude`, `radiusMeters` 값을 조정하세요.
