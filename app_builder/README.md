# AI Web App Generator

> A multi-agent AI system that transforms natural-language application requirements into structured engineering plans and generates the corresponding project files using tool-using LLM agents.

## Overview

The AI Web App Generator uses a multi-agent workflow to convert a user's natural-language requirement into an implementation-ready web application.

Instead of asking a single LLM to generate an entire application in one step, the system separates the software-development process into specialized stages:

```text
User Requirement
       │
       ▼
┌──────────────┐
│   Planner    │
│              │
│ Requirements │
│ → Structured │
│     Plan     │
└──────┬───────┘
       │
       ▼
┌──────────────┐
│  Architect   │
│              │
│ Plan →       │
│ Implementation│
│    Tasks     │
└──────┬───────┘
       │
       ▼
┌──────────────┐
│    Coder     │
│              │
│ Tool-using   │
│ Agent        │
└──────┬───────┘
       │
       ▼
 Generated Project
```

## Key Features

* Multi-agent software development workflow
* Planner → Architect → Coder architecture
* LangGraph-based stateful orchestration
* Structured LLM outputs using Pydantic
* Tool-using coding agent
* File creation and modification through dedicated tools
* Safe project-path handling
* Iterative implementation of development tasks
* Example generated applications for testing the workflow

## Architecture

The system is organized into three primary agents.

### 1. Planner Agent

The Planner converts the user's natural-language request into a structured project plan.

It identifies:

* Application requirements
* Features
* Technology stack
* Project structure
* Required implementation components

The output is validated using a Pydantic schema.

### 2. Architect Agent

The Architect takes the structured project plan and converts it into an ordered implementation plan.

Each implementation task contains information such as:

* File path
* Task description
* Implementation requirements
* Execution order

This separates high-level planning from actual code generation.

### 3. Coder Agent

The Coder is a tool-using agent responsible for implementing the architecture.

It can:

* Read existing files
* Write generated code
* List project files
* Inspect the current project directory

The coder processes implementation tasks iteratively instead of attempting to generate the complete project in a single LLM response.

## Technology Stack

| Technology    | Purpose                                     |
| ------------- | ------------------------------------------- |
| Python        | Application and agent implementation        |
| LangGraph     | Agent orchestration and workflow management |
| LangChain     | LLM and agent abstractions                  |
| Groq          | LLM inference                               |
| Pydantic      | Structured output validation                |
| python-dotenv | Environment configuration                   |

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
└── README.md
```

## How the Workflow Works

Given a request such as:

```text
Create a simple todo web application with the ability
to add, complete, and delete tasks.
```

The system performs the following steps:

```text
1. User submits requirement
          ↓
2. Planner creates structured project plan
          ↓
3. Architect decomposes plan into implementation tasks
          ↓
4. Coder processes each implementation task
          ↓
5. Coder uses file tools to create/update project files
          ↓
6. Generated project is produced
```

## Example Applications

The repository includes example generated projects demonstrating the type of applications the system can produce.

### Calculator

A simple calculator application demonstrating basic UI and application logic generation.

### Todo Application

A todo application demonstrating multi-file project generation and feature-based implementation.

## Design Decisions

### Why multiple agents?

A single prompt that asks an LLM to plan, architect, and implement an entire application can become difficult to control and debug.

Separating these responsibilities allows each stage to focus on a specific software-development task.

### Why LangGraph?

LangGraph provides explicit workflow orchestration and state management, making the sequence of planning, architecture, and implementation easier to control.

### Why structured outputs?

The Planner and Architect return structured Pydantic models instead of relying only on free-form text.

This allows downstream agents to work with predictable data structures.

### Why tool-based code generation?

The Coder interacts with the project through file tools instead of returning a large block of code as plain text.

This makes the implementation process closer to an actual software-development workflow.

## Security Considerations

Generated files are restricted to the configured project workspace through path validation.

The coding agent is provided with controlled file-operation tools rather than unrestricted system access.

A future improvement is to execute generated applications inside an isolated sandbox before allowing them to run.

## Limitations

The current implementation is a research/prototype system and generated applications may require additional validation or manual refinement.

Current limitations include:

* Limited automated validation of generated applications
* No isolated runtime sandbox
* Limited evaluation benchmark
* Limited support for different application frameworks

## Future Improvements

* Automated generated-code validation
* Reviewer agent for code quality and requirement verification
* Sandboxed execution of generated applications
* Automated testing of generated projects
* Evaluation benchmark for generation quality
* Streaming agent progress
* Web-based user interface
* Support for additional application frameworks
* Improved observability and tracing

## Getting Started

### Prerequisites

* Python 3.11+
* A Groq API key
* `uv` or `pip`

### Installation

Clone the repository:

```bash
git clone https://github.com/ssrj25/ai-web-app-generator.git
cd ai-web-app-generator/app_builder
```

Create a virtual environment:

```bash
python -m venv .venv
```

Activate it on Windows:

```bash
.venv\Scripts\activate
```

Install dependencies:

```bash
pip install -e .
```

Create an environment file:

```text
GROQ_API_KEY=your_api_key_here
```

Run the application:

```bash
python main.py
```

## Project Status

🚧 **Active development**

The current version demonstrates the core multi-agent planning, architecture, and code-generation workflow. Automated validation, evaluation, and sandboxed execution are planned improvements.

## License

This project is licensed under the MIT License.
