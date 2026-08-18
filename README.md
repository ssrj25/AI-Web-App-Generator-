# AI Web App Generator

> A multi-agent AI system that converts natural-language application requirements into structured engineering plans and generates project files using LLM-powered coding agents.

## Overview

**AI Web App Generator** uses a Planner → Architect → Coder workflow to automate parts of the software development process.

Instead of relying on a single LLM prompt, each agent has a specific responsibility:

```text
User Requirement
       │
       ▼
┌─────────────────┐
│  Planner Agent  │
│ Requirements →  │
│ Structured Plan │
└────────┬────────┘
         │
         ▼
┌─────────────────┐
│ Architect Agent │
│ Plan → Tasks    │
└────────┬────────┘
         │
         ▼
┌─────────────────┐
│   Coder Agent   │
│ ReAct + Tools   │
└────────┬────────┘
         │
         ▼
   Generated Project
```

## Key Features

* Multi-agent development workflow
* LangGraph-based agent orchestration
* Structured outputs with Pydantic
* Tool-based file creation and modification
* Safe project path validation
* ReAct-based coding agent
* Modular prompts, states, tools, and graph design
* Example generated applications

## How It Works

### Planner

Converts the user's requirement into a structured project plan containing the application's features, technology stack, and implementation requirements.

### Architect

Breaks the plan into concrete implementation tasks that can be executed by the coding agent.

### Coder

Uses a ReAct agent with file-system tools to inspect and modify the generated project.

Available tools include:

```text
read_file
write_file
list_files
get_current_directory
```

### Output

The generated files are stored inside the project workspace.

Example applications included in the repository:

* Calculator
* Todo application

## Tech Stack

| Technology    | Purpose                      |
| ------------- | ---------------------------- |
| Python        | Core application             |
| LangGraph     | Agent workflow orchestration |
| LangChain     | LLM and agent framework      |
| Groq          | LLM inference                |
| Pydantic      | Structured output validation |
| python-dotenv | Environment configuration    |
| uv            | Dependency management        |

## Project Structure

```text
app_builder/
├── agent/
│   ├── graph.py
│   ├── prompts.py
│   ├── states.py
│   └── tools.py
│
├── pre_generated_project_calculator/
├── pre_generated_project_todo_app/
├── main.py
├── pyproject.toml
├── uv.lock
└── README.md
```

## Getting Started

### Prerequisites

* Python 3.11+
* Groq API key
* Git

### 1. Clone

```bash
git clone https://github.com/ssrj25/ai-web-app-generator.git
cd ai-web-app-generator/app_builder
```

### 2. Install Dependencies

Using `uv`:

```bash
uv sync
```

Or:

```bash
pip install -e .
```

### 3. Configure Environment

Create a `.env` file:

```env
GROQ_API_KEY=your_groq_api_key
```

Never commit API keys or `.env` files to GitHub.

### 4. Run

```bash
python main.py
```

## Example

Input:

```text
Build a todo application where users can create,
update, delete, and mark tasks as completed.
```

Processing:

```text
Requirement
    ↓
Planner
    ↓
Architect
    ↓
Coder
    ↓
Generated Project
```

## Design Decisions

### Why Multiple Agents?

Separating planning, architecture, and implementation gives each stage a clear responsibility and makes the workflow easier to debug and extend.

### Why LangGraph?

LangGraph provides explicit state and graph-based orchestration for coordinating the different stages of the agent workflow.

### Why Pydantic?

Pydantic provides structured schemas for validating intermediate outputs before they are passed to the next stage.

### Why File-System Tools?

The coding agent needs to inspect and modify multiple project files. Tools provide a controlled interface for these operations.

### Why Path Validation?

Generated file paths are validated to keep file operations inside the intended project workspace.

* Web-based interface

## Learning Outcomes

This project demonstrates practical experience with:

* Agentic AI
* LangGraph
* LangChain
* LLM tool calling
* Structured LLM outputs
* Stateful workflows
* Prompt engineering
* Software architecture
* Secure file operations


