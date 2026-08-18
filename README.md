# AI Web App Generator

> A multi-agent AI system that transforms natural-language application requirements into structured engineering plans and generates web application files using tool-using LLM agents.

## Overview

AI Web App Generator is an agentic software development system built with Python, LangGraph, LangChain, Groq, and Pydantic.

Instead of asking a single LLM to generate an entire application in one step, the system separates the software development process into specialized agents:

```text
User Requirement
       │
       ▼
┌─────────────────┐
│  Planner Agent  │
│                 │
│ Requirements →  │
│ Structured Plan │
└────────┬────────┘
         │
         ▼
┌───────────────────┐
│ Architect Agent   │
│                   │
│ Plan → Technical  │
│ Implementation    │
└─────────┬─────────┘
          │
          ▼
┌───────────────────┐
│   Coder Agent     │
│                   │
│ Tool-using agent  │
│ generates files   │
└─────────┬─────────┘
          │
          ▼
   Generated Project
```

## Key Features

* Multi-agent software development workflow
* Planner → Architect → Coder architecture
* LangGraph-based stateful agent orchestration
* Structured LLM outputs using Pydantic
* Tool-based file creation and modification
* Safe project-path handling for generated files
* Iterative code generation
* Pre-generated example applications
* Modular prompts, state models, graph logic, and tools

## How It Works

### 1. Planner Agent

The Planner converts the user's natural-language request into a structured project plan.

The plan identifies:

* Application purpose
* Required features
* Technology stack
* Project structure
* Files required for implementation

### 2. Architect Agent

The Architect takes the project plan and decomposes it into implementation tasks.

Each task describes what needs to be implemented and which files are involved.

### 3. Coder Agent

The Coder is a tool-using ReAct agent responsible for implementing the planned tasks.

It can interact with the generated project through tools such as:

* `read_file`
* `write_file`
* `list_files`
* `get_current_directory`

This allows the agent to inspect existing files before creating or modifying them.

### 4. Generated Project

The resulting files are created inside the generated project workspace.

Example generated applications are included in this repository:

* Calculator application
* Todo application

## Architecture

```text
                         User Prompt
                              │
                              ▼
                    ┌──────────────────┐
                    │  Planner Agent   │
                    └────────┬─────────┘
                             │
                     Structured Plan
                             │
                             ▼
                    ┌──────────────────┐
                    │ Architect Agent  │
                    └────────┬─────────┘
                             │
                    Implementation Tasks
                             │
                             ▼
                    ┌──────────────────┐
                    │   Coder Agent    │
                    │   ReAct Agent    │
                    └────────┬─────────┘
                             │
                      Tool Operations
                             │
              ┌──────────────┼──────────────┐
              ▼              ▼              ▼
         Read Files     Write Files     List Files
              │              │              │
              └──────────────┼──────────────┘
                             │
                             ▼
                    Generated Application
```

## Project Structure

```text
app_builder/
│
├── agent/
│   ├── graph.py
│   ├── prompts.py
│   ├── states.py
│   └── tools.py
│
├── pre_generated_project_calculator/
│
├── pre_generated_project_todo_app/
│
├── main.py
├── pyproject.toml
├── uv.lock
└── README.md
```

## Technology Stack

| Technology    | Purpose                              |
| ------------- | ------------------------------------ |
| Python        | Application and agent implementation |
| LangGraph     | Agent workflow orchestration         |
| LangChain     | LLM and agent abstractions           |
| Groq          | LLM inference                        |
| Pydantic      | Structured output validation         |
| Python-dotenv | Environment configuration            |
| uv            | Dependency management                |

## Getting Started

### Prerequisites

* Python 3.11+
* A Groq API key
* Git

### Clone the Repository

```bash
git clone https://github.com/ssrj25/AI-Web-App-Generator-.git
cd AI-Web-App-Generator-/app_builder
```

### Install Dependencies

Using `uv`:

```bash
uv sync
```

Or using pip:

```bash
pip install -e .
```

### Configure Environment Variables

Create a `.env` file:

```env
GROQ_API_KEY=your_groq_api_key
```

Never commit your `.env` file or API keys to the repository.

### Run the Application

```bash
python main.py
```

## Example

A user can provide a requirement such as:

```text
Build a todo application where users can create,
update, delete, and mark tasks as completed.
```

The system processes the request through:

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

A single LLM call can generate code, but separating planning, architecture, and implementation makes the workflow easier to reason about, debug, and extend.

### Why LangGraph?

LangGraph provides an explicit graph-based approach for coordinating stateful agent workflows and makes the transitions between development stages visible.

### Why Pydantic?

Structured schemas make the output of planning and architecture stages predictable and easier to validate before passing information to the next agent.

### Why Tool-Based Code Generation?

The coding agent needs to inspect and modify multiple files. File-system tools provide a controlled interface between the agent and the generated project.

### Why Path Validation?

Generated file paths are validated so file operations remain inside the intended project workspace.

## Current Limitations

The current version is a prototype focused on demonstrating the multi-agent software-generation workflow.

Current limitations include:

* Generated applications are not yet automatically executed in a sandbox.
* Automated code validation is limited.
* The system currently focuses on a small set of project types.
* LLM-generated code can still require manual review.
* Production deployment and isolated execution are not yet implemented.

## Future Improvements

* Add automated code validation
* Add a dedicated reviewer agent
* Add automatic test generation
* Add sandboxed execution of generated applications
* Add retry and self-correction workflows
* Add evaluation benchmarks
* Add LangSmith-based observability
* Add support for additional application frameworks
* Add a web interface for interacting with the generator

## Security Considerations

Generated code should not be treated as trusted code.

The project separates generated files into a dedicated workspace and validates file paths before performing file operations.

Future versions should execute generated applications inside an isolated sandbox or container before allowing arbitrary commands to run.

## Learning Goals

This project was built to strengthen practical understanding of:

* Agentic AI
* LangGraph
* LangChain
* LLM tool calling
* Structured LLM outputs
* State-based workflows
* Prompt engineering
* Software architecture
* AI-generated code validation
* Secure file operations

## License

This project is intended for educational and portfolio purposes.
