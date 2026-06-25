🎵 Spotify OAuth 2.0 API Testing via Postman
Completed as part of the Coursera Guided Project:
"API Testing a Real Web Application via Postman" by Saurabh Dhingra
🏅https://coursera.org/share/329966d3bad203ae1aa7092e69da88f5 
________________________________________
📌 Project Overview
This project demonstrates end-to-end API testing of the Spotify Web API using Postman, covering the full OAuth 2.0 Authorization Code Flow — from getting the authorization grant to refreshing the access token.
________________________________________
🔐 OAuth 2.0 Flow Tested
User → GET /authorize → Auth Code → POST /api/token → Access Token → API Requests
________________________________________
🧪 Test Scenarios Covered
Step 1 — OAuth 2.0 Authorization Grant (GET)
•	Sent GET request to https://accounts.spotify.com/authorize
•	Parameters: client_id, response_type=code, redirect_uri, scope, state
•	Result: Spotify login/consent page → Authorization Code returned in redirect URL
⚠️ Note: Spotify enforced new redirect URI security rules from November 2025.
http://localhost and https://oauth.pstmn.io/v1/callback are now blocked.
Resolved by using loopback IP: http://127.0.0.1:8080 ✅
________________________________________
Step 2 — Access Token Exchange (POST)
•	Sent POST request to https://accounts.spotify.com/api/token
•	Headers: Authorization: Basic BASE64(client_id:client_secret)
•	Body: grant_type=authorization_code, code, redirect_uri
•	Result: access_token, refresh_token, expires_in returned ✅
________________________________________
Step 3 — Add Item (POST Request)
•	Tested authenticated POST endpoint
•	Validated 201 Created status code
•	Verified response body matches sent data
________________________________________
Step 4 — Get Item (GET Request)
•	Tested authenticated GET endpoint using Bearer token
•	Validated 200 OK status code
•	Verified correct data returned in response
________________________________________
Step 5 — Edit Item (PUT Request)
•	Tested authenticated PUT endpoint
•	Validated 200 OK status code
•	Verified updated values reflected in response
________________________________________
Step 6 — Assertions Added
•	Added Postman test scripts to automate validation:
// Status code check
pm.test("Status code is 200", function () {
    pm.response.to.have.status(200);
});

// Response time check
pm.test("Response time is less than 500ms", function () {
    pm.expect(pm.response.responseTime).to.be.below(500);
});

// Token exists check
pm.test("Access token is present", function () {
    var jsonData = pm.response.json();
    pm.expect(jsonData.access_token).to.exist;
});
________________________________________
Step 7 — Refresh Token (POST)
•	Sent POST to https://accounts.spotify.com/api/token
•	Body: grant_type=refresh_token, refresh_token
•	Result: New access_token returned without re-authenticating ✅
________________________________________
📸 Screenshots
Step	Screenshot
Spotify Developer Dashboard	screenshots/01-spotify-dashboard.png
GET Authorize Request (Postman)	screenshots/02-get-authorize-params.png
Spotify Consent Page	screenshots/03-spotify-consent-page.png
Auth Code in Browser URL	screenshots/04-auth-code-url.png
POST Token Exchange	screenshots/05-post-token-request.png
Access Token Response	screenshots/06-access-token-response.png
GET /v1/me Response	screenshots/07-get-me-response.png
Assertions Passing	screenshots/08-assertions-passing.png
Refresh Token Request	screenshots/09-refresh-token.png
________________________________________
🛠️ Tools Used
Tool	Purpose
Postman	API Testing & OAuth 2.0 flow
Spotify Web API	Real-world API under test
Spotify Developer Dashboard	App registration & redirect URI setup
________________________________________
🔑 API Endpoints Tested
Method	Endpoint	Purpose
GET	https://accounts.spotify.com/authorize	Authorization Grant
POST	https://accounts.spotify.com/api/token	Get Access Token
GET	https://api.spotify.com/v1/me	Get Current User Profile
POST	Spotify API endpoint	Add Item
GET	Spotify API endpoint	Get Item
PUT	Spotify API endpoint	Edit Item
POST	https://accounts.spotify.com/api/token	Refresh Token
________________________________________
⚠️ Key Challenge — Spotify's 2025 Security Update
During this project I encountered INVALID_CLIENT: Insecure redirect URI errors.
Root cause: Spotify enforced new OAuth security rules from April 2025 (mandatory from November 2025):
•	❌ http://localhost — blocked
•	❌ http://localhost:3000 — blocked
•	❌ https://oauth.pstmn.io/v1/callback — blocked
•	✅ http://127.0.0.1:8080 — allowed (loopback IP literal)
This real-world troubleshooting experience demonstrates the importance of staying current with API security changes.
________________________________________
📂 Repository Structure
spotify-oauth2-postman/
├── README.md
├── spotify.postman_collection.json   ← Import this into Postman
├── spotify.postman_environment.json  ← Environment variables
└── screenshots/
    ├── 01-spotify-dashboard.png
    ├── 02-get-authorize-params.png
    ├── 03-spotify-consent-page.png
    ├── 04-auth-code-url.png
    ├── 05-post-token-request.png
    ├── 06-access-token-response.png
    ├── 07-get-me-response.png
    ├── 08-assertions-passing.png
    └── 09-refresh-token.png
________________________________________
🚀 How to Use This Collection
1.	Clone this repo
2.	Open Postman → Import spotify.postman_collection.json
3.	Import spotify.postman_environment.json
4.	Add your own client_id and client_secret from Spotify Developer Dashboard
5.	Add http://127.0.0.1:8080 as redirect URI in your Spotify app settings
6.	Run the collection in order
________________________________________
📜 Certificate
Completed: API Testing a Real Web Application via Postman
Platform: Coursera | Instructor: Saurabh Dhingra
🔗https://coursera.org/share/329966d3bad203ae1aa7092e69da88f5
________________________________________
Portfolio: vlakshmin09.github.io/portfolio

