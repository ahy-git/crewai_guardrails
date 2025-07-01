# 🤖 CrewAI + Guardrails: JSON Validation with Intelligent Rewriting

This project demonstrates how to use **CrewAI (Flow)** together with **Guardrails.ai** to generate, validate, and automatically fix structured JSON outputs based on a predefined schema.

The architecture implements a complete cycle of agents that:

1. Generate profiles in JSON format (with fields `name`, `age`, `profession`)
2. Validate the output using a `Guardrails` schema (via Pydantic)
3. In case of failure, use another agent to **rewrite the prompt** based on the returned error
4. Repeat the cycle until a valid output is generated or the maximum number of retries is reached (`max_retries=10`)

---

## 🧩 Components

- **`crew_profile_generator.py`**  
  Agent responsible for generating JSON from the provided prompt.

- **`tool_guardrails_validator.py`**  
  Tool that validates the output using `Guardrails` and a schema defined with `Pydantic`.

- **`crew_prompt_rewriter.py`**  
  Agent that receives the error message from Guardrails and rewrites the prompt for a new attempt.

- **`main_flow.py`**  
  Orchestrator using `CrewAI.Flow`, controlling execution with `@start`, `@router`, and `@listen`.

---

## ⚙️ Execution Flow

```mermaid
flowchart TD
    Start([Start])
    Gen[🔁 Profile Generation]
    Validate[✅ Validation with Guardrails]
    Success[🎉 Validation OK]
    Rewrite[✍️ Prompt Rewriting]
    Retry[🔁 Retry with New Prompt]
    Fail[❌ Max Retries Reached]

    Start --> Gen
    Gen --> Validate
    Validate -->|Valid| Success
    Validate -->|Invalid| Rewrite
    Rewrite --> Retry
    Retry --> Gen
    Validate -->|10 attempts| Fail
````

---

## 🚀 How to Run

1. Install dependencies (virtualenv is recommended):

```bash
pip install -r requirements.txt
```

2. Run the main flow:

```bash
python main_flow.py
```

---

## 🎯 Educational Purpose

This project serves as a foundation to understand:

* Integration of structural validation with LLMs
* Agent-oriented flow control using `CrewAI`
* Iterative prompt rewriting based on real errors
* Practical usage of `Guardrails` with `Pydantic schemas`

---

```

