# DAV CBT – Candidate Data Submission Portal

A complete, self-hosted web application for collecting data from DAV CBT qualified candidates. Hosted on **GitHub Pages** (free), with **Firebase** as the backend for database and file storage.

---

## Features

- ✅ Candidate form with all required fields
- ✅ PDF upload (1 MB limit, PDF only)
- ✅ **Duplicate prevention** – CBT Roll Number is unique; re-entry is blocked
- ✅ **Preview before submit** – candidates review all entries before final submission
- ✅ Password-protected **admin panel**
- ✅ **Download as Excel** with all submissions
- ✅ Search & filter in admin panel
- ✅ View submitted PDF from admin panel

---

## Setup Instructions

### Step 1 — Create a Firebase Project

1. Go to [https://console.firebase.google.com](https://console.firebase.google.com)
2. Click **Add Project** → give it a name (e.g. `dav-cbt-portal`) → Continue
3. Disable Google Analytics (optional) → **Create Project**

---

### Step 2 — Enable Firestore Database

1. In Firebase Console → **Build → Firestore Database**
2. Click **Create database**
3. Choose **Start in production mode** → Next
4. Select your region (e.g. `asia-south1` for India) → **Enable**
5. Go to **Rules** tab and paste:

```
rules_version = '2';
service cloud.firestore {
  match /databases/{database}/documents {
    match /applicants/{rollNumber} {
      allow read, write: if true;
    }
  }
}
```
6. Click **Publish**

---

### Step 3 — Enable Firebase Storage

1. In Firebase Console → **Build → Storage**
2. Click **Get Started** → **Start in production mode** → **Done**
3. Go to **Rules** tab and paste:

```
rules_version = '2';
service firebase.storage {
  match /b/{bucket}/o {
    match /applicants/{rollNumber}/{allPaths=**} {
      allow read, write: if true;
    }
  }
}
```
4. Click **Publish**

---

### Step 4 — Get Your Firebase Config

1. In Firebase Console → click the **gear icon** → **Project Settings**
2. Scroll down to **Your apps** → Click **`</>`** (Web)
3. Register app (any nickname) → Copy the `firebaseConfig` object

---

### Step 5 — Paste Config into Both Files

Open **`index.html`** and **`admin.html`** and replace the placeholder config block:

```javascript
const firebaseConfig = {
  apiKey: "YOUR_API_KEY",               // ← paste your values here
  authDomain: "YOUR_PROJECT.firebaseapp.com",
  projectId: "YOUR_PROJECT",
  storageBucket: "YOUR_PROJECT.appspot.com",
  messagingSenderId: "YOUR_SENDER_ID",
  appId: "YOUR_APP_ID"
};
```

---

### Step 6 — Set Your Admin Password

In **`admin.html`**, find this line and change the password:

```javascript
const ADMIN_PASSWORD = "DAVAdmin@2024";   // ← change this!
```

Choose a strong password. This is the only thing protecting your admin panel.

---

### Step 7 — Host on GitHub Pages

1. Create a new **public** repository on GitHub (e.g. `dav-cbt-portal`)
2. Upload `index.html` and `admin.html` to the repository
3. Go to repo → **Settings → Pages**
4. Under **Source**, select `main` branch → `/ (root)` → **Save**
5. Your site will be live at:  
   `https://YOUR_USERNAME.github.io/dav-cbt-portal/`

---

## Usage

### Candidates
- Visit `https://YOUR_USERNAME.github.io/dav-cbt-portal/`
- Enter CBT Roll Number → verify
- Fill all fields → Preview → Submit
- If already submitted, previous entry is shown (read-only)

### Admin
- Visit `https://YOUR_USERNAME.github.io/dav-cbt-portal/admin.html`
- Enter admin password
- View all submissions in a searchable table
- Click **Download Excel** to export all data as `.xlsx`

---

## Security Notes

- The admin password is stored in the HTML file. For higher security, consider using Firebase Authentication for the admin login.
- Firestore rules currently allow any read/write. This is needed for the candidate form to work without authentication. If you want extra security, you can tighten the rules for reads (admin only) while keeping writes open for candidates.
- Aadhaar numbers are stored in plain text in Firebase. Ensure your Firebase project is secured and access is restricted.

---

## Files

| File | Purpose |
|------|---------|
| `index.html` | Candidate-facing submission form |
| `admin.html` | Admin panel with data table and Excel export |
| `README.md` | This setup guide |
