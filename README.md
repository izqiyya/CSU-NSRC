# CSU NSRC Tracking System
**Centralized Incident Intake, Action Monitoring & External Donation Coordination**

A web-based emergency response and relief management platform built for the **Caraga State University — National Service Reserve Corps (CSU NSRC)** and the seven academic colleges of CSU Main Campus (*CAA, CCIS, CED, CEGS, CHASS, CMNS, and COFES*).

---

## Key Features

### 1. Role-Based Dual Portals
* **CSU NSRC Central Admin (`CSU-NSRC`):**
  * Declare, switch, and close active calamity/emergency incidents (`Active` / `Closed`).
  * Monitor university-wide KPIs and drill down into per-college reports across all **7 CSU colleges**.
  * Update student intervention statuses (*Submitted*, *Received / Verified*, *Action in Progress*, *Assisted / Sheltered*, *Resolved*), assign evacuation centers, and record chronological `action_logs`.
  * Manage **Evacuation Centers** (live occupancy tracking & automatic `Open` / `Near Full` / `Full` thresholds), **External Partner Institutions**, and **Relief Goods Inventory** (`IN` donations & `OUT` distributions with low-stock alerts).
  * Export consolidated incident aftermath reports in **CSV format**.
* **College Focal Person Portals (`CSU-CAA` to `CSU-COFES`):**
  * Submit affected student reports with optional off-campus boarding house details.
  * Automatic case-insensitive duplicate prevention (`UNIQUE(incident_id, student_id)`).
  * Edit submitted records and inspect real-time NSRC response logs (`action_logs`) strictly scoped to their assigned college.

### 2. Live Synchronization & Cloud Resilience
* **15-Second Live Polling:** Automatic dashboard and table synchronization across active browser sessions.
* **Cloud-Ready PostgreSQL Backend:** Configured for persistent deployment on **Render** with SSL-enabled PostgreSQL (`DATABASE_URL`) and automatic schema/account initialization on first startup.

---

## Technology Stack

* **Frontend:** HTML5, CSS3 (Responsive CSS Grid), Vanilla JavaScript (ES6+)
* **Backend:** Python 3.11+, Flask 3.x, Gunicorn
* **Database:** PostgreSQL 16 (via pure-Python `pg8000` driver with custom dict cursor wrapper)
* **Security:** Salted password hashing (`werkzeug.security`), signed HTTP session cookies, parameterized SQL queries, and strict 7-college API authorization

---

## Project Structure

```text
CSU-NSRC/
├── app.py                  # Flask REST API, session auth, PostgreSQL schema & CSV export
├── requirements.txt        # Python dependencies (flask, pg8000, werkzeug, gunicorn)
├── README.md               # Project documentation
├── templates/
│   └── index.html          # Single-page multi-portal UI & modals
└── public/
    └── static/
        ├── css/
        │   └── styles.css  # CSU-themed responsive styling
        └── js/
            └── app.js      # Frontend state, API calls, modals & 15s live polling
```

---

## Default Seeded Accounts

Upon initial startup, `init_db()` automatically seeds the 7 CSU colleges and 8 office accounts:

| Office / College Handle | Role | College Name | Default Password |
| :--- | :--- | :--- | :--- |
| `CSU-NSRC` | `CSU_NSRC_ADMIN` | CSU NSRC Central Command | `csu123` |
| `CSU-CAA` | `COLLEGE_FOCAL` | College of Agriculture and Agri-Industries | `csu123` |
| `CSU-CCIS` | `COLLEGE_FOCAL` | College of Computing and Information Sciences | `csu123` |
| `CSU-CED` | `COLLEGE_FOCAL` | College of Education | `csu123` |
| `CSU-CEGS` | `COLLEGE_FOCAL` | College of Engineering and Geosciences | `csu123` |
| `CSU-CHASS` | `COLLEGE_FOCAL` | College of Humanities and Social Sciences | `csu123` |
| `CSU-CMNS` | `COLLEGE_FOCAL` | College of Mathematics and Natural Sciences | `csu123` |
| `CSU-COFES` | `COLLEGE_FOCAL` | College of Forestry and Environmental Sciences | `csu123` |

> **Security Note:** Change default credentials and set a strong `SECRET_KEY` before production use.

---

## Local Development Setup

1. **Clone the repository:**
   ```bash
   git clone https://github.com/izqiyya/CSU-NSRC.git
   cd CSU-NSRC
   ```

2. **Create and activate a virtual environment:**
   ```bash
   python -m venv venv
   # Windows:
   venv\Scripts\activate
   # macOS / Linux:
   source venv/bin/activate
   ```

3. **Install dependencies:**
   ```bash
   pip install -r requirements.txt
   ```

4. **Configure PostgreSQL & Run:**
   Ensure PostgreSQL is running locally (default DB: `csu_nsrc_db`, user: `postgres`) or set `DATABASE_URL` in your environment:
   ```bash
   python app.py
   ```
   Open `http://127.0.0.1:5000` in your browser.

---

## Deployment on Render

1. **Create a PostgreSQL Database** on [Render](https://dashboard.render.com) and copy its **Internal Database URL**.
2. **Create a Web Service** connected to `izqiyya/CSU-NSRC` with the following settings:
   * **Runtime:** `Python 3`
   * **Build Command:** `pip install -r requirements.txt`
   * **Start Command:** `gunicorn app:app`
3. **Add Environment Variables:**
   * `DATABASE_URL`: *(Your Render PostgreSQL Internal Database URL)*
   * `SECRET_KEY`: *(A strong random secret string)*
4. Click **Deploy Web Service**. The database schema and default office accounts will initialize automatically upon the first request.

---

## License

Developed for academic and institutional disaster response coordination at **Caraga State University (CSU)**. Licensed under the **MIT License**.