# 🌍 Payana One

> **Your journey, planned smarter.**
> Built for the **REVHACK Hackathon**.

Payana One is an intelligent, full-featured smart travel planning application designed to simplify how travelers research, organize, and experience their trips. By combining AI-driven day-wise itinerary generation, interactive Leaflet route mapping, dynamic budget allocation, curated accommodation discovery, destination visual highlights, and offline travel utilities into a single glassmorphic interface, Payana One transforms trip planning from a fragmented chore into a seamless journey.

---

## 🔗 Quick Links

- **Live Web Application:** [live link](https://payana-one.onrender.com)
- **GitHub Repository:** [repo](https://github.com/Goniswamy/rev_hack)

---

## ✨ Key Features (Implemented)

### 🗓️ Smart Day-Wise Itinerary Planning
- Generates balanced schedules organized into **Morning 🌅**, **Afternoon ☀️**, and **Evening 🌙** time slots.
- Every activity includes operational hours, estimated time spent, individual slot expense, and transit time to the next destination.
- Enriched with recommended local cuisine spots, optimal transit modes (metro, taxi, auto-rickshaw), tags, and destination-specific tips.
- Expandable and collapsible day cards with interactive accordion navigation.

### 🧠 Dual-Engine Architecture (Online AI + Offline Engine)
- **Online Mode:** Powered by OpenRouter (`openai/gpt-4o-mini`) when an API key is provided, generating highly context-aware, customized travel plans.
- **Offline Smart Knowledge Engine:** Runs **100% offline with zero configuration** when no API key is present or in case of network timeouts. Includes rich, verified hand-curated travel databases for **Goa**, **Manali**, and **Dandeli** (attractions, verified hospitals, safety guidelines, and emergency contacts), alongside intelligent heuristic synthesis for any destination globally.

### 💰 Live Budget Distribution & Dynamic Preview
- Live calculation allocating budgets across **Stay**, **Food**, **Transport**, **Activities**, and **Miscellaneous** based on duration, budget amount, travel style (**Budget**, **Mid-range**, **Luxury**), and destination tier (Metro vs. Scenic Tourist Center).
- Real-time debounced preview on the input form as you type budget, days, or style.
- Visual progress meters and category breakdowns inside the trip dashboard tools tab.

### 🗺️ Interactive Split-Screen Route Maps
- Embedded **Leaflet.js** map powered by OpenStreetMap Carto tiles.
- Attractions marked with custom, category-colored pins (🏖️ Beach, 🏛️ Monument, 🛕 Temple, 🌿 Nature, 🛍️ Market, 🎨 Museum).
- Connected route polylines with **Haversine geodesic distance calculation** showing total route mileage and estimated commute duration.
- Interactive popups with attraction imagery, visit durations, timings, and direct one-click **Google Maps navigation links**.
- Selecting a day in the timeline automatically pans and zooms the map to that day's destination.

### 🏨 Curated Stays & Accommodation Finder
- Categorized hotel recommendations divided into **Matching Stays** (based on selected travel style) and **Budget Alternatives**.
- Displays estimated price ranges in INR (₹), star ratings, distance from city center or attractions, and descriptive highlights.
- In-browser filtering by price tier (Budget, Mid-Range, Luxury), minimum star rating, and maximum distance.
- Direct links to view hotel locations on Google Maps and search live booking availability. *(Note: Hotel prices and ratings are curated estimates, not direct real-time transaction bookings).*

### 📍 50 km Nearby Attractions Discovery
- Surfaces real attractions located within a 50 km radius of the destination.
- Filter nearby sights by radius (10 km, 25 km, 50 km) and category (Nature, Adventure, Historical, Religious, Food, Relaxation).
- One-click **"Add to Map"** capability dynamically inserts newly discovered spots into the Leaflet route map.

### 📸 Multi-Tiered Image Retrieval Pipeline
- High-resolution place imagery fetched through an asynchronous multi-strategy pipeline:
  1. **Wikipedia REST Page Summary API** (primary, verified article imagery).
  2. **Wikimedia Commons Search API** (secondary, community media library).
  3. **Curated Unsplash Thematic Photography** (reliable fallback CDN categorized by query type: beach, mountain, heritage, etc.).
- Handled through an in-memory cached Flask proxy endpoint (`/image-proxy`) with 302 redirects.

### 🧰 Integrated Travel Utilities & Local Toolkit
- **Live Weather Forecast:** Multi-day weather forecasts powered by the Open-Meteo API with condition codes, icons, and daily high/low temperatures.
- **Interactive Packing Checklist:** Pre-populated checklist with item addition, completion checkmarks, and deletion, persisted across reloads via browser `localStorage`.
- **Travel Notes Scratchpad:** In-app notepad for jotting down travel thoughts, reminders, and reservation notes.
- **Local Emergency Directory:** Instant access to dispatch contacts (Police, Ambulance, Tourist Helpline, Universal 112) and verified local hospitals with names, addresses, and phone numbers.
- **Favorites Bookmarking:** Toggle heart icons on map markers to collect favorite spots in a dedicated list.

### ⚡ Autocomplete, Persistence & Export
- **Debounced Geocoding Autocomplete:** Location search powered by the Photon (Komoot / OpenStreetMap) geocoder with contextual icons (🏙️ City, 🗺️ Region, 🌍 Country).
- **Recent Searches & Trending Strip:** Quick-select carousel of trending destinations with one-click form auto-fill and local search history.
- **Trip Persistence:** Complete trip state (inputs, generated plan, geocoded coordinates, active tab) preserved in `sessionStorage` across browser reloads.
- **Export & Share Options:** Save trips to local history, download complete plans as formatted JSON, export/print to PDF via print stylesheets, or copy shareable trip summaries to clipboard.

---

## 🛠️ Technology Stack & External APIs

| Layer | Technology | Purpose |
| :--- | :--- | :--- |
| **Backend Framework** | Python 3, Flask | Core server, REST API routing, request handling |
| **WSGI Server** | Gunicorn | Production-grade WSGI HTTP server for Render deployment |
| **Concurrency** | `concurrent.futures.ThreadPoolExecutor` | Parallel geocoding and image enrichment for fast responses |
| **Frontend UI** | HTML5, CSS3, Vanilla JavaScript (ES6+) | Modern glassmorphic responsive interface; zero heavy frontend frameworks |
| **Interactive Maps** | Leaflet.js (v1.9.4) | Interactive route mapping, custom div-icon markers, polyline rendering |
| **Map Tiles** | OpenStreetMap Carto | Free, open-source cartographic map tiles |
| **Geocoding & Autocomplete** | Photon API (Komoot / OSM) & Nominatim | Fast location autocomplete and coordinates lookup |
| **AI Planning Service** | OpenRouter API (`openai/gpt-4o-mini`) | Generative day-wise itinerary and budget split synthesis |
| **Weather Service** | Open-Meteo API | Free multi-day weather forecasts and temperature metrics |
| **Imagery Pipeline** | Wikipedia REST API & Wikimedia Commons | Authentic destination and attraction photo resolution |
| **Fallback Photography** | Unsplash CDN | Curated high-resolution fallback photos for destinations and stays |
| **Client Storage** | `localStorage` & `sessionStorage` | Client-side persistence for trips, packing items, notes, and search history |
| **Server Cache** | In-memory LRU-trimmed dictionaries | Bounded caching for autocomplete, geocoding, plans, and image queries |

---

## 🔄 How the Application Works (User Flow)

```text
[ User Inputs Parameters ]
(Destination, Start Point, Days, Budget, Travel Style, Interests)
            │
            ├─► Real-time Photon Autocomplete & Recent Search History
            └─► Debounced /budget-preview calculates dynamic allocation
            │
[ User clicks "Generate Plan" ]
            │
            ▼
    Is API Key configured?
       ├── YES ──► OpenRouter API (openai/gpt-4o-mini)
       └── NO  ──► Offline Smart Knowledge Engine (Goa/Manali/Dandeli KB or Heuristic Synthesis)
            │
            ▼
[ Multithreaded Parallel Enrichment (ThreadPoolExecutor) ]
       ├── Coordinates: Photon Geocoder / Nominatim Fallback
       └── High-Res Photos: Wikipedia REST ──► Wikimedia Commons ──► Thematic Unsplash
            │
            ▼
[ Fullscreen Immersive Dashboard ]
       ├── Split-Screen: Day-by-Day Itinerary Accordion + Leaflet Route Map (Haversine Distance)
       ├── Hotels Tab: Curated stays with filters & booking discovery links
       ├── Gallery Tab: Destination visual highlights
       └── Tools Tab: Weather forecast, Packing checklist, Notes, Emergency contacts, Budget tracker
            │
            ▼
[ Export / Save ]
       ├── Local History Storage (localStorage)
       ├── Download JSON Itinerary
       ├── Print / Export to PDF
       └── Shareable Clipboard Summary
```

---

## 📂 Project Structure

```text
rev_hack/
├── .gitignore                      # Git ignore file (virtual environments, cache, logs)
├── README.md                       # Project documentation
└── hack cipher/                    # Main application directory
    ├── app.py                      # Flask application entry point & REST API routes
    ├── config.py                   # Environment configuration, port, debug mode & cache limits
    ├── requirements.txt            # Python dependencies (Flask, requests, gunicorn)
    ├── services/                   # Application service modules
    │   ├── ai_service.py           # OpenRouter LLM client & Offline Smart Knowledge Engine
    │   ├── geo_service.py          # Photon autocomplete, Nominatim geocoding & parallel enrichment
    │   └── image_service.py        # Multi-tiered Wikipedia/Wikimedia/Unsplash image pipeline
    ├── static/                     # Frontend static assets
    │   ├── script.js               # Client controller, Leaflet map management & UI state
    │   └── style.css               # Glassmorphic CSS design system, responsive layout & print styles
    └── templates/                  # Server-rendered HTML templates
        └── index.html              # Single-page application shell and result overlays
```

---

## ⚙️ Environment Variables & Configuration

Payana One is designed to run **out-of-the-box with zero configuration**. Supplying an LLM API key is entirely optional; if omitted, the application automatically uses its built-in Smart Knowledge Engine.

| Variable | Required? | Default | Description |
| :--- | :--- | :--- | :--- |
| `OPENROUTER_API_KEY` | Optional | `""` | OpenRouter API key for online LLM trip generation (`openai/gpt-4o-mini`). |
| `GEMINI_API_KEY` | Optional | `""` | Optional fallback key checked during engine readiness detection. |
| `OPENAI_API_KEY` | Optional | `""` | Optional fallback key checked during engine readiness detection. |
| `PORT` | Optional | `5001` | Server listening port (automatically populated by Render in production). |
| `FLASK_DEBUG` | Optional | `True` | Enables Flask debug and live reloader mode (`True`/`False`). |

> [!TIP]
> **Safe API Key Configuration:** Never hardcode API keys or commit secrets to Git. Always pass keys via shell environment variables or a local `.env` file (which is excluded by `.gitignore`).

---

## 🚀 Local Installation & Setup (Ubuntu / Linux)

### 1. Prerequisites
Ensure you have Python 3.8+ and `git` installed:
```bash
python3 --version
git --version
```

### 2. Clone the Repository
```bash
git clone https://github.com/Goniswamy/rev_hack.git
cd rev_hack
```

### 3. Create and Activate a Virtual Environment
```bash
python3 -m venv venv
source venv/bin/activate
```

### 4. Install Dependencies
Install the required packages using the official requirements file:
```bash
pip install -r "hack cipher/requirements.txt"
```

### 5. (Optional) Set Environment Variables
To enable online AI planning with OpenRouter:
```bash
export OPENROUTER_API_KEY="your-openrouter-api-key"
```
*(If skipped, the application seamlessly runs in offline mode using the Smart Knowledge Engine).*

### 6. Run the Application

#### Option A: Running with Flask Development Server
```bash
cd "hack cipher"
python3 app.py
```

#### Option B: Running with Gunicorn WSGI Server (Production-Style)
```bash
cd "hack cipher"
gunicorn -w 4 -b 0.0.0.0:5001 app:app
```

### 7. Access the Application
Open your web browser and navigate to:
```text
http://localhost:5001
```

---

## ☁️ Deployment Guide (Render)

Payana One is pre-configured for deployment as a Web Service on [Render](https://render.com).

### Render Service Settings

- **Environment:** `Python 3`
- **Build Command:**
  ```bash
  pip install -r "hack cipher/requirements.txt"
  ```
- **Start Command:**
  ```bash
  gunicorn --chdir "hack cipher" app:app
  ```
  *(Alternatively, if setting the Render **Root Directory** to `hack cipher`, use Build Command: `pip install -r requirements.txt` and Start Command: `gunicorn app:app`).*

### Recommended Environment Variables on Render
Configure these in the Render Dashboard under **Environment**:

| Key | Value | Note |
| :--- | :--- | :--- |
| `FLASK_DEBUG` | `False` | Disables debug mode in production |
| `PYTHON_VERSION` | `3.10.12` (or your Python version) | Ensures consistent runtime environment |
| `OPENROUTER_API_KEY` | `your-api-key-here` | Optional: Enables online AI generation |

*(Note: Render automatically sets the `PORT` variable, and `config.py` binds to it automatically).*

---

## 🔍 Implementation Notes & Current Limitations

To provide complete transparency for judges and contributors:

1. **Estimated Pricing & Hotel Recommendations:** Hotel options, price ranges (in ₹), and ratings are curated estimates or algorithmic syntheses designed for budgeting and planning guidance. Booking buttons redirect to targeted Google Search queries for reservation discovery rather than completing live GDS/OTA financial transactions.
2. **Transit Distance & Duration:** Route distances are computed via the geodesic Haversine formula between geocoded attraction coordinates, with travel durations calculated using an average urban transit speed (35 km/h) plus congestion buffers. It does not integrate live train, bus, or airline ticketing systems.
3. **Client-Side Storage vs. Cloud Database:** The current implementation uses browser `localStorage` and `sessionStorage` for saving trips, notes, packing items, and search history, combined with bounded server-side in-memory LRU caches. It does not include a centralized SQL/NoSQL database or user authentication system.
4. **Offline Knowledge Base Scope:** The offline Smart Knowledge Engine contains comprehensive, verified attraction, hospital, and emergency data for **Goa**, **Manali**, and **Dandeli**. For other destinations, it relies on an intelligent algorithmic fallback generator unless connected to an online LLM API key.

---

## 🔮 Future Improvements & Roadmap

- [ ] **Persistent Database & User Accounts:** Integrate PostgreSQL/SQLite with SQLAlchemy to support multi-device user authentication and saved itinerary syncing.
- [ ] **Live Ticketing & Transit APIs:** Connect flight and rail reservation aggregators (e.g., Skyscanner, IRCTC) for real-time fares and booking.
- [ ] **Collaborative Planning:** Add WebSockets for real-time multiplayer itinerary editing and voting among travel groups.
- [ ] **Progressive Web App (PWA):** Implement service workers with cached vector map tiles for offline mobile travel support in remote regions.
- [ ] **Multi-Currency & Internationalization:** Support real-time currency conversions (USD, EUR, GBP) and multi-lingual itinerary generation.

