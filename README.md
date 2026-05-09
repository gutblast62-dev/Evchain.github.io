# EVCHAIN — Evidence Management System

> **Law Enforcement Edition** | Role-based access · Chain of custody · QR code scanning

Live site: **[https://gutblast62-dev.github.io/Evchain](https://gutblast62-dev.github.io/Evchain)**

---

## 📁 Repository Structure

```
Evchain/                  ← GitHub Pages root (deploy from here)
├── index.html            ← Main application (frontend)
├── 404.html              ← Handles QR deep-link redirects
├── .nojekyll             ← Disables Jekyll so filenames work correctly
└── README.md
```

---

## 🚀 Deploying to GitHub Pages — Step by Step

### 1. Create the Repository

1. Go to [github.com](https://github.com) and sign in.
2. Click **New repository**.
3. Name it exactly: `Evchain`
4. Set visibility to **Public** (required for free GitHub Pages).
5. Click **Create repository**.

### 2. Upload the Files

**Option A — GitHub Web UI (easiest):**
1. Open your new repository.
2. Click **Add file → Upload files**.
3. Drag and drop all 4 files: `index.html`, `404.html`, `.nojekyll`, `README.md`.
4. Click **Commit changes**.

**Option B — Git command line:**
```bash
git clone https://github.com/YOUR_USERNAME/Evchain.git
cd Evchain
# Copy index.html, 404.html, .nojekyll, README.md into this folder
git add .
git commit -m "Initial EVCHAIN deployment"
git push origin main
```

### 3. Enable GitHub Pages

1. In your repository, go to **Settings → Pages** (left sidebar).
2. Under **Source**, select **Deploy from a branch**.
3. Set **Branch** to `main` and folder to `/ (root)`.
4. Click **Save**.
5. Wait ~60 seconds, then your site will be live at:
   ```
   https://YOUR_USERNAME.github.io/Evchain/
   ```

### 4. Point the Frontend to Your Backend

Open `index.html` and find this line (around line 507):

```js
const EVCHAIN_API_BASE = 'https://evchain-production.up.railway.app/api';
```

Replace the URL with your own backend (Railway, Render, etc.), then re-upload.

**Or** test with a different backend without editing the file by appending `?api=` to the URL:
```
https://YOUR_USERNAME.github.io/Evchain/?api=https://your-backend.com
```

---

## 🔐 Default Login

| Field    | Value      |
|----------|------------|
| Username | `admin`    |
| Password | `admin123` |

> **Change this immediately** after first login via Administration → Personnel → Edit.

---

## 📡 Backend Setup

The frontend requires the Node.js backend (`server-fixed.js`) running separately.

### Quick Start (local):
```bash
npm install
node init-db.js       # Creates evchain.db with tables + default admin
npm start             # Starts API on port 3000
```

### Deploy Backend to Railway:
1. Push `server-fixed.js`, `init-db.js`, `package.json`, `.env` to a GitHub repo.
2. Go to [railway.app](https://railway.app) → **New Project → Deploy from GitHub**.
3. Set environment variables in Railway dashboard:
   - `JWT_SECRET` — a long random string
   - `NODE_ENV` — `production`
   - `FRONTEND_URL` — `https://YOUR_USERNAME.github.io`
4. Railway will auto-detect Node.js and run `npm start`.
5. Copy the Railway URL (e.g. `https://evchain-xxx.up.railway.app`) into `EVCHAIN_API_BASE` in `index.html`.

### Deploy Backend to Render:
1. Push backend files to GitHub.
2. Go to [render.com](https://render.com) → **New Web Service**.
3. Connect your repo, set **Build Command**: `npm install && node init-db.js`
4. Set **Start Command**: `node server-fixed.js`
5. Add the same environment variables as above.

---

## 📱 QR Code Scanning

Each evidence item has a **QR** button that generates a scannable label. When scanned:

1. Device browser opens `https://YOUR_USERNAME.github.io/Evchain/?evid=EV-XXXX-XXXX`
2. Login screen appears (no bypass)
3. If credentials are valid and user is authorized for that case → Evidence detail opens
4. If unauthorized → Access denied (logged in audit trail)

---

## 🔒 Roles

| Feature             | Admin | Personnel       |
|---------------------|-------|-----------------|
| View all evidence   | ✅    | ❌ Assigned only |
| Log new evidence    | ✅    | ❌               |
| Delete evidence     | ✅    | ❌               |
| Log custody events  | ✅    | ❌               |
| View custody timeline | ✅  | ✅ Assigned only |
| Generate QR codes   | ✅    | ✅ Assigned only |
| Manage personnel    | ✅    | ❌               |

---

## ⚠️ Security Checklist Before Going Live

- [ ] Change the default `admin` password
- [ ] Set a strong `JWT_SECRET` in the backend `.env`
- [ ] Set `NODE_ENV=production`
- [ ] Use HTTPS for both the frontend (GitHub Pages provides this) and backend
- [ ] Set `FRONTEND_URL` in backend `.env` to your exact GitHub Pages URL
- [ ] Enable database file encryption at rest on the server

---

*EVCHAIN Evidence Management System — Law Enforcement Edition v1.0*
