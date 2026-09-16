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
2. **Authentication → Sign-in method**에서 `Anonymous(익명)` 로그인을 사용 설정합니다.
3. **Firestore Database**를 생성합니다. 리전은 운영 환경에 맞게 선택하세요.
4. 프로젝트 설정에서 받은 `firebaseConfig` 값을 `index.html` 안의 같은 이름 객체에 붙여 넣습니다.
5. Firestore의 **규칙** 탭에서 `firestore.rules` 내용을 붙여 넣고 게시합니다.
6. `index.html`을 GitHub Pages, Firebase Hosting, Netlify, Vercel 등 HTTPS 호스팅에 올립니다.

Firebase 웹 설정의 `apiKey`는 브라우저에서 사용하는 식별 설정값입니다. 보호는 키를 숨기는 방식이 아니라 Authentication, Firestore 보안 규칙, 필요하면 App Check로 구성해야 합니다.

## 운영 확인

- 휴대폰에서 HTTPS 주소를 열고 위치 권한을 허용합니다.
- 인증 장소 반경 안에서 이름을 입력하고 방문 인증을 누릅니다.
- Firebase 콘솔의 **Firestore Database → Data → checkins**에서 공용 기록을 확인합니다.
- 문서 ID가 `익명 UID_날짜` 형식이어서 같은 브라우저에서는 하루 한 번만 기록됩니다.

## 꼭 알아둘 점

이 앱은 동아리용 간단 인증에 맞춘 클라이언트 방식입니다. 개발자 도구로 코드를 변조하거나 브라우저 저장소를 초기화하면 우회할 수 있으므로 시험·근태처럼 강한 부정 방지가 필요한 용도에는 적합하지 않습니다. 더 강한 인증이 필요하면 회원 로그인, 서버 측 검증, App Check, 현장 QR을 추가하세요.

좌표는 공개 지도 검색 결과의 **36.3621354, 127.3505065**를 사용했습니다. 현장 운영 전에 실제 건물 입구에서 한 번 테스트하고 필요하면 `index.html`의 `CHECKIN.latitude`, `CHECKIN.longitude`, `radiusMeters` 값을 조정하세요.
