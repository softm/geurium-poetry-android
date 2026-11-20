# 그리움과 시 Android 초기 빌드 검증

| 확인 | 결과 |
|---|---|
| JDK / SDK | Java 17 / Android 36 |
| 공통 모듈 경계 테스트 | 5개 통과, 실패 0 |
| Debug | assembleDebug 성공, APK Signature Scheme v2 검증 통과 |
| Release | assembleRelease 및 lintVitalRelease 성공, unsigned |
| Release 광고 설정 | ADMOB_ENABLED=false |
| Release APK manifest | MobileAdsInitProvider 및 AdMob APP_ID metadata 없음 |
| 번들 웹 파일 | 12개, APK 내부 SHA-256과 web-assets.lock.json 전부 일치 |
| 웹 원본 commit | `06bef2a4e655f916acb056ef38bfba18c9e96dda` |
| WebView 공통 commit | `1d0ca7a4974addc46a66952d028c2f71bd28e523` |

검증 명령:

```sh
./gradlew -PhannunWebViewDir=../hannun-webview-android :hannun-webview:testDebugUnitTest :app:assembleDebug :app:assembleRelease
```

로컬 검증에서는 동일한 고정 소스를 인접 경로로 참조했습니다. 일반 clone과 CI는 저장소의 submodule gitlink를 사용합니다.

## 생성 APK

| 구분 | 크기(bytes) | SHA-256 |
|---|---:|---|
| debug | 7607180 | `7df39e47f35458b5271543de56c6f37a87317c2bd6009fe133f7fda2260e88ac` |
| release | 6166720 | `dd5ce6f49092b89c33a23bd091353b44de390ec1bb0f764d0a73cb0f605e216d` |

Debug APK는 테스트 광고와 Debug 서명을 사용합니다. Release APK는 운영 광고 ID와 서명 설정이 없어 광고가 꺼진 unsigned 산출물입니다.

## 아직 실기기 확인이 필요한 항목

- SAF 파일 선택/저장/취소와 앱 복귀, 프로세스 종료 중 미완료 작업.
- 실제 인쇄/PDF 저장과 인쇄 취소.
- UMP 지역별 동의, 테스트 광고 노출과 네트워크 실패, 운영 ID로 전환한 서명 Release.
- Google Play 내부 테스트 설치본의 업데이트 다운로드/설치.

실기기 광고 송출, Google Play 출시, 운영 서명 완료를 이 기록으로 주장하지 않습니다.
