# The ORTHO-X Multi-Agent Architecture

**Designing safe, interoperable and human-governed multi-agent systems for orthopaedic care**

ORTHO-X Agentic Architecture is an applied research and development repository demonstrating how modern multi-agent system principles can be translated into clinical orthopaedics.

The project adapts the principles and implementation patterns demonstrated in Victor Dibia's (fmr. Microsoft Core AI Principal Research Software Engineer) **Designing Multi-Agent Systems** project to the particular requirements of medicine: longitudinal patient journeys, heterogeneous clinical systems, human professional authority, evidence provenance, patient consent, safety, regulatory accountability and outcomes.

The objective is not to create an autonomous "AI doctor."

The objective is to build an architecture in which multiple specialized AI capabilities can participate safely in an orthopaedic episode of care while clinicians and patients retain clearly defined authority.

> **ORTHO-X turns heterogeneous AI capabilities into a governed digital orthopaedic care team.**

---

## Why Multi-Agent Orthopaedics?

An orthopaedic episode is already a multi-agent system.

A patient with a proximal femur fracture, for example, may interact with:

- emergency physicians;
- radiologists;
- orthopaedic surgeons;
- anesthesiologists;
- nurses;
- physiotherapists;
- rehabilitation providers;
- implant manufacturers;
- registries;
- insurers;
- outcome-measurement systems;
- hospital IT systems.

Artificial intelligence will not replace this ecosystem with one model.

Instead, hospitals will increasingly operate many specialized AI capabilities:

- imaging agents;
- fracture-classification agents;
- clinical documentation agents;
- planning systems;
- surgical video intelligence;
- navigation and robotics;
- discharge agents;
- rehabilitation agents;
- PROM collection agents;
- registry agents;
- quality-management agents;
- device-surveillance agents.

The important architectural question therefore becomes:

> **How do these capabilities collaborate around one patient, under defined authority, while maintaining safety, provenance, interoperability and accountability?**

That is the problem ORTHO-X Multi-Agent Design addresses.

---

# The ORTHO-X Model

ORTHO-X separates six fundamental responsibilities.

```text
                     PATIENT
                        │
              Patient / MUOVITI Agent
                        │
                  CASE CAPSULE
                        │
                 ORTHO-X OrthoFlow
                   ORCHESTRATOR
                        │
       ┌────────────────┼────────────────┐
       │                │                │
 Diagnostic         Treatment         Outcome
   Agents             Agents           Agents
       │                │                │
 Imaging           SurgiCoach         PROMs
 AO/OTA            Planning           ICHOM
 Risk               SurgiNaut          DISH
       │                │                │
       └────────────────┼────────────────┘
                        │
             External Agent Ecosystem
                        │
   Hospital • EHR • PACS • Robot • Registry
     MedTech • Payer • Research • Society
                        │
                 MCP • FHIR • A2A
                        │
               Local / Sovereign Runtime
                        │
                   ORTHO-X GCC
       Simulation • Evaluation • Validation
       Certification • Monitoring • Governance
```

## 1. OrthoFlow — orchestration

**ORTHO-X OrthoFlow** coordinates the longitudinal episode of care.

It determines:

- what needs to happen;
- which capability should perform it;
- what information may be used;
- which outputs must be produced;
- when human review is required;
- what is written into the Case Capsule;
- which subsequent step may proceed.

OrthoFlow is therefore not simply another AI agent.

It is the **clinical orchestration layer**.

Some OrthoFlow processes should remain deterministic.

Others may use bounded agentic reasoning.

High-risk decisions should require human authority.

---

## 2. OrthoSkills — bounded clinical agents

An **OrthoSkill** is a bounded, testable clinical capability.

Each OrthoSkill should define:

```yaml
purpose:
inputs:
permitted_tools:
clinical_context:
outputs:
confidence:
authority_level:
human_approval:
escalation_rule:
evidence_provenance:
evaluation_suite:
version:
```

Example:

```yaml
name: AO_OTA_Proximal_Femur_Classification
purpose: Classify proximal femoral fractures
inputs:
  - radiographs
  - relevant Case Capsule context
tools:
  - vision-language model
  - AO/OTA Fracture and Dislocation Classification Compendium 2018
  - AO Surgery Reference
output:
  - structured AO/OTA classification
  - confidence score
authority_level: advisory
human_approval: orthopaedic surgeon
escalation_rule: confidence below validated threshold
```

The objective is not maximum agent autonomy.

The objective is **maximum useful capability under clearly bounded authority**.

---

## 3. Case Capsule — shared authoritative state

Multi-agent systems become unsafe if each agent maintains a different version of the patient.

ORTHO-X therefore uses the **Case Capsule** as the longitudinal, provenance-aware state of the orthopaedic episode:
https://github.com/MAIVAN-ai/ORTHO-X-CaseCapsule

It may contain:

- patient-provided information;
- diagnoses;
- imaging;
- classifications;
- treatment decisions;
- surgical plans;
- implant and UDI information;
- operative events;
- postoperative findings;
- rehabilitation;
- complications;
- PROMs;
- clinical outcomes;
- permissions and consent;
- AI outputs;
- human confirmations;
- provenance.

Agents should interact through structured state whenever possible:

```text
READ authorized Case Capsule state
            ↓
PERFORM bounded task
            ↓
GENERATE structured result
            ↓
VALIDATE / ESCALATE
            ↓
WRITE provenance-stamped result
```

The Case Capsule therefore becomes the **transaction ledger of the agentic orthopaedic episode**.

---

## 4. Human Authority

Healthcare multi-agent systems require explicit authority.

ORTHO-X uses a simple principle:

> **AI capabilities may perform work without automatically acquiring clinical authority.**

A useful safety pattern is:

```text
ACTOR
  ↓
CRITIC / VERIFIER
  ↓
CLINICAL AUTHORITY
  ↓
RECORDER
```

For example:

1. a planning agent proposes a treatment;
2. an independent agent checks contraindications and evidence;
3. the surgeon accepts, modifies or rejects the proposal;
4. the Case Capsule records the complete decision pathway.

Different workflows can require different levels of human intervention.

---

# Deterministic Workflow Before Agentic Autonomy

ORTHO-X does not assume that adding more agents improves healthcare.

Use deterministic software where deterministic software is sufficient.

Use AI agents where reasoning, interpretation, uncertainty management or adaptive planning genuinely adds value.

A typical OrthoFlow therefore combines:

```text
RULES
+
WORKFLOWS
+
AI AGENTS
+
HUMAN AUTHORITY
```

rather than attempting:

```text
AUTONOMOUS AI
EVERYWHERE
```

This distinction is central to safe clinical deployment.

---

# MCP, FHIR and A2A

ORTHO-X separates access to resources from collaboration between intelligent services.

## MCP and APIs

Used for **Agent ↔ Tool / Data** interaction.

Examples:

- EHR;
- PACS;
- clinical databases;
- guidelines;
- registries;
- robotics;
- medical devices;
- literature;
- analytics tools.

## FHIR / DICOM

Used where appropriate for standardized healthcare data and imaging interoperability.

## A2A

Used for **Agent ↔ Agent** collaboration.

Examples:

```text
Imaging Agent
     ↓
Classification Agent
     ↓
Planning Agent
```

or across organizational boundaries:

```text
Hospital Agent
      ↔
Patient Agent
      ↔
MedTech Agent
      ↔
Registry Agent
```

The architecture is designed so that computation can remain close to the hospital's data while authorized structured results move between participants.

---

# Example: Proximal Femur Fracture

A possible multi-agent OrthoFlow:

```text
Patient / EMS Intake
        ↓
Clinical Intake Agent
        ↓
Imaging Agent
        ↓
AO/OTA Classification OrthoSkill
        ↓
Risk / Comorbidity Skill
        ↓
Treatment Planning Agent
        ↓
Surgeon Confirmation
        ↓
Surgical Planning OrthoSkill
        ↓
SurgiCorder
        ↓
Implant / UDI Agent
        ↓
Postoperative Assessment
        ↓
Discharge Agent
        ↓
Rehabilitation Agent
        ↓
PROM / ICHOM Agent
        ↓
Outcome Analysis
        ↓
Registry / PMCF / Quality Outputs
```

The entire pathway remains anchored to one Case Capsule.

---

# Evaluation: Test the Journey, Not Only the Answer

A medically acceptable multi-agent system cannot be evaluated only by asking whether its final answer happened to be correct.

ORTHO-X evaluates the **trajectory**.

Questions include:

- Did the correct information enter the workflow?
- Did the appropriate agent act?
- Did it use permitted tools?
- Was relevant evidence retrieved?
- Was uncertainty represented correctly?
- Was escalation triggered when required?
- Did the clinician receive the right information?
- Was human approval obtained?
- Was the decision recorded?
- Did execution correspond to the plan?
- What was the resulting patient outcome?

This allows evaluation at several levels:

```text
MODEL
↓
ORTHOSKILL
↓
MULTI-AGENT WORKFLOW
↓
CLINICAL EPISODE
↓
PATIENT OUTCOME
```

---

# ORTHO-X Case Arena

The **ORTHO-X Case Arena** is envisaged as a simulation and evaluation environment for these workflows:
https://github.com/MAIVAN-ai/ORTHO-X-Case-Arena-Demonstrator

Historical, synthetic and prospectively collected cases can be replayed through competing configurations.

Examples:

```text
Agent A vs Agent B
Model A vs Model B
Workflow A vs Workflow B
Human-only vs Human+AI
Hospital A vs Hospital B
Plan vs Actual
Predicted vs Observed Outcome
```

This converts clinical AI evaluation from isolated benchmark questions into **case-based clinical rehearsal**.

---

# ORTHO-X Global Collaboration Center

See https://maivan.ai/2026/09/30/eine-zukunftsperspektive-fuer-den-medtech-standort-schweiz-ortho-x-global-collaboration-center/

The ORTHO-X Global Collaboration Center provides the wider development and governance environment in which OrthoSkills and agentic workflows can be:

```text
DESIGNED
   ↓
SIMULATED
   ↓
EVALUATED
   ↓
CLINICALLY VALIDATED
   ↓
CERTIFIED
   ↓
DEPLOYED
   ↓
MONITORED
   ↓
IMPROVED
```

The GCC is therefore not primarily a centralized data repository.

It is an **agentic clinical engineering, evaluation and governance environment**.

Sensitive clinical information can remain within sovereign hospital environments while models, skills, evaluation procedures and authorized outputs can participate in wider collaborative networks.

---

# Patient Agency

ORTHO-X also considers the patient an active participant in the multi-agent architecture.

A patient-facing agent — for example through MUOVITI.life — may:

- maintain preferences;
- manage permissions;
- carry the longitudinal Case Capsule;
- collect PROMs;
- explain care pathways;
- support second opinions;
- coordinate appointments;
- assist rehabilitation;
- return outcomes to clinicians;
- authorize secondary use of information.

This leads to an important architectural principle:

> **The patient should not merely be a data source consumed by other agents. The patient should be represented by an agent of their own.**

---

# Design Principles

ORTHO-X Multi-Agent Design follows ten principles:

1. **Patient-centered state**
2. **Human clinical authority**
3. **Bounded agent capabilities**
4. **Deterministic workflows where sufficient**
5. **Explicit uncertainty and escalation**
6. **Interoperability instead of vendor lock-in**
7. **Provenance by default**
8. **Sovereign execution where required**
9. **Trajectory-based evaluation**
10. **Patient outcomes as the ultimate performance measure**

---

# Repository Roadmap

Planned ORTHO-X extensions include:

```text
orthox/
├── agents/
│   ├── intake/
│   ├── imaging/
│   ├── classification/
│   ├── planning/
│   ├── surgery/
│   ├── outcomes/
│   └── patient/
│
├── orthoflow/
│   ├── workflows/
│   ├── orchestration/
│   └── human_gates/
│
├── case_capsule/
│   ├── schemas/
│   ├── provenance/
│   └── permissions/
│
├── interoperability/
│   ├── mcp/
│   ├── fhir/
│   ├── dicom/
│   └── a2a/
│
├── case_arena/
│   ├── scenarios/
│   ├── simulation/
│   └── trajectory_eval/
│
└── examples/
    ├── proximal_femur/
    ├── tibial_plateau/
    ├── arthroplasty/
    └── surgical_site_outcomes/
```

---

# Clinical Development Lifecycle

ORTHO-X proposes progressive levels of deployment:

```text
DEVELOPMENT
      ↓
SIMULATION
      ↓
SILENT MODE
      ↓
SUPERVISED MODE
      ↓
VALIDATED USE
      ↓
CERTIFIED / GOVERNED USE
```

Authority should expand only as evidence supports it.

---

# Open Ecosystem

ORTHO-X is intended to orchestrate capabilities from multiple organizations rather than require a single AI technology stack.

Potential participants include:

- hospitals;
- universities;
- professional societies;
- MedTech companies;
- AI companies;
- EHR providers;
- medical-device manufacturers;
- registries;
- outcome organizations;
- regulators;
- payers;
- patient organizations.

The long-term goal is an open, interoperable infrastructure for **Surgical Intelligence**.

---

# Upstream Project and Attribution

This project is derived from and inspired by Victor Dibia's open-source repository:

`victordibia/designing-multiagent-systems`

and the accompanying book:

**Designing Multi-Agent Systems: Principles, Patterns, and Implementation for AI Agents.**

The upstream project provides general-purpose implementations and educational examples for understanding agents, workflows, orchestration, evaluation and production multi-agent systems.

ORTHO-X extends these concepts toward healthcare and orthopaedic applications.

Please retain all upstream copyright notices, attribution and license requirements.

---

# License

Upstream components remain subject to their original Apache License 2.0 terms.

ORTHO-X-specific additions should identify their applicable licensing terms separately where appropriate.

---

# Disclaimer

This repository is a research, architecture and software-development environment.

Nothing contained here should be interpreted as medical advice or as authorization for unsupervised clinical use.

Any clinical deployment requires appropriate validation, governance, regulatory assessment and professional oversight.

---

# ORTHO-X

**One patient. One Case Capsule. Many specialized agents. Human authority. Measurable outcomes.**

From isolated AI applications toward interoperable Surgical Intelligence.

Finally:
# Kudos to https://github.com/victordibia/
Here's where this all has to be credited to:
# Designing Multi-Agent Systems

Official code repository for [Designing Multi-Agent Systems: Principles, Patterns, and Implementation for AI Agents](https://buy.multiagentbook.com/?utm_source=github&utm_medium=readme) by [Victor Dibia](https://victordibia.com).

[![Open in GitHub Codespaces](https://github.com/codespaces/badge.svg)](https://codespaces.new/victordibia/designing-multiagent-systems?quickstart=1)

[![Designing Multi-Agent Systems](./docs/images/bookcover.png)](https://buy.multiagentbook.com/?utm_source=github&utm_medium=readme)

Learn to build effective multi-agent systems from first principles through complete, tested implementations. This repository includes **PicoAgents**—a full-featured multi-agent framework built entirely from scratch for the sole purpose of teaching you how multi-agent systems work. Every component, from agent reasoning loops to orchestration patterns, is implemented with clarity and transparency.

[Buy Digital Edition](https://buy.multiagentbook.com/?utm_source=github&utm_medium=readme) | [Paperback on Amazon](https://www.amazon.com/dp/B0G2BCQQJY) | [Hardcover on Amazon](https://www.amazon.com/dp/B0G2F6T2BZ)

---

## Why This Book & Code Repository?

As the AI agent space evolves rapidly, clear patterns are emerging for building effective multi-agent systems. This book focuses on identifying these patterns and providing practical guidance for applying them effectively.

**What makes this approach unique:**

- **Fundamentals-first**: Build from scratch to understand every component and design decision
- **Complete implementations**: Every theoretical concept backed by working, tested code
- **Framework-agnostic**: Core patterns that transcend any specific framework (avoids the lock in or outdated api issue common with books that focus on a single framework)
- **Production considerations**: Evaluation, optimization, and deployment guidance from real-world experience

## What You'll Learn & Build

The book is organized across 4 parts, taking you from theory to production:

### Part I: Foundations of Multi-Agent Systems

| Chapter  | Title                                        | Code                                                                              | Learning Outcome                                         |
| -------- | -------------------------------------------- | --------------------------------------------------------------------------------- | -------------------------------------------------------- |
| **Ch 1** | Understanding Multi-Agent Systems            | Poet/critic example, references [`yc_analysis/`](examples/workflows/yc_analysis/) | Understand when multi-agent systems are needed           |
| **Ch 2** | Multi-Agent Patterns                         | -                                                                                 | Master coordination strategies (workflows vs autonomous) |
| **Ch 3** | UX Design Principles for Multi-Agent Systems | -                                                                                 | Principles for building intuitive agent interfaces       |

### Part II: Building Multi-Agent Systems from Scratch

| Chapter  | Title                                 | Code                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                         | Learning Outcome                                                                             |
| -------- | ------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | -------------------------------------------------------------------------------------------- |
| **Ch 4** | Building Your First Agent             | [`agents/_agent.py`](picoagents/src/picoagents/agents/_agent.py), [`basic-agent.py`](examples/agents/basic-agent.py), [`memory.py`](examples/agents/memory.py), [`middleware.py`](examples/agents/middleware.py), [`structured-output.py`](examples/agents/structured-output.py), [`agent_as_tool.py`](examples/agents/agent_as_tool.py), [`otel/`](examples/otel/), [`memory/`](examples/memory/), [`tools/approval_example.py`](examples/tools/approval_example.py) <br> [![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/victordibia/designing-multiagent-systems/blob/main/examples/notebooks/01_basic_agent.ipynb) | Build agents with tools, memory, streaming, middleware, observability, and human-in-the-loop |
| **Ch 5** | Computer Use Agents                   | [`agents/_computer_use/`](picoagents/src/picoagents/agents/_computer_use/), [`computer_use.py`](examples/agents/computer_use.py)                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                             | Build browser automation agents with multimodal reasoning                                    |
| **Ch 6** | Building Multi-Agent Workflows        | [`workflow/`](picoagents/src/picoagents/workflow/), [`workflows/`](examples/workflows/)                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                      | Build type-safe workflows with streaming observability                                       |
| **Ch 7** | Autonomous Multi-Agent Orchestration  | [`orchestration/`](picoagents/src/picoagents/orchestration/), [`round-robin.py`](examples/orchestration/round-robin.py), [`ai-driven.py`](examples/orchestration/ai-driven.py), [`plan-based.py`](examples/orchestration/plan-based.py)                                                                                                                                                                                                                                                                                                                                                                                                                                                      | Implement GroupChat, LLM-driven, and plan-based orchestration (Magentic One patterns)        |
| **Ch 8** | Building Modern Agent UX Applications | [`app/`](examples/app/) (minimal FastAPI+SSE example), [`webui/`](picoagents/src/picoagents/webui/) (production React UI)                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                    | Build interactive agent applications with web UI, auto-discovery, and real-time streaming    |
| **Ch 9** | Multi-Agent Frameworks                | [`frameworks/`](examples/frameworks/) (Microsoft Agent Framework, Google ADK, LangGraph comparisons)                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                         | Evaluate and choose the right multi-agent framework                                          |

### Part III: Evaluating and Optimizing Multi-Agent Systems

| Chapter   | Title                          | Code                                                                                                         | Learning Outcome                                          |
| --------- | ------------------------------ | ------------------------------------------------------------------------------------------------------------ | --------------------------------------------------------- |
| **Ch 10** | Evaluating Multi-Agent Systems | [`eval/`](picoagents/src/picoagents/eval/), [`agent-evaluation.py`](examples/evaluation/agent-evaluation.py) | Build evaluation frameworks with LLM-as-judge and metrics |

### Part IV: Real-World Applications

| Chapter   | Title                                     | Code                                              | Learning Outcome                                                                         |
| --------- | ----------------------------------------- | ------------------------------------------------- | ---------------------------------------------------------------------------------------- |
| **Ch 14** | Business Questions from Unstructured Data | [`yc_analysis/`](examples/workflows/yc_analysis/) | Production case study: Analyze 5,000+ companies with cost optimization and checkpointing |
| **Ch 17** | Software Engineering Agent                | [`swe_agent/`](examples/agents/swe_agent/)        | Build a complete software engineering agent with coding tools and workspace management   |

## Getting Started

### Option 1: Interactive Notebooks

Click Colab badges in the chapter tables above to run examples in your browser. No installation required.

### Option 2: GitHub Codespaces

<a href="https://codespaces.new/victordibia/designing-multiagent-systems?quickstart=1" target="_blank"><img src="https://github.com/codespaces/badge.svg" alt="Open in GitHub Codespaces"></a>

Pre-configured development environment in your browser. Once open:

1. Add your API key: `export OPENAI_API_KEY='your-key'`
2. Run examples: `python examples/agents/basic-agent.py`
3. Launch Web UI: `picoagents ui`

Free tier: 60 hours/month

### Option 3: Local Installation

```bash
# Clone the repository
git clone https://github.com/victordibia/designing-multiagent-systems.git
cd designing-multiagent-systems

# Navigate to the PicoAgents framework directory
cd picoagents

# Create virtual environment (recommended)
python -m venv venv
source venv/bin/activate  # On Windows: venv\Scripts\activate

# Basic installation
pip install -e .

# Or install with optional features
pip install -e ".[web]"           # Web UI and API server
pip install -e ".[mcp]"           # MCP client and playground (mcp>=2.0.0)
pip install -e ".[persist]"       # Run/eval persistence behind the History page
pip install -e ".[computer-use]"  # Browser automation
pip install -e ".[examples]"      # Run example scripts
pip install -e ".[all]"           # Most extras (not persist, otel, dev, frameworks)

# Set up your API key
export OPENAI_API_KEY="your-api-key-here"
```

### Quick Start: Your First Agent

In this book, we will cover the fundamentals of building multi-agent systems, and incrementally build up the `Agents` abstractions shown below:

```python
from picoagents import Agent, OpenAIChatCompletionClient

def get_weather(location: str) -> str:
    """Get current weather for a given location."""
    return f"The weather in {location} is sunny, 75°F"

# Create an agent
agent = Agent(
    name="assistant",
    instructions="You are helpful. Use tools when appropriate.",
    model_client=OpenAIChatCompletionClient(model="gpt-4.1-mini"),
    tools=[get_weather]
)

# Use the agent
response = await agent.run("What's the weather in Paris?")
print(response.messages[-1].content)
```

**Want a simpler starting point?** The [`code_along/`](code_along/) directory builds a minimal agent from zero in four progressive steps: [core agent loop](code_along/ch04_v1_agent.py) → [tool calling](code_along/ch04_v2_tools.py) → [memory](code_along/ch04_v3_memory.py) → [streaming](code_along/ch04_v4_streaming.py). PicoAgents is an expanded, production-ready version of the same ideas.

### Model Client Setup

PicoAgents supports multiple LLM providers through a unified interface. Each provider requires minimal setup—just API credentials and switching the client class. Chapter 4 covers building custom model clients for any provider.

| Provider          | Client Class                                                                             | Setup                                                                                                                                                                              | Example                                                          | Source                                                               |
| ----------------- | ---------------------------------------------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ---------------------------------------------------------------- | -------------------------------------------------------------------- |
| **OpenAI**        | [`OpenAIChatCompletionClient`](picoagents/src/picoagents/llm/_openai.py)                 | 1. Get API key from [platform.openai.com](https://platform.openai.com)<br>2. `export OPENAI_API_KEY='sk-...'`                                                                      | [`basic-agent.py`](examples/agents/basic-agent.py)               | [`_openai.py`](picoagents/src/picoagents/llm/_openai.py)             |
| **Azure OpenAI**  | [`AzureOpenAIChatCompletionClient`](picoagents/src/picoagents/llm/_azure_openai.py)      | 1. Deploy model on [Azure Portal](https://portal.azure.com)<br>2. Set endpoint, key, deployment name                                                                               | See [`swe_agent/agent.py`](examples/agents/swe_agent/agent.py)   | [`_azure_openai.py`](picoagents/src/picoagents/llm/_azure_openai.py) |
| **Anthropic**     | [`AnthropicChatCompletionClient`](picoagents/src/picoagents/llm/_anthropic.py)           | 1. Get API key from [console.anthropic.com](https://console.anthropic.com)<br>2. `export ANTHROPIC_API_KEY='sk-...'`                                                               | [`agent_anthropic.py`](examples/agents/agent_anthropic.py)       | [`_anthropic.py`](picoagents/src/picoagents/llm/_anthropic.py)       |
| **GitHub Models** | [`OpenAIChatCompletionClient`](picoagents/src/picoagents/llm/_openai.py)<br>+ `base_url` | 1. Get token from [github.com/settings/tokens](https://github.com/settings/tokens)<br>2. `export GITHUB_TOKEN='ghp_...'`<br>3. Set `base_url="https://models.github.ai/inference"` | [`agent_githubmodels.py`](examples/agents/agent_githubmodels.py) | Uses [`_openai.py`](picoagents/src/picoagents/llm/_openai.py)        |
| **Local/Custom**  | [`OpenAIChatCompletionClient`](picoagents/src/picoagents/llm/_openai.py)<br>+ `base_url` | Point to any OpenAI-compatible endpoint<br>(Ollama, LM Studio, vLLM, etc.)                                                                                                         | Use `base_url="http://localhost:8000"`                           | Uses [`_openai.py`](picoagents/src/picoagents/llm/_openai.py)        |

**Quick Examples:**

```python
# OpenAI (default)
from picoagents import OpenAIChatCompletionClient
client = OpenAIChatCompletionClient(model="gpt-4.1-mini")

# Anthropic
from picoagents import AnthropicChatCompletionClient
client = AnthropicChatCompletionClient(model="claude-3-5-sonnet-20241022")

# GitHub Models (free tier)
client = OpenAIChatCompletionClient(
    model="openai/gpt-4.1-mini",
    api_key=os.getenv("GITHUB_TOKEN"),
    base_url="https://models.github.ai/inference"
)

# Local LLM (e.g., Ollama)
client = OpenAIChatCompletionClient(
    model="llama3.2",
    base_url="http://localhost:11434/v1"
)
```

### Launch the Web UI

![PicoAgents Web UI](./docs/images/picoagents_screenshot.png)

```bash
# Auto-discover agents, orchestrators, and workflows in current directory
picoagents ui

# Or specify a directory
picoagents ui --dir ./examples
```

The Web UI discovers the agents, orchestrators, and workflows in your codebase and gives you a place to run them: streaming chat, a live debug rail, and recorded run history.

It also includes an **MCP Playground** for connecting to MCP servers, invoking their tools, and reading the raw JSON-RPC traffic, plus an **evaluation dashboard** for datasets, targets, and batch runs. Five demo MCP servers ship with the package, covering tools, mid-call input, notifications, interactive UIs, and OAuth-protected access.

### Run Examples

Examples are now at the root level for easy access:

```bash
# Basic agent with tools (Chapter 4)
python examples/agents/basic-agent.py

# Browser automation agent (Chapter 5)
python examples/agents/computer_use.py

# Autonomous orchestration (Chapter 7)
python examples/orchestration/round-robin.py
python examples/orchestration/ai-driven.py

# Production workflow (Chapter 14)
python examples/workflows/yc_analysis/workflow.py
```

## PicoAgents Framework

This repository is organized into two main components:

### 1. Framework Source ([`picoagents/`](picoagents/))

Complete multi-agent framework built from scratch:

```
picoagents/
├── src/picoagents/
│   ├── agents/            # Core Agent implementation (Ch 4)
│   │   ├── _agent.py      # Complete agent with streaming, tools, memory
│   │   └── _computer_use/ # Browser automation agents (Ch 5)
│   ├── workflow/          # Type-safe workflow engine (Ch 5)
│   │   ├── core/          # DAG-based execution with streaming
│   │   └── steps/         # Reusable workflow steps
│   ├── orchestration/     # Autonomous coordination (Ch 7)
│   │   ├── _round_robin.py  # Sequential turn-taking
│   │   ├── _ai.py           # LLM-driven speaker selection
│   │   └── _plan.py         # Plan-based orchestration (Magentic One)
│   ├── tools/             # 15+ built-in tools (core, research, coding)
│   ├── eval/              # Evaluation framework (Ch 10)
│   │   ├── judges/        # LLM-as-judge, reference-based
│   │   └── _runner.py     # Test execution and metrics
│   ├── webui/             # Web UI, MCP playground (Ch 8, Ch 12)
│   ├── llm/               # OpenAI and Azure clients
│   ├── memory/            # Memory implementations
│   ├── termination/       # 9 termination conditions
│   └── middleware/        # Extensible middleware system
└── tests/                 # Comprehensive test suite

### 2. Examples ([`examples/`](examples/))
50+ runnable examples organized by chapter:

examples/
├── agents/            # Ch 4-5: Basic agents, tools, computer use
├── memory/            # Ch 4: Long-term memory & RAG patterns
├── mcp/               # Ch 4: Model Context Protocol agents
├── tools/             # Ch 4: Tool creation, approval loops & patterns
├── workflows/         # Ch 6: Sequential, parallel, production workflows
├── orchestration/     # Ch 7: Round-robin, AI-driven, plan-based
├── app/               # Ch 8: Modern Agent UX (FastAPI + SSE)
├── webui/             # Ch 8: Web UI integration examples
├── frameworks/        # Ch 9: Comparisons (LangGraph, AutoGen, etc.)
├── evaluation/        # Ch 10: Agent evaluation patterns
├── notebooks/         # Interactive Jupyter notebooks
├── otel/              # Production: OpenTelemetry & Observability
└── contextengineering/# Production: Context management strategies
```

## Key Features

**Production-Ready Patterns**

Illustrated through real-world case studies (see [YC Analysis workflow](examples/workflows/yc_analysis/)):

- Cost optimization: Two-stage filtering for 90% LLM cost reduction
- Type safety: Structured outputs with Pydantic validation
- Reliability: Checkpointing and resumable workflows
- Advanced reasoning: Think tool for improved problem-solving (54% performance gain)

**Computer Use Agents**

- Playwright-based browser automation
- Multimodal reasoning with vision models
- Built-in tools: navigate, click, type, scroll, extract content

**Web UI & CLI**

- Auto-discovery of agents, orchestrators, workflows
- Real-time streaming with Server-Sent Events
- Session management with conversation history
- Launch: `picoagents ui`

**Evaluation Framework**

- LLM-as-judge evaluation patterns
- Reference-based validation (exact, fuzzy, contains)
- Composite scoring with multiple judges
- Comprehensive metrics collection

## Framework Comparisons

The patterns in PicoAgents transfer well to production frameworks. To demonstrate this, this repo includes equivalent implementations across popular frameworks:

| Framework                                                         | Examples                         | Description                             |
| ----------------------------------------------------------------- | -------------------------------- | --------------------------------------- |
| [Microsoft Agent Framework](examples/frameworks/agent-framework/) | Agents, workflows, orchestration | Microsoft's agent framework             |
| [Google ADK](examples/frameworks/google-adk/)                     | Agents, workflows, orchestration | Google's Agent Development Kit          |
| [LangGraph](examples/frameworks/langgraph/)                       | Agents, workflows, orchestration | LangChain's graph-based agent framework |

These comparisons show that whether you use PicoAgents, LangGraph, or another framework, the core patterns—tool-calling agents, sequential workflows, round-robin orchestration etc —remain the same. Learn the patterns once, apply them anywhere.

## Get the Book

**"Designing Multi-Agent Systems: Principles, Patterns, and Implementation for AI Agents"**

This repository implements every concept from the book. The book provides the theory, design trade-offs, and production considerations you need to build effective multi-agent systems.

- [Buy Digital Edition](https://buy.multiagentbook.com/?utm_source=github&utm_medium=readme)
- [Paperback on Amazon](https://www.amazon.com/dp/B0G2BCQQJY)
- [Hardcover on Amazon](https://www.amazon.com/dp/B0G2F6T2BZ)

## Questions and Feedback

Questions or feedback about the book or code? Please [open an issue](https://github.com/victordibia/designing-multiagent-systems/issues).

## Citation

```bibtex
@book{dibia2025multiagent,
  title={Designing Multi-Agent Systems: Principles, Patterns, and Implementation for AI Agents},
  author={Dibia, Victor},
  year={2025},
  github={https://github.com/victordibia/designing-multiagent-systems}
}
```
