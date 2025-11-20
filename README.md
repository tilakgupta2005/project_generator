# 🤖 Project Generator Automation (GDG) Workflow

This n8n workflow automates the process of generating personalized project ideas for students based on their known technology stack and stated daily challenges. It then compiles the project idea into a professional HTML file, commits it to a GitHub repository, updates a tracking spreadsheet, and emails the student with a link to their personalized project.

## ✨ Features

* **Google Sheets Trigger:** Automatically starts the workflow when a new submission is added to a linked Google Form/Sheet.
* **Filtering:** Skips entries that have already been processed by checking the `Mail Sent` column.
* **AI-Powered Project Generation:** Uses the **Google Gemini Chat Model** and a structured output parser to generate a project idea, role, tech stack, and a 5-step guide, strictly adhering to the student's known technologies.
* **Personalized HTML Generation:** Generates a professional, personalized HTML page using the **Code** node.
* **GitHub Deployment:** Commits the generated HTML file to a public GitHub repository for easy sharing and hosting via GitHub Pages.
* **Email Notification:** Sends the student a personalized email via **Gmail** with a direct link to their generated project page.
* **Tracking:** Updates the original Google Sheet entry with "Sent" in the `Mail Sent` column to prevent duplicate processing.

## 🔗 Trial & Testing

Experience the automation firsthand by filling out the sample form linked below. This form mimics the trigger event that starts the n8n workflow.

* **Sample Google Form:** `https://forms.gle/sguZYWZhTb7Jt6Cy6`

## 🛠️ Requirements & Setup

Before using this workflow, you must set up the necessary credentials and external services in your n8n instance.

### 1. Credentials (Redacted for Security)

This workflow requires the following credentials to be configured. The IDs listed here are placeholders to avoid exposing sensitive configuration data.

| Credential Type | Node | Purpose |
| :--- | :--- | :--- |
| Google Sheets (OAuth2) | `Google Sheets Trigger` | Read new form submissions. |
| Google Gemini(PaLM) API | `Google Gemini Chat Model` | Access the Gemini API for project generation. |
| Gmail (OAuth2) | `Send a message` | Send the final project email. |
| GitHub (API Key/OAuth) | `Create a file` | Commit the generated HTML file. |
| Google Sheets (OAuth2) | `Update row in sheet` | Mark the row as "Sent" after processing. |

### 2. External Service Links (Redacted/Generalized)

The workflow is configured to interact with the following external resources. **You must replace the bracketed placeholders with your own secure IDs and details.**

1.  **Google Sheet / Form:**
    * **Document ID:** `[REDACTED_SHEET_ID]`
    * **Sheet Name:** `Form Responses 1`
    * **Note:** This sheet must contain columns for student input and a tracking column named **`Mail Sent`**.

2.  **GitHub Repository:**
    * **Owner/Repository:** `[REDACTED_GITHUB_USER]/[REDACTED_REPO_NAME]`
    * **URL Template:** The final email links to the file hosted via GitHub Pages, confirming the repository is public and set up for hosting: `https://[REDACTED_GITHUB_USER].github.io/[REDACTED_REPO_NAME]/{{ $json.content.name }}`.

## ⚙️ Workflow Breakdown

The workflow follows a standard sequence: **Trigger $\rightarrow$ Filter $\rightarrow$ Processing Loop $\rightarrow$ Generation $\rightarrow$ Deployment $\rightarrow$ Notification $\rightarrow$ Tracking**.

| Node Name | Type | Key Configuration/Function |
| :--- | :--- | :--- |
| **Google Sheets Trigger** | `googleSheetsTrigger` | Starts the workflow on new row additions. |
| **Filter** | `filter` | Checks if the `Mail Sent` column is **not equal** to "Sent". |
| **Basic LLM Chain2** | `chainLlm` | Sends a detailed prompt and input data to the AI model. |
| **Structured Output Parser1** | `outputParserStructured` | Confirms the LLM output conforms to the expected JSON schema. |
| **Code** | `code` | Generates the final **HTML content** and the dynamic filename. |
| **Create a file** | `github` | Commits the HTML file to the GitHub repository. |
| **Send a message** | `gmail` | Sends the personalized email with the project link. |
| **Update row in sheet** | `googleSheets` | Marks the processed row in the Google Sheet as "Sent". |

---
*Created by: Tilak Gupta*
