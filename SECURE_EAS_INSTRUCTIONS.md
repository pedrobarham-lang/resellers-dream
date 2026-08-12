EAS (Expo Application Services) build & signing — instructions

This document explains how to provide the credentials the EAS workflow needs and how to run the CI build.

What the workflow does
- Uses EXPO_TOKEN (GitHub secret) to authenticate with Expo.
- Triggers an EAS Android build using the project profile `release` defined in eas.json.
- Waits for the build to finish and downloads the signed APK artifact, then uploads it as a GitHub Actions artifact.

Secrets you must add to the repository
- EXPO_TOKEN — required. Create this token from your Expo account (see steps below) and add it at: Repository → Settings → Secrets and variables → Actions → New repository secret

Optional (only if you want automatic Play Store submit)
- GOOGLE_SERVICE_ACCOUNT_JSON — the contents of the Google Play service account JSON (for EAS submit). Add it as a secret if you plan to auto-submit.

How to create EXPO_TOKEN (recommended via eas-cli)
1) Install EAS CLI locally:
   npm install -g eas-cli

2) Login to Expo:
   eas login

3) Create a token:
   eas token:create --non-interactive

   Copy the token string and add it into the repo secrets as EXPO_TOKEN.

Letting EAS manage your keystore vs providing your own
- EAS-manage-keystore (recommended): do nothing. When you first run an EAS Android build for a project, EAS can generate and manage the Android keystore for you. You can then download it from expo.dev if needed.
- I-will-upload-keystore: If you prefer to upload your keystore, run locally:
   eas credentials -p android
  and follow prompts to upload your keystore. This associates the keystore with your Expo project (server-side) and CI builds will use it.

Running the workflow
1) Add EXPO_TOKEN as a repo secret.
2) Go to Actions → EAS Android APK (signed via EAS) → Run workflow. Choose branch: feature/resellers-dream-init and click Run.
3) Wait for the run to finish. After success, open the run and download the artifact named `resellers-dream-eas-apk`.

Troubleshooting
- If the workflow fails with authentication errors, double-check EXPO_TOKEN value and permissions.
- If the build errors (native build issues), check the EAS build logs linked from the workflow logs — they will show the full remote build console.
- If you prefer to debug locally first, run:
   eas build --platform android --profile release --non-interactive

Security notes
- Do not put EXPO_TOKEN or service-account JSON into your code — use GitHub Secrets.
- EAS can manage your keystore so you don't have to store it in this repository.

If you want, I can:
- Help you create EXPO_TOKEN step-by-step and show exactly how to add it as a secret.
- After you add EXPO_TOKEN, I can re-run the workflow and inspect logs for issues.
