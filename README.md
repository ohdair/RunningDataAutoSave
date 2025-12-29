## 🏃🏻‍➡️ 런닝 기록 자동 저장 📋
해당 설정하는 자동화는 5분에 한 번씩, 저장된 런닝 기록 사진들을 Google Sheet에 자동으로 데이터를 기입합니다

**Google Sheet 저장된 화면**
|Main|Detail|Shoes|
|:---:|:---:|:---:|
|<img width="1263" height="347" alt="스크린샷 2025-12-29 오후 5 05 58" src="https://github.com/user-attachments/assets/ca782f24-0668-466c-a035-63ac9ddb6e5a" />|<img width="616" height="752" alt="스크린샷 2025-12-29 오후 5 06 44" src="https://github.com/user-attachments/assets/d3b03654-4234-4634-aa1b-3da9aed5a7eb" />|<img width="889" height="267" alt="스크린샷 2025-12-29 오후 5 07 08" src="https://github.com/user-attachments/assets/71b33a5d-a548-4f19-a378-94a44a8005dd" />|

**Google Drive에서 "RunningResults" 폴더에 런닝기록 저장**
Strava 및 런닝 앱에서 기록들 스크린샷
|ScreenShot1|ScreenShot2|
|:---:|:---:|
|![IMG_0525](https://github.com/user-attachments/assets/2d062fd3-34fd-4a3c-9d4e-8d8afecc3113)|![IMG_0526](https://github.com/user-attachments/assets/6242875b-becd-4c48-9bd0-3d067eae7808)|
> 저는 런린이라 아직 시계가 없어서 2장만 넣습니다 😅

### Google API Key 발급
1. [Google AI Studio](https://aistudio.google.com/) 접속
2. 좌측 하단에 Get API key 클릭
3. API key 만들기
  <img width="1155" height="213" alt="스크린샷 2025-12-29 오후 4 19 53" src="https://github.com/user-attachments/assets/0c1ed1a1-43af-4356-8f60-5676a55b3a15" />
  
4. API key 복사
> Apps script 에서 API key 사용

### Google Drive 내 폴더 생성 및 ID 복사
1. Google Drive 에서 새 폴더 "RunningResults" 생성
2. 해당 폴더 클릭 -> url 끝 부분 ID 복사
  <img width="537" height="284" alt="스크린샷 2025-12-29 오후 4 42 36" src="https://github.com/user-attachments/assets/e949786f-cf75-48ab-aa02-5e4fc095f573" />
  
3. Google Drive 에서 새 폴더 "RunningArchive" 생성
4. 동일하게 ID 복사
> Apps script 에서 두 폴더에 대한 ID 사용

### Google sheet 자동화 설정
1. [파일 복사 링크](https://docs.google.com/spreadsheets/d/1eF70Y6alC1dL6Sg6gz24EVbgo5AIRek5xSA9O55IWW0/copy) 저장
2. 저장된 Googe sheet 설정
  1. Extension -> App script 접속
  2. Editor -> Code 내 텍스트 삭제 후, [Script 내용](https://github.com/ohdair/RunningDataAutoSave/blob/main/script.txt)을 붙여넣기
  3. 위에서 복사해야 하는 항목들을 붙여넣기
  <img width="576" height="143" alt="스크린샷 2025-12-29 오후 4 55 59" src="https://github.com/user-attachments/assets/ed3f5fdd-ab7e-44ea-a45d-f3fcc5984174" />
  
  4. processAllImagesTogether 설정 및 저장
  5. Run 클릭
  6. 권한 검토 설정 -> 고급 설정 -> 프로젝트로 이동(안전하지 않음) 클릭
> Shoes에서 "브랜드", "모델명", "수명 한계"는 직접 기입
> Shoes Sheet는 개인 확인용

### Google sheet 트리거 설정
1. App script 접속
2. Trigger -> Add Trigger 클릭
  <img width="711" height="801" alt="스크린샷 2025-12-29 오후 5 00 28" src="https://github.com/user-attachments/assets/0ca2b3dc-3ce0-4f74-aa43-ed47cca6d9ba" />
  
3. interval -> Every 5 minutes 설정
4. 권한 검토 설정 -> 고급 설정 -> 프로젝트로 이동(안전하지 않음) 클릭

