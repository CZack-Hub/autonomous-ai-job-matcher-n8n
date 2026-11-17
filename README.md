# Autonomous AI Job Matcher & Application Generator (n8n + Gemini)

An end-to-end, human-in-the-loop AI automation pipeline built in **n8n** that parses a candidate's PDF resume, scrapes live entry-level to senior job openings across India, scores top matches using **Google Gemini**, logs results to **Google Sheets**, and generates tailored cover letters on demand.

![Workflow Architecture](assets/workflow-architecture.png)

## Key Features
- **Resume Ingestion & Parsing:** Extracts text from uploaded PDF resumes via n8n's `Extract from File` node.
- **Experience-Filtered Live Job Scraping:** Dynamically queries LinkedIn's public job feed filtered by target role, location (`India`), and experience tier (`Fresher 0–2 yrs`, `Intermediate 2–5 yrs`, `Senior 5+ yrs`).
- **AI Candidate-Job Scoring:** Uses **Google Gemini** to rank the top 5 matching roles with match scores and concise fit reasoning, protected by self-healing JSON extraction.
- **Human-in-the-Loop Selection:** Presents matched opportunities in a dynamic second-stage dropdown form so the user chooses which role to apply for.
- **Automated Application Tracking & Cover Letter Drafting:** Generates a tailored cover letter, logs both matched jobs and generated applications into separate Google Sheets tabs (`Matched Jobs` and `Applications`), and displays a quick-apply directory of remaining matches.

## Tech Stack
- **Orchestration:** n8n Cloud
- **LLM:** Google Gemini (`gemini-2.5-flash` / `gemini-3.5-flash-lite`)
- **Database & Tracking:** Google Sheets API
- **Data Extraction:** HTTP Request + HTML CSS Selector Scraping

## Quick Setup
1. **Import Workflow:** Download `ai-job-matcher-workflow.json` and import it into your n8n instance (`Workflows` -> `Import from File`).
2. **Configure Credentials:**
   - Add your **Google Gemini (PaLM) API** key in the `AI Job Matcher` and `Draft Cover Letter` nodes.
   - Connect your **Google Sheets OAuth2** account in `Save Matched Jobs` and `Save Application`.
3. **Prepare Google Sheet:** Create a spreadsheet with two tabs:
   - `Matched Jobs` with columns: `Job_ID`, `Title`, `Company`, `Match_Score`, `Reason`, `URL`, `Resume_Summary`
   - `Applications` with columns: `Job_ID`, `Title`, `Company`, `Status`, `Tailored_Cover_Letter`, `Apply_URL`
4. **Publish & Run:** Click **Publish**, open the `On form submission` **Production URL**, upload a PDF resume, and test the pipeline.
