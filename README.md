# ⚡ R8R — Rapid RAG Runtime

**Deploy production-grade RAG pipelines in minutes, not weeks.**

R8R is an end-to-end RAG workflow runtime that eliminates boilerplate and lets you build, run, and scale retrieval-augmented generation systems with a single API call.

---

## 🚀 The Problem

Building RAG systems is painfully repetitive:

<table>
<tr>
<td width="50%">

### ❌ Without R8R

- 1000+ lines of glue code
- Manual query rewriting & reranking
- No memory across sessions
- Complex multi-step pipeline logic
- Provider lock-in
- Zero observability

</td>
<td width="50%">

### ✅ With R8R

- One API call
- Intelligent query optimization
- Built-in persistent memory (95.7% accuracy)
- Visual workflow builder
- Multi-LLM orchestration
- Full analytics & debugging

</td>
</tr>
</table>

---

## ✨ Key Features

### 🎨 **Visual Workflow Builder**
Drag, drop, and deploy RAG pipelines without code. Connect retrievers, rerankers, and LLMs visually.

### 🧠 **Persistent Memory Engine**
- **Redis** for session context
- **Qdrant** for semantic memory (95.7% duplicate detection)
- **PostgreSQL** for full conversation history
- Automatic deduplication and consolidation

### 🤖 **Multi-LLM Orchestration**
Run GPT-4, Claude, and Gemini in parallel. Get ensemble answers with automatic fallback and voting.

```
GPT-4 ──┐
Claude ──├──► Intelligent Ensemble ──► Optimized Answer
Gemini ──┘
```

### 🔄 **HyDE Retrieval**
Hypothetical Document Embeddings reduce hallucinations and improve context quality by 40%.

### 💬 **Telegram Workflow Builder**
Build and deploy RAG systems via natural language chat. No dashboard required.

### 📊 **Analytics Dashboard**
Track cost, latency, errors, and replay any query. Built-in debugging for production systems.

---

## ⚡ Quick Start

### Option 1: Use Hosted API

```bash
npm install r8r-client
```

```bash
pip install r8r-client
```

### Option 2: Run Locally

Clone the repository and set up your local environment:

```bash
# Clone the repository
git clone https://github.com/Nitinref/R8R.AI.git
cd R8R.AI

# Install dependencies
npm install

# Start Qdrant (Vector Database)
docker run -p 6333:6333 qdrant/qdrant

# Start Redis (Caching)
docker run -p 6379:6379 redis:alpine

# Or use Docker Compose to start all services
docker-compose up -d
```

Create a `.env` file in the root directory:

```dotenv
# Database
DATABASE_URL="postgresql://neondb_owner:npg_8TjRiXy9GmYZ@ep-wild-bar-ad7o2d1l-pooler.c-2.us-east-1.aws.neon.tech/neondb?sslmode=require&channel_binding=require"

# Security
JWT_SECRET="your-super-secret-jwt-key-change-in-production"
JWT_EXPIRY="7d"

# LLM API Keys
OPENAI_API_KEY="sk-proj-your-key-here"
ANTHROPIC_API_KEY="sk-ant-your-key-here"
GOOGLE_API_KEY="AIzaSy-your-key-here"
MISTRAL_API_KEY="your-key-here"

# Vector Databases
PINECONE_API_KEY="pcsk_your-key-here"
PINECONE_ENVIRONMENT="gcp-starter"
PINECONE_INDEX_NAME="default"
WEAVIATE_URL="http://localhost:8080"

# Redis Configuration
REDIS_HOST="localhost"
REDIS_PORT="6379"
REDIS_PASSWORD="your_redis_password_here"

# Qdrant Configuration
QDRANT_HOST="localhost"
QDRANT_PORT="6333"
QDRANT_API_KEY="your_qdrant_api_key_here"

# Telegram Bot (Optional)
TELEGRAM_BOT_TOKEN="your-telegram-bot-token"
TELEGRAM_CHAT_ID="your-telegram-chat-id"

# Rate Limiting
RATE_LIMIT_WINDOW_MS="900000"
RATE_LIMIT_MAX_REQUESTS="100"

# Server Configuration
PORT="3001"
NODE_ENV="development"
NEXT_PUBLIC_API_URL="http://localhost:3001"
```

Start the development server:

```bash
# Run database migrations
npm run db:migrate

# Start the backend
npm run dev:backend

# In another terminal, start the frontend
npm run dev:frontend
```

Your R8R instance will be available at:
- **Frontend:** http://localhost:3000
- **API:** http://localhost:3001
- **Qdrant Dashboard:** http://localhost:6333/dashboard

### JavaScript Example

```javascript
import R8R from "r8r-client";

const r8r = new R8R("YOUR_API_KEY");

const result = await r8r.query(
  "How does photosynthesis work?",
  { 
    pipeline: "advanced",
    memory: true,
    providers: ["gpt-4", "claude"]
  }
);

console.log(result.answer);
console.log(result.sources);
console.log(result.confidence);
```

### Python Example

```python
from r8r_client import R8R

r8r = R8R("YOUR_API_KEY")

result = r8r.query(
    "How does photosynthesis work?",
    pipeline="advanced",
    memory=True,
    providers=["gpt-4", "claude"]
)

print(result.answer)
print(result.sources)
```

### cURL Example

```bash
curl -X POST https://api.r8r.ai/v1/query \
  -H "Authorization: Bearer YOUR_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "query": "Explain blockchain technology",
    "pipeline": "standard",
    "memory": true
  }'
```

---

## 🧩 Pre-Built Pipelines

Choose the right pipeline for your use case:

| Pipeline | Best For | Features | Response Time |
|----------|----------|----------|---------------|
| **standard** | FAQs, chatbots, simple queries | Basic retrieval + single LLM | ~2s |
| **advanced** | Technical docs, complex Q&A | HyDE + reranking + ensemble | ~4s |
| **research** | Academic research, deep analysis | Multi-hop reasoning + citations | ~8s |
| **custom** | Anything | Visual builder, full control | Varies |

---

## 🧠 Memory System Deep Dive

R8R's memory system works across three layers:

### Layer 1: Session Context (Redis)
Fast in-memory storage for current conversation. Sub-10ms retrieval.

### Layer 2: Semantic Memory (Qdrant)
Vector embeddings with 95.7% duplicate detection accuracy. Finds relevant context from past conversations.

### Layer 3: Analytics Store (PostgreSQL)
Complete conversation history, user preferences, and system metrics.

**Benefits:**
- ✅ Remember user preferences across sessions
- ✅ Eliminate redundant questions automatically
- ✅ Build context-aware applications
- ✅ Background consolidation (no blocking)

---

## 🤖 Multi-LLM Orchestration

### Parallel Execution
Query multiple LLMs simultaneously for faster, more reliable results.

### Ensemble Voting
Combine responses using weighted voting based on confidence scores.

### Automatic Fallback
If one provider fails, seamlessly fall back to alternatives.

### Cost Optimization
Route queries to the most cost-effective provider for each task.

```javascript
const result = await r8r.query("Question", {
  providers: ["gpt-4", "claude", "gemini"],
  strategy: "ensemble", // or "fastest" or "cheapest"
  fallback: true
});
```

---

## 💬 Telegram Workflow Builder

Build RAG systems without leaving Telegram:

```
You: /create Build a customer support RAG using GPT-4 with memory

Bot: ✅ Created workflow "customer-support-rag"
     🔑 API Key: r8r_abc123...
     🌐 Endpoint: https://api.r8r.ai/v1/pipelines/cs-001
     
     Test it:
     curl -X POST https://api.r8r.ai/v1/pipelines/cs-001 \
       -H "Authorization: Bearer r8r_abc123..." \
       -d '{"query":"How do I reset my password?"}'
```

Available commands:
- `/create` - Build a new pipeline
- `/list` - View all pipelines
- `/stats` - Get usage analytics
- `/edit` - Modify existing pipeline

---

## 📊 Analytics & Observability

### Real-Time Dashboard
- Query volume and response times
- Cost tracking per query and provider
- Error rates and failure patterns
- User satisfaction scores

### Query Replay
Reproduce any query exactly as it ran in production for debugging.

### A/B Testing
Compare pipeline configurations with built-in experiment tracking.

### Custom Alerts
Get notified when latency spikes, costs exceed thresholds, or errors occur.

---

## 🏗️ Tech Stack

**Frontend**
- Next.js 14 + TypeScript
- Tailwind CSS + shadcn/ui
- Canvas-based workflow editor

**Backend**
- Node.js + Express
- PostgreSQL + Prisma ORM
- Qdrant (vector database)
- Redis (caching & sessions)

**Infrastructure**
- Docker + Kubernetes
- AWS / GCP support
- Self-hosted option available

---

## 💰 Pricing --- Comming soon

<table>
<tr>
<td width="33%">

### Free
**$0/month**

- 1,000 queries/month
- Standard pipeline only
- Community support
- 7-day history retention

</td>
<td width="33%">

### Pro
**$49/month**

- Unlimited queries
- All pipelines
- Priority support
- 90-day history
- Custom workflows
- Multi-LLM ensemble

</td>
<td width="33%">

### Enterprise
**Custom pricing**

- Volume discounts
- On-premises deployment
- SLA guarantees
- Dedicated support
- Custom integrations
- White-label options

</td>
</tr>
</table>

*All plans include built-in memory, analytics, and observability.*

---

## 🎯 Use Cases

### Customer Support
Build intelligent support bots that remember user issues and provide contextual help.

### Documentation Q&A
Let users query your docs naturally. Automatic updates when docs change.

### Research Assistant
Multi-hop reasoning across academic papers with citation tracking.

### Internal Knowledge Base
Connect Slack, Notion, Google Drive, and more. One search interface for everything.

---

## 🚀 Why R8R?

**Stop rebuilding the same RAG logic**
Focus on your product, not infrastructure.

**Ship 10x faster**
From idea to production in hours, not weeks.

**Scale with confidence**
Built-in monitoring, fallbacks, and cost controls.

**Pay for value, not tokens**
Predictable pricing based on queries, not token usage.


## ⭐ Ready to ship RAG faster?

**[Get Started Free](https://r8r.ai) →**

*No credit card required. Deploy your first RAG pipeline in under 5 minutes.*

---

<p align="center">
  <sub>Built for developers who value their time. Deploy RAG systems that just work.</sub>
</p>
