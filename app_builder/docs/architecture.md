# System Architecture

## Overview

AI Web App Generator uses a multi-agent architecture to transform a natural-language application requirement into a generated project.

```text
User Requirement
       │
       ▼
┌───────────────┐
│ Planner Agent │
└───────┬───────┘
        │
        ▼
   Structured Plan
        │
        ▼
┌─────────────────┐
│ Architect Agent │
└────────┬────────┘
         │
         ▼
 Implementation Tasks
         │
         ▼
┌─────────────────┐
│   Coder Agent   │
│  ReAct + Tools  │
└────────┬────────┘
         │
         ▼
┌─────────────────────────┐
│      File Tools         │
│ read / write / list     │
└───────────┬─────────────┘
            │
            ▼
     Generated Project
```

## Components

### Planner Agent

Converts the user's requirements into a structured project plan containing the application goals, features, technology stack, and required implementation details.

### Architect Agent

Transforms the project plan into implementation tasks that can be executed by the coding agent.

### Coder Agent

Uses a ReAct-based workflow and file-system tools to implement the planned tasks.

### State Management

Pydantic models define structured state exchanged between the different stages of the workflow.

### File Tools

The coding agent interacts with the generated project through controlled file operations such as reading, writing, and listing files.

## Workflow

1. User provides an application requirement.
2. Planner creates a structured project plan.
3. Architect decomposes the plan into implementation tasks.
4. Coder executes the tasks using file tools.
5. Generated files are stored inside the project workspace.

## Design Goals

* Separate planning from implementation.
* Maintain structured communication between agents.
* Allow agents to inspect existing files before modifying them.
* Keep generated file operations inside the intended project workspace.
* Make the workflow modular and extensible.

## Current Limitations

The current system does not automatically execute and validate generated applications in an isolated environment.

A future version can add automated validation, testing, and a reviewer agent.
