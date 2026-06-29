# 💰 IntelliPrice 2.0

AI-powered electronics price comparison web app. Searches multiple e-commerce platforms, groups identical product listings using NLP, scores deals, and predicts future prices using machine learning.

Built as a final year project.

---

## ✨ Features

- **Live web scraping** — Pulls real-time product listings (name, price, MRP, discount %, rating, image) from Amazon India.
- **NLP product matching** — Uses character n-gram TF-IDF + cosine similarity to group near-duplicate listings of the same product (e.g. different colour/storage variants) so prices can be compared fairly.
- **AI deal scoring** — Ranks platforms per product using a weighted score based on price, rating, and discount.
- **Fake discount detection** — Flags listings where the "original price" looks artificially inflated.
- **Price prediction** — Uses `LinearRegression` on historical price data to forecast the next price point, with a sanity cap and a plain-English explanation of the trend.
- **RandomForest deal model** — Trains on scraped listing features to learn a general "deal quality" score.
- **User accounts, wishlist & price alerts** — Sign up, save products, set a target price, and get notified when it's hit.
- **Search history** — Revisit past searches with one click.
- **Interactive dashboard** — Built with Streamlit: sortable/filterable comparison cards, light/dark theme, and a price trend chart.

## ⚠️ A note on data sources

Amazon India is scraped live. **Flipkart, Croma, and Reliance Digital prices are estimated** — derived from the live Amazon price using realistic platform-specific multipliers — whenever live scraping is blocked (which most retailers do aggressively). Every estimated row is explicitly flagged as `is_estimated: true` in the API response and labelled in the UI, so the distinction is never hidden from the user.

## 🏗️ Tech Stack

| Layer | Tech |
|---|---|
| Backend API | FastAPI |
| Frontend | Streamlit |
| Database | SQLite |
| Scraping | Requests + BeautifulSoup |
| NLP | scikit-learn (TF-IDF) |
| ML | scikit-learn (LinearRegression, RandomForestRegressor) |
| Charts | Matplotlib |

## 📁 Project Structure
intelliprice-2.0/

├── backend/

│   ├── main.py        # FastAPI app & all API routes

│   ├── scraper.py     # Live scraping + estimation engine

│   ├── ai_engine.py   # NLP grouping, deal scoring, price prediction

│   └── db.py          # SQLite data layer (users, wishlist, alerts, history)

├── frontend/

│   └── app.py         # Streamlit UI

├── models/

│   └── ml_model.pkl   # Trained RandomForest deal-scoring model

├── logo.png

├── requirements.txt

└── start.bat           # Launches backend + frontend together (Windows)

## 🚀 Getting Started

### Prerequisites
- Python 3.10+

### Installation
```bash
git clone https://github.com/<your-username>/intelliprice-2.0.git
cd intelliprice-2.0
pip install -r requirements.txt
```

### Running locally

**Windows** — just double-click `start.bat`, or run:
```bash
start.bat
```

**Manual / macOS / Linux** — run the backend and frontend in two separate terminals:
```bash
# Terminal 1 — API
python -m uvicorn backend.main:app --host 0.0.0.0 --port 8000 --reload

# Terminal 2 — UI
streamlit run frontend/app.py --server.port 8501
```

Then open:
- App: http://localhost:8501
- API docs: http://localhost:8000/docs

> Note: the SQLite database is created automatically on first run (it's git-ignored, so each clone starts fresh).

## 🔮 How Price Prediction Works

1. Every search records the current price to the database.
2. Once a product has 3+ price points, a `LinearRegression` model fits a trend line.
3. The next price is predicted, capped at ±10% of the current price to avoid unrealistic jumps.
4. The app translates the numeric trend into plain-English buying advice (e.g. "Price is rising — buy now").

## 📝 License

This project was built for academic purposes.
