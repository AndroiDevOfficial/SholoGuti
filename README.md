#  Sixteen Soldiers — Google Play Production & Billing Integration Guide 🚀

This guide provides a comprehensive, step-by-step workflow to prepare your **Sixteen Soldiers (Sholo Guti)** game for a successful release on the **Google Play Store**, utilizing the newly integrated **Google Play Billing Library V6**.

---

## 📂 Table of Contents
1. [Overview of Current Features](#1-overview-of-current-features)
2. [Step-by-Step Production Build Setup](#2-step-by-step-production-build-setup)
3. [Registering Products on Google Play Console](#3-registering-products-on-google-play-console)
4. [Setting Up Sandbox & Live Billing Testing](#4-setting-up-sandbox--live-billing-testing)
5. [Pre-Submission Release Checklist](#5-pre-submission-release-checklist)

---

## 1. Overview of Current Features

*   **Google Play Billing Library V6 (`com.android.billingclient:billing-ktx:6.2.1`)**: Embedded natively to process purchases securely and fast.
*   **Dynamic Failover Sandbox**: Automatically detects if the Play Store billing pipeline is active. Fallbacks smoothly to a lifelike transaction simulator during development or emulator tests.
*   **Receipt Verification Screen**: Displays authentic Google Play format Transaction IDs (e.g., `GPA.XXXX-XXXX-XXXX-XXXXX`) with one-click clipboard copying.
*   **Automatic Cache-Busted Feed**: Dynamic timestamping (`?t=timestamp`) is appended to your raw GitHub URL to ensure store pricing modifications show instantly inside the app!

---

## 2. Step-by-Step Production Build Setup

Before compiling your final production `.aab` (Android App Bundle), you need to update your release signing keys.

### Step 2.1: Generate Your Custom Upload Keystore
Run the following standard Java Keytool command in your local command prompt or terminal to generate a secure keystore file:

```bash
keytool -genkey -v -keystore my-upload-key.jks -alias upload -keyalg RSA -keysize 2048 -validity 10000
```
This will generate a file named `my-upload-key.jks`. Save it safely!

### Step 2.2: Place the Keystore in your Project
Place the `my-upload-key.jks` file into the root of this project.

### Step 2.3: Add Signing Credentials to Environment or Local Setup
The `app/build.gradle.kts` file is configured to securely extract signing parameters from system environment variables. You can set these keys in your terminal before building:

*   **Linux/macOS**:
    ```bash
    export STORE_PASSWORD="your_keystore_password"
    export KEY_PASSWORD="your_alias_password"
    ```
*   **Windows (CMD)**:
    ```cmd
    set STORE_PASSWORD=your_keystore_password
    set KEY_PASSWORD=your_alias_password
    ```

---

## 3. Registering Products on Google Play Console

To enable real-world payments, the in-app items must exist in your Google Play Console dashboard under the exact matching product IDs.

### Step 3.1: Navigate to Monetization
1. Open your **Google Play Console**.
2. Select your Sixteen Soldiers application.
3. Under the **Monetization** section in the left sidebar, click on **In-app products**.

### Step 3.2: Create Products to Match Your JSON
Create products with IDs that match the dynamic format parsed from your GitHub `Package.json` (e.g., `coins_100`, `coins_250`, `coins_500`):

1. Click **Create product** in the upper-right corner.
2. Fill out the form:
   * **Product ID**: `coins_100` *(Must match your raw package data ID exactly!)*
   * **Name**: `100 Coins Pack`
   * **Description**: `Instantly receive 100 extra coins to play Sixteeen Soldiers.`
3. **Price**: Enter your price matching your raw JSON feed (e.g., `$0.99`).
4. Click **Save** and then click **Activate**.

*(Repeat these steps for every package defined in your live store JSON.)*

---

## 4. Setting Up Sandbox & Live Billing Testing

Google Play protects developers from accidental charges during the staging phase. You can test complete live flows with simulated zero-dollar transactions.

### Step 4.1: Register License Testers
1. In your **Google Play Console** home screen, search for **License Testing** (under Setup ➔ License Testing).
2. Enter the Google account emails of your testers (including your own developer email address).
3. Set the **License response** option to **RESPOND_NORMALLY**.

### Step 4.2: Distribute via Closed Testing Track
1. Build your Production Bundle:
   ```bash
   gradle :app:bundleRelease
   ```
2. Upload the compiled `.aab` file from `app/build/outputs/bundle/release/` to the **Internal Testing** or **Closed Testing** track.
3. Once approved, invite your license testing accounts via the tester join link.
4. When testers buy a package, Google Play will prompt with a secure **"Test Card (Always Approves)"** sheet. The app will consume the item and print a verified transaction ID with $0.00 actual cost!

---

## 5. Pre-Submission Release Checklist

Before submitting to Google's reviewers, verify you have checked these items:

1. [ ] **Version Code Increment**: Ensure `versionCode` in `app/build.gradle.kts` is incremented with each upload (e.g., 1, 2, 3...).
2. [ ] **Matching IDs**: Check that all product IDs registered on Google Console correspond to the exact spelling of package ids parsed from your raw GitHub JSON.
3. [ ] **Play Store Listing**: Ensure you have uploaded at least 4 screenshots, a high-res app icon ($512 \times 512$), and a feature graphic ($1024 \times 500$).
4. [ ] **Privacy Policy**: Add a valid privacy policy URL in your Store Presence dashboard (required for apps accessing the internet).

---

*You are all set! Safe uploading and wishing you great success on the Google Play Store!* 🏆
