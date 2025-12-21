# 🦅 GSOC Watchdog
### The Automated "Good First Issue" Hunter for GSOC Aspirants

![Project Screenshot](/image.png)


## 💡 The Inspiration
As a Computer Science student preparing for **Google Summer of Code (GSOC) 2026**, I realized the biggest pain point is finding "Good First Issues" before other contributors claim them. Manually refreshing GitHub pages is inefficient and distracting.

I built **GSOC Watchdog** to solve this. It is a "set it and forget it" backend agent that works for me while I sleep.

## 🚀 What It Does
GSOC Watchdog is a serverless backend workflow built on **Motia**.
1.  **Scans:** Every 30 minutes, it wakes up and queries the GitHub API for specific repositories (e.g., VS Code, FastAPI).
2.  **Filters:** It intelligently looks for open issues tagged `good first issue`.
3.  **Alerts:** When a fresh issue is found, it triggers an event that logs the details immediately (and can be extended to send Discord/Email notifications).

## 🛠️ Tech Stack
* **Language:** Python 3.13
* **Framework:** [Motia](https://motia.dev) (Backend Orchestration)
* **APIs:** GitHub REST API
* **Libraries:** `requests`

## ⚡ How It Uses Motia
This project demonstrates the power of Motia's event-driven architecture by decoupling "Finding" from "Acting".

* **Step 1: The Hunter (`gsoc_hunter_step.py`)**
    * **Type:** `cron` (Scheduled Task)
    * **Logic:** specific Python logic to fetch and parse GitHub JSON data.
    * **Motia Feature:** Uses `context.emit()` to broadcast a "found_issue" event only when relevant data appears.

* **Step 2: The Notifier (`printer_step.py`)**
    * **Type:** `event` (Listener)
    * **Logic:** Listens purely for the "found_issue" event.
    * **Why this matters:** This separation of concerns means I can easily add a "Send to Discord" step later without breaking the scanning logic.

## 📦 How to Run Locally

1.  **Clone the Repo**
    ```bash
    git clone <your-repo-url>
    cd gsoc-watchdog
    ```

2.  **Install Dependencies**
    ```bash
    npm install       # Installs Motia
    npx motia install # Installs Python requirements (requests)
    ```

3.  **Run the Server**
    ```bash
    npx motia dev
    ```

4.  **See it in Action**
    * Open `http://localhost:3000`
    * Click "Trigger" on the **GSOC-Hunter** step.
    * Watch the logs light up with fresh issues!

## 🔮 Future Roadmap
* **Discord Integration:** Connect the `printer_step` to a Discord Webhook.
* **Multi-Repo Support:** Allow users to pass a list of 10+ organizations to watch.
* **AI Filtering:** Use an LLM step to analyze if the issue is *actually* easy or just mislabeled.

---
*Built for MotiaHack25 Backend Reloaded.*
