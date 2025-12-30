# 🏃🏻‍♂️ 런닝 기록 자동 아카이빙 가이드
> **개요:** 런닝 앱(Strava 등)의 기록 스크린샷을 Google Drive에 업로드하면, AI가 이미지를 분석하여 5분마다 Google Sheet에 자동으로 정리해주는 자동화 시스템입니다.

## 📊 1. 결과 예시 (자동화 목표)

**✅ Google Sheet 자동 저장 화면**
AI가 스크린샷을 분석하여 런닝 상세 데이터와 신발 정보를 정리합니다.

| Main (요약) | Detail (상세) | Shoes (장비) |
| :---: | :---: | :---: |
| <img width="1263" alt="스크린샷 Main" src="https://github.com/user-attachments/assets/ca782f24-0668-466c-a035-63ac9ddb6e5a" /> | <img width="616" alt="스크린샷 Detail" src="https://github.com/user-attachments/assets/d3b03654-4234-4634-aa1b-3da9aed5a7eb" /> | <img width="889" alt="스크린샷 Shoes" src="https://github.com/user-attachments/assets/71b33a5d-a548-4f19-a378-94a44a8005dd" /> |

**📂 Google Drive 폴더 구성**
업로드된 원본 스크린샷은 `RunningResults` 폴더에서 처리 후, 이미지들은 삭제됩니다.

| ScreenShot 1 | ScreenShot 2 |
| :---: | :---: |
| <img width="300" src="https://github.com/user-attachments/assets/2d062fd3-34fd-4a3c-9d4e-8d8afecc3113"> | <img width="300" src="https://github.com/user-attachments/assets/6242875b-becd-4c48-9bd0-3d067eae7808"> |

> 💡 **참고:** 예시는 런닝 초보라 시계 데이터 없어서 앱 스크린샷 2장(심박수 정보 없이)만 사용했습니다.

---

## 🛠️ 2. 사전 준비 (API 및 폴더 설정)

### ① Google API Key 발급
Gemini 모델을 사용하기 위해 API 키가 필요합니다.

1. [Google AI Studio](https://aistudio.google.com/)에 접속합니다.
2. 좌측 하단의 **Get API key**를 클릭합니다.
3. **Create API key**를 눌러 키를 생성합니다.

   <img width="1155" alt="API Key 생성 화면" src="https://github.com/user-attachments/assets/0c1ed1a1-43af-4356-8f60-5676a55b3a15" />
   
5. 생성된 **API Key를 복사**해 둡니다. (추후 Apps Script에서 사용)

### ② Google Drive 폴더 생성 및 ID 확인
이미지를 업로드할 폴더와 처리 완료된 이미지를 보관할 폴더를 만듭니다.

1. Google Drive에서 새 폴더 **`RunningResults`** (업로드용)를 생성합니다.
2. 해당 폴더에 들어가서 **URL 주소의 끝부분 ID**를 복사합니다.

   <img width="537" alt="폴더 ID 확인 방법" src="https://github.com/user-attachments/assets/e949786f-cf75-48ab-aa02-5e4fc095f573" />
   
---

## ⚙️ 3. Google Sheet 및 스크립트 설정

### ① 시트 복사 및 코드 적용
1. **[파일 복사 링크](https://docs.google.com/spreadsheets/d/1eF70Y6alC1dL6Sg6gz24EVbgo5AIRek5xSA9O55IWW0/copy)**를 클릭하여 스프레드시트를 내 드라이브로 가져옵니다.
2. 상단 메뉴에서 **확장 프로그램 (Extensions) > Apps Script**로 이동합니다.
3. 기존 코드를 모두 지우고, **[Script 내용](https://github.com/ohdair/RunningDataAutoSave/blob/main/script.txt)**을 복사하여 붙여넣습니다.

### ② 변수 입력 및 실행
1. 코드 상단의 변수 설정 부분에 앞서 복사해둔 정보를 입력합니다.
    * `GEMINI_API_KEY`: 위에서 발급받은 API Key
    * `SOURCE_FOLDER_ID`: `RunningResults` 폴더 ID
    * ~~`ARCHIVE_FOLDER_ID`: `RunningArchive` 폴더 ID~~(런닝 앱에 기록되므로 저장이 아닌 삭제로 변경)
    
   <img width="576" alt="스크립트 변수 입력 화면" src="https://github.com/user-attachments/assets/ed3f5fdd-ab7e-44ea-a45d-f3fcc5984174" />

2. 상단 함수 선택창에서 `processAllImagesTogether`를 선택하고 **실행 (Run)** 버튼을 누릅니다.
3. **권한 검토** 창이 뜨면 다음 순서로 승인합니다:
    * `권한 검토` 클릭 -> 계정 선택 -> `고급(Advanced)` 클릭 -> `Project(으)로 이동(안전하지 않음)` 클릭 -> `허용`

> 📝 **Note:**
> * **Shoes 시트:** "브랜드", "모델명", "수명 한계" 등은 자동 인식되지 않으므로 직접 기입해주세요. (개인 관리용)

---

## ⏰ 4. 자동화 트리거 설정 (5분 주기)

매번 코드를 실행할 필요 없이, 자동으로 동작하도록 설정합니다.

1. Apps Script 화면 좌측의 **트리거 (시계 아이콘)** 메뉴로 이동합니다.
2. 우측 하단의 **+ 트리거 추가 (Add Trigger)** 버튼을 클릭합니다.

   <img width="500" alt="트리거 설정 화면" src="https://github.com/user-attachments/assets/0ca2b3dc-3ce0-4f74-aa43-ed47cca6d9ba" />
   
3. 아래와 같이 설정합니다:
    * **실행할 함수:** `processAllImagesTogether`
    * **이벤트 소스:** `시간 기반 (Time-driven)`
    * **트리거 유형:** `분 단위 타이머 (Minutes timer)`
    * **간격:** `5분마다 (Every 5 minutes)`
4. 저장 버튼을 누르고 권한 확인 과정을 한 번 더 진행하면 설정이 완료됩니다.
