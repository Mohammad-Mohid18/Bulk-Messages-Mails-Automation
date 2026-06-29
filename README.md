# 🤖 Bulk Messaging Automation System

A Python-based automation toolkit for sending bulk messages via **WhatsApp Web** and **Gmail** using keyboard & mouse automation (`pyautogui`) and direct SMTP.

---

## 📁 Project Structure

```
Auto_(1).ipynb        ← Main notebook with all automation systems
DemoCon_numbers.xlsx  ← WhatsApp contacts (phone numbers)
test_contacts.csv     ← Gmail contacts (email addresses)
not_found.png         ← Reference image for WhatsApp "no results" detection
```

---

## 📦 Requirements

```bash
pip install pyautogui keyboard pandas openpyxl
```

For SMTP email sending (no extra install needed — built into Python):
```python
import smtplib  # already in Python standard library
```

---

## 🟢 System 1 — WhatsApp Web Bulk Messaging

### How it works
1. Reads phone numbers from `DemoCon_numbers.xlsx`
2. Opens WhatsApp Web search bar and types each number
3. If contact is not found (detected via image recognition), skips it
4. Opens the chat, types the message in multiple parts, and sends it
5. Moves to the next contact and repeats

### Setup
- Open **WhatsApp Web** in your browser and make sure you are logged in
- Run the notebook and switch focus to the browser within the 10 second sleep
- Make sure `not_found.png` exists — a cropped screenshot of WhatsApp's "No chats, contacts or messages found" text

### Screen Coordinates (update if your screen resolution differs)
| Element | X | Y |
|---------|---|---|
| Search bar | 713 | 168 |
| First search result | 456 | 532 |
| Message input bar | 1064 | 1017 |

To find coordinates on your screen:
```python
import pyautogui, time
time.sleep(3)  # move mouse to target within 3 seconds
print(pyautogui.position())
```

### Phone Number Format
Numbers must be in international format e.g. `+923001234567`.  
A utility cell in the notebook converts Pakistani numbers from `03XX` → `+923XX` format automatically.

### Emergency Stop
- Move mouse to **top-left corner** of screen (pyautogui FailSafe)

---

## 📧 System 2 — Gmail Bulk Email Sender

Three methods available — use whichever fits your needs:

### Method A — Web UI (One email per contact) ✅ Recommended for small lists
- Automates Gmail's Compose window for each contact individually
- No BCC — each person gets a clean individual email
- Rate: 1 email per 5 seconds to avoid Gmail rate limiting
- **Limit: 500 emails/day** on free Gmail

### Method B — Web UI (BCC batches)
- Adds up to 10 contacts per BCC batch
- Opens one compose window per batch
- **Known issue:** Gmail Web UI reliably accepts only ~10 BCC entries via automation

### Method C — SMTP (Recommended for large lists) ✅ Most reliable
- Sends directly through Gmail's SMTP server
- No browser required — runs entirely in background
- Much faster than UI automation
- **Limit: 500 emails/day** on free Gmail

#### SMTP Setup — Gmail App Password
1. Go to **myaccount.google.com**
2. Enable **2-Step Verification** under Security
3. Go to **myaccount.google.com/apppasswords**
4. Create a new App Password (name it anything e.g. `BulkMailer`)
5. Copy the 16-character password and paste it into the notebook:
```python
your_app_password = "abcd efgh ijkl mnop"
```

### Gmail Rate Limits
| Limit | Value |
|-------|-------|
| Free Gmail daily cap | 500 emails/day |
| Recommended send rate | 1 email per 5 seconds |
| Safe batch pause | 60 seconds every 50 emails |
| Error #2008 | Rate limit hit — slow down or wait until next day |

---

## ⏹️ How to Stop the Automation

| Method | How |
|--------|-----|
| WhatsApp automation | Move mouse to **top-left corner** of screen |
| Gmail Web UI automation | Click **⬛** in VS Code toolbar OR press **I, I** on the cell |
| SMTP automation | Press **I, I** on the cell in Jupyter/VS Code |

---

## 📝 CSV Format for Gmail

```csv
email
user1@gmail.com
user2@gmail.com
user3@gmail.com
```

Column header must be exactly `email`.

---

## ⚠️ Important Notes

- Keep your screen **unlocked and active** during Web UI automation
- Do **not** move your mouse during automation (except to top-left to stop)
- Gmail's **500/day limit** applies across all sending methods
- WhatsApp may temporarily restrict accounts sending too many messages too fast
- All phone numbers must be saved as contacts in WhatsApp for the search to find them
