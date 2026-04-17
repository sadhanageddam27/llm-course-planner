# LLM-Based Course Planner

![Python](https://img.shields.io/badge/Python-3.9+-blue?logo=python)
![LLM](https://img.shields.io/badge/LLM-Local%20API-lime)
![Jupyter](https://img.shields.io/badge/Jupyter-Notebook-orange?logo=jupyter)
![Prompting](https://img.shields.io/badge/Prompt-Engineering-green)

Personalized academic course planning system powered by a locally-hosted
LLM — takes a student's background and target course, then generates
a structured step-by-step learning path using four progressively
sophisticated prompting strategies.

## Live Project Report
Full results, prompt examples, and technique comparison:
**https://sadhanageddam27.github.io/llm-course-planner/**

---

## Project Structure

    llm-course-planner/
    └── LLM.ipynb    # All 4 parts in one notebook

---

## What This Project Covers

### System Pipeline

    Student Input → Prompt Builder → LLM API → JSON Parser → Learning Path

Takes: department, degree level, completed courses, target course.
Returns: ordered prerequisite steps mapped to real course catalog entries.

### Part 1 — Zero-Shot JSON Prompting
Single prompt instructing the LLM to return only a valid JSON object
with prerequisites and a suggested learning path. No examples provided.

Sample output for student targeting Deep Learning:

    {
      "recommended_prerequisites": [
        { "course_number": "CSCI 356", "course_name": "Data Structures" },
        { "course_number": "CSCI 343", "course_name": "Data Science" }
      ],
      "suggested_learning_path": [
        { "step": 1, "course_number": "CSCI 632", "course_name": "Machine Learning" },
        { "step": 2, "course_number": "CSCI 492", "course_name": "Deep Learning" }
      ]
    }

### Part 2 — Retry Logic and JSON Repair
LLMs occasionally return malformed JSON with comments or missing quotes.
Added automatic retry — strips comments, sanitizes response, re-queries
up to 3 times before graceful fallback. Handles hallucinations reliably.

### Part 3a — Chain-of-Thought Prompting
Explicit multi-step reasoning prompt:
1. Identify foundational concepts required for the target course
2. Compare with student background and identify gaps
3. Recommend only courses from the available catalog

Forces the model to justify each recommendation before committing —
significantly reduces hallucinated course suggestions.

### Part 3b — Multi-Step Decomposition
Breaks the single task into 3 independent LLM calls, each feeding the next:

- Step 1 — Required knowledge topics for the target course
- Step 2 — Gap analysis against student completed courses
- Step 3 — Course recommendations mapped to identified gaps

Each step is independently verifiable and debuggable — the most
reliable and explainable approach across all four parts.

---

## Technique Comparison

| Part | Technique | Reliability | Key Feature |
|------|-----------|------------|-------------|
| 1 | Zero-shot JSON | Moderate | Simple, fast |
| 2 | Retry + JSON repair | High | Handles malformed output |
| 3a | Chain-of-Thought | High | Explicit reasoning |
| **3b** | **Multi-step decomposition** | **Highest** | **Each step verifiable** |

---

## Setup and Usage

    git clone https://github.com/sadhanageddam27/llm-course-planner.git
    cd llm-course-planner

    # Set your LLM API credentials as environment variables
    export LLM_API_KEY="your-api-key"
    export LLM_HOST="your-host"
    export LLM_PORT="50001"

    pip install requests jupyter
    jupyter notebook LLM.ipynb

The notebook uses a locally-hosted LLM endpoint. Swap the base URL
to point to any OpenAI-compatible API (OpenAI, Ollama, etc.).

---

## Tech Stack
Python · Jupyter · requests · JSON · Prompt Engineering · LLM API

## Topics
`llm` `prompt-engineering` `nlp` `python` `chain-of-thought`
`json` `course-planning` `machine-learning`
