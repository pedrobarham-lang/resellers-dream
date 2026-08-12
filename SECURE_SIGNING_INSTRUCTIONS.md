# How to add signing keys and API secrets

To create a signed release automatically via GitHub Actions, add the following repository secrets (Repository -> Settings -> Secrets and variables -> Actions -> New repository secret):

- KEYSTORE_BASE64 — base64-encoded contents of your Android release keystore file (single-line base64 output).
- KEYSTORE_PASSWORD — password for the keystore.
- KEY_ALIAS — key alias used when generating the keystore.
- KEY_ALIAS_PASSWORD — password for the key alias (often the same as the keystore password).
- EBAY_APP_ID — optional: your eBay App ID to enable eBay sold-item research.

Generate base64-encoded keystore (example):

macOS / Linux:
  base64 -w 0 release.keystore > release.keystore.base64

Windows (PowerShell):
  [Convert]::ToBase64String([IO.File]::ReadAllBytes('release.keystore')) > release.keystore.base64

Then copy the single-line base64 string into KEYSTORE_BASE64.

Once you add the secrets, trigger the workflow from the Actions tab or push to the branch feature/resellers-dream-init. The workflow will:
- Run expo prebuild to generate the native Android project
- Build the release APK (unsigned)
- Decode your keystore and sign the APK using apksigner
- Upload the signed APK as a workflow artifact

Security note: Do not commit secrets into the repository. Use GitHub Secrets or EAS secrets.
