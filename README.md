# Construction Project Manager - AI-Powered Cost Tracking System

A full-stack construction project management application with an intelligent AI copilot powered by **Retrieval Augmented Generation (RAG)**, **LangChain**, **pgvector**, and **OpenAI GPT-5 Nano API**. This system streamlines project tracking, cost management, and profitability analysis with semantic search capabilities and intelligent automation.

## 🎯 Problem Statement

Construction project management involves tracking numerous projects, items, costs, and financial metrics. Traditional systems require manual data entry, keyword-based searches, and lack intelligent insights. Key challenges include:

- **Inefficient Data Retrieval**: Finding relevant project information requires exact keyword matches
- **Limited Contextual Understanding**: Systems can't understand semantic relationships between projects, items, and costs
- **Manual Analysis**: Profitability analysis and insights require manual calculations and interpretation
- **No Intelligent Assistance**: No AI-powered assistant to answer questions about projects, costs, or profitability
- **Scalability Issues**: As data grows, traditional search methods become slow and ineffective

## 💡 Solution

This application solves these problems by integrating **advanced AI/ML technologies**:

1. **Semantic Search with pgvector**: Store 1536-dimensional embeddings directly in PostgreSQL for fast, meaning-based search
2. **RAG Pipeline with LangChain**: Retrieve relevant context and generate intelligent responses using your actual project data
3. **Automatic Embedding Generation**: Every project item automatically generates embeddings via OpenAI API for instant searchability
4. **AI Copilot**: Natural language interface to query projects, analyze profitability, and get insights
5. **Real-time Analytics**: Automatic profitability tracking

## 🚀 Key Features

### Core Functionality
- **Project & Item Management**: Create and manage construction projects with detailed items
- **Cost Tracking**: Track materials, labor, equipment, and subcontractor costs
- **Profitability Analysis**: Real-time profit/loss calculations at project and item levels
- **Role-Based Access Control**: Multi-tenant system with admin and user roles
- **Row-Level Security**: Enterprise-grade PostgreSQL RLS policies for data isolation

### AI/ML Capabilities
- **🤖 AI Copilot with RAG**: Ask natural language questions about your projects
- **🔍 Vector Similarity Search**: Find similar items using semantic meaning, not just keywords
- **⚡ Automatic Embeddings**: Every new item automatically generates embeddings for instant searchability
- **🧠 Multi-Step Reasoning**: LangGraph integration for complex analysis workflows
- **📊 Context-Aware Insights**: AI responses based on your actual project data

## 🏗️ Architecture

```
┌─────────────────────────────────────────────────────────────┐
│                    Frontend Layer                            │
│  Next.js 14 + TypeScript + Tailwind CSS                      │
│  Real-time UI with React 18                                  │
└───────────────────────┬───────────────────────────────────────┘
                        │
                        ↓
┌─────────────────────────────────────────────────────────────┐
│                    API Layer                                 │
│  FastAPI (Python 3.11) + Pydantic Validation               │
│  RESTful endpoints with comprehensive error handling         │
└───────────────────────┬───────────────────────────────────────┘
                        │
                        ↓
┌─────────────────────────────────────────────────────────────┐
│              Database & Vector Layer                         │
│  Supabase PostgreSQL + pgvector Extension                   │
│  • 1536-dimensional embeddings (text-embedding-3-small)     │
│  • HNSW indexing for fast similarity search                  │
│  • Row-Level Security (RLS) policies                        │
│  • JWT authentication                                        │
└───────────────────────┬───────────────────────────────────────┘
                        │
                        ↓
┌─────────────────────────────────────────────────────────────┐
│                  AI & ML Layer                               │
│  • OpenAI GPT-5 Nano API (reasoning-capable model)         │
│  • LangChain (RAG orchestration)                            │
│  • LangGraph (multi-step reasoning workflows)             │
│  • Custom SupabaseVectorStore (LangChain integration)       │
│  • Automatic embedding generation pipeline                   │
└─────────────────────────────────────────────────────────────┘
```

## 🤖 AI/ML Technology Deep Dive

### Retrieval Augmented Generation (RAG)

**What is RAG?**
RAG combines the power of vector similarity search with large language models to generate accurate, context-aware responses. Instead of relying solely on the LLM's training data, RAG retrieves relevant information from your database and uses it as context for generating responses.

**How it works in this application:**
1. **Query Embedding**: User's question is converted to a 1536-dimensional vector using OpenAI's `text-embedding-3-small` model
2. **Vector Search**: pgvector performs cosine similarity search to find the 5 most relevant project items
3. **Context Retrieval**: LangChain retrieves full details for matched items and builds rich context
4. **Response Generation**: LangChain orchestrates the RAG pipeline, sending context + query to GPT-5 Nano for intelligent response generation

### pgvector & Vector Similarity Search

**What is pgvector?**
pgvector is a PostgreSQL extension that enables storing and querying high-dimensional vectors directly in your database. This eliminates the need for separate vector databases and enables fast semantic search.

**Key Features:**
- **1536-dimensional embeddings**: Using OpenAI's `text-embedding-3-small` model
- **HNSW Indexing**: Hierarchical Navigable Small World (HNSW) algorithm for sub-millisecond similarity search
- **Native PostgreSQL Integration**: No separate vector database required
- **Cosine Similarity**: Finds items based on semantic meaning, not just keywords

**Example Query:**
```sql
SELECT * FROM match_embeddings(
  p_query_embedding := [0.123, 0.456, ...],  -- 1536 dimensions
  p_match_count := 5,
  p_org_id := 'org-uuid',
  p_entity_type := 'project_item'
);
```

### LangChain Integration

**What is LangChain?**
LangChain is a framework for building applications with LLMs. It provides abstractions for chains, agents, and retrieval systems, making it easier to build complex AI applications.

**Components Used:**
- **ProjectRAGChain**: Custom RAG chain that orchestrates the entire retrieval and generation process
- **SupabaseVectorStore**: Custom vector store implementation that integrates LangChain with pgvector
- **ChatOpenAI**: LangChain wrapper for OpenAI GPT-5 Nano API
- **OpenAIEmbeddings**: LangChain wrapper for OpenAI embedding models

**RAG Pipeline Flow:**
```
User Query
    ↓
OpenAI Embeddings API (text-embedding-3-small)
    ↓
pgvector Similarity Search (HNSW index)
    ↓
Context Retrieval (LangChain)
    ↓
OpenAI GPT-5 Nano API (with context)
    ↓
Intelligent Response
```

### LangGraph for Multi-Step Reasoning

**What is LangGraph?**
LangGraph extends LangChain with graph-based workflows, enabling multi-step reasoning and complex problem-solving.

**Use Cases:**
- Complex project analysis requiring multiple data gathering steps
- Multi-step profitability calculations
- Advanced insights generation workflows

### Automatic Embedding Generation

**How it works:**
1. When a project item is created or updated, the backend automatically:
   - Generates a text representation of the item (name, description, status, etc.)
   - Sends it to OpenAI API (`text-embedding-3-small`) to generate a 1536-dimensional embedding
   - Stores the embedding in the `ai_embeddings` table using pgvector
2. The item is instantly searchable via semantic similarity
3. No manual backfill required - embeddings are generated in real-time

**Benefits:**
- **Zero Manual Work**: Automatic embedding generation on create/update
- **Instant Searchability**: Items are immediately available for semantic search
- **Always Up-to-Date**: Embeddings are regenerated when items are updated

## 📊 Tech Stack

### Frontend
- **Next.js 14** (App Router) with TypeScript
- **React 18** for UI components
- **Tailwind CSS** for styling
- **Supabase Client** for database operations
- **Axios** for API calls

### Backend
- **FastAPI** (Python 3.11) for RESTful API
- **Pydantic** for data validation
- **Uvicorn** as ASGI server

### Database & Auth
- **Supabase** (PostgreSQL)
- **pgvector** extension for vector storage and search
- **Row-Level Security (RLS)** for multi-tenant data isolation
- **JWT Authentication** via Supabase Auth

### AI/ML Stack
- **OpenAI GPT-5 Nano API** - Latest reasoning-capable model for chat completions
- **OpenAI text-embedding-3-small** - 1536-dimensional embeddings
- **LangChain 0.3.0** - RAG orchestration and LLM integration
- **LangGraph 0.2.40** - Multi-step reasoning workflows
- **pgvector** - PostgreSQL extension for vector similarity search

### Infrastructure
- **Docker** and **Docker Compose** for containerization
- **Supabase Edge Functions** (Deno) for serverless functions
- **Nginx** (via Docker) for reverse proxy

## 📁 Project Structure

```
project_manager/
├── frontend/                 # Next.js application
│   ├── app/                  # Next.js app directory
│   │   ├── admin/           # Admin-only pages
│   │   │   └── ai-copilot/  # AI Copilot chat interface
│   │   ├── projects/        # Project management pages
│   │   └── ...
│   ├── components/          # React components
│   ├── contexts/            # React contexts (AuthContext)
│   ├── lib/                 # Utilities and API clients
│   └── types/               # TypeScript type definitions
├── backend/                 # FastAPI application
│   ├── routers/             # API route handlers
│   │   ├── ai.py           # AI endpoints (chat, insights, RAG)
│   │   ├── ai_langgraph.py  # LangGraph integration
│   │   └── ...
│   ├── ai/                  # AI/ML modules
│   │   ├── openai_client.py # OpenAI API client
│   │   ├── embeddings.py   # Embedding generation
│   │   ├── vector_store.py # SupabaseVectorStore (LangChain)
│   │   └── rag_chain.py    # ProjectRAGChain implementation
│   ├── database.py         # Database connection
│   └── main.py             # FastAPI app entry point
├── supabase/
│   └── functions/
│       └── complete-signup/ # Edge Function for signup
├── webpage/                 # Static showcase website
│   ├── index.html          # Landing page
│   ├── demo.html           # Interactive demo page
│   └── gifs/               # Demo GIFs
└── docker-compose.yml       # Docker orchestration
```

## 🗄️ Database Schema

### Core Tables
- **profiles** - User profiles
- **organizations** - Organizations/companies
- **organization_members** - User-organization relationships with roles
- **projects** - Construction projects
- **project_items** - Tasks/components within projects
- **cost_entries** - Individual cost records (materials, labor, etc.)
- **invitation_keys** - Invitation system for user onboarding

### AI/ML Tables
- **ai_embeddings** - Vector embeddings for semantic search
  - `embedding vector(1536)` - pgvector column for embeddings
  - `entity_type` - Type of entity (e.g., 'project_item')
  - `entity_id` - UUID of the entity
  - `content` - Text content used for embedding
  - Indexed with HNSW for fast similarity search

### Functions
- **match_embeddings()** - pgvector function for similarity search
- **user_is_in_org()** - RLS helper function
- **user_is_admin_in_org()** - RLS helper function

## 🚀 Getting Started

### Prerequisites

- Node.js 20+ and npm
- Python 3.11+
- Docker and Docker Compose
- Supabase account and project
- OpenAI API key with access to:
  - GPT-5 Nano API
  - text-embedding-3-small model

### Setup

1. **Clone the repository**
   ```bash
   git clone https://github.com/zeinalasali/project-manager-showcase.git
   cd project-manager-showcase
   ```

2. **Set up environment variables**

   Create `.env` files in both `backend/` and root directory:
   
   **Backend `.env`:**
   ```env
   SUPABASE_URL=your_supabase_url
   SUPABASE_KEY=your_supabase_anon_key
   SUPABASE_SERVICE_ROLE_KEY=your_service_role_key
   OPENAI_API_KEY=your_openai_api_key
   OPENAI_CHAT_MODEL=gpt-5-nano-2025-08-07
   OPENAI_EMBED_MODEL=text-embedding-3-small
   ```

   **Frontend `.env.local`:**
   ```env
   NEXT_PUBLIC_SUPABASE_URL=your_supabase_url
   NEXT_PUBLIC_SUPABASE_ANON_KEY=your_supabase_anon_key
   NEXT_PUBLIC_API_URL=http://localhost:8000
   ```

3. **Set up database**

   Run the following migrations in your Supabase SQL Editor:
   - Enable pgvector extension
   - Create `ai_embeddings` table
   - Create `match_embeddings()` function
   - Set up RLS policies
   - (See migration files in the repository)

4. **Install dependencies**

   **Frontend:**
   ```bash
   cd frontend
   npm install
   ```

   **Backend:**
   ```bash
   cd backend
   pip install -r requirements.txt
   ```

### Running the Application

#### Option 1: Docker Compose (Recommended)

```bash
docker-compose up --build
```

This will start:
- Frontend on `http://localhost:3000`
- Backend API on `http://localhost:8000`
- API docs at `http://localhost:8000/docs`

#### Option 2: Run Separately

**Backend:**
```bash
cd backend
uvicorn main:app --reload --host 0.0.0.0 --port 8000
```

**Frontend:**
```bash
cd frontend
npm run dev
```

## 🔌 API Endpoints

### AI/ML Endpoints

- `POST /api/ai/chat` - Chat with AI copilot (simple chat)
- `POST /api/ai/rag/query` - RAG-powered query (uses LangChain + pgvector)
- `GET /api/ai/insights/item/{item_id}` - Get AI insights for a specific item
- `POST /api/ai/langgraph/analyze` - Multi-step analysis using LangGraph

### Core Endpoints

- `GET /api/projects` - Get all projects
- `POST /api/projects` - Create project
- `GET /api/project-items` - Get project items
- `POST /api/project-items` - Create item (automatically generates embedding)
- `GET /api/cost-entries` - Get cost entries
- `POST /api/cost-entries` - Create cost entry

Full API documentation available at `http://localhost:8000/docs` when the backend is running.

## 🎨 Showcase Website

Visit the interactive showcase website to see the application in action:

**🔗 [View Showcase Website](https://your-showcase-url.com)** *(Update this URL when you deploy your showcase website)*

The showcase includes:
- Interactive demo with GIFs showing all features
- Detailed explanation of AI/ML technologies (LangChain, pgvector, RAG)
- Architecture diagrams
- Tech stack overview
- Step-by-step walkthrough of the RAG pipeline
- Visual demonstrations of vector similarity search

## 🧪 How RAG Works in This Application

### Example: "Which items are most profitable?"

1. **User asks question** in AI Copilot interface
2. **Query embedding generated**:
   - Question sent to OpenAI API
   - Returns 1536-dimensional vector: `[0.123, -0.456, 0.789, ...]`
3. **Vector similarity search**:
   - pgvector searches `ai_embeddings` table
   - Uses HNSW index for fast search
   - Finds 5 most similar items by cosine similarity
4. **Context retrieval**:
   - LangChain retrieves full details for matched items
   - Includes: name, description, revenue, costs, profitability
5. **Response generation**:
   - LangChain builds prompt with context
   - Sends to GPT-5 Nano API
   - Returns intelligent, context-aware response

### Example: Creating a New Project Item

1. **User creates item** via frontend
2. **Backend receives request** at `POST /api/project-items`
3. **Item saved** to `project_items` table
4. **Automatic embedding generation**:
   - Text content created: `"{name} - {description} - Status: {status}"`
   - Sent to OpenAI API (`text-embedding-3-small`)
   - Embedding generated: `[0.234, -0.567, 0.890, ...]` (1536 dimensions)
   - Stored in `ai_embeddings` table with pgvector
5. **Item is instantly searchable** via semantic similarity

## 🔒 Security

- **Row-Level Security (RLS)**: PostgreSQL policies ensure users only access their organization's data
- **JWT Authentication**: Secure token-based authentication via Supabase
- **Role-Based Access Control**: Admin and user roles with different permissions
- **API Key Protection**: OpenAI API keys stored securely in environment variables
- **Service Role Isolation**: Backend uses service role key only for admin operations

## 📈 Performance

- **Vector Search**: Sub-millisecond similarity search with HNSW indexing
- **Automatic Embeddings**: Generated asynchronously, doesn't block item creation
- **Caching**: LangChain caches embeddings and responses where appropriate
- **Database Optimization**: Indexed queries and efficient RLS policies

## 🛠️ Development

### Frontend Development

```bash
cd frontend
npm run dev
```

### Backend Development

```bash
cd backend
uvicorn main:app --reload
```

### Testing AI Features

1. Create a project and add some items
2. Navigate to `/admin/ai-copilot`
3. Ask questions like:
   - "Which items are most profitable?"
   - "What's the total cost for project X?"
   - "Show me items with high revenue"

## 📚 Additional Resources

- [LangChain Documentation](https://python.langchain.com/)
- [pgvector Documentation](https://github.com/pgvector/pgvector)
- [OpenAI API Documentation](https://platform.openai.com/docs)
- [Supabase Documentation](https://supabase.com/docs)

## 🤝 Contributing

This is a private project. For questions or suggestions, please open an issue in the repository.

## 📄 License

This project is private and proprietary.

---

**Built with ❤️ using OpenAI API, LangChain, pgvector, and modern web technologies.**
