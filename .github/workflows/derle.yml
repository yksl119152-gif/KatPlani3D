name: APK ve JAR derle

on:
  push:
  workflow_dispatch:

jobs:
  derle:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4

      - name: Zip dosyasini ac
        run: unzip -q -o KatPlani3D-proje.zip

      - uses: actions/setup-java@v4
        with:
          distribution: temurin
          java-version: '17'

      - uses: gradle/actions/setup-gradle@v4
        with:
          gradle-version: '8.7'

      - name: Sayfayi ve three.js kutuphanesini hazirla
        working-directory: KatPlani3D
        run: |
          mkdir -p android/app/src/main/assets/lib jar/web/lib
          curl -fsSL https://cdnjs.cloudflare.com/ajax/libs/three.js/r128/three.min.js -o android/app/src/main/assets/lib/three.min.js
          curl -fsSL https://cdn.jsdelivr.net/npm/three@0.128.0/examples/js/controls/OrbitControls.js -o android/app/src/main/assets/lib/OrbitControls.js
          cp web/index.html android/app/src/main/assets/index.html
          cp web/index.html jar/web/index.html
          cp android/app/src/main/assets/lib/*.js jar/web/lib/

      - name: APK derle
        working-directory: KatPlani3D/android
        run: gradle assembleDebug --no-daemon

      - name: JAR derle
        working-directory: KatPlani3D/jar
        run: |
          mkdir -p out
          javac --release 11 -encoding UTF-8 -d out src/KatPlani3D.java
          cp -r web out/
          jar cfe ../KatPlani3D.jar KatPlani3D -C out .

      - name: APK dosyasini topla
        working-directory: KatPlani3D
        run: cp android/app/build/outputs/apk/debug/app-debug.apk KatPlani3D.apk

      - uses: actions/upload-artifact@v4
        with:
          name: KatPlani3D-APK
          path: KatPlani3D/KatPlani3D.apk

      - uses: actions/upload-artifact@v4
        with:
          name: KatPlani3D-JAR
          path: KatPlani3D/KatPlani3D.jar
