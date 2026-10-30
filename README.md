# Radio_Alreef--android.
تطبيق راديو الريف – منصة إعلامية مجتمعية تهدف إلى نقل أخبار وقضايا الريف اليمني، وتقديم الأخبار والبرامج والمحتوى الصوتي، وتسليط الضوء على المجتمع والبيئة والتنمية المحلية، عبر تجربة استخدام بسيطة وخفيفة تناسب مستخدمي الهواتف الذكية.
name: Build Radio Alreef APK

on:
  workflow_dispatch:
  push:
    paths:
      - '.github/workflows/build-android.yml'
      - 'Radio_Alreef_Android_Prototype_v0.5_SectionIdentity.zip'

permissions:
  contents: read

jobs:
  build:
    runs-on: ubuntu-latest

    steps:
      - name: Checkout
        uses: actions/checkout@v4

      - name: Set up Java
        uses: actions/setup-java@v4
        with:
          distribution: temurin
          java-version: '17'

      - name: Set up Gradle
        uses: gradle/actions/setup-gradle@v4
        with:
          gradle-version: '8.9'

      - name: Extract project
        run: |
          mkdir project
          unzip -q Radio_Alreef_Android_Prototype_v0.5_SectionIdentity.zip -d project
          echo "PROJECT_DIR=project/radio_alreef_android" >> $GITHUB_ENV

      - name: Build APK
        run: gradle -p "$PROJECT_DIR" assembleDebug

      - name: Upload APK
        uses: actions/upload-artifact@v4
        with:
          name: Radio-Alreef-APK
          path: project/radio_alreef_android/app/build/outputs/apk/debug/*.apk
          if-no-files-found: error
