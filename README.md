# 🌍 Smart Travel Planner

An AI-powered web application built for the **Rev Hackathon** that simplifies trip planning. It generates personalized, day-by-day itineraries, interactive route maps, live budget breakdowns, and accommodation recommendations based on user destination, budget, style, and interests.

---

## ✨ Key Features

- **Day-Wise Itinerary Generation:** Structured morning, afternoon, and evening activity schedules with time estimates, activity expenses, and transit times.
- **Dual-Engine Architecture:** Uses OpenRouter (`openai/gpt-4o-mini`) when an API key is provided, or seamlessly switches to an offline Smart Knowledge Engine with curated destination data (Goa, Manali, Dandeli) and heuristic fallbacks.
- **Live Budget Distribution:** Real-time budget allocation across Stay, Food, Transport, Activities, and Misc based on travel style and destination tier.
- **Interactive Split-Screen Maps:** Leaflet.js map with geocoded pins, Haversine route distance calculations, and direct Google Maps navigation links.
- **Curated Accommodation:** Hotel recommendations categorized by budget tier with estimated price ranges (in ₹), ratings, and booking search links.
- **Real-Time Autocomplete:** Debounced destination search powered by the Photon (OpenStreetMap) geocoder with local search history.
- **Integrated Travel Tools:** Live multi-day weather forecasts (Open-Meteo), an interactive packing checklist, travel notes scratchpad, and local emergency contacts.

---

## 🛠️ Technology Stack

- **Backend:** Python 3, Flask, Requests, `concurrent.futures` (ThreadPoolExecutor)
- **Frontend:** HTML5, CSS3 (Modern Glassmorphic UI), Vanilla JavaScript (ES6+)
- **Mapping & Geocoding:** Leaflet.js (v1.9.4), OpenStreetMap Carto tiles, Photon API, Nominatim
- **AI & Data Services:** OpenRouter API (`openai/gpt-4o-mini`), Wikipedia REST API, Wikimedia Commons, Open-Meteo Weather API
- **Storage:** Backend in-memory LRU caches, browser `localStorage` & `sessionStorage`

---

## 🚀 How to Run Locally

### 1. Clone the Repository
```bash
git clone https://github.com/Goniswamy/rev_hack.git
cd rev_hack
```

### 2. Set Up Virtual Environment & Dependencies
```bash
python3 -m venv venv
source venv/bin/activate
pip install flask requests
```

### 3. Launch the Application
```bash
cd "hack cipher"
python3 app.py
```

Open **http://localhost:5001** in your browser.

---

## 📂 Project Structure

```text
rev_hack/
├── .gitignore
├── README.md
└── hack cipher/
    ├── app.py              # Flask server entry point & REST API routes
    ├── config.py           # App configuration and environment variable loading
    ├── services/
    │   ├── ai_service.py   # OpenRouter LLM client & Smart Knowledge Engine
    │   ├── geo_service.py  # Autocomplete, geocoding & multithreaded enrichment
    │   └── image_service.py# Wikipedia/Wikimedia image fetching with fallbacks
    ├── static/
    │   ├── script.js       # Client logic, Leaflet map management & UI state
    │   └── style.css       # Responsive styling & animations
    └── templates/
        └── index.html      # Single-page web interface
```

---

## ⚙️ Configuration & Environment Variables

The application runs **out-of-the-box with zero configuration** using the offline Smart Knowledge Engine. To enable online AI features, set environment variables:

| Variable | Required? | Default | Description |
| :--- | :--- | :--- | :--- |
| `OPENROUTER_API_KEY` | Optional | `""` | OpenRouter API key for live LLM planning (`openai/gpt-4o-mini`). |
| `PORT` | Optional | `5001` | Local server port. |
| `FLASK_DEBUG` | Optional | `True` | Flask debug/reloader mode (`True`/`False`). |

To supply an API key in Linux/Ubuntu:
```bash
export OPENROUTER_API_KEY="your-api-key"
```

---

## 🔮 Future Improvements (Planned)

- **Transit Integration:** Connect live train and flight booking APIs (e.g., IRCTC, Skyscanner).
- **User Accounts & Cloud Sync:** Multi-user authentication backed by a database (PostgreSQL/SQLite) to save itineraries across devices.
- **Collaborative Planning:** Real-time shared trip editing and voting using WebSockets.
- **Offline PWA Support:** Service worker implementation with cached vector tiles for low-connectivity travel.

