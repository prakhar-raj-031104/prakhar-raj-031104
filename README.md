<!-- Profile README · github.com/prakhar-raj-031104 -->

<a href="https://agentline.cloud">
  <img width="100%" src="https://capsule-render.vercel.app/api?type=waving&color=gradient&customColorList=12,20,24,6&height=200&section=header&text=Prakhar%20Raj%20Singhal&fontSize=44&fontColor=ffffff&animation=fadeIn&fontAlignY=36&desc=Founding%20Engineer%20%40%20AgentLine%20%C2%B7%20Backend%20%26%20Infrastructure%20%C2%B7%20Developer%20Platforms%20for%20AI%20Agents&descSize=16&descAlignY=58" alt="Prakhar Raj Singhal"/>
</a>

<p align="center">
  <a href="https://agentline.cloud"><img src="https://img.shields.io/badge/AgentLine-Founding%20Engineer-4FE3D0?style=for-the-badge&logo=phone&logoColor=black" alt="AgentLine"/></a>
  <a href="https://github.com/AgentLineHQ/AgentLine"><img src="https://img.shields.io/github/stars/AgentLineHQ/AgentLine?style=for-the-badge&logo=github&label=AgentLine%20OSS&color=131822" alt="AgentLine stars"/></a>
  <a href="https://www.linkedin.com/in/praiit"><img src="https://img.shields.io/badge/LinkedIn-praiit-0A66C2?style=for-the-badge&logo=linkedin&logoColor=white" alt="LinkedIn"/></a>
  <a href="mailto:prakhar@agentline.cloud"><img src="https://img.shields.io/badge/Email-prakhar%40agentline.cloud-EA4335?style=for-the-badge&logo=gmail&logoColor=white" alt="Email"/></a>
</p>

I build infrastructure that other developers (and their AI agents) build on. I like owning a thing end to end: deciding what to build, shipping it to production, watching how people actually use it, and fixing what's wrong.

Right now that thing is **[AgentLine](https://agentline.cloud)** — an open-source phone + SMS API for AI agents. Before that: production WhatsApp Business API and payment flows for B2B clients, a Go job queue that does 3k jobs/s, and a P2P logistics startup that made it to Shark Tank India round 2.

---

## What I'm building now

### ☎️ [AgentLine](https://github.com/AgentLineHQ/AgentLine) — give any AI agent a real phone number
`FastAPI` `PostgreSQL` `Redis` `WebSockets` `MCP` `Docker` `SignalWire / Twilio / Plivo / Telnyx` `Deepgram` `Cartesia` `LiveKit / Pipecat` · **MIT**

One API for outbound/inbound PSTN calls, SMS, IVR navigation and transcripts, consumed over REST or MCP from Claude Code, Cursor, Codex, OpenClaw and Hermes. Self-host with one `docker-compose up`, or use the hosted version at [agentline.cloud](https://agentline.cloud).

What I own in it:

| Area | What's there |
|---|---|
| **Core platform** | Async FastAPI over Postgres + Redis, hashed API keys, auth middleware, migrations, one-command Docker Compose, Railway deploy |
| **Developer surface** | REST + OpenAPI, built-in MCP server (14 tools), one-file installable [skill](https://agentline.cloud/skill.md), Python (`agentline-ai`) & Node clients, `/.well-known/agentline.json` discovery |
| **Reliable delivery** | Persistent outbound WebSocket relay with reconnect + ACK/replay + turn-safe context, HMAC-signed per-agent webhooks as fallback, durable event mailbox — no caller turn is dropped if your runtime is offline |
| **Voice pipeline** | Deepgram STT → OpenAI-compatible LLM → Cartesia TTS on the carrier media socket; semantic end-of-turn detection, barge-in, real DTMF for IVR menus; pluggable runtimes (built-in / LiveKit / Pipecat) |
| **Telephony** | Provider registry + router with per-country routing and automatic failover across four carriers |

```
Your agent (Claude, Cursor, Codex, OpenClaw…)
   └─ REST / MCP ─▶ AgentLine API ─▶ carrier (SignalWire · Twilio · Plivo · Telnyx) ─▶ PSTN
                        │
                        └─ events & transcripts ─▶ relay WS / signed webhook / mailbox
```

---

## Proof of work

> Numbers below are measured, and the repos include the commands to reproduce them.

#### ⚙️ [TaskForge](https://github.com/prakhar-raj-031104/TaskForge) — distributed job queue on Go + PostgreSQL, no broker
`Go` `PostgreSQL` `Docker` `Prometheus / Grafana` `k6`

- **3,000+ jobs/s** in local load tests; `SELECT … FOR UPDATE SKIP LOCKED` + fencing tokens → **zero duplicate claims** across 200 jobs × 16 workers (tested under `-race`)
- Partial index cut claim latency **31 ms → 0.22 ms**; idempotency keys enforced by a partial unique index
- Retries with equal-jitter backoff, dead-letter queue with replay, lease renewal, crash recovery (SIGKILL a worker mid-job, another reclaims it)
- DNS-level SSRF prevention for outbound webhook jobs; 28 MB distroless image; Go stdlib HTTP only

#### 💬 WhatsApp Business API + payments in production — Miar International
`TypeScript` `Node / Express` `PostgreSQL / Prisma` `React`

- Real-time booking with WhatsApp template notifications and delivery-status webhooks for live B2B clients
- Razorpay HMAC-SHA256 webhook verification + an idempotent order state machine — no duplicate bookings or charges under concurrent load
- JWT auth, RBAC, input validation, rate limiting on every public route

#### 📦 [RaahiKart](https://raahikart.in) — co-founder · live P2P parcel logistics · 🦈 Shark Tank India Round 2
`Node.js` `PostgreSQL` `WebSockets` `JWT` `Docker` `CI/CD`

- Real-time route matching over WebSockets, 100+ concurrent users; took it from zero to first paying users

#### 🔗 Domino — change-impact intelligence for a whole repo
`FastAPI` `NetworkX` `LangChain`

- 4-layer dependency graph (files → calls → classes → services) → a 0–100 blast-radius score, affected APIs and a recommended test set before you deploy

#### 🧠 Cortex AI — always-on multimodal agent backend
`asyncio` `Whisper` `Qwen2.5-VL` `Groq` `RAG`

- Concurrent voice + vision agent sustaining 50+ streams; sub-200 ms gateway routing; episodic memory at 94% factual consistency (RAGAS)

#### 💊 [SmartAMR](https://github.com/prakhar-raj-031104/Smart-AMR) — clinical decision support with grounded RAG
`FastAPI` `XGBoost + SHAP` `FAISS + BM25` `LangChain`

- Hybrid retrieval grounds every recommendation in cited guidelines; 3-provider LLM fallback chain (Groq → Gemini → OpenRouter) with 429-aware retries

#### 📡 Research — agentic LLMs for 5G/6G network diagnostics (IIT Bhilai, paper under review)
- PCAP Analysis Agent: 80+ deterministic diagnostic tests fused with an LLM loop that writes Wireshark filters, with a validator → rewriter loop and headless TShark re-checks; benchmarked 6 LLMs on 3GPP understanding (best 84.1%)

---

## How I work

- **Decide → build → ship → iterate.** At AgentLine the whole engineering surface is a few people, so "someone else will handle it" is not an option. I'd rather own a feature from the issue to the changelog.
- **Developer experience is a product feature.** If an agent can't install it from one URL, or a developer can't get a first message/call working in five minutes, it isn't done.
- **Reliability is the boring stuff done consistently.** Idempotency keys, signed webhooks, ACK/replay, backoff with jitter, dead letters, and measuring before claiming a number.
- **AI-first, not AI-only.** I use coding agents daily and build the MCP servers / skill files they consume — but the LLM sits behind deterministic validation, not in front of it.

## Stack

**Languages** — TypeScript / Node, Python, Go, SQL, C++

**Platforms** — REST & OpenAPI, webhooks (signing · retries · idempotency), WebSockets, MCP servers, SDK & CLI design, WhatsApp Business API, telephony (PSTN carriers, SIP)

**Infra** — PostgreSQL, Redis, Kafka, Docker, AWS (EC2, S3), Nginx, GitHub Actions, Prometheus / Grafana, k6, Linux

**AI** — tool-calling agents, LangGraph / LangChain, RAG (Qdrant, FAISS), evals (RAGAS), PyTorch, LoRA / QLoRA

---

<details>
<summary><b>More</b> — education, community, stats</summary>
<br/>

- 🎓 **IIT Bhilai** — B.Tech, Electronics & Communication Engineering (2024–2028)
- 🧑‍🏫 AI/ML Lead, Google Developer Groups IIT Bhilai — mentoring 30+ students on LLMs, RAG and agents
- 🏆 AlgoUniversity — selected nationally from 70,000+ students · Inter-IIT Tech Meet quant strategy (Sharpe 1.5–2.0)
- 📚 ICLR 2025 & NAACL 2025 paper review and reproduction studies

<p align="center">
  <img height="160" src="https://github-readme-stats.vercel.app/api?username=prakhar-raj-031104&show_icons=true&theme=tokyonight&hide_border=true&count_private=true&include_all_commits=true" alt="GitHub stats"/>
  <img height="160" src="https://github-readme-stats.vercel.app/api/top-langs/?username=prakhar-raj-031104&layout=compact&theme=tokyonight&hide_border=true&langs_count=8" alt="Top languages"/>
</p>

<picture>
  <source media="(prefers-color-scheme: dark)" srcset="https://raw.githubusercontent.com/prakhar-raj-031104/prakhar-raj-031104/output/github-snake-dark.svg"/>
  <source media="(prefers-color-scheme: light)" srcset="https://raw.githubusercontent.com/prakhar-raj-031104/prakhar-raj-031104/output/github-snake.svg"/>
  <img width="100%" alt="Contribution snake" src="https://raw.githubusercontent.com/prakhar-raj-031104/prakhar-raj-031104/output/github-snake-dark.svg"/>
</picture>

</details>

<p align="center">
  <sub>📫 <a href="mailto:prakhar@agentline.cloud">prakhar@agentline.cloud</a> · <a href="https://www.linkedin.com/in/praiit">LinkedIn</a> · <a href="https://agentline.cloud">agentline.cloud</a></sub>
</p>
