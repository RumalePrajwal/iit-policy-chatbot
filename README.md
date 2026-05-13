# IIT International Student Policy Chatbot

A policy-grounded chatbot for Illinois Institute of Technology international students.  
The system answers questions on CPT, OPT, STEM OPT, F-1 status maintenance, SSN, health insurance, and related compliance topics using only official IIT policy content.

## Project Overview

This project was built as an educational, source-grounded assistant for international student policy guidance. The chatbot combines:

- Hybrid retrieval over IIT policy documents
- Rule-based eligibility evaluation for compliance-sensitive questions
- Slot filling and session memory for multi-turn advisory flows
- LLM-based answer synthesis grounded only in retrieved policy content
- A deployed full-stack architecture with frontend, backend, cloud retrieval, and feedback collection

The final system was designed to prioritize:
- correctness over guesswork
- explainability over unsupported generation
- official IIT policies as the single source of truth

## Main Features

- Policy-grounded answers with source links
- Support for Employment, F-1 Status Maintenance, and Health/SSN topics
- Deterministic eligibility logic for questions like “Can I do CPT?” or “Am I allowed to take fewer classes?”
- Multi-turn follow-up questions for missing information
- Hybrid retrieval using BM25 and semantic search
- FastAPI backend and Next.js frontend
- Feedback logging for evaluation and improvement

## Supported Policy Areas

### Employment
- CPT
- OPT
- STEM OPT
- On-campus work
- OPT reporting / SEVP portal guidance

### F-1 Status Maintenance
- Enrollment requirements
- Reduced Course Load (RCL)
- Travel and re-entry
- Check-in
- Processing times
- I-20 and form requests

### Health / Life in the U.S.
- Student health insurance
- SHIP waiver
- Student Health and Wellness Center
- Social Security Number (SSN)
- Mandatory fees

## High-Level Architecture

User Question -> Topic & Intent Detection -> Hybrid Retrieval -> Optional Slot Filling / Rule Engine -> LLM Answer Synthesis -> Response with Sources

### Backend logic
- **Topic routing** maps the query to the correct policy family.
- **Intent detection** classifies the query as definition, procedure, eligibility, timing, contact, portal/form, etc.
- **Hybrid retrieval** uses Elasticsearch with BM25 + vector similarity.
- **Rule engine** evaluates deterministic policy logic for eligibility questions.
- **Slot filling** collects only the missing user facts required for a safe answer.
- **Session memory** preserves context across turns.
- **Answer synthesis** uses GPT-4.1 to generate grounded, readable responses from retrieved policy chunks only.

## Tech Stack

### Frontend
- Next.js
- TypeScript
- Tailwind CSS

### Backend
- FastAPI
- Python
- Pydantic
- Uvicorn

### Retrieval / Search
- Elasticsearch / Elastic Cloud
- BM25
- Vector similarity search
- Reciprocal Rank Fusion (RRF)

### Models
- Azure OpenAI GPT-4.1
- text-embedding-3-small

### Data
- Manually cleaned IIT policy pages stored as Markdown
- Section-aware chunking for indexing

### Feedback / Logging
- Neon PostgreSQL or JSON fallback, depending on deployment mode

## Project Structure

```text
iit_chatbot/
├── cd_backend          # Core orchestration, retrieval, rules, answer synthesis
├── cb_frontend
└── README.md
```

## Setup Instructions

### 1. Clone the repository

```bash
git clone <your-repo-url>
cd <repo-folder>
```

### 2. Create a Python virtual environment

Open a new terminal and run:

```bash
cd cb_backend
```

#### Windows
```powershell
python -m venv .venv
.\.venv\Scripts\activate
```

#### macOS / Linux
```bash
python3 -m venv .venv
source .venv/bin/activate
```

### 3. Install backend dependencies

```bash
pip install -r requirements.txt
```

### 4. Configure environment variables

Create a `.env` file in the project root and add the required keys.

Example:

```env
AZURE_OPENAI_API_KEY=your_key
AZURE_OPENAI_ENDPOINT=your_endpoint
AZURE_OPENAI_API_VERSION=2024-02-01
AZURE_OPENAI_CHAT_DEPLOYMENT=gpt-4.1
AZURE_OPENAI_EMBEDDING_DEPLOYMENT=text-embedding-3-small

ELASTIC_URL=your_elastic_url
ELASTIC_API_KEY=your_elastic_api_key
ELASTIC_INDEX=iit_policy_chunks

DATABASE_URL=your_neon_or_postgres_url
```

If your code still supports standard OpenAI fallback or username/password Elasticsearch auth, keep only the variables that match your current backend configuration.

### 5. Start Elasticsearch and ingest policy data

If Elasticsearch is running locally or on Elastic Cloud, create the index and ingest the corpus:

```bash
python ingest/es_setup.py
python ingest/ingest.py
```

### 6. Run the FastAPI backend

```bash
uvicorn api_server:app --host 127.0.0.1 --port 8000 --reload
```

### 7. Start the frontend

Open a new terminal and run:

```bash
cd cb_frontend
npm install
npm run dev
```

Then open the local frontend URL shown in the terminal.

## Example Questions

- What is CPT?
- Can I do CPT in my first semester?
- How do I apply for OPT?
- Can I take fewer classes this semester?
- How long does OGS take to process an I-20 request?
- What documents are required to apply for an SSN?
- Is health insurance mandatory for international students at IIT?

## Evaluation Summary

The chatbot was evaluated using a curated 52-question offline benchmark across:
- Employment
- F-1 Status Maintenance
- Health / SSN / Compliance

Final offline performance:
- Phase 1: 72.1%
- Phase 2: 90.4%

The system was also compared against an AutoRAG / NotebookLM-style baseline.  
Our chatbot achieved a higher overall score, while the AutoRAG baseline was more consistent on some detail-heavy procedural questions.

## Deployment Notes

The deployed stack used:
- Netlify for the frontend
- Render for the FastAPI backend
- Azure OpenAI for LLM + embeddings
- Elastic Cloud for retrieval
- Neon for feedback storage

For local development, the same backend logic can be used with local or hosted Elasticsearch.

## License / Academic Use

This project was created as part of ITMD 537 at Illinois Tech and is intended for academic and educational use.

## Team

- Rumale Prajwal Lnu — Project Lead, Backend & Chatbot Logic
- Hari Prasad Reddy Etta — Cloud Infrastructure, Azure OpenAI Integration, Feedback System
- Roshan Kumar Reddy Kotha — Cloud Systems Design, Pipeline Maintenance, API Engineering
- Sai Abhinav Kolli — FastAPI Development, Test Set Design, Performance Analysis
- Sanjana Chowdary Muppuri — Frontend Development, UI/UX Design, Policy Data Curation
