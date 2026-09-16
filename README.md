# Sixteen Soldiers — Google Play Production & Billing Integration Guide 🚀

This guide provides a comprehensive, step-by-step workflow to prepare your **Sixteen Soldiers (Sholo Guti)** game for a successful release on the **Google Play Store**, utilizing the newly integrated **Google Play Billing Library V6** and your live dynamic Coin Store feed.

---

## 📂 Table of Contents
1. [Updated Store Package JSON Feed](#1-updated-store-package-json-feed)
2. [Step-by-Step Production Build Setup](#2-step-by-step-production-build-setup)
3. [Registering Products on Google Play Console](#3-registering-products-on-google-play-console)
4. [Setting Up Sandbox & Live Billing Testing](#4-setting-up-sandbox--live-billing-testing)
5. [Pre-Submission Release Checklist](#5-pre-submission-release-checklist)

---

## 1. Updated Store Package JSON Feed

We have successfully updated the app parser to extract and utilize the official Google Play `id` directly from your dynamic remote JSON feed. 

Please copy the JSON block below and update your hosted file on GitHub Pages (`https://androidevs.github.io/SixteenSoldiers/Store/Package.json`):

```json
[
  {
    "id": "coins_100",
    "coins": 100,
    "price": 1.01,
    "title": "Starter Bead Pack"
  },
  {
    "id": "coins_250",
    "coins": 250,
    "price": 1.99,
    "title": "Warrior Guti Pack"
  },
  {
    "id": "coins_400",
    "coins": 400,
    "price": 3.99,
    "title": "Commander Vault"
  },
  {
    "id": "coins_600",
    "coins": 600,
    "price": 5.99,
    "title": "Tactician Cache"
  },
  {
    "id": "coins_800",
    "coins": 800,
    "price": 7.99,
    "title": "Challenger Stash"
  },
  {
    "id": "coins_1000",
    "coins": 1000,
    "price": 9.99,
    "title": "General Chest"
  },
  {
    "id": "coins_1500",
    "coins": 1500,
    "price": 14.99,
    "title": "King's Treasure"
  },
  {
    "id": "coins_2500",
    "coins": 2500,
    "price": 24.99,
    "title": "Emperor's Palace"
  },
  {
    "id": "coins_5000",
    "coins": 5000,
    "price": 44.99,
    "title": "Sholo Guti Overlord"
  },
  {
    "id": "coins_10000",
    "coins": 10000,
    "price": 79.99,
    "title": "Infinite Legend Chest"
  }
]
```

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
Create products with IDs that match the `id` field defined in your dynamic JSON feed (e.g., `coins_100`, `coins_250`, `coins_400`):

1. Click **Create product** in the upper-right corner.
2. Fill out the form:
   * **Product ID**: `coins_100` *(Must match the `id` string in the JSON exactly!)*
   * **Name**: `Starter Bead Pack` *(Can be any name, but matching the JSON title is recommended)*
   * **Description**: `Instantly receive 100 extra coins to play Sixteeen Soldiers.`
3. **Price**: Enter your price matching your raw JSON feed (e.g., `$1.01`).
4. Click **Save** and then click **Activate**.

*(Repeat these steps for every package defined in your live store JSON. Product IDs must match the `id` fields precisely.)*

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
5. [ ] **Policy & Support URLs**: Add the dynamic support, terms and privacy policy pages hosted on your landing page to the Google Play Store Console settings.

---

*You are all set! Safe uploading and wishing you great success on the Google Play Store!* 🏆
