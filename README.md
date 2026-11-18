# PhishShield – Browser-Level Phishing URL Shield

A Chrome browser extension that identifies potentially phishing websites using URL and webpage features, a locally hosted FastAPI backend, and a pre-trained XGBoost machine learning model.

PhishShield is a final-year academic project prototype developed to demonstrate browser-level phishing detection and website classification.

---

## 1. Project Overview

Phishing is a type of cyberattack in which attackers create deceptive websites to trick users into revealing sensitive information, such as usernames, passwords, banking details, and personal information.

PhishShield aims to help users identify potentially suspicious websites while browsing.

The project uses:

- **Chrome Extension:** Detects website navigation and displays scan results.
- **FastAPI Backend:** Receives URLs and performs feature extraction and prediction.
- **XGBoost Model:** Classifies websites as legitimate or phishing.
- **Feature Extraction:** Extracts 22 features from URLs and webpage content.
- **Chrome Popup:** Displays prediction results and scan history.

The backend runs locally on the user's computer.

### Main objective

To develop an academic prototype that demonstrates how machine learning can be integrated into a browser extension to classify potentially legitimate and phishing websites.

---

## 2. Key Features

### Browser-Level Website Scanning

- Monitors website navigation in Chrome.
- Sends supported HTTP and HTTPS URLs to the local backend.
- Extracts website-related features.
- Uses an existing trained XGBoost model for classification.

### Phishing Detection

The model produces two classifications:

| Prediction | Classification | Display |
|---|---|---|
| 1 | Legitimate | Safe / Legitimate |
| 0 | Phishing | Unsafe / Phishing |

### Chrome Extension Interface

- Displays website scan results.
- Shows phishing warnings.
- Displays safe website notifications.
- Maintains scan history.
- Provides error and unknown states.

### Error Handling

The extension distinguishes between:

- Safe / Legitimate
- Unsafe / Phishing
- Error / Unknown
- Not Scanned

A failed API request is not treated as a safe prediction.

### Trusted Website Handling

Some trusted domains are handled through an allowlist.

These websites may be marked safe without being evaluated by the machine learning model. The extension distinguishes trusted-list results from model predictions.

### Local Backend

- Uses FastAPI.
- Runs on localhost.
- Does not require cloud deployment.
- Communicates with the Chrome extension through HTTP requests.

---

## 3. Technologies Used

### Frontend

- HTML5
- CSS3
- JavaScript
- Chrome Extensions API
- Chrome Manifest V3

### Backend

- Python
- FastAPI
- Uvicorn
- Requests
- BeautifulSoup4

### Machine Learning

- XGBoost
- Scikit-learn
- Pandas
- NumPy
- Pre-trained XGBoost model
- Pre-trained feature scaler

### Development Tools

- Visual Studio Code
- Google Chrome
- PowerShell
- Git
- GitHub

---

## 4. Project Architecture

PhishShield follows a browser-extension and local-backend architecture.

```text
                 USER
                   |
                   v
             Google Chrome
                   |
                   v
          PhishShield Extension
                   |
                   v
           Website Navigation
                   |
                   v
          HTTP / HTTPS URL Check
                   |
                   v
           Trusted Domain Check
                   |
             Not Trusted?
                   |
                   v
        Local FastAPI Backend
          http://127.0.0.1:8000
                   |
                   v
             URL Extraction
                   |
                   v
          22 Feature Extraction
                   |
                   v
             Feature Scaler
                   |
                   v
          Existing XGBoost Model
                   |
                   v
            Prediction Result
                   |
          +--------+--------+
          |                 |
          v                 v
      Legitimate          Phishing
      Prediction 1       Prediction 0
          |                 |
          v                 v
      Safe Result       Warning Result
          |                 |
          +--------+--------+
                   |
                   v
            Chrome Popup
```

**Important:** The backend independently fetches the webpage using Python requests. It does not directly analyse the browser's fully rendered DOM.

---

## 5. Project Folder Structure

```text
PhishShield_Extension/
│
├── backend/
│   ├── app.py
│   ├── url_feature_extractor.py
│   ├── requirements.txt
│   ├── scaler.pkl
│   ├── xgb_model.json
│   │
│   ├── model/
│   │
│   └── tests/
│       └── test_api.py
│
├── Frontend/
│   ├── manifest.json
│   ├── background.js
│   ├── scan_logic.js
│   ├── popup.html
│   ├── popup.js
│   ├── styles.css
│   │
│   ├── icons/
│   │
│   └── tests/
│       └── scan.test.mjs
│
├── demo/
│   └── login_like.html
│
├── experiments/
│   ├── datasets/
│   │   └── raw/
│   └── reports/
│
├── .gitignore
├── README.md
│
└── venv/
```

The `venv/` directory is a local Python virtual environment and should not be uploaded to GitHub.

The `experiments/datasets/raw/` directory contains locally stored research datasets and should not be committed.

---

## 6. System Requirements

Recommended development environment:

| Requirement | Description |
|---|---|
| Operating System | Windows 10 / Windows 11 |
| Browser | Google Chrome |
| Programming Language | Python |
| Backend Framework | FastAPI |
| Machine Learning | XGBoost |
| Code Editor | Visual Studio Code |
| Internet | Required for fetching external webpages |
| Local Server | Uvicorn |

Use a Python version compatible with the dependencies and the existing model/scaler artifacts. The project was tested using its existing virtual environment.

---

## 7. Installation and Setup

### Step 1: Clone the repository

Open PowerShell:

```powershell
git clone https://github.com/nnaidev02Amer/browser-level-phishing-url-shield.git
```

Navigate into the project:

```powershell
cd browser-level-phishing-url-shield
```

### Step 2: Create a Python virtual environment

```powershell
python -m venv venv
```

Activate it:

```powershell
.\venv\Scripts\Activate.ps1
```

If PowerShell blocks script execution, use:

```powershell
Set-ExecutionPolicy -Scope Process -ExecutionPolicy Bypass
.\venv\Scripts\Activate.ps1
```

### Step 3: Install backend dependencies

Install the packages listed in the backend requirements file:

```powershell
python -m pip install --upgrade pip
python -m pip install -r backend/requirements.txt
```

### Step 4: Verify model files

Make sure these files exist:

```text
backend/scaler.pkl
backend/xgb_model.json
```

These are the existing trained model artifacts required by the backend.

Do not delete, replace, or regenerate them when setting up the project.

---

## 8. Running the Backend

The backend is implemented using FastAPI.

From the project root, run:

```powershell
.\venv\Scripts\python.exe -m uvicorn app:app --app-dir backend --host 127.0.0.1 --port 8000
```

The backend will run at:

```text
http://127.0.0.1:8000
```

### Verify the backend

Open:

```text
http://127.0.0.1:8000/
```

Expected response:

```json
{
  "message": "PhishShield API is running 🚀"
}
```

### Open Swagger API documentation

```text
http://127.0.0.1:8000/docs
```

Swagger provides an interactive interface to inspect and test the available API endpoints.

**Keep the backend terminal running while using the Chrome extension.**

---

## 9. Installing the Chrome Extension

PhishShield is a Chrome Manifest V3 extension.

It does not require a separate npm development server.

### Step 1: Open Chrome Extensions

Navigate to:

```text
chrome://extensions/
```

### Step 2: Enable Developer Mode

Enable the Developer mode toggle in the top-right corner.

### Step 3: Load the extension

1. Click **Load unpacked**.
2. Navigate to the cloned project directory.
3. Select the `Frontend` folder.
4. Click Select Folder.

The extension should now appear in Chrome.

### Step 4: Pin the extension

1. Open the Chrome Extensions menu.
2. Find PhishShield.
3. Click the pin icon.

### Step 5: Start scanning

Make sure the FastAPI backend is running.

Visit an HTTP or HTTPS website. The extension will process the supported navigation and display the result.

After changing extension files, return to `chrome://extensions/` and click Reload.

---

## 10. Machine Learning Model

PhishShield uses an existing pre-trained XGBoost classification model.

The backend extracts 22 features and processes them using the existing scaler before passing them to the model.

### Feature Extraction

The current feature pipeline uses the following 22 features:

| No. | Feature | Description |
|---:|---|---|
| 1 | URLLength | Length of the URL |
| 2 | DomainLength | Length of the domain |
| 3 | TLDLength | Length of the top-level domain |
| 4 | NoOfImage | Number of images |
| 5 | NoOfJS | Number of JavaScript references |
| 6 | NoOfCSS | Number of CSS references |
| 7 | NoOfSelfRef | Number of internal references |
| 8 | NoOfExternalRef | Number of external references |
| 9 | IsHTTPS | Indicates HTTPS usage |
| 10 | HasObfuscation | Indicates detected obfuscation |
| 11 | HasTitle | Indicates the presence of a page title |
| 12 | HasDescription | Indicates a page description |
| 13 | HasSubmitButton | Indicates a submit button |
| 14 | HasSocialNet | Indicates social network references |
| 15 | HasFavicon | Indicates favicon availability |
| 16 | HasCopyrightInfo | Indicates copyright information |
| 17 | popUpWindow | Popup-related feature |
| 18 | Iframe | Iframe-related feature |
| 19 | Abnormal_URL | Abnormal URL indicator |
| 20 | LetterToDigitRatio | Ratio of letters to digits in the URL |
| 21 | Redirect_0 | Redirect-related feature |
| 22 | Redirect_1 | Redirect-related feature |

The exact feature definitions, order and formulas are maintained in the existing feature extraction implementation.

### Prediction Mapping

The existing model uses the following class mapping:

```text
Prediction 1 = Legitimate
Prediction 0 = Phishing
```

The API also returns a `probability_legitimate` value for successful URL predictions.

This probability is the model's output for class 1. It should not be interpreted as a guarantee that a website is safe.

---

## 11. API Endpoints

The backend exposes FastAPI endpoints.

### 1. GET /

Checks whether the backend is running.

```text
GET http://127.0.0.1:8000/
```

Expected response:

```json
{
  "message": "PhishShield API is running 🚀"
}
```

### 2. POST /predict_url

Accepts a URL, extracts its features and returns a model prediction.

```text
POST http://127.0.0.1:8000/predict_url
```

Example request:

```json
{
  "url": "https://example.com"
}
```

A successful response includes:

- Extracted features
- Prediction
- Classification result
- Probability of the legitimate class

The exact extracted values and prediction depend on the webpage and the existing model.

### 3. POST /predict

Accepts an already prepared feature input for model prediction.

```text
POST http://127.0.0.1:8000/predict
```

Use the Swagger documentation at `/docs` to inspect the exact request schema.

### Error Responses

The backend uses HTTP status codes for request failures:

| Status | Meaning |
|---|---|
| 200 | Successful request |
| 422 | Invalid request or URL |
| 502 | Website fetch failure |
| 500 | Internal server error |

---

## 12. Controlled Demonstration

A controlled local webpage can be used to demonstrate the phishing classification without visiting an actual phishing website.

The project includes:

```text
demo/login_like.html
```

### Step 1: Open a new PowerShell terminal

Navigate to the demo folder:

```powershell
cd demo
```

### Step 2: Start the local web server

```powershell
python -m http.server 8765 --bind 127.0.0.1
```

### Step 3: Open the test page

```text
http://127.0.0.1:8765/login_like.html
```

### Step 4: Observe the extension

The extension scans the URL and displays the model's classification.

The controlled login-style page was previously classified as phishing during testing.

The result is dependent on the extracted features and model prediction; it is not guaranteed for every modification of the demonstration page.

The test page is for academic demonstration only. It does not collect or transmit credentials.

### Stop the local server

Press:

```text
Ctrl + C
```

---

## 13. Testing

The project includes automated tests for the backend and extension.

### Backend Tests

Run from the project root:

```powershell
.\venv\Scripts\python.exe -I -W ignore -m unittest discover -s backend/tests -v
```

Previously verified result:

```text
15 backend tests passed
```

### Chrome Extension Tests

Node.js is required.

Run:

```powershell
node --test Frontend/tests/scan.test.mjs
```

Previously verified result:

```text
12 extension tests passed
```

### Total Automated Tests

| Test Suite | Passed |
|---|---:|
| Backend | 15 |
| Chrome Extension | 12 |
| Total | 27 |

These results refer to the recorded local test run. Run the commands again to verify the current checkout.

Automated tests do not replace manual testing in a real Chrome browser.

---

## 14. Security and Error Handling

The project includes several safeguards:

- Only HTTP and HTTPS URLs are eligible for scanning.
- Browser-internal pages are skipped.
- Trusted-domain matching uses hostname boundaries.
- API failures are not classified as legitimate.
- Only valid prediction values are accepted.
- Failed scans are not reused as successful cached predictions.
- Scan history distinguishes error and unknown results.
- URL and error text are escaped before HTML insertion.

These features improve the reliability of the prototype's behaviour.

They do not make the extension a complete browser security product.

---

## 15. Project Limitations

PhishShield is an academic prototype and has limitations.

### Model Evaluation

The existing model has not been independently evaluated on a verified, compatible labelled test dataset.

Therefore, no verified real-world accuracy, precision, recall or F1-score is claimed.

### HTTP-Only Websites

The existing model relies heavily on the HTTPS feature. Some legitimate HTTP-only websites may be classified as phishing.

### Trusted Domain Allowlist

Some allowlisted websites are marked safe without being evaluated by the machine learning model.

### Server-Side Page Fetching

The backend independently fetches webpages using Python requests.

It may not see:

- JavaScript-rendered content
- Content requiring authentication
- Content available only through browser cookies
- Content selectively served to different clients

### Redirect and Feature Definitions

The original model training notebook and training dataset were not available during the project audit.

Some training-time feature definitions, particularly abnormal URL and redirect encoding, could not be independently verified.

### Local Backend Dependency

The extension depends on the local FastAPI backend.

If the backend is unavailable, the extension displays Error / Unknown rather than a safe result.

### Academic Scope

The system is intended for academic demonstration and experimentation. It must not be treated as a guarantee that a website is safe or as a replacement for professional security software.

---

## 16. Future Enhancements

Possible future improvements include:

- More extensive evaluation using compatible labelled datasets.
- Better analysis of browser-rendered webpage content.
- Improved redirect analysis.
- Additional phishing detection features.
- More comprehensive Chrome browser testing.
- Improved scan history and reporting.
- Better handling of dynamic webpages.
- More robust detection of suspicious domains.

These are potential future improvements and are not claimed as completed functionality.

---

## 17. GitHub Repository

**Project:** Browser-Level Phishing URL Shield

**Repository:**

https://github.com/nnaidev02Amer/browser-level-phishing-url-shield

### Clone the project

```bash
git clone https://github.com/nnaidev02Amer/browser-level-phishing-url-shield.git
cd browser-level-phishing-url-shield
```

---

## 18. Author

**Project Name:** PhishShield – Browser-Level Phishing URL Shield

**Project Type:** Final-Year Academic Project

**Technologies:** Python, FastAPI, XGBoost, JavaScript, HTML, CSS, Chrome Extensions API

---

## 19. License

Add a license file if you intend to distribute the project under a particular open-source license.

Check the licenses of the original code, model and dependencies before redistributing them.

---

## 20. Final Note

PhishShield demonstrates how a browser extension, a Python backend, feature extraction and a machine learning classifier can work together to identify potentially phishing websites.

The project provides a local academic prototype for demonstrating legitimate and phishing classifications through a Chrome extension.

**PhishShield – Helping users identify potentially suspicious websites while browsing.**
