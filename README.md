# Brand Guardian AI - Video Compliance Audit System

> **AI-powered video compliance auditing system** that automatically analyzes video content against brand compliance rules using Retrieval-Augmented Generation (RAG) and advanced AI models.

[![Python 3.11+](https://img.shields.io/badge/Python-3.11%2B-blue)](https://www.python.org/downloads/)
[![FastAPI](https://img.shields.io/badge/FastAPI-0.135+-green)](https://fastapi.tiangolo.com/)
[![Azure](https://img.shields.io/badge/Azure-Integrated-0078D4)](https://azure.microsoft.com/)
[![LangChain](https://img.shields.io/badge/LangChain-Community-1f425f)](https://python.langchain.com/)

---

## 📋 Table of Contents

- [Overview](#overview)
- [Features](#features)
- [System Architecture](#system-architecture)
- [Prerequisites](#prerequisites)
- [Installation](#installation)
- [Configuration](#configuration)
- [Usage](#usage)
- [API Documentation](#api-documentation)
- [Project Structure](#project-structure)
- [Development](#development)
- [Contributing](#contributing)
- [License](#license)

---

## 🎯 Overview

**Brand Guardian AI** is an enterprise-grade compliance audit system that analyzes video content to detect violations of brand guidelines, FTC regulations, and advertising standards. It combines multiple Azure AI services with LangGraph orchestration to provide fast, accurate compliance insights.

### Key Capabilities

- **Automated Video Processing**: Downloads and processes YouTube videos on demand
- **Multi-Modal Content Extraction**: Extracts both audio transcripts and on-screen text (OCR)
- **Intelligent Compliance Audit**: Uses RAG pattern to match content against customizable compliance rules
- **Real-Time Reporting**: Generates structured compliance reports with severity levels
- **REST API**: Simple HTTP endpoints for integration with external systems
- **Observability**: Built-in telemetry tracking with Azure Monitor

---

## ✨ Features

### Core Features
- ✅ **YouTube Video Support** - Direct processing of public YouTube videos
- ✅ **Automatic Transcription** - Speech-to-text extraction via Azure Video Indexer
- ✅ **OCR Recognition** - On-screen text detection and extraction
- ✅ **RAG-Based Auditing** - Vector-based semantic search against compliance rules
- ✅ **Severity Categorization** - Issues classified as CRITICAL or WARNING
- ✅ **Session Tracking** - Unique IDs for audit trails and monitoring

### Technical Features
- ✅ **LangGraph Workflows** - Composable, reliable workflow orchestration
- ✅ **FastAPI Server** - High-performance async API with auto-documentation
- ✅ **Azure Integration** - Seamless integration with Azure AI services
- ✅ **Pydantic Validation** - Type-safe API contracts
- ✅ **Structured Logging** - Comprehensive application instrumentation
- ✅ **Error Handling** - Graceful error management with meaningful messages

---

## 🏗️ System Architecture

```
┌─────────────────────────────────────────────────────────────┐
│                    API Request Handler                       │
│                    (FastAPI Endpoint)                        │
└────────────────────────┬────────────────────────────────────┘
                         │
                         ▼
┌─────────────────────────────────────────────────────────────┐
│                   LangGraph Workflow                          │
│  ┌──────────────────────────────────────────────────────┐  │
│  │  START                                               │  │
│  └────────────┬─────────────────────────────────────────┘  │
│               │                                             │
│               ▼                                             │
│  ┌──────────────────────────────────────────────────────┐  │
│  │  INDEXER NODE                                        │  │
│  │  • Download YouTube video (yt-dlp)                  │  │
│  │  • Upload to Azure Video Indexer                    │  │
│  │  • Extract Transcript (Speech-to-Text)              │  │
│  │  • Extract OCR Text (On-Screen)                     │  │
│  │  • Return Indexed Data                              │  │
│  └────────────┬─────────────────────────────────────────┘  │
│               │                                             │
│               ▼                                             │
│  ┌──────────────────────────────────────────────────────┐  │
│  │  AUDITOR NODE                                        │  │
│  │  • Embed transcript using Azure OpenAI              │  │
│  │  • Search rules in Azure AI Search (Vector DB)      │  │
│  │  • Query GPT-4 with RAG context                     │  │
│  │  • Parse structured compliance issues               │  │
│  │  • Generate Final Report                            │  │
│  └────────────┬─────────────────────────────────────────┘  │
│               │                                             │
│               ▼                                             │
│  ┌──────────────────────────────────────────────────────┐  │
│  │  END                                                 │  │
│  │  Return Final State                                  │  │
│  └──────────────────────────────────────────────────────┘  │
└─────────────────────────────────────────────────────────────┘
                         │
                         ▼
            ┌─────────────────────────────┐
            │   Compliance Report JSON    │
            │  {                          │
            │    status: "FAIL"           │
            │    violations: [...]        │
            │    report: "AI Summary"     │
            │  }                          │
            └─────────────────────────────┘
```

### Data Flow

1. **Ingestion**: YouTube video → Downloaded via yt-dlp → Uploaded to Azure
2. **Indexing**: Azure Video Indexer → Extracts transcript & OCR
3. **Retrieval**: Embed transcript → Vector search in Azure AI Search → Retrieve relevant rules
4. **Reasoning**: Send RAG context to GPT-4 → Generate compliance audit
5. **Output**: Parse results → Create structured report with violations

---

## 📦 Prerequisites

### Required

- **Python**: 3.11 or higher
- **Package Manager**: `uv` (lightweight Python package manager)
- **Azure Account** with the following services:
  - Azure Video Indexer (for video processing)
  - Azure OpenAI (for GPT-4 and embeddings)
  - Azure AI Search (for vector storage)
  - Azure Storage Account (for video uploads)

### Optional

- **Git**: For version control
- **Docker**: For containerization
- **Postman/cURL**: For API testing

---

## 🚀 Installation

### Step 1: Clone the Repository

```bash
git clone https://github.com/yourusername/brand-guardian.git
cd ComplianceQAPipeline
```

### Step 2: Install `uv` Package Manager

On Windows using Git Bash:

```bash
curl -LsSf https://astral.sh/uv/install.sh | sh
export PATH=$HOME/.local/bin:$PATH
```

Or download from: https://astral.sh/uv/

### Step 3: Install Dependencies

```bash
uv sync
```

This reads `pyproject.toml` and installs all required packages in an isolated virtual environment.

### Step 4: Verify Installation

```bash
uv run python --version
# Should show Python 3.11+ with project dependencies
```

---

## ⚙️ Configuration

### Step 1: Create `.env` File

Create a `.env` file in the project root with your Azure credentials:

```bash
# Create .env file (Windows)
echo. > .env
```

### Step 2: Add Azure Configuration

Add your Azure credentials to `.env`:

```env
# Azure Credentials
AZURE_SUBSCRIPTION_ID="your-subscription-id"
AZURE_RESOURCE_GROUP="brand-guardian-project"
AZURE_TENANT_ID="your-tenant-id"

# Azure Storage
AZURE_STORAGE_ACCOUNT_NAME="bgstorage603"
AZURE_STORAGE_CONNECTION_STRING="DefaultEndpointsProtocol=https;..."

# Azure OpenAI (LLM & Embeddings)
AZURE_OPENAI_API_KEY="your-api-key"
AZURE_OPENAI_ENDPOINT="https://your-instance.cognitiveservices.azure.com/"
AZURE_OPENAI_API_VERSION="2024-12-01-preview"
AZURE_OPENAI_CHAT_DEPLOYMENT="gpt-4o"
AZURE_OPENAI_EMBEDDING_DEPLOYMENT="text-embedding-3-small"

# Azure AI Search (Vector Database)
AZURE_SEARCH_ENDPOINT="https://your-search.search.windows.net"
AZURE_SEARCH_API_KEY="your-search-api-key"
AZURE_SEARCH_INDEX_NAME="brand-compliance-rules"

# Azure Video Indexer
AZURE_VI_ACCOUNT_ID="account-uuid"
AZURE_VI_LOCATION="eastus"
AZURE_VI_NAME="your-vi-resource-name"
```

### Step 3: Populate Knowledge Base

Place compliance rule documents in `backend/data/`:

```bash
backend/data/
├── fda-regulations.pdf
├── ftc-endorsement-guides.pdf
└── brand-guidelines.pdf
```

Then index them:

```bash
uv run python backend/scripts/index_documents.py
```

### Step 4: Verify Configuration

The application will validate all environment variables on startup. Missing variables will log clear error messages.

---

## 💻 Usage

### Option 1: CLI (Command Line)

Run a single compliance audit from the command line:

```bash
uv run python main.py
```

**Example Output:**

```
--- 1. Input Payload: INITIALIZING WORKFLOW ---
{
  "video_url": "https://youtu.be/dT7S75eYhcQ",
  "video_id": "vid_ce6c43bb",
  "compliance_results": [],
  "errors": []
}

--- 2. WORKFLOW EXECUTION COMPLETE ---

=== COMPLIANCE AUDIT REPORT ===
Video ID:    vid_ce6c43bb
Status:      FAIL

[ VIOLATIONS DETECTED ]
- [CRITICAL] Misleading Claims: Absolute guarantee detected at 00:32
- [WARNING] Unsubstantiated Claims: "Proven to work" without evidence at 01:15

[ FINAL SUMMARY ]
Video contains 2 compliance violations. The video makes unsubstantiated health 
claims without proper scientific backing and uses absolute guarantee language
which violates FTC guidelines.
```

### Option 2: REST API

Start the FastAPI server:

```bash
uv run uvicorn backend.src.api.server:app --reload --host 0.0.0.0 --port 8000
```

**Interactive API Documentation:**

- **Swagger UI**: http://localhost:8000/docs
- **ReDoc**: http://localhost:8000/redoc

---

## 📡 API Documentation

### Available Endpoints

The API provides the following HTTP endpoints:

1. **Audit Video** - `POST /audit` (Main compliance auditing endpoint)
2. **Health Check** - `GET /health` (Server status verification)

### Audit Video Endpoint

**Endpoint**: `POST /audit`

**Description**: Submits a YouTube video for compliance audit

**Request Body:**
```json
{
  "video_url": "https://youtu.be/dT7S75eYhcQ"
}
```

**Response** (200 OK):
```json
{
  "session_id": "ce6c43bb-c71a-4f16-a377-8b493502fee2",
  "video_id": "vid_ce6c43bb",
  "status": "FAIL",
  "final_report": "Video contains 2 critical violations...",
  "compliance_results": [
    {
      "category": "Misleading Claims",
      "severity": "CRITICAL",
      "description": "Absolute guarantee detected at 00:32"
    },
    {
      "category": "Unsubstantiated Claims",
      "severity": "WARNING",
      "description": "Proven to work without evidence at 01:15"
    }
  ]
}
```

**Error Response** (422 Validation Error):
```json
{
  "detail": [
    {
      "loc": ["body", "video_url"],
      "msg": "field required",
      "type": "value_error.missing"
    }
  ]
}
```

### Status Codes

| Code | Description |
|------|-------------|
| 200  | Audit completed successfully |
| 422  | Invalid request format |
| 500  | Server error (check logs) |

### Example cURL Request
Health Check Endpoint

**Endpoint**: `GET /health`

**Description**: Verifies the API server is running and healthy

**Response** (200 OK):
```json
{
  "status": "healthy",
  "service": "Brand Guardian AI"
}
```

**Example cURL Request**:
```bash
curl http://localhost:8000/health
```

### Example Python Client

```python
import requests

url = "http://localhost:8000/audit"
payload = {"video_url": "https://youtu.be/dT7S75eYhcQ"}

response = requests.post(url, json=payload)
result = response.json()

print(f"Status: {result['status']}")
for violation in result['compliance_results']:
    print(f"- [{violation['severity']}] {violation['category']}\"

response = requests.post(url, json=payload)
result = response.json()

print(f"Status: {result['status']}")
for violation in result['compliance_results']:
    print(f"- [{violation['severity']}] {violation['category']}")
```

---

## 📂 Project Structure

```
ComplianceQAPipeline/
│
├── main.py                          # CLI entry point (run compliance audit)
├── pyproject.toml                   # Project dependencies & metadata
├── README.md                        # This file
│
├── backend/
│   ├── data/                        # Compliance rule documents (PDFs)
│   │   ├── fda-regulations.pdf
│   │   └── ftc-endorsement-guides.pdf
│   │
│   ├── scripts/
│   │   └── index_documents.py       # Vector DB population script
│   │
│   ├── src/
│   │   ├── api/
│   │   │   ├── server.py            # FastAPI application & endpoints
│   │   │   └── telemetry.py         # Azure Monitor integration
│   │   │
│   │   ├── graph/
│   │   │   ├── workflow.py          # LangGraph DAG definition
│   │   │   ├── nodes.py             # Indexer & Auditor node implementations
│   │   │   ├── state.py             # Graph state schema (TypedDict)
│   │   │   └── __init__.py
│   │   │
│   │   └── services/
│   │       ├── video_indexer.py     # Azure Video Indexer client
│   │       └── __init__.py
│   │
│   └── tests/                       # Unit & integration tests
│
└── .env                             # Environment variables (DO NOT COMMIT)
```

### Key Files Explained

| File | Purpose |
|------|---------|
| `main.py` | Entry point for CLI execution; orchestrates the workflow and displays results |
| `backend/src/graph/workflow.py` | Defines the LangGraph DAG structure: START → Indexer → Auditor → END |
| `backend/src/graph/nodes.py` | Implements the actual node logic (video processing & compliance checking) |
| `backend/src/graph/state.py` | TypedDict schema defining the state passed between nodes |
| `backend/src/services/video_indexer.py` | Azure Video Indexer API client for video processing |
| `backend/src/api/server.py` | FastAPI server with HTTP endpoints |
| `backend/scripts/index_documents.py` | Script to populate Azure AI Search with compliance rules |

---

## 🔧 Development

### Adding Custom Compliance Rules

1. **Add PDF documents** to `backend/data/`
2. **Run the indexer**:
   ```bash
   uv run python backend/scripts/index_documents.py
   ```
3. **Modify the audit prompt** in `backend/src/graph/nodes.py` to reference new rules

### Extending the Workflow

The workflow is modular and can be extended:

```python
# Add a new processing node
def custom_node(state: VideoAuditState) -> Dict[str, Any]:
    # Process state
    return {"updated_field": "value"}

# Add to workflow
workflow.add_node("custom_processor", custom_node)
workflow.add_edge("auditor", "custom_processor")
```

### Logging

View detailed logs by adjusting the logging level:

```python
logging.basicConfig(level=logging.DEBUG)  # Show all messages
```

---

## 🐛 Troubleshooting

### Common Issues

**Problem**: "No module named 'langchain'"
```bash
# Solution: Ensure dependencies are installed
uv sync
```

**Problem**: "Azure credentials not found in environment"
```bash
# Solution: Verify .env file exists and has correct format
cat .env
# Double-check no quotes or spaces around values
```
*Estimated timings based on Azure service SLAs*:

- **Video Download & Upload**: 1-5 minutes
- **Azure Video Indexing**: 5-15 minutes (depends on video length)
- **Transcript Extraction**: Included in indexing
- **Compliance Auditing**: 10-30 seconds (RAG search + LLM processing)
- **Total End-to-End**: ~5-20 minutes for complete audit

*Note: Actual performance varies based on video length, Azure service load, and network conditions.*
az login
az role assignment list --assignee <your-email>
```

**Problem**: "Azure Search index not found"
```bash
# Solution: Run the document indexing script first
uv run python backend/scripts/index_documents.py
```

### Enable Debug Logging

```bash
# Set environment variable
export PYTHONUNBUFFERED=1
uv run python -u main.py
```

---

## 📊 Performance Metrics

- **Video Processing**: 5-15 minutes (depends on video length and Azure processing)
- **Compliance Auditing**: 10-30 seconds (RAG search + LLM processing)
- **Total Flow**: ~5-20 minutes for end-to-end audit
- **API Response Time**: <50ms (for API overhead, excluding processing)

---

## 🔐 Security Notes

⚠️ **Important**:

1. **Never commit `.env`** - Add to `.gitignore`
2. **Rotate API keys** regularly in Azure Portal
3. **Use Managed Identities** in production (avoid explicit credentials)
4. **Validate user inputs** before processing
5. **Audit logs** are stored in Azure Monitor

---

## 📝 Environment Variables Reference

| Variable | Required | Description |
|----------|----------|-------------|
| `AZURE_SUBSCRIPTION_ID` | ✅ | Your Azure subscription ID |
| `AZURE_RESOURCE_GROUP` | ✅ | Resource group name |
| `AZURE_OPENAI_API_KEY` | ✅ | OpenAI API key |
| `AZURE_OPENAI_ENDPOINT` | ✅ | OpenAI endpoint URL |
| `AZURE_OPENAI_CHAT_DEPLOYMENT` | ✅ | Deployment name (e.g., "gpt-4o") |
| `AZURE_SEARCH_ENDPOINT` | ✅ | AI Search endpoint |
| `AZURE_SEARCH_API_KEY` | ✅ | AI Search API key |
| `AZURE_SEARCH_INDEX_NAME` | ✅ | Vector index name |
| `AZURE_VI_ACCOUNT_ID` | ✅ | Video Indexer account ID |
| `AZURE_VI_LOCATION` | ✅ | Video Indexer location |

---

## 🤝 Contributing

Contributions are welcome! Please:

1. Fork the repository
2. Create a feature branch (`git checkout -b feature/amazing-feature`)
3. Commit your changes (`git commit -m 'Add amazing feature'`)
4. Push to the branch (`git push origin feature/amazing-feature`)
5. Open a Pull Request

---

## 📄 License

This project is licensed under the MIT License - see [LICENSE](LICENSE) file for details.

---

## 🙏 Acknowledgments

- **LangChain**: For RAG and agent abstractions
- **LangGraph**: For workflow orchestration
- **Azure AI Services**: For video processing and LLM capabilities
- **FastAPI**: For the modern web framework

---

## 📞 Support

For issues, questions, or suggestions:

1. **GitHub Issues**: Create an issue in the repository
2. **Documentation**: Check the troubleshooting section
3. **Azure Help**: Azure-specific issues → Azure Support Portal

---

## 🗺️ Roadmap

### Current Implementation Status
- ✅ Core compliance audit workflow (Indexer → Auditor nodes)
- ✅ REST API with `/audit` and `/health` endpoints
- ✅ Azure Video Indexer integration for video processing
- ✅ RAG-based compliance checking with GPT-4
- ❌ *Not implemented yet*: Automated test suite, UI dashboard, batch processing

### Planned Features

- [ ] Comprehensive unit and integration test suite
- [ ] Web UI with Streamlit dashboard
- [ ] Batch processing API for multiple videos
- [ ] Custom compliance rule builder interface
- [ ] Real-time monitoring dashboard with analytics
- [ ] Export audit reports to PDF format
- [ ] Direct integration with TikTok, Instagram, YouTube platforms
- [ ] Multi-language support for video transcripts
- [ ] Docker containerization and Kubernetes deployment configs
- [ ] PostgreSQL persistence for audit history and trending
- [ ] Webhook notifications for critical compliance violations

---

**Last Updated**: March 2026  
**Version**: 0.1.0  
**Status**: Active Development
