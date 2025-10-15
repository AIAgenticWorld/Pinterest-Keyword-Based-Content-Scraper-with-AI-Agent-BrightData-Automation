<img width="710" height="386" alt="image" src="https://github.com/user-attachments/assets/0d50bfba-e28f-471d-bc0b-ec4e4762ccce" />

# 🧲 Pinterest Scraper Workflow (AI-Powered)

This is an automated Pinterest scraping pipeline powered by Claude Sonnet 4 and BrightData, with results stored in Google Sheets. Designed for efficiency, the workflow is triggered by keyword input and fully managed by AI and automation components.

---

## 🧠 Architecture Components

### AI-Powered Controller
- **Claude Sonnet 4**: Processes and understands search keywords before initiating scrape
- **AI Agent**: Orchestrates and monitors the scraping process end-to-end

### 📥 Data Input
- **Form Trigger**: Clean interface for keyword submission
- **Keywords Field**: Required field for Pinterest search terms

### 🚀 Scraping Pipeline
1. **Launch Scraping Job**: Sends keywords to BrightData API
2. **Status Monitoring**: Tracks scraping job progress
3. **Data Retrieval**: Downloads scraped data
4. **Data Processing**: Extracts and formats Pinterest content
5. **Google Sheets Storage**: Appends structured results

---

## 🔧 Workflow Nodes

### 1. 📝 Pinterest Keyword Input
- **Type**: Form Trigger
- **Title**: "Pinterest"
- **Field**: `Keywords` (required)

### 2. 🧠 Claude Sonnet 4
- **Type**: Language Model
- **Model**: `claude-sonnet-4-20250514`
- **Purpose**: Processes keywords and manages scraping logic

### 3. 🤖 Keyword-Based Scraping Agent
- **Type**: AI Agent
- **Instructions**:
  - Start BrightData scraping with keywords
  - Monitor progress
  - Retrieve and return raw data

### 4. 🔗 BrightData Scraping Job
- **Type**: HTTP Request (POST)
- **Endpoint**: `https://api.brightdata.com/datasets/v3/trigger`
- **Parameters**:
  - `dataset_id`: `gd_lk0sjs4d21kdr7cnlv`
  - `type`: `discover_new`
  - `discover_by`: `keyword`
  - `limit_per_input`: `2`
  - `include_errors`: `true`

### 5. ⏳ Check Scraping Status
- **Type**: HTTP Request (GET)
- **Endpoint**: `https://api.brightdata.com/datasets/v3/progress/{snapshot_id}`
- **Checks for**: `running`, `ready`

### 6. 📥 Fetch Pinterest Snapshot Data
- **Type**: HTTP Request (GET)
- **Endpoint**: `https://api.brightdata.com/datasets/v3/snapshot/{snapshot_id}`
- **Triggers**: When status is `ready`

### 7. 🛠 Format & Extract Pinterest Content
- **Type**: JavaScript Code Node
- **Extracted Fields**:
  - `URL`
  - `Post ID`
  - `Title`
  - `Content`
  - `Date Posted`
  - `User`
  - `Likes & Comments`
  - `Media`
  - `Image URL`
  - `Categories`
  - `Hashtags`

### 8. 📊 Save Data to Google Sheets
- **Type**: Google Sheets Node
- **Operation**: Append rows
- **Mapped Columns**:
  - `Post URL`
  - `Title`
  - `Content`
  - `Image URL`

### 9. ⏱ (Optional) Wait Node
- **Purpose**: Adds a 60-second delay between status checks
- **Status**: Disabled by default

---

## ⚙️ Setup Instructions

### 🔐 Required Credentials
- **Anthropic API**
  - Credential ID: `ANTHROPIC_CREDENTIAL_ID`
- **BrightData API**
  - API Key: `BRIGHT_DATA_API_KEY`
- **Google Sheets OAuth2**
  - Credential ID: `GOOGLE_SHEETS_CREDENTIAL_ID`

### 🧩 Replace Placeholders
Ensure you update the following in your workflow:
- `WEBHOOK_ID_PLACEHOLDER`
- `GOOGLE_SHEET_ID_PLACEHOLDER`
- `WORKFLOW_ID_PLACEHOLDER`
- `WORKFLOW_VERSION_ID`
- `INSTANCE_ID_PLACEHOLDER`

---

## 🧪 How to Use

### ✅ Initial Setup
1. Configure all credentials in n8n
2. Replace placeholder values
3. Set up your Google Sheet using the [template](https://docs.google.com/spreadsheets/d/SAMPLE_SHEET_ID/edit)

### ▶️ Run the Workflow
1. Access the form trigger URL
2. Enter Pinterest keywords
3. Submit to begin scraping

### 🔍 Monitoring
- AI agent handles scraping status checks automatically

### 📁 Accessing Results
- Scraped content is appended to your linked Google Sheets document

---

## 📦 Output Data Structure

Each Pinterest post will include:
| Field          | Description                         |
|----------------|-------------------------------------|
| `URL`          | Direct link to the pin              |
| `Post ID`      | Unique identifier                   |
| `Title`        | Pin title                           |
| `Content`      | Pin description                     |
| `Date Posted`  | Publication date                    |
| `User`         | Username                            |
| `Likes & Comments` | Engagement details              |
| `Media`        | Media type                          |
| `Image URL`    | Direct image link                   |
| `Categories`   | Tags/categories                     |
| `Hashtags`     | Associated hashtags                 |
| `Comments`     | Comment text (if available)         |

---

## 🔧 Customization Options

- **📌 Adjust Data Volume**: Change `limit_per_input` in scraping node
- **⏲ Enable Delay**: Activate the wait node for long scraping sessions
- **🛠 Extract More Data**: Modify the JavaScript formatting node
- **🗃 Change Storage**: Replace Google Sheets with any storage tool

---

## 📋 Sample Google Sheet Template

Use this template as a base:
👉 [Google Sheet Template](https://docs.google.com/spreadsheets/d/SAMPLE_SHEET_ID/edit)

**Required Columns:**
- `Post URL`
- `Title`
- `Content`
- `Image URL`

---

## 📝 Notes

- BrightData API has rate limits
- Scraping currently limited to **2 pins per keyword**
- Supports async scraping and polling
- Error handling included for scraping steps

---

## 📣 Support

For issues, questions, or ideas — open a discussion or submit a pull request 💬

---

**Happy Scraping!** 🧩📌📊
