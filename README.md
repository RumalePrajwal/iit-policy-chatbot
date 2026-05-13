# 🎓 IIT International Student Policy Chatbot

A policy-grounded chatbot for Illinois Institute of Technology international students.  
It answers questions on **CPT, OPT, STEM OPT, F-1 status, SSN, health insurance, and compliance topics** using official IIT policy content.

---

## ✨ Highlights

- 🔎 **Hybrid retrieval** with **BM25 + vector search + RRF + reranking**
- 🧠 **Deterministic rule engine** for eligibility-style questions
- 💬 **Slot filling + session memory** for multi-turn conversations
- 🤖 **LLM-grounded answer synthesis** using retrieved policy chunks only
- 🌐 **Full-stack deployment** with FastAPI, Next.js, Elasticsearch, and Azure OpenAI

---

## 🛠️ Tech Stack

- **Backend:** FastAPI, Python
- **Frontend:** Next.js, TypeScript, Tailwind CSS
- **Retrieval:** Elasticsearch / Elastic Cloud
- **LLM:** Azure OpenAI
- **Data:** Manually cleaned IIT policy pages in Markdown

---

## 🧭 How It Works

**User Question → Topic & Intent Detection → Hybrid Retrieval → Optional Slot Filling / Rule Engine → LLM Answer Synthesis → Response with Sources**

---

## 📂 Project Structure

```bash
iit_chatbot/
├── cb_backend/      # Backend, retrieval, ingestion, rule engine
├── cb_frontend/     # Next.js frontend
└── README.md
```

---

## 🚀 Getting Started

### 1. Clone the repository
```bash
git clone <your-repo-url>
cd iit_chatbot
```

### 2. Set up the backend
```bash
cd cb_backend
python -m venv .venv
```

#### Windows
```powershell
.\.venv\Scripts\activate
```

#### macOS / Linux
```bash
source .venv/bin/activate
```

Install dependencies:

```bash
pip install -r requirements.txt
```

### 3. Configure environment variables
Create a `.env` file inside `cb_backend/` and add your keys:

```env
OPENAI_API_KEY=your_key
OPENAI_EMBED_MODEL=text-embedding-3-small
OPENAI_CHAT_MODEL=gpt-4o-mini

ES_URL=http://localhost:9200
ES_INDEX=iit_policy_chunks
ES_USERNAME=elastic
ES_PASSWORD=changeme
```

### 4. Start Elasticsearch and ingest data
```bash
docker compose up -d
python -m ingest.es_setup
python -m ingest.ingest
```

### 5. Run the backend
```bash
uvicorn api_server:app --host 127.0.0.1 --port 8000 --reload
```

### 6. Run the frontend
Open a new terminal:

```bash
cd cb_frontend
npm install
npm run dev
```

---

## 💡 Example Questions

- What is CPT?
- Can I do CPT in my first semester?
- How do I apply for OPT?
- Can I take fewer classes this semester?
- What documents are required to apply for an SSN?
- Is health insurance mandatory for international students at IIT?

---

## 📊 Results

- 📈 **Offline evaluation improved from 72.1% to 90.4%**
- ✅ Built on a **52-question** benchmark
- 🌟 Achieved **4.63/5.0** average user rating
- 👍 **100%** of users said they would use it again and recommend it

---

## 🔐 Notes

- This chatbot answers only from **official IIT policy documents**
- Eligibility decisions are handled with **deterministic logic**, not free-form generation
- Do **not** commit `.env`, secrets, `node_modules`, or `.venv` to GitHub

---

## 👥 Team

- **Rumale Prajwal Lnu** — Project Lead, Backend & Chatbot Logic
- **Hari Prasad Reddy Etta** — Cloud Infrastructure, Azure OpenAI Integration, Feedback System
- **Roshan Kumar Reddy Kotha** — Cloud Systems Design, Pipeline Maintenance, API Engineering
- **Sai Abhinav Kolli** — FastAPI Development, Test Set Design, Performance Analysis
- **Sanjana Chowdary Muppuri** — Frontend Development, UI/UX Design, Policy Data Curation
