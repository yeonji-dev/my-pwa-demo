# Progressive Web App
- 웹사이트를 안드로이드/iOS 모바일 앱처럼 사용할 수 있게 만드는 일종의 웹개발 기술
- service-worker.js 파일, Cache storage 가 있어 오프라인에서도 동작
- 설치 유도 비용이 매우 적음
- manifest.json과 service-worker.js 파일 존재하고, HTTPS 사이트면 브라우저가 PWA로 인식
- PWAbuilder 이용하면 apk 파일로 변환 가능

### Create Project
- `npx create-react-app 프로젝트명 --template cra-template-pwa`
- index.js `serviceWorkerRegistration.unregister();`를 `serviceWorkerRegistration.register();` 로 변경
- manifest.json
  ```json
  {
    "version" : "앱 버전",
    "short_name" : "설치후 앱런처나 바탕화면에 표시할 짧은 12자 이름",
    "name" : "기본이름",
    "icons" : [
      {사이즈1 아이콘 이미지 경로}, 
      {사이즈2 아이콘 이미지 경로}, 
      {사이즈3 아이콘 이미지 경로}
    ],
    "start_url" : "메인페이지 경로",
    "display" : "standalone 또는 fullscreen", //앱 켜면 브라우저 상단바 제거할지 말지. standalone이 제거된 상태임
    "background_color" : "앱 처음 실행시 잠깐 뜨는 splashscreen의 배경색",
    "theme_color" : "상단 탭색상 등 원하는 테마색상"
  }
  ```
- service-worker.js
  - 웹앱 설치시 어떤 css, js, html, 이미지 파일이 하드에 설치될지 설정: 오프라인에서도 사이트 열수 있게 도와줌
  - 빌드할때마다 파일 이름, 경로가 무작위로 바뀌어 만일 업데이트 되었다면 서버에서 새로 요청에서 받아옴
  - 튜토리얼: [https://developer.chrome.com/docs/workbox/service-worker-overview/](https://developer.chrome.com/docs/workbox/service-worker-overview/)
  
- 빌드 후 생성되는 파일 중 asset-manifest.json에 캐싱할 파일 목록 들어가있음

- 디버깅: live-server 활용
  - `npm i -g live-server`
  - build 폴더 이동 후 `live-server` 실행하면 브라우저 열림
  - 개발자 도구 - Application - Manifest, Service Workers, CacheStorage 에서 확인 가능

  - 캐싱 목록 커스터마이징(특정 파일 캐싱 안되게 설정) : 아래 파일 exclude에 정규식으로 파일 이름 작성
      - node_modules/react_scripts/webpack.config.js
    ```js
    // Generate a service worker script that will precache, and keep up to date,
    // the HTML & assets that are part of the webpack build.
        isEnvProduction &&
          fs.existsSync(swSrc) &&
          new WorkboxWebpackPlugin.InjectManifest({
            swSrc,
            dontCacheBustURLsMatching: /\.[0-9a-f]{8}\./,
            exclude: [/\.map$/, /asset-manifest\.json$/, /LICENSE/],
            // Bump up the default maximum size (2mb) that's precached,
            // to make lazy-loading failure scenarios less likely.
            // See https://github.com/cra-template/pwa/issues/13#issuecomment-722667270
            maximumFileSizeToCacheInBytes: 5 * 1024 * 1024,
          }),
  
    ``` 
