# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

This is a sandbox project for experimenting with the CrewAI framework, which enables building multi-agent AI systems. The project contains a sample crew implementation called `latest_ai_development` that demonstrates a research and reporting workflow.

## Environment Setup

The project uses conda for environment management:

```bash
# Create environment
conda env create -f environment.yml

# Activate environment
conda activate crewai-sandbox
```

Environment is configured with Python 3.11 and includes:
- crewai with tools and embeddings extensions
- python-dotenv for environment variable management

## Running the Project

```bash
# Run the main crew (from project root)
python main.py

# Or run using the crew's entry point
cd latest_ai_development
run_crew

# Train the crew
train <n_iterations> <filename>

# Test the crew
test <n_iterations> <openai_model_name>

# Replay a specific task
replay <task_id>
```

## Architecture

### Crew Structure

The project follows CrewAI's decorator-based architecture pattern:

- **Crew Definition**: `latest_ai_development/src/latest_ai_development/crew.py`
  - Uses `@CrewBase` decorator to define the crew class
  - Agents and tasks are defined using `@agent` and `@task` decorators
  - Configuration loaded from YAML files in `config/` directory
  - Crew uses sequential process by default (hierarchical process available but commented out)

- **Configuration Files**: `latest_ai_development/src/latest_ai_development/config/`
  - `agents.yaml`: Defines agent roles, goals, and backstories with template variables (e.g., `{topic}`)
  - `tasks.yaml`: Defines task descriptions, expected outputs, and agent assignments

- **Entry Points**: Two main entry points exist
  - `main.py` at root: Simple wrapper for running the crew
  - `latest_ai_development/src/latest_ai_development/main.py`: Full implementation with run, train, replay, and test functions

### Current Crew Implementation

The `latest_ai_development` crew implements a two-agent research workflow:

1. **Researcher Agent**: Conducts thorough research on a given topic and outputs 10 bullet points of relevant information
2. **Reporting Analyst Agent**: Expands research findings into a full report with detailed sections

Tasks execute sequentially with the reporting task outputting to `report.md`.

### Template Variable Pattern

The crew uses a flexible input system where `{topic}` is interpolated into agent roles, goals, and task descriptions. To run with different topics, modify the `inputs` dictionary passed to `crew().kickoff(inputs=inputs)`.

## Configuration

Create a `.env` file based on `.env.example`:
```bash
OPENAI_API_KEY=your-api-key-here
```

## Important Notes

- README.md must be kept up to date with any significant project changes
