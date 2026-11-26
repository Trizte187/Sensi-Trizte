
name: Build Sensi Trizte IPA

on:
  workflow_dispatch:

permissions:
  contents: read

jobs:
  build:
    runs-on: macos-15
    steps:
      - uses: actions/checkout@v4

      - name: Extract project
        run: |
          unzip -q Sensi-Trizte-source.zip
          cd 3106-main
          bash Tests/run_policy_tests.sh

      - name: Build IPA
        run: |
          cd 3106-main
          xcodebuild \
            -project ThreeOneOSSix.xcodeproj \
            -scheme 3106 \
            -configuration Release \
            -sdk iphoneos \
            -derivedDataPath "$RUNNER_TEMP/Build" \
            CODE_SIGNING_ALLOWED=NO \
            CODE_SIGNING_REQUIRED=NO \
            CODE_SIGN_IDENTITY="" \
            build

          APP="$RUNNER_TEMP/Build/Build/Products/Release-iphoneos/3106.app"
          VERSION=$(/usr/libexec/PlistBuddy -c "Print :CFBundleShortVersionString" "$APP/Info.plist")

          mkdir Payload
          cp -R "$APP" Payload/
          zip -qry "Sensi-Trizte-${VERSION}-unsigned.ipa" Payload

      - name: Upload IPA
        uses: actions/upload-artifact@v4
        with:
          name: Sensi-Trizte-IPA
          path: "3106-main/Sensi-Trizte-*-unsigned.ipa"
