# 🎬 Movie Recommendation System

A web-based movie discovery and recommendation app built with **Streamlit**, **FastAPI**, **scikit-learn**, and **The Movie Database (TMDB) API**.

Search for a movie, explore movie details and posters, and discover similar titles using a TF-IDF-based recommendation model.

## ✨ Features

- **Movie search:** Search movies by title using keyword suggestions.
- **Movie details:** View available information such as overview, release date, genres, and artwork.
- **TF-IDF recommendations:** Recommend similar titles using a precomputed TF-IDF matrix and cosine similarity.
- **Genre-based discovery:** Explore movies from TMDB categories such as trending, popular, top-rated, now playing, and upcoming.
- **Poster artwork:** Load movie posters and backdrops from TMDB.
- **Web interface:** Browse results and open individual movie detail pages.

> Movie metadata, posters, and live discovery results depend on the TMDB API and an internet connection. Recommendations from the local TF-IDF dataset depend on the included model/data files.

## 🧰 Tech Stack

| Technology | Purpose |
|---|---|
| Python | Application logic |
| Streamlit | User interface |
| FastAPI | REST API backend |
| pandas and NumPy | Data handling and numerical operations |
| scikit-learn and SciPy | TF-IDF and similarity calculations |
| httpx and Requests | HTTP/API requests |
| TMDB API | Movie metadata and poster images |
| Pickle files | Store preprocessed data and recommendation resources |

## 🏗️ Architecture

```text
User
  |
  v
Streamlit frontend (app.py)
  |
  v
FastAPI backend (main.py)
  |                     |
  v                     v
Local TF-IDF data      TMDB API
and similarity model   movie metadata and images
```

The Streamlit frontend sends requests to the FastAPI backend. The backend uses the local preprocessed dataset and TF-IDF resources for similarity-based recommendations, and the TMDB API for current movie information and artwork.

## 📁 Project Structure

```text
Movie-Recommendation-System/
├── app.py                 # Streamlit frontend
├── main.py                # FastAPI backend
├── df.pkl                 # Preprocessed movie dataframe
├── indices.pkl            # Movie-title index mapping
├── tfidf.pkl              # Saved TF-IDF vectorizer/resource
├── tfidf_matrix.pkl       # Precomputed TF-IDF matrix
├── movies_metadata.csv    # Movie metadata dataset
├── movies.ipynb           # Notebook used for exploration/model work
├── requirements.txt       # Python dependencies
├── .python-version        # Python version configuration
└── runtime.txt            # Runtime version configuration
```

## 🚀 Live Demo

- **Streamlit frontend:** https://movie-recommendation-system-klxyzshxjbxtevvibu2y48.streamlit.app/
- **Backend API:** https://movie-recommendation-system-2yv1.onrender.com
- **Backend health check:** https://movie-recommendation-system-2yv1.onrender.com/health

If a live link is unavailable, the hosting service may be restarting, sleeping, or experiencing a deployment issue.

## ⚙️ Run Locally

### 1. Prerequisites

- Python **3.11** (the repository includes `.python-version` and `runtime.txt` set to 3.11.9)
- pip
- A TMDB API key
- Internet access for TMDB requests

### 2. Clone the repository

```bash
git clone <YOUR_GITHUB_REPOSITORY_URL>
cd Movie-Recommendation-System
```

Replace `<YOUR_GITHUB_REPOSITORY_URL>` with the URL of your GitHub repository.

### 3. Create and activate a virtual environment

**Windows (PowerShell):**

```powershell
py -3.11 -m venv .venv
.venv\Scripts\Activate.ps1
```

**macOS/Linux:**

```bash
python3.11 -m venv .venv
source .venv/bin/activate
```

### 4. Install dependencies

```bash
python -m pip install --upgrade pip
pip install -r requirements.txt
```

### 5. Configure the TMDB API key

Create a file named `.env` in the project root:

```env
TMDB_API_KEY=your_tmdb_api_key_here
```

Get an API key from the official TMDB website: https://www.themoviedb.org/settings/api

**Security:** Never commit your `.env` file or publish your API key. Keep it in your hosting provider's secrets/environment-variable settings for deployment.

### 6. Start the FastAPI backend

In the project root, run:

```bash
uvicorn main:app --reload --host 127.0.0.1 --port 8000
```

Check the backend at:

- Health: http://127.0.0.1:8000/health
- Interactive API docs: http://127.0.0.1:8000/docs

### 7. Configure the frontend backend URL

The Streamlit frontend currently uses the deployed backend URL in `app.py`. To run the whole project locally, set `API_BASE` to:

```python
API_BASE = "http://127.0.0.1:8000"
```

For deployment, use the public URL of your deployed FastAPI backend.

### 8. Start Streamlit

Open a second terminal in the project folder, activate the same virtual environment, and run:

```bash
streamlit run app.py
```

Streamlit will print a local URL, usually http://localhost:8501.

## 🔌 API Endpoints

The FastAPI backend includes these endpoints:

| Endpoint | Purpose |
|---|---|
| `GET /health` | Check backend health |
| `GET /home` | Retrieve a home-feed category |
| `GET /tmdb/search?query=<movie>` | Search TMDB for movies |
| `GET /movie/id/{tmdb_id}` | Get details for a TMDB movie ID |
| `GET /recommend/genre` | Get genre-based recommendations |
| `GET /recommend/tfidf` | Get TF-IDF recommendations |
| `GET /movie/search` | Return a combined movie search/recommendation response |

See `http://127.0.0.1:8000/docs` while the backend is running for parameter details and interactive testing.

## 🧠 How Recommendations Work

1. Movie data is preprocessed and saved in local pickle files.
2. Movie descriptions/features are represented using TF-IDF.
3. The backend calculates similarity scores using the TF-IDF matrix and cosine similarity.
4. Similar movie titles are returned to the frontend.
5. TMDB is used to look up movie information and poster artwork where available.

Recommendations depend on the coverage and quality of the included dataset. A movie available in TMDB may not have a TF-IDF recommendation if its title is absent from the local dataset.

## 🛠️ Troubleshooting

- **App opens but search fails:** Check that the FastAPI backend is running and that `API_BASE` points to the correct backend URL.
- **TMDB requests fail:** Verify the `TMDB_API_KEY` environment variable and check API/network availability.
- **No recommendations appear:** Confirm that `df.pkl`, `indices.pkl`, and `tfidf_matrix.pkl` are present and compatible with each other.
- **Posters are missing:** Some movies do not have poster artwork, or TMDB may not return a poster path.
- **Deployment dependency errors:** Use Python 3.11 as configured in this repository and redeploy after checking package compatibility.
- **Backend is slow on its first request:** A free hosting service may need to wake up after inactivity.

## 🔐 Secrets and Data

- Do not commit `.env` or API keys.
- Keep the model/data pickle files from the same preprocessing run so their indices stay aligned.
- Only load pickle files from sources you trust; pickle files can execute code when loaded.

## 🙌 Acknowledgements

- [The Movie Database (TMDB)](https://www.themoviedb.org/) for movie metadata and artwork.
- [Streamlit](https://streamlit.io/) for the web app framework.
- [FastAPI](https://fastapi.tiangolo.com/) for the backend framework.
- [scikit-learn](https://scikit-learn.org/) for machine-learning utilities.

This project uses TMDB data and imagery. It is not endorsed or certified by TMDB.

---

Made as a movie discovery and recommendation project.
