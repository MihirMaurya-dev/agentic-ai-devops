# Agentic AI for DevOps — 16-Week Roadmap

A week-by-week plan from Python fundamentals to production-style DevOps agents. Pacing assumes roughly 8–10 hours a week alongside your other commitments — compress weeks where you already have a head start (especially Week 1, if your Python is already solid), and don't hesitate to stretch a hard week into two. The order matters more than the calendar.

You already bring Terraform, Docker, Kubernetes, CI/CD (Jenkins/GitHub Actions), Linux/Bash, and monitoring (Grafana/Prometheus) to this — Phase 4 is built entirely around putting that experience to work, which is exactly the part most people learning agentic AI don't have.

---

## Phase 1 — Foundations (Weeks 1–2)

### Week 1 — Python for agent-building
- [ ] Done

You don't need advanced Python, but agents live and die by clean function calls, JSON handling, and API requests. Broken into a day-by-day path:

**Day 1–2 — Type hints, functions, error handling**
Agents rely on strict data structures — type hints work like a schema, and a failing network call should never be allowed to crash the whole pipeline.
- Resources: Real Python's "Python Type Checking (Guide)," plus any Try/Except walkthrough (official Python docs or W3Schools both work)
- Mini-exercise:
  ```python
  def read_log_file(path: str) -> list[str]:
      try:
          with open(path, 'r') as file:
              return file.readlines()
      except FileNotFoundError:
          print(f"Error: Log file missing at {path}")
          return []
  ```

**Day 3 — JSON**
LLMs communicate exclusively in strings; Python thinks in dictionaries. Getting fast at converting between the two is the actual translation layer your agents will run on.
- Resource: Corey Schafer's "Python Tutorial: Working with JSON Data using the json Module" (YouTube)
- Key calls: `json.dumps(dict)` turns a dictionary into a JSON string (what you send to an LLM or API); `json.loads(string)` turns a JSON string back into a dictionary (so you can pull `data["status"]` out of a response)

**Day 4 — API calls with `requests`**
This is how your agents will read from the outside world — Kubernetes, cloud APIs, GitHub.
- Resource: Corey Schafer's "Python Requests Tutorial: Request Web Pages, Download Images, POST Data, Read JSON, and More" — skip the image-download and BeautifulSoup sections, focus only on `GET`/`POST` calls and JSON responses
- Key concepts: HTTP status codes (200, 404, 401), and `response.raise_for_status()` to fail loudly instead of silently

**Day 5 — Async/await**
When an agent checks five Kubernetes pods, synchronous code checks them one at a time; async hits all five at once.
- Resource: ArjanCodes' "Next-level Python: Asyncio explained simply" (there isn't a dedicated Corey Schafer beginner asyncio video)
- Key concepts: `async def` declares a coroutine; `await` pauses that function until a call completes, without blocking everything else

**Build — Local System Monitor API Client**
Try this without copying pre-made scripts — it's the real test of whether you're ready for Week 2:
1. Write an async script that fetches two endpoints at once: Open-Meteo's weather API (`https://api.open-meteo.com/v1/forecast?latitude=35.6895&longitude=139.6917&current_weather=true`) and CoinGecko's keyless price endpoint (`https://api.coingecko.com/api/v3/simple/price?ids=bitcoin&vs_currencies=usd`) — no API key needed for either (the classic CoinDesk BPI endpoint you may see in older tutorials was deprecated in 2022, so CoinGecko replaces it here)
2. Type-hint every function
3. Catch network/timeout errors cleanly, without crashing
4. Pretty-print the combined result with `json.dumps(..., indent=2)`

### Week 2 — LLM fundamentals and raw API calls
- [ ] Done

Before any framework hides the mechanics from you, get a direct feel for how a model actually behaves.

- **Learn:** tokens, context windows, temperature and top-p, the difference between system and user prompts, why models hallucinate
- **Build:** get an Anthropic or OpenAI API key and write a small CLI chatbot using nothing but raw HTTP calls — no SDK, no framework. Run the same prompt at temperature 0 and temperature 1 and actually look at the difference.

---

## Phase 2 — Core Agentic Concepts (Weeks 3–5)

### Week 3 — The agent loop and tool use
- [ ] Done

This is the week "agentic" stops being an abstract word.

- **Learn:** the ReAct pattern (reason → act → observe → repeat), and function/tool calling — how a model decides to invoke a function and what to do with the result
- **Build:** add one real tool to last week's chatbot (a "get current time" or "run a read-only shell command" tool) using function calling. Watch the model decide, on its own, when to call it.

### Week 4 — Model Context Protocol (MCP), part 1
- [ ] Done

MCP matters more for your DevOps direction than any single framework — treat this week as non-negotiable.

- **Learn:** the client-server architecture, and MCP's three primitives (tools, resources, prompts)
- **Resource:** the free MCP course from Anthropic Academy or DeepLearning.AI ("MCP: Build Rich-Context AI Apps with Anthropic," about 90 minutes)
- **Build:** your first MCP server — something simple like a "system info" or "todo list" server — connected to Claude Desktop

### Week 5 — MCP part 2, plus a first real tool wrap
- [ ] Done

Go from "toy server" to something that touches real infrastructure.

- **Learn:** building an MCP client (not just consuming one), testing with the MCP Inspector, and just enough RAG/vector-database theory to recognize when you'd need it (searching large unstructured docs — useful to know, less central to what you're building)
- **Build:** wrap one real, read-only `kubectl` command (`get pods`, `get events`) as an MCP tool, called from Claude Desktop against a local cluster (minikube or kind)

---

## Phase 3 — Agent Frameworks (Weeks 6–9)

### Week 6 — Vendor SDK: single-agent
- [ ] Done

Frameworks add power and overhead in equal measure — start with the lightest option.

- **Learn:** the Claude Agent SDK or OpenAI Agents SDK (pick one — Claude Agent SDK pairs naturally with the MCP work you just did). Its agent loop, hooks, and how tools plug in.
- **Build:** rebuild your Week 3 tool-using chatbot with the SDK instead of raw API calls. Notice exactly what the SDK now does for you that you were doing by hand.

### Week 7 — CrewAI: multi-agent, role-based
- [ ] Done

Your first taste of agents working together, not just one agent working alone.

- **Learn:** agents (role, goal, backstory), tasks, crews, process types (sequential vs. hierarchical)
- **Resource:** DeepLearning.AI's free "Multi AI Agent Systems with crewAI," taught by crewAI's founder
- **Build:** a 2–3 agent crew on a non-DevOps warm-up task first (research → draft → review), so you learn the framework's mechanics before pointing it at infrastructure

### Week 8 — LangGraph: stateful and production-grade
- [ ] Done

Where CrewAI is fast to prototype, LangGraph gives you the control production incident-response systems actually need.

- **Learn:** graphs, nodes and edges, conditional branching, state persistence/checkpointing, human-in-the-loop pauses
- **Resource:** DeepLearning.AI's free "AI Agents in LangGraph," taught by LangChain's founder
- **Build:** rebuild your Week 3 ReAct agent as a LangGraph graph, and add one deliberate checkpoint that pauses for human approval before a "risky" action

### Week 9 — Consolidate and choose your stack
- [ ] Done

- **Learn:** nothing new — instead, compare your four implementations (raw API → SDK → CrewAI → LangGraph) side by side and notice where each one earned its extra complexity
- **Build:** pick the framework you'll lean on for Phase 4 (a common split: LangGraph for anything needing branching and an audit trail, CrewAI for quick multi-role prototypes) and refactor one earlier project into it

---

## Phase 4 — DevOps-Specific Agentic AI (Weeks 10–15, then ongoing)

This is the phase that actually differentiates you. Almost nobody else applying for fresher DevOps roles will have built this exact combination.

### Week 10 — Study the existing tools
- [ ] Done

Learn from people who've already solved pieces of this problem before you build your own.

- **Do:** install K8sGPT and run it against a local cluster; try kubectl-ai for conversational troubleshooting; read K8sGPT's MCP documentation to see how a real project exposes cluster operations as MCP tools

### Week 11 — Build: Kubernetes troubleshooting agent (read-only)
- [ ] Done

- **Build:** your own mini K8sGPT — an MCP server exposing "get pod status," "get pod logs," and "get recent events" as read-only tools. Deliberately break something in a local cluster (bad image tag, mismatched selector) and have your agent diagnose it.

### Week 12 — Build: CI/CD failure-triage agent
- [ ] Done

- **Build:** an agent that fetches failed GitHub Actions logs (easier than Jenkins to showcase publicly — a public repo with free Actions minutes), classifies the failure (test failure vs. dependency error vs. infra issue), and drafts a plain-English explanation or PR comment.

### Week 13 — Build: Terraform plan reviewer
- [ ] Done

- **Build:** an agent that ingests `terraform plan` JSON output, flags high-risk changes (resource deletion, security group edits), and summarizes them in plain English before anyone runs `apply`. Combine simple hard rules ("always flag destroy actions") with LLM judgment for the nuanced cases.

### Week 14 — Build: incident-response assistant
- [ ] Done

The most complex build in the roadmap — give it the full week.

- **Build:** connect an agent to your existing Prometheus/Grafana setup. When an alert fires, it correlates recent logs, metrics, and deploys, then proposes a likely root cause and next diagnostic step.

### Week 15 — Add governance and orchestration
- [ ] Done

This is the week that separates a portfolio piece from a toy demo.

- **Build:** an approval gate before any write/execute action across your projects, audit logging (every reasoning step and tool call, logged somewhere), and — optionally — chain two of your agents into a simple pipeline, e.g. a triage agent that hands off to a fix-proposing agent.

*Phase 4 doesn't really end at week 15 — treat it as the phase you keep returning to as you pick up new integrations.*

---

## Phase 5 — Portfolio and Job Positioning (parallel, weeks 10–16)

Run this alongside Phase 4, not after it.

- **From week 10 onward:** document each project as you finish it — a README with an architecture diagram, a short demo GIF, and an honest section on what it doesn't handle yet
- **As you go:** turn each build into a LinkedIn post in the same style as your existing posts — "I built an agent that does X" is a strong, specific format
- **Week 16 — consolidate and apply:** polish your top 2–3 repos, update your resume/LinkedIn with the agentic AI + DevOps angle, and prepare to talk through one project in real depth — architecture, failure modes, what you'd change next. Then start applying, and lead with this combination as your differentiator.

---

*This totals 16 weeks at a steady pace. If a week runs long, let it.*
