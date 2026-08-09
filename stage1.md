# JARVIS AI ASSISTANT — STAGE 1

## The Brain

**Project:** JARVIS — Personal AI Assistant
**Stage:** 1 / 6
**Codename:** The Brain
**Primary Language:** Python
**Status:** Development Specification

---

# 1. Mission

Build the first functional version of JARVIS.

Stage 1 is about creating the **core intelligence and tool-use system** that every later stage will build upon.

At the end of this stage, JARVIS must be able to:

* Understand natural-language requests.
* Hold a conversation.
* Decide whether it needs a tool.
* Select the appropriate tool.
* Generate structured tool arguments.
* Execute the selected tool.
* Receive the tool result.
* Use that result to produce a final answer.
* Handle errors gracefully.
* Expose its functionality through an API.

The fundamental capability being built is:

> **JARVIS can think with an LLM and act through tools.**

---

# 2. What Stage 1 Is NOT

Do not build the complete JARVIS system yet.

The following belong to later stages:

* Long-term memory
* RAG
* Vector databases
* Autonomous planning
* Multi-agent systems
* Browser automation
* Desktop automation
* Voice
* Speech recognition
* Text-to-speech
* Computer vision
* Persistent user profiles
* Complex frontend
* Distributed architecture
* Microservices
* Kubernetes
* Production cloud infrastructure

Stage 1 must remain focused on the fundamental AI/tool architecture.

---

# 3. Stage 1 Architecture

The system should follow this conceptual architecture:

```text
                         USER
                           │
                           ▼
                    ┌─────────────┐
                    │   FastAPI   │
                    │   Backend   │
                    └──────┬──────┘
                           │
                           ▼
                    ┌─────────────┐
                    │   JARVIS    │
                    │     CORE    │
                    └──────┬──────┘
                           │
              ┌────────────┴────────────┐
              │                         │
              ▼                         ▼
       Conversation Manager       Tool Registry
              │                         │
              ▼                         │
           ┌──────┐                     │
           │  LLM │                     │
           └──┬───┘                     │
              │                         │
              │ Tool Call               │
              └─────────────┐           │
                            ▼           │
                      Tool Executor ◄───┘
                            │
              ┌─────────────┼─────────────┐
              │             │             │
              ▼             ▼             ▼
         Calculator      Weather      Web Search
              │             │             │
              └─────────────┼─────────────┘
                            │
                            ▼
                       Tool Result
                            │
                            ▼
                           LLM
                            │
                            ▼
                         Response
                            │
                            ▼
                          USER
```

The architecture should remain modular so later stages can extend it without rewriting the entire project.

---

# 4. Core Technology Stack

## Required

### Language

* Python 3.12+

### AI

* OpenAI API
* Official OpenAI Python SDK

### Backend

* FastAPI
* Uvicorn

### Data Validation

* Pydantic

### HTTP

* HTTPX

### Testing

* Pytest

### Development

* Git
* GitHub
* Python virtual environment
* Environment variables

---

# 5. Dependency Philosophy

Do not install a framework merely because it is popular.

For Stage 1, do NOT use:

* LangChain
* LangGraph
* CrewAI
* AutoGen
* ChromaDB
* Pinecone
* Redis
* Celery
* React
* Docker

unless explicitly requested later.

The purpose of Stage 1 is to understand the underlying mechanics of an AI assistant.

The developer should be able to explain:

> How does JARVIS decide to call a tool?

without relying on an agent framework to hide the process.

---

# 6. Project Structure

Use the following structure as the starting point:

```text
jarvis/
│
├── app/
│   ├── __init__.py
│   ├── main.py
│   ├── config.py
│   │
│   ├── core/
│   │   ├── __init__.py
│   │   ├── assistant.py
│   │   ├── conversation.py
│   │   └── executor.py
│   │
│   ├── llm/
│   │   ├── __init__.py
│   │   └── client.py
│   │
│   ├── tools/
│   │   ├── __init__.py
│   │   ├── registry.py
│   │   ├── calculator.py
│   │   ├── weather.py
│   │   └── web_search.py
│   │
│   └── api/
│       ├── __init__.py
│       ├── routes.py
│       └── schemas.py
│
├── tests/
│   ├── test_calculator.py
│   ├── test_conversation.py
│   ├── test_tools.py
│   └── test_api.py
│
├── .env
├── .env.example
├── .gitignore
├── README.md
├── requirements.txt
└── STAGE_1.md
```

The coding agent may modify the structure if there is a clear architectural reason.

Do not reorganize the project unnecessarily.

---

# 7. Phase 1 — Project Foundation

## Goal

Create a clean Python project.

## Tasks

* Initialize Git.
* Create virtual environment.
* Create project structure.
* Create `.gitignore`.
* Create `.env.example`.
* Create `requirements.txt`.
* Create `README.md`.
* Create `app/main.py`.
* Verify the application runs.

Initial output:

```text
JARVIS v0.1
System online.
```

## Knowledge

Understand:

* Python modules
* Packages
* Imports
* Virtual environments
* Environment variables
* Git basics

---

# 8. Phase 2 — LLM Integration

## Goal

Connect JARVIS to an LLM.

Create:

```text
app/llm/client.py
```

The rest of the application should communicate with the LLM through this abstraction instead of scattering API calls throughout the codebase.

Conceptually:

```python
class LLMClient:
    ...
```

The exact implementation may differ.

## Configuration

Use environment variables:

```text
OPENAI_API_KEY=
OPENAI_MODEL=
```

Never hardcode credentials.

## Requirements

The LLM client must:

* Send messages.
* Receive responses.
* Handle API errors.
* Support configurable model selection.
* Avoid leaking credentials.
* Be testable independently.

## Milestone

JARVIS can answer:

```text
User:
What is a barycenter?
```

with an LLM-generated response.

---

# 9. Phase 3 — Conversation Management

## Goal

Allow JARVIS to understand conversation context.

Create:

```text
app/core/conversation.py
```

The conversation manager should handle:

* System messages
* User messages
* Assistant messages
* Tool messages
* Retrieving conversation history
* Clearing conversation

Conceptually:

```text
Conversation
│
├── System message
├── User message
├── Assistant message
├── User message
├── Assistant message
└── ...
```

Example:

```text
User:
Who is Isaac Newton?

JARVIS:
Isaac Newton was...

User:
When was he born?

JARVIS:
He was born in 1643...
```

JARVIS should understand that "he" refers to Newton.

## Important

Do not implement persistent memory yet.

Conversation history exists only for the current session.

Persistent memory belongs to Stage 2.

---

# 10. Phase 4 — Structured Data

## Goal

Make communication between JARVIS components predictable.

Use Pydantic models where appropriate.

Example:

```python
class ChatRequest(BaseModel):
    message: str
```

```python
class ChatResponse(BaseModel):
    response: str
```

Tool arguments must also be structured and validated.

Do not use fragile logic such as:

```python
if "calculate" in message:
    ...
```

The LLM should use structured tool calling.

---

# 11. Phase 5 — Calculator Tool

## Goal

Give JARVIS its first real capability.

Create:

```text
app/tools/calculator.py
```

Conceptual interface:

```python
def calculator(expression: str) -> float:
    ...
```

## Supported operations

The calculator should support basic arithmetic:

```text
+
-
*
/
%
**
()
```

Example:

```text
25 * 37
```

Result:

```text
925
```

## Security

NEVER use unrestricted:

```python
eval(user_input)
```

Do not allow arbitrary Python code execution.

The calculator must safely parse/evaluate supported mathematical expressions.

## Tests

Test:

* Addition
* Subtraction
* Multiplication
* Division
* Modulo
* Exponents
* Parentheses
* Invalid expressions
* Division by zero
* Malicious input

---

# 12. Phase 6 — Tool Calling Loop

## Goal

Implement the fundamental JARVIS loop.

This is the most important phase of Stage 1.

The system must behave like:

```text
User request
     │
     ▼
    LLM
     │
     ├───────────────┐
     │               │
     ▼               ▼
No tool required   Tool required
     │               │
     ▼               ▼
Final answer     Tool call
                     │
                     ▼
                Validate args
                     │
                     ▼
                Execute tool
                     │
                     ▼
                 Tool result
                     │
                     ▼
                    LLM
                     │
                     ▼
                Final answer
```

## Example

User:

```text
What is 25 × 37?
```

LLM determines:

```text
Use calculator.
```

Tool receives:

```text
25 * 37
```

Tool returns:

```text
925
```

The result is sent back to the LLM.

JARVIS then answers:

```text
25 × 37 is 925.
```

## Critical requirement

Do not hardcode tool selection based on keywords.

Bad:

```python
if "calculate" in user_message:
    calculator(...)
```

Good:

```text
User
 ↓
LLM
 ↓
Structured tool call
 ↓
Tool registry
 ↓
Tool
```

The LLM must decide whether a tool is needed.

---

# 13. Phase 7 — Multiple Tools

## Goal

Create a scalable tool system.

Create:

```text
app/tools/registry.py
```

The registry should allow JARVIS to discover and execute available tools.

Initial tools:

### Calculator

```text
calculator
```

### Weather

```text
get_weather
```

### Web Search

```text
search_web
```

---

## Weather Tool

The weather tool should:

* Accept a location.
* Call a weather API.
* Return structured weather data.
* Handle API failures.
* Use timeouts.
* Validate responses.

Example:

```text
User:
What's the weather in Dhaka?

JARVIS:
[Uses weather tool]

JARVIS:
The current weather in Dhaka is...
```

---

## Web Search Tool

The web-search implementation should be isolated behind a clean interface.

The search provider must be replaceable later.

JARVIS should be able to perform requests such as:

```text
Search the web for the latest Python release.
```

The tool should return structured search results to the LLM.

---

# 14. Phase 8 — Reliability

## Goal

Make JARVIS robust.

The application must gracefully handle:

* Invalid user input.
* Invalid tool arguments.
* Tool failures.
* API failures.
* Network failures.
* Timeouts.
* Invalid LLM responses.
* Missing credentials.
* Unexpected external API responses.

Bad:

```text
Traceback (most recent call last):
...
```

Better:

```text
I couldn't retrieve the requested information because the external service is unavailable.
```

Internal logs may contain technical details.

User-facing errors should remain understandable.

---

# 15. Logging

Implement useful application logging.

Log events such as:

```text
LLM request started
Tool selected
Tool execution started
Tool execution completed
API request failed
```

Do NOT log:

* API keys
* Passwords
* Secrets
* Authentication tokens
* Unnecessary personal information

---

# 16. Phase 9 — FastAPI Backend

## Goal

Expose JARVIS through an HTTP API.

Create:

```text
app/api/routes.py
app/api/schemas.py
```

## Endpoint

Implement:

```http
POST /chat
```

Request:

```json
{
    "message": "What is a barycenter?"
}
```

Response:

```json
{
    "response": "A barycenter is..."
}
```

---

## Health endpoint

Implement:

```http
GET /health
```

Response:

```json
{
    "status": "ok"
}
```

---

# 17. API Architecture

The API layer must not contain the AI logic.

Correct:

```text
FastAPI
   │
   ▼
Assistant
   │
   ├── Conversation
   ├── LLM
   ├── Tool Registry
   └── Tool Executor
```

Incorrect:

```text
FastAPI route
   │
   ├── LLM calls
   ├── tool selection
   ├── calculator
   ├── weather
   └── conversation logic
```

Keep responsibilities separated.

---

# 18. Phase 10 — CLI

Before building a frontend, create a simple terminal interface.

Example:

```text
================================
          JARVIS v1
================================

You: What's 25 * 37?

JARVIS: 25 × 37 = 925.

You: What's the weather in Dhaka?

JARVIS: The current weather is...

You: exit

JARVIS: Goodbye.
```

The CLI should use the same JARVIS core used by the FastAPI backend.

Do not duplicate AI logic inside the CLI.

---

# 19. JARVIS Personality

The assistant should be:

* Calm
* Helpful
* Intelligent
* Clear
* Concise by default
* Professional
* Honest
* Action-oriented

JARVIS must never pretend it performed an action that it did not actually perform.

For example, if browser automation is not implemented:

Bad:

```text
I've opened the browser.
```

Correct:

```text
Browser automation is not available in this version yet.
```

---

# 20. Tool Architecture

Every tool should have:

```text
Tool
├── Name
├── Description
├── Input schema
├── Implementation
├── Validation
├── Error handling
└── Tests
```

Tool descriptions should be written so an LLM can clearly understand:

1. What the tool does.
2. When it should be used.
3. What arguments it requires.
4. What it returns.
5. What limitations it has.

---

# 21. Security Requirements

Security must be considered from the beginning.

## Required

* Never hardcode secrets.
* Use environment variables.
* Never commit `.env`.
* Validate tool arguments.
* Never use unrestricted `eval`.
* Never execute arbitrary Python.
* Never execute arbitrary shell commands.
* Validate external API responses.
* Use network timeouts.
* Do not expose stack traces through public API responses.
* Keep external integrations isolated.

---

# 22. Testing

Testing must be performed continuously.

## Unit tests

Test:

* Calculator
* Tool registry
* Conversation manager
* Input validation
* Error handling

## Integration tests

Test:

```text
User
 ↓
JARVIS
 ↓
LLM
 ↓
Tool
 ↓
Tool result
 ↓
LLM
 ↓
Final response
```

External services should be mocked where appropriate.

## API tests

Test:

```text
GET /health
POST /chat
```

Also test invalid requests.

---

# 23. Development Milestones

The project should be developed incrementally.

## Milestone 1

```text
✓ Project starts
✓ Git configured
✓ Environment configured
```

## Milestone 2

```text
✓ LLM responds
```

## Milestone 3

```text
✓ Conversation context works
```

## Milestone 4

```text
✓ Structured data works
```

## Milestone 5

```text
✓ Calculator works independently
```

## Milestone 6

```text
✓ LLM can request calculator
✓ Tool executes
✓ Result returns to LLM
```

## Milestone 7

```text
✓ Multiple tools work
✓ Tool registry works
```

## Milestone 8

```text
✓ Errors handled
✓ Logging implemented
```

## Milestone 9

```text
✓ FastAPI backend works
```

## Milestone 10

```text
✓ CLI works
✓ Tests pass
✓ Documentation complete
✓ JARVIS v1 is operational
```

---

# 24. Final Capabilities

At completion, JARVIS should be capable of:

### Direct conversation

```text
Explain quantum computing.
```

### Calculation

```text
Calculate 847 × 392.
```

### Weather

```text
What's the weather in Dhaka?
```

### Web search

```text
Search the web for the latest Python release.
```

### Contextual conversation

```text
User:
Who was Newton?

JARVIS:
Isaac Newton was...

User:
When was he born?

JARVIS:
He was born in 1643...
```

### Tool selection

JARVIS should automatically determine whether to:

```text
Answer directly
       OR
Use a tool
```

---

# 25. Definition of Done

Stage 1 is complete only when all of the following are satisfied.

## Foundation

* [ ] Python project works.
* [ ] Git repository works.
* [ ] Environment configuration works.
* [ ] Project structure is clean.

## LLM

* [ ] LLM integration works.
* [ ] Model is configurable.
* [ ] Errors are handled.
* [ ] Credentials are protected.

## Conversation

* [ ] Conversation history works.
* [ ] Follow-up questions work.
* [ ] Conversation can be cleared.

## Structured Data

* [ ] Pydantic models are used appropriately.
* [ ] Tool arguments are validated.

## Tools

* [ ] Calculator works.
* [ ] Weather works.
* [ ] Web search works.
* [ ] Tool registry works.
* [ ] Tool descriptions are clear.

## Tool Calling

* [ ] LLM can request tools.
* [ ] Tool calls are validated.
* [ ] Tools execute correctly.
* [ ] Results return to the LLM.
* [ ] Final responses incorporate tool results.

## Reliability

* [ ] Exceptions are handled.
* [ ] Network timeouts exist.
* [ ] Logging exists.
* [ ] Secrets are protected.

## API

* [ ] `/health` works.
* [ ] `/chat` works.
* [ ] API validation works.

## Testing

* [ ] Unit tests exist.
* [ ] Tool tests exist.
* [ ] Integration tests exist.
* [ ] API tests exist.

## Documentation

* [ ] README exists.
* [ ] Installation instructions work.
* [ ] Architecture is documented.
* [ ] Tools are documented.
* [ ] Known limitations are documented.

---

# 26. Stage 1 Final System

The final Stage 1 system should look like:

```text
                         ┌───────────┐
                         │   USER    │
                         └─────┬─────┘
                               │
                               ▼
                         ┌───────────┐
                         │  FastAPI  │
                         └─────┬─────┘
                               │
                               ▼
                    ┌──────────────────┐
                    │  JARVIS CORE     │
                    └────────┬─────────┘
                             │
                   ┌─────────┴─────────┐
                   │                   │
                   ▼                   ▼
            Conversation          Tool Registry
               Manager                 │
                   │                   │
                   ▼                   ▼
                 ┌─────┐          ┌──────────┐
                 │ LLM │          │  Tools   │
                 └──┬──┘          └────┬─────┘
                    │                  │
                    │ Tool Call        │
                    └────────┬─────────┘
                             ▼
                       Tool Executor
                             │
                             ▼
                        Tool Result
                             │
                             ▼
                            LLM
                             │
                             ▼
                       Final Response
                             │
                             ▼
                           USER
```

---

# 27. Future Stages

Stage 1 establishes the foundation.

The full JARVIS roadmap is:

```text
STAGE 1
🧩 THE BRAIN
LLM + Tools + Conversation
        │
        ▼
STAGE 2
🧠 THE MEMORY
Database + Embeddings + RAG
        │
        ▼
STAGE 3
🤖 THE AGENT
Planning + Autonomous Execution
        │
        ▼
STAGE 4
🌐 THE OPERATOR
Browser + OS + External Services
        │
        ▼
STAGE 5
👁️ THE SENSES
Voice + Vision + Multimodal
        │
        ▼
STAGE 6
⚡ JARVIS
Complete Personal AI Assistant
```

Do not implement these future stages during Stage 1.

Stage 1 should end with a clean, reliable, understandable foundation.

---

# 28. Instructions to the Coding Agent

You are implementing Stage 1 of the JARVIS project.

Follow this document as the primary specification.

Before making changes:

1. Inspect the existing project.
2. Determine what has already been implemented.
3. Do not overwrite working functionality unnecessarily.
4. Identify the current milestone.
5. Implement only the next logical milestone.

While coding:

* Prefer simple solutions.
* Use type hints.
* Keep modules focused.
* Write tests.
* Handle errors.
* Keep secrets out of source code.
* Avoid unnecessary dependencies.
* Do not implement future stages.
* Update documentation when behavior changes.

When introducing a new dependency, explain why it is needed.

When making an architectural decision, document the reasoning.

Do not claim a feature is complete without testing it.

---

# 29. Final Objective

The ultimate objective of Stage 1 is not to create an impressive-looking chatbot.

It is to understand and implement the fundamental architecture:

```text
LLM
 +
Conversation
 +
Tool Calling
 +
Tool Execution
 +
Structured Data
 +
Error Handling
 +
API
```

Once this foundation works reliably, JARVIS is ready for Stage 2:

> **THE MEMORY — giving JARVIS persistent knowledge and the ability to remember.**
