# Agent Flow

The system uses a sequential multi-agent workflow where each stage produces structured information for the next stage.

## Workflow

```text
User Request
     │
     ▼
 Planner
     │
     │ Plan
     ▼
 Architect
     │
     │ Implementation Tasks
     ▼
 Coder
     │
     │ Tool Calls
     ▼
Generated Project
```

## 1. Planner

**Input:** Natural-language user requirement

**Output:** Structured project plan

The Planner identifies:

* Application purpose
* Features
* Technology stack
* Required files
* Implementation requirements

## 2. Architect

**Input:** Planner output

**Output:** Implementation task plan

The Architect converts the high-level plan into smaller development tasks that the Coder can execute.

## 3. Coder

**Input:** Implementation tasks

**Output:** Generated project files

The Coder uses a ReAct-based agent and controlled tools to:

* Inspect existing files
* Create files
* Modify files
* List project files

## Tool Interaction

```text
Coder Agent
    │
    ├── read_file
    │
    ├── write_file
    │
    ├── list_files
    │
    └── get_current_directory
```

## State Flow

```text
User Input
    ↓
Plan
    ↓
TaskPlan
    ↓
CoderState
    ↓
Generated Files
```

Pydantic models are used to keep the data exchanged between workflow stages structured and predictable.

## Why This Design?

Separating planning, architecture, and coding provides:

* Clear separation of responsibilities
* Easier debugging
* Structured intermediate outputs
* Better control over agent behavior
* Easier extension with validation or review agents

## Future Workflow

The architecture can be extended with validation and self-correction:

```text
Planner
   ↓
Architect
   ↓
Coder
   ↓
Validator
   │
   ├── PASS → END
   │
   └── FAIL → Coder
```

This would allow the system to automatically detect and correct generated-code issues.
