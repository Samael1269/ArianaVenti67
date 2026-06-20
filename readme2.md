# Advisor360 AI (Venti.ai)

Advisor360 AI is a modern, high-fidelity platform built for financial advisors and advisory firms. Designed with a striking "Newsprint" Neo-Brutalist aesthetic, the platform integrates live client relationship management (CRM) with smart calendar scheduling, regulatory compliance tracking, and AI-driven intelligence.

---

## 🏛️ System Architecture

The platform operates as a secure, distributed multi-tier architecture:

```text
       ┌────────────────────────────────────────────────────────┐
       │                     React Frontend                     │
       │                   (Vite Dev Server)                    │
       └──────────────────────────┬─────────────────────────────┘
                                  │
                                  ├───────────────┐ (Proxied via Vite Config)
                                  ▼               ▼
                        ┌───────────┐       ┌───────────┐
                        │Node Server│       │Spring Boot│
                        │(Port 3001)│       │(Port 8080)│
                        └─────┬─────┘       └─────┬─────┘
                              │                   │ (Spring Data JPA)
                              ▼                   ▼
                        ┌───────────┐       ┌───────────┐
                        │Google Cal │       │MySQL DB   │
                        │    API    │       │(Port 3306)│
                        └───────────┘       └───────────┘
```

1. **Frontend (Vite + React)**: Serves the visual experience. Styled with a Neo-Brutalist, custom newsprint-inspired design system. 
2. **Node.js Helper Server**: Proxies `/api/calendar` calls to fetch, insert, and delete calendar events via the **Google Calendar API** using Service Account OAuth credentials.
3. **Spring Boot Backend**: Exposes client record management REST endpoints under `/api/clients` on port `8080`.
4. **MySQL Database**: Acts as the system of record for client memory and advisor settings.

---

## 🛠️ Technology Stack

| Layer | Technologies |
| :--- | :--- |
| **Frontend** | React (v19), Vite, Tailwind CSS, Lucide React icons, local storage |
| **Backend API** | Java 17, Spring Boot (v3.5.x), Spring Data JPA, Jakarta Validation, Lombok |
| **Helper Server** | Node.js, Express, CORS, Googleapis v173 |
| **Database** | MySQL / MariaDB (port 3306) |
| **AI Integration**| Google Gemini Pro API, callGrafilab intelligence endpoints |

---

## 🌟 Core Features

1. **Smart Calendar & Schedule AI**
   - Live synchronization with Google Calendar via Service Account keys.
   - Interactive calendar grid allowing advisors to manage scheduling meetings.
   - Conversational schedule AI to add, edit, or remove meetings through chat commands.
   
2. **Client Memory (CRM)**
   - End-to-end client record tracking (name, email, phone, background notes, last contact date) synced directly with MySQL.
   - Opportunities & Vulnerability Index: AI analyzes stored notes to suggest custom opportunities and warning indicators.
   - AI Meeting Summarizer: Paste raw meeting logs to extract structured summaries and follow-ups.
   - Interactive local timeline storage mapped by database ID to log historical interactions.

3. **Learning Hub & CPD Tracker**
   - Tracks continuing professional development (CPD) points against compliance targets.
   - Interactive knowledge center for regulatory, tax, and compliance topics.
   - AI Regulatory Inquiry Desk: Conversational assistant answering compliance questions based on library materials.
   - Custom publishing form to add new literature directly to the curriculum.

4. **Partner Finder**
   - Directory of trusted tax, legal, and mortgage professionals.
   - AI Referral recommendation matching engine based on client requirements.

---

## 🚀 Getting Started & Local Setup

Ensure you have **Node.js**, **Java 17 (JDK)**, and **MySQL** (such as via XAMPP) installed on your system.

### Step 1: Database Setup
1. Start your MySQL Server (via XAMPP, Docker, or local Windows Service) on port `3306`.
2. Create a new database named:
   ```sql
   CREATE DATABASE advisor360_db;
   ```
3. The Spring Boot backend uses `spring.jpa.hibernate.ddl-auto=validate`. Ensure you run schema creation scripts first or configure Spring Boot to generate the tables (`update` or `create`).

### Step 2: Run the Spring Boot Backend
1. Open a terminal at your Spring Boot project directory (`C:\Users\ASUS\Desktop\ArianaVenti67`).
2. Run the application using the Maven wrapper:
   - **On Linux/macOS:**
     ```bash
     ./mvnw spring-boot:run
     ```
   - **On Windows:**
     ```cmd
     mvnw.cmd spring-boot:run
     ```
   *(Or import the project into IntelliJ IDEA and run `Advisor360Application.java` directly).*
3. The server runs at: **`http://localhost:8080`**

### Step 3: Configure Calendar Credentials (Optional)
If you want to sync meetings with Google Calendar:
1. Place your Service Account credentials inside `credentials.json` in the frontend root directory (`c:\Users\ASUS\Desktop\advisor360-ai\credentials.json`).
2. Update the target calendar ID inside `server.js` (`const TARGET_CALENDAR_ID = "your-email@gmail.com"`).

### Step 4: Run the Frontend & Node Helper
1. Open a terminal at the React project directory (`c:\Users\ASUS\Desktop\advisor360-ai`).
2. Install dependencies:
   ```bash
   npm install
   ```
3. Run the development server:
   ```bash
   npm run dev
   ```
   *(This starts concurrently both the Node calendar server on port `3001` and the Vite dev server on port `5173`).*
4. Navigate to **`http://localhost:5173/`** in your browser.

> [!NOTE]
> **Windows Script Execution Restrictions**
> If you get a script execution policy error while running `npm run dev` on Windows PowerShell, run the following bypass command:
> ```powershell
> powershell -ExecutionPolicy Bypass -Command "npm run dev"
> ```
> 
# Advisor360 AI — Telegram Bot Integration (Venti.ai)

Advisor360 AI Telegram Bot is a high-availability, asynchronous communication layer built for financial advisors. It acts as the customer-facing bridge to the Advisor360 AI dashboard, allowing clients to inquire about services, log conversations, and instantly book calendar appointments that sync right onto the advisor's central schedule. Powered by Grafilab's Qwen-3.6-Flash model, the bot parses open-ended natural language into structured, executable calendar inputs.

---

## 🏛️ System Architecture

The Telegram Bot integrates natively into the wider Advisor360 AI eco-system:

```
┌────────────────────────────────────────────────────────┐
│                      Telegram Client                   │
│                     (User Interaction)                 │
└──────────────────────────┬─────────────────────────────┘
                           │
                           ▼
                    ┌──────────────┐
                    │  Telegram    │
                    │  Bot API     │
                    └──────┬───────┘
                           │
                           ▼
                    ┌──────────────┐       ┌────────────────────┐
                    │ Python Bot   │◄─────►│ Grafilab LLM API   │
                    │ Engine       │       │ (Qwen-3.6-Flash)   │
                    └──────┬───────┴───────└────────────────────┘
                           │
            ┌──────────────┴──────────────┐
            ▼                             ▼
     ┌──────────────┐              ┌──────────────┐
     │  SQLite DB   │              │ Google Cal   │
     │(Interactions)│              │     API      │
     └──────────────┘              └──────────────┘

```

* **Telegram Bot Engine (`bot.py`)**: Asynchronously listens for messages, managing states and interactions.
* **Grafilab LLM Engine**: Employs `grafilab/qwen3.6-flash` over an OpenAI-compatible endpoint to extract dates, intents, and client requirements.
* **Google Calendar API**: Books and patches appointment intervals directly using shared Service Account access tokens.
* **SQLite Database**: Serves as the localized interaction layer, logging historical data points to keep information aligned with the core dashboard tracking.
* **Follow-up Engine (`followup.py`)**: Background cron engine evaluating dormant leads to trigger smart re-engagement outreach.

---

## 🛠️ Technology Stack

| Layer | Technologies |
| --- | --- |
| **Bot Framework** | Python, `pyTelegramBotAPI` (Async runtimes) |
| **AI Parsing Engine** | OpenAI Python SDK, Pydantic (Structured Outputs), Grafilab API |
| **Automation & Sync** | `google-api-python-client`, `google-auth`, `schedule` |
| **Local Storage** | SQLite 3 |
| **Containerization** | Docker (Multi-stage production build) |

---

## 🌟 Core Features

* **Natural Language Appointment Booking**: Clients talk to the bot casually (e.g., *"Can we meet tomorrow at 3 PM?"*). The AI handles time zone parameters and registers the event instantly.
* **Automated Calendar Synchronization**: Direct server-to-server interaction with the Advisor's calendar using Service Account keys—no client-side Google auth loops required.
* **Local CRM Interaction Logging**: Captures chat entries locally into `interactions.db`, tracking conversation states, background notes, and last contact timestamps.
* **Proactive Client Re-engagement**: A dedicated background process (`followup.py`) scans for idle users and issues smart context-aware follow-ups automatically.

---

## 🚀 Getting Started & Local Setup

Ensure you have Python 3.10+, Docker (Optional), and a Google Cloud account ready.

### Step 1: External Services & Account Setup

1. **Telegram Bot Token**: Message `@BotFather` on Telegram, run `/newbot`, and save your provided HTTP API Token.
2. **Google Calendar API Key**:
* Create a project in the Google Cloud Console and enable the **Google Calendar API**.
* Create a **Service Account** under *IAM & Admin*, and generate a new **JSON Key**.
* Save this downloaded file as `credentials.json` in the root of your project directory.
* Copy the `client_email` string inside that JSON, open your target Google Calendar settings in a browser, and share the calendar with that email address with full **"Make changes to events"** permissions.


3. **Grafilab API Key**: Generate an API key inside your Grafilab Cloud Console.

### Step 2: Project Layout Review

Ensure your project files match the structure below:

```plaintext
├── bot.py                # Main interactive Telegram Bot loop
├── followup.py           # Background cron worker for client re-engagement
├── Dockerfile            # Multi-stage production deployment configuration
├── requirements.txt      # Python dependencies manifest
├── credentials.json      # Google Service Account Key (Configured in Step 1)
└── interactions.db       # SQLite Database (Auto-generated on initialization)

```

Your `requirements.txt` must contain:

```plaintext
pyTelegramBotAPI
openai
pydantic
google-auth
google-auth-oauthlib
google-api-python-client
schedule

```

### Step 3: Run the Application

#### Option A: Local Native Deployment (Python)

1. **Set Environment Variables**:
* **Windows (PowerShell):**
```powershell
$env:TELEGRAM_TOKEN="your_telegram_bot_token_here"
$env:GRAFILAB_API_KEY="your_grafilab_api_key_here"

```





```
   * **Linux/macOS:**
     ```bash
     export TELEGRAM_TOKEN="your_telegram_bot_token_here"
     export GRAFILAB_API_KEY="your_grafilab_api_key_here"

```

2. **Install and Run**:
```bash
python -m venv venv
source venv/bin/activate  # On Windows use: venv\Scripts\activate
pip install -r requirements.txt

# Start the interactive bot
python bot.py

```



```

3. **Start the Follow-up Engine** (In a separate terminal instance with the same environment variables):
   ```bash
python followup.py

```

#### Option B: Containerized Deployment (Docker)

1. **Build the Image**:
```bash

```



docker build -t telegram-booking-bot .

```

2. **Run the Interactive Bot Container**:
   ```bash
   docker run -d \
     --name booking-bot-instance \
     -e TELEGRAM_TOKEN="your_telegram_bot_token_here" \
     -e GRAFILAB_API_KEY="your_grafilab_api_key_here" \
     -v $(pwd)/credentials.json:/app/credentials.json \
     -v $(pwd)/interactions.db:/app/interactions.db \
     telegram-booking-bot

```

3. **Run the Background Follow-up Worker Container**:
```bash
docker run -d \
  --name fallback-worker-instance \
  -e TELEGRAM_TOKEN="your_telegram_bot_token_here" \
  -e GRAFILAB_API_KEY="your_grafilab_api_key_here" \
  -v $(pwd)/interactions.db:/app/interactions.db \
  telegram-booking-bot python followup.py

```



```

---

> 🔍 **Verification & Testing Note**
> Open your bot on Telegram and type `/start`. Send an unstructured request like: *"Hey, can I book a session for tomorrow afternoon at 3 PM to look over my policy variables?"* The AI will parse the intent, reply with a structured confirmation layout, and instantly append the item to your central Advisor360 dashboard calendar.

```
