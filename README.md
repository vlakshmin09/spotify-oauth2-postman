# 🎵 Spotify Web API Testing via Postman

> Completed as part of the Coursera Guided Project:  
> **"API Testing a Real Web Application via Postman"** by Saurabh Dhingra  
> 🏅 🏅 [View Certificate](https://coursera.org/share/329966d3bad203ae1aa7092e69da88f5)
 

---

## 📌 Project Overview

This project demonstrates end-to-end API testing of the **Spotify Web API** using **Postman**, covering the full **OAuth 2.0 Authorization Code Flow** and real Spotify features — accessing user profile, creating playlists, and modifying playlists.

---

## 🔐 OAuth 2.0 Flow

```
User → GET /authorize → Auth Code → POST /api/token → Access Token → Spotify API Requests
```

---

## 🧪 Features Tested

### 1️⃣ Access User Profile (GET)
- Endpoint: `GET https://api.spotify.com/v1/me`
- Auth: Bearer token in header
- Validated: `200 OK` status, user `id`, `display_name`, `email`, `country` in response
- Scope required: `user-read-private user-read-email`

---

### 2️⃣ Create Playlist (POST)
- Endpoint: `POST https://api.spotify.com/v1/users/{user_id}/playlists`
- Auth: Bearer token in header
- Body:
```json
{
  "name": "My Test Playlist",
  "description": "Created via Postman API testing",
  "public": false
}
```
- Validated: `201 Created` status, playlist `id` and `name` returned in response
- Scope required: `playlist-modify-private`

---

### 3️⃣ Modify Playlist (PUT)
- Endpoint: `PUT https://api.spotify.com/v1/playlists/{playlist_id}`
- Auth: Bearer token in header
- Body:
```json
{
  "name": "Updated Playlist Name",
  "description": "Modified via Postman",
  "public": true
}
```
- Validated: `200 OK` status, updated values reflected in response
- Scope required: `playlist-modify-public`

---

## 🔐 OAuth 2.0 Authorization Flow (Step by Step)

### Step 1 — Authorization Grant (GET)
- Sent GET request to `https://accounts.spotify.com/authorize`
- Parameters used:

| Parameter | Value |
|-----------|-------|
| `client_id` | Your Spotify App Client ID |
| `response_type` | `code` |
| `redirect_uri` | `http://127.0.0.1:8080` |
| `scope` | `user-read-private user-read-email playlist-modify-private playlist-modify-public` |
| `state` | `abc123` |

- Result: Spotify login/consent page → Authorization Code returned in redirect URL ✅

> ⚠️ **Note:** Spotify enforced new redirect URI security rules from November 2025.  
> `http://localhost` and `https://oauth.pstmn.io/v1/callback` are now blocked.  
> Resolved by using loopback IP: `http://127.0.0.1:8080` ✅

---

### Step 2 — Access Token Exchange (POST)
- Sent POST to `https://accounts.spotify.com/api/token`
- Headers: `Authorization: Basic BASE64(client_id:client_secret)`
- Body: `grant_type=authorization_code`, `code`, `redirect_uri`
- Result: `access_token`, `refresh_token`, `expires_in` returned ✅

---

### Step 3 — Refresh Token (POST)
- Sent POST to `https://accounts.spotify.com/api/token`
- Body: `grant_type=refresh_token`, `refresh_token`
- Result: New `access_token` returned without re-authenticating ✅

---

## ✅ Assertions Added

```javascript
// Status code check
pm.test("Status code is 200", function () {
    pm.response.to.have.status(200);
});

// Response time check
pm.test("Response time is less than 500ms", function () {
    pm.expect(pm.response.responseTime).to.be.below(500);
});

// Profile - display name exists
pm.test("User display name is present", function () {
    var jsonData = pm.response.json();
    pm.expect(jsonData.display_name).to.exist;
});

// Playlist created successfully
pm.test("Playlist ID is returned", function () {
    var jsonData = pm.response.json();
    pm.expect(jsonData.id).to.exist;
});
```

---

## 🔑 All API Endpoints Tested

| Method | Endpoint | Feature |
|--------|----------|---------|
| GET | `https://accounts.spotify.com/authorize` | Authorization Grant |
| POST | `https://accounts.spotify.com/api/token` | Get Access Token |
| POST | `https://accounts.spotify.com/api/token` | Refresh Token |
| GET | `https://api.spotify.com/v1/me` | Access User Profile |
| POST | `https://api.spotify.com/v1/users/{user_id}/playlists` | Create Playlist |
| PUT | `https://api.spotify.com/v1/playlists/{playlist_id}` | Modify Playlist |

---

## 📸 Screenshots

| Step | Screenshot |
|------|-----------|
| Spotify Developer Dashboard | `screenshots/01-spotify-dashboard.png` |
| GET Authorize Request (Postman) | `screenshots/02-get-authorize-params.png` |
| Auth Code in Browser URL | `screenshots/03-auth-code-url.png` |
| POST Token Exchange | `screenshots/04-post-token-request-response.png` |
| GET /v1/me — User Profile | `screenshots/05-get-profile-response.png` |
| POST — Create Playlist | `screenshots/06-create-playlist.png` |
| PUT — Modify Playlist | `screenshots/07-modify-playlist.png` |
 
---

## 🛠️ Tools Used

| Tool | Purpose |
|------|---------| 
| Postman | API Testing & OAuth 2.0 flow |
| Spotify Web API | Real-world API under test |
| Spotify Developer Dashboard | App registration & redirect URI setup |

---

## ⚠️ Key Challenge — Spotify's 2025 Security Update

During this project I encountered `INVALID_CLIENT: Insecure redirect URI` errors.

**Root cause:** Spotify enforced new OAuth security rules from **November 2025**:
- ❌ `http://localhost` — blocked
- ❌ `http://localhost:3000` — blocked
- ❌ `https://oauth.pstmn.io/v1/callback` — blocked
- ✅ `http://127.0.0.1:8080` — allowed (loopback IP literal)

This real-world troubleshooting experience demonstrates the importance of staying current with API security changes.

---

## 📂 Repository Structure

```
spotify-oauth2-postman/
├── README.md
├── spotify.postman_collection.json   
├── spotify.postman_environment.json  
└── screenshots/
    ├── 01-spotify-dashboard.png
    ├── 02-get-authorize-params.png
    ├── 03-auth-code-url.png
    ├── 04-post-token-request-response.png
    ├── 05-get-profile-response.png
    ├── 06-create-playlist.png
    ├── 07-modify-playlist.png

```

---

## 🚀 How to Use This Collection

1. Clone this repo
2. Open Postman → Import `spotify.postman_collection.json`
3. Import `spotify.postman_environment.json`
4. Add your own `client_id` and `client_secret` from [Spotify Developer Dashboard](https://developer.spotify.com/dashboard)
5. Add `http://127.0.0.1:8080` as redirect URI in your Spotify app settings
6. Run the collection in order

---

## 📜 Certificate

Completed: **API Testing a Real Web Application via Postman**  
Platform: Coursera | Instructor: Saurabh Dhingra  
🔗 [View Certificate](https://coursera.org/share/329966d3bad203ae1aa7092e69da88f5)


---

*Portfolio: vlakshmin09.github.io/portfolio - https://vlakshmin09.github.io/portfolio/
________________________________________

