# gh-pages-site/

GitHub Pages 로 배포되는 공개 랜딩 페이지. main 브랜치의 이 디렉터리가 변경되면
`.github/workflows/pages.yml` 이 자동 deploy.

## 활성화 절차

1. GitHub 저장소 → Settings → Pages
2. Source: **GitHub Actions**
3. main 브랜치에 push → Actions 탭에서 "Deploy GitHub Pages landing" 확인
4. 배포 URL: `https://<owner>.github.io/<repo>/`

## 호스트 / 릴리즈 링크 커스터마이즈

`index.html` 하단 스크립트의 상수 (`HOST`, `RELEASES`, `APK_DOWNLOAD`) 를 직접 수정하거나,
배포 시 별도 `config.js` 를 만들어서 `<script>` 로 전역 변수 주입.

```html
<script>
  window.JIKJANGIN_HOST = "jikjangin-a.example.com";
  window.JIKJANGIN_RELEASES = "https://github.com/JSungMin/jikjangin-a-releases/releases";
  window.JIKJANGIN_APK = "https://github.com/JSungMin/jikjangin-a-releases/releases/latest/download/app-release-signed.apk";
</script>
```

## Digital Asset Links

`.well-known/assetlinks.json` 의 SHA-256 fingerprint 를 실제 APK 서명 key 값으로 교체:

```bash
cd apps/mobile/android
keytool -list -v -keystore android.keystore -alias jikjangin-a | grep SHA256
```

Chrome 이 이 파일을 다운로드해서 TWA APK 서명과 비교, 일치하면 주소 표시줄 숨김.
