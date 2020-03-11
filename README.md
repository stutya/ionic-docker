[![MIT licensed](https://img.shields.io/badge/license-MIT-blue.svg)](https://tldrlegal.com/license/mit-license#summary) [![Docker Hub](https://img.shields.io/badge/docker-ready-blue.svg)](https://registry.hub.docker.com/u/stutya/ionic)

# Build APK for Ionic 1/2/3/4 Cordova App from Gitlab

1. Create a .gitlab-ci.yml similar to the following:
```
image: stutya/ionic:latest

stages:
  - deploy

cache:
  untracked: true
  key: "$CI_PROJECT_ID"
  paths:
    - node_modules/

build_android:
  stage: deploy
  only:
    - master
    // - <another-branch> // Put applicable branches here.
  script:
    - npm i
    - ionic cordova platform rm android
    - ionic cordova platform add android
    - ionic cordova build android
    - cp platforms/android/app/build/outputs/apk/*/*.apk ./app.apk
  artifacts:
    paths:
      - app.apk
```
It should build the APK and will be available to download.

# Build APK for Ionic 4/5 Capacitor App from Gitlab

1. Create a .gitlab-ci.yml similar to the following:
```
image: stutya/ionic:latest

stages:
  - deploy
  
cache:
  untracked: true
  key: "$CI_PROJECT_ID"
  paths:
    - node_modules/
    
build_android:
  stage: deploy
  only:
    - master
    - ci-test
  script:
    - npm i
    - npm run build
    - npx cap sync android
    - cd android/ && ./gradlew assembleRelease
    - cp app/build/outputs/apk/*/*.apk ./app.apk
  artifacts:
    paths:
      - app.apk
```
It should build the APK and will be available to download.
