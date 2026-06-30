# WombatCombat Plant Care

Academic Google Colab project for the **Introduction to Cloud Computing** course.  
The system monitors and manages **Milk Thistle (Silybum marianum)** using cloud services, Firebase, IoT sensor data, academic search, RAG, Gemini AI, Hugging Face image comparison, and an interactive Colab UI.

---

## Project Overview

**WombatCombat Plant Care** is a cloud-based smart agriculture prototype built inside a single Google Colab notebook.  
The system is designed for users who want to monitor plant condition, analyze sensor readings, search academic plant-related articles, ask AI-based questions, upload plant images, and maintain a plant care journal.

The application runs as an interactive Colab app using `ipywidgets`, with a sidebar navigation menu and multiple screens.

---

## Main Features

### 1. Dashboard
- Displays the monitored plant profile.
- Shows latest temperature and humidity readings.
- Calculates plant health status based on configured thresholds.
- Displays daily average temperature.
- Displays temperature fluctuation and trend.
- Uses green/purple visual styling.

### 2. Sensors
- Connects to the course IoT server.
- Fetches temperature and humidity readings.
- Saves sensor history and latest summaries to Firebase when Firebase is configured.
- Handles unavailable feeds safely.

### 3. Academic Search + RAG
- Loads academic PDF articles about Milk Thistle.
- Extracts text from PDFs.
- Builds an inverted index.
- Saves/loads the index from Firebase to avoid rebuilding every run.
- Supports search queries over academic articles.
- Uses Gemini for RAG answers when an API key is configured.
- Falls back to a simple local RAG answer when Gemini is unavailable.

### 4. Chatbot
- Uses Gemini when available for intelligent plant-care responses.
- Includes sensor context in chatbot answers.
- Falls back to an NLTK rule-based chatbot when Gemini is disabled or unavailable.

### 5. Upload Image
- Allows uploading a plant image.
- Compares the image to samples from the Hugging Face PlantNet-300K dataset.
- Uses simple RGB feature extraction and cosine similarity.
- Can use Gemini to generate a plant diagnosis when configured.

### 6. Plant Journal
- Allows users to save plant-care events such as watering, fertilizing, leaf checks, pests, problems, and general notes.
- Stores journal events in Firebase when configured.
- Supports deleting journal events.
- Includes a simple care-points gamification mechanism.

---

## Technologies Used

- **Google Colab** – main runtime environment.
- **Python** – project implementation language.
- **ipywidgets** – interactive UI and navigation.
- **Firebase Realtime Database** – cloud database for index, sensors, journal, and care points.
- **Gemini API** – AI chatbot and RAG answer generation.
- **Hugging Face Datasets** – PlantNet-300K image dataset.
- **PyPDF2** – PDF text extraction.
- **gdown** – downloading academic articles from Google Drive.
- **NLTK** – chatbot fallback and text processing.
- **Pandas / Matplotlib** – data handling and visualization.
- **Requests** – IoT server API calls.

---

## Notebook File

The main project file is:

```text
Wombat_Prj_Final_Git_Safe.ipynb
```

Run the notebook from top to bottom in Google Colab.

---

## Installation

The first code cell installs all required Python packages:

```python
!pip install -q PyPDF2 gdown ipywidgets pandas matplotlib nltk google-genai firebase-admin datasets pillow
```

No manual installation is required when running in Colab.

---

## Configuration

Before running the full project, open the credentials/configuration cell and fill in only the values you need.

```python
GEMINI_API_KEY = ""  # Paste your Gemini API key here before running, or keep empty for fallback mode.

FIREBASE_DATABASE_URL = ""

FIREBASE_SERVICE_ACCOUNT_JSON = {
}
```

### Gemini

To enable Gemini features, set:

```python
GEMINI_API_KEY = "YOUR_GEMINI_API_KEY"
```

If this value is empty, the project automatically disables Gemini and uses fallback behavior.

### Firebase

To enable Firebase, set:

```python
FIREBASE_DATABASE_URL = "YOUR_FIREBASE_REALTIME_DATABASE_URL"

FIREBASE_SERVICE_ACCOUNT_JSON = {
    "type": "service_account",
    ...
}
```

If Firebase credentials are incomplete, the project continues in offline/fallback mode where possible.

### Hugging Face

The Hugging Face token should not be committed to GitHub.  
When running the notebook, paste the token only inside Colab if needed.

---

## Important Security Notes

Do **not** commit real credentials to GitHub.

Never upload:
- Gemini API keys
- Hugging Face tokens
- Firebase service account JSON
- Firebase private keys
- `.env` files
- Any notebook output that exposes credentials

Recommended workflow:
1. Keep placeholders in the notebook.
2. Paste real keys only inside Colab before running.
3. Clear all outputs before committing.
4. Re-check the notebook before pushing to GitHub.

---

## How to Run

1. Open the notebook in Google Colab.
2. Run the installation/import cells.
3. Fill credentials if you want Gemini/Firebase/Hugging Face features.
4. Run all cells from top to bottom.
5. The final cell displays the WombatCombat application.
6. Use the sidebar to navigate between:
   - Dashboard
   - Chatbot
   - Search
   - Upload Image
   - Sensors
   - Journal

---

## Project Architecture

The notebook is organized into service-style sections:

```text
Configuration
│
├── Text Processing Utilities
├── Search and RAG Service
│   ├── PDF Loading
│   ├── Inverted Index
│   └── Gemini / Local RAG
│
├── Firebase Service
│   ├── Index storage
│   ├── Sensor history
│   ├── Journal events
│   └── Care points
│
├── Sensor Service
│   └── IoT server API integration
│
├── Gemini Service
├── Chatbot Service
├── Plant Analytics Service
├── Journal Service
├── Hugging Face Image Service
│
└── UI Layer
    ├── Dashboard
    ├── Journal
    ├── Search
    ├── Chatbot
    ├── Upload Image
    ├── Sensors
    └── Navigation Router
```

This structure follows a modular service-based style inside a single Colab notebook.

---

## Microservices / Service Separation

The project separates logic into independent services:

### Sensor Service
Responsible for fetching IoT data from the course server and returning clean sensor data.

### Firebase Service
Responsible for reading and writing persistent cloud data such as sensor history, journal events, care points, and search index.

### Search and RAG Service
Responsible for loading academic articles, building the inverted index, searching documents, and producing RAG-based answers.

### Hugging Face Service
Responsible for loading image samples and comparing uploaded images to PlantNet references.

### Plant Analytics Service
Responsible for calculating health status, daily averages, fluctuations, and trends from raw sensor data.

This separation improves readability, maintainability, and makes the notebook easier to explain in the final presentation.

---

## Academic Articles

The project uses academic PDF articles related to **Silybum marianum / Milk Thistle**, including metadata such as article title, authors, and DOI/URL.

The PDF folder is configured using:

```python
FOLDER_ID = "1zhHkG9eYIy6__Tnt7V-o_7293m3mz80W"
FOLDER_URL = f"https://drive.google.com/drive/folders/{FOLDER_ID}"
```

Make sure the Google Drive folder is accessible when running the notebook.

---

## KPI Examples

Relevant KPIs for this project include:

- Sensor data freshness – how recently temperature/humidity data was received.
- Plant health status – whether readings are within recommended thresholds.
- Search relevance – whether academic search results match the query.
- RAG answer quality – whether answers are grounded in retrieved article context.
- Journal activity – number of care events saved by the user.
- Care points – gamification score based on plant-care actions.

---

## Known Limitations

- The project depends on external services, so some features may be unavailable if:
  - Gemini quota is exceeded.
  - Firebase credentials are missing.
  - The IoT server is unavailable.
  - Hugging Face dataset access requires a token.
  - Google Drive article links are not accessible.
- Image analysis is based on simple visual similarity and is not a medical/agricultural diagnosis.
- The system is a course prototype, not a production agriculture platform.

---

## GitHub Upload Checklist

Before pushing to GitHub:

- [ ] Use `Wombat_Prj_Final_Git_Safe.ipynb`.
- [ ] Make sure no real API keys are inside the notebook.
- [ ] Make sure no Firebase private key is inside the notebook.
- [ ] Make sure no Hugging Face token is inside the notebook.
- [ ] Clear all notebook outputs.
- [ ] Keep only placeholder credentials.
- [ ] Add this `README.md`.
- [ ] Add a `.gitignore` if needed.

Suggested `.gitignore`:

```gitignore
.env
*.env
secrets/
credentials/
firebase-key.json
serviceAccountKey.json
.ipynb_checkpoints/
__pycache__/
```

---

## Authors

WombatCombat Team  
Introduction to Cloud Computing Course  
Braude College of Engineering

---

## Disclaimer

This project was created for academic purposes.  
The recommendations and analysis generated by the system should not replace professional agricultural advice.
