# 🚀 Telegram Forwarder Bot Platform

A sophisticated, multi-user Telegram bot for orchestrating complex message forwarding jobs — built for reliability, performance, and security.

---
## 📑 Table of Contents

- [✨ Features](#-features)
- [🛠️ Tech Stack](#-tech-stack)
- [⚙️ Getting Started](#️-getting-started)
- [✅ Testing](#-testing)
- [📖 Command Reference](#-command-reference)
- [🤝 Contributing](#-contributing)
- [📜 License](#-license)

---
## ✨ Features

* **Multi-User & Session Management:** Securely handles multiple user accounts with encrypted session storage.
* **Job-Based Configuration:** Create, save, and manage multiple named jobs with unique configurations.
* **Context-Aware Configuration:** Configure job-specific settings with `/settings` or define global defaults with `/defaults`.
* **Flexible Forwarding Modes:** Choose per-job how to forward content:
    * **Fast Forward:** Maximum speed, preserves original metadata with a "forwarded from" tag.
    * **Smart Copy:** The recommended default. Removes the "forwarded from" tag, handles both text and media, and automatically falls back to re-uploading protected content.
    * **Private Re-upload:** Maximum privacy. Downloads and re-uploads every file to strip all metadata and enable advanced features like renaming.
* **Unified Destination Menu:** Choose your upload destination per-job.
    * **Cloud Storage:** Upload directly to **Google Drive** or **Koofr**.
    * **Telegram (Custom):** Send to any channel, group, or topic.
    * **Telegram (Private):** Send directly to your private chat with the bot or to your "Saved Messages".
* **Subscription Tiers:** Flexible plan system (FREE, STANDARD, PRO, POWER) with a unified `/myplan` hub.
* **Centralized Subscription Tiers:** Manages user access to premium features (FREE, STANDARD, PRO, POWER) through the central `@UpgradeNowBot` service.
* **Advanced Captioning Engine:** Dynamically generate custom message captions with prefixes, suffixes, word removal, and powerful regex replacements.
* **Asynchronous Account Verification:** On-demand, non-blocking status checks for external cloud connections from the `/my_account` menu.
* **Persistent Retry Queue (PRQ):** Automatically queues and retries failed messages, ensuring no data loss.
* **Administrative Tooling:** Owner and Admin commands for user management, system monitoring, and broadcasts.

### 3. Configuration Hierarchy

The bot uses a three-tiered configuration system, loaded in the following order of priority (lower numbers override higher ones):

**1. Secrets (`.env` file or Doppler)**
This layer is for secrets and critical bootstrap variables ONLY. It has the highest priority and overrides all other settings.
* **Required:** `API_ID`, `API_HASH`, `BOT_TOKEN`, `DATABASE_URL`, `SESSION_ENCRYPTION_KEY`, etc.
* **Management:** Managed externally via a `.env` file or a secrets manager like Doppler. **Never commit this file.**

**2. Dynamic Settings (Database)**
This is the **primary source of truth for global admin settings**. These are settings that you can change live via the bot's `/admin` panel.
* **Examples:** `ALLOW_UPLOADS`, `ENABLE_HEALTH_CHECK`.
* **Management:** Modified in real-time via the bot's UI. Changes are persistent and take effect immediately without a restart.

**3. Base Defaults (`bot_config.json`)**
This file contains the foundational, non-sensitive default values for the application. It should be considered a fallback and is the lowest priority.
* **Management:** This file is part of the Git repository and should only be changed by developers to set new baseline defaults for features.




---
## 🛠️ Tech Stack

* **Language:** Python 3.11+
* **Core Library:** Telethon (async)
* **Database:** PostgreSQL
* **Cache & State:** Redis
* **Cloud Integration:** `rclone` (as a subprocess)
* **Secrets Management:** Doppler
* **Testing:** Pytest, pytest-asyncio, pytest-mock

---
## ⚙️ Getting Started

### 1. Prerequisites

* Python 3.11 or higher
* The **`rclone`** command-line tool installed and in your system's PATH.
* A running PostgreSQL database.
* (Recommended) A running Redis instance.
* (Recommended) The Doppler CLI.

### 2. Installation

```bash
# Clone the repository
git clone <your-repo-url>
cd telegram_bot_project

# Create and activate a virtual environment (use the standard 'venv' name)
python3 -m venv venv
source venv/bin/activate

# Install the bot and all its dependencies
pip install --upgrade pip
pip install -e .[dev]

3. Configuration
Secrets are managed via Doppler (recommended) or a local .env file. Populate your chosen method with the following variables:
| Variable | Description |
|---|---|
| API_ID | Telegram API ID from my.telegram.org. |
| API_HASH | Telegram API Hash from my.telegram.org. |
| BOT_TOKEN | Bot token from @BotFather. |
| OWNER_TELEGRAM_ID | Numeric Telegram ID of the bot’s owner. |
| OWNER_SESSION_STRING | Telethon session string for the owner’s account. |
| SESSION_ENCRYPTION_KEY | 32-byte Fernet key for encrypting sessions. |
| DATABASE_URL | PostgreSQL connection string. |
| UPSTASH_REDIS_URL | (Optional) Redis instance URL. |
| GDRIVE_CLIENT_ID | (Optional) The public client ID for rclone. |
| GDRIVE_CLIENT_SECRET | (Optional) The public client secret for rclone. |
| THUMBNAIL_STORAGE_CHANNEL_ID | Private channel ID for storing thumbnails. |
| OWNER_USERNAME | (Optional) Owner’s username for payment links. |
| LOG_LEVEL | Logging level (DEBUG, INFO, WARNING). Default: INFO. |
4. Running the Bot
bash run.sh

The bot will connect to PostgreSQL, run migrations, and start automatically.
✅ Testing
The project includes a full test suite.
# Ensure dev dependencies are installed (pip install -e .[dev])

# Set up test database (use a dedicated test DB, not production)
export TEST_DATABASE_URL="postgresql://user:pass@host:port/test_db_name"

# Run tests
pytest

📖 Command Reference
User Commands
| Command | Description |
|---|---|
| /start | Initialize your account with the bot. |
| /help | Show a brief command reference. |
| /features | Display a detailed list of bot features and plan requirements. |
| /myplan | Access the Subscription Hub to view or upgrade your plan. (Aliases: /upgrade, /pay) |
| /my_account | Manage your linked Telegram, Google Drive, and Koofr accounts. Includes a live connection verifier. |
| /status | Show a live, auto-updating status of your active jobs. |
Configuration & Management Commands
| Command | Description |
|---|---|
| /jobs | The main menu for managing your saved job configurations (create, switch, delete). |
| /start_job | Interactively start a saved job configuration. |
| /settings | The main dashboard to configure the active job, including destinations, content, and performance. |
| /defaults | Set global default settings that will apply to all new jobs you create. |
| /prq | Manage the Persistent Retry Queue for failed messages. |
Admin & Owner Commands
| Command | Description | Access |
|---|---|---|
| /system | Access system controls (restart, shutdown, health report). | Admin |
| /admin | Access the user management panel (list, ban, change plan). | Admin |
| /logs | View the live bot activity logs. | Admin |
| /broadcast | Send a message to all active users of the bot. | Owner |
| /setplan | Manually set a user's subscription plan. | Owner |
🤝 Contributing
Contributions are welcome! Please open an issue to discuss bug reports or feature requests, or submit a pull request with a clear description of your changes.
📜 License
This project is licensed under the MIT License.
