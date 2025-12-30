# AI-Agent to generate insights & Content Automation

Production-ready workflow implemented in n8n (running in Docker) to generate trend signals from product data, produce content & insights via an LLM, and export HTML while persisting results to a database.

## What this does
- Merge product data from multiple sources
- Generate trend signals and insights using an LLM (e.g., Google Gemini Chat Model)
- Produce content (HTML template) from signals and allow download
- Persist outputs (create/update rows) via database nodes

The exported n8n workflow is included in this repo as:
- AI-Generated Insights & Content Automation.json

## Quick start (Docker on Windows)
Prerequisites: Docker Desktop, Git.

1) Clone the repo and enter the folder
```powershell
git clone https://github.com/Rohit-Somisetty/AI--Agent-to-generate-insights-Content-Automation.git
cd AI--Agent-to-generate-insights-Content-Automation
```

2) Start n8n via Docker (simple run)
```powershell
docker run -it --rm \
	-p 5678:5678 \
	-v %CD%\n8n_data:/home/node/.n8n \
	-v %CD%\workflows:/workflows \
	-e N8N_SKIP_OWNER_SETUP=true \
	-e N8N_PROTOCOL=http \
	-e N8N_HOST=localhost \
	-e N8N_PORT=5678 \
	-e N8N_EDITOR_BASE_URL=http://localhost:5678 \
	n8nio/n8n:latest
```

Alternatively, use docker-compose (recommended):
```yaml
version: '3.8'
services:
	n8n:
		image: n8nio/n8n:latest
		ports:
			- "5678:5678"
		environment:
			- N8N_SKIP_OWNER_SETUP=true
			- N8N_PROTOCOL=http
			- N8N_HOST=localhost
			- N8N_PORT=5678
			- N8N_EDITOR_BASE_URL=http://localhost:5678
		volumes:
			- ./n8n_data:/home/node/.n8n
			- ./workflows:/workflows
```
Bring it up with:
```powershell
docker compose up -d
```

3) Open n8n UI
- http://localhost:5678

4) Import the workflow
- In n8n, click “Import from File” and choose the file: AI-Generated Insights & Content Automation.json

5) Configure credentials (as required by your environment)
- LLM: Google Gemini Chat Model → requires GOOGLE_API_KEY
- HTTP Request nodes: endpoints for product data (auth as needed)
- Database nodes: connection to your DB (Postgres/MySQL/SQLite)
- Optional storage: if saving HTML externally (S3, etc.), add credentials

6) Run the workflow
- Trigger manually or wire a schedule/HTTP trigger depending on your use case
- Outputs include generated HTML content and DB updates

## Workflow overview
The workflow (see attached screenshots) follows this path:
- Merge product data → Calculate trend signals
- Generate content & insights from signals via LLM
- Download/produce HTML and save a reusable template
- Write results to the database (update/create rows)

## Files in this repo
- README.md – this documentation
- .gitignore – common development ignores
- AI-Generated Insights & Content Automation.json – n8n exported workflow

## Notes
- Keep credentials in environment variables or n8n’s credential manager
- Review outputs for compliance and accuracy (human-in-the-loop recommended)
- If you need additional automation (e.g., publishing to CMS), we can extend the workflow

## Contributing
- Open an issue with a concise proposal
- Keep changes minimal, documented, and tested

## License
To be defined. Suggestion: MIT or Apache-2.0.
