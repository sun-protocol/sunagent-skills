# SunAgent Skills

Welcome to the **SunAgent Skills** repository. This project serves as a centralized collection of specialized capability modules ("Skills") for AI Agents, designed to be platform-agnostic.

## 🧠 What is a Skill?

A **Skill** is a structured package of instructions, workflow rules, and resources that extends an AI agent's capabilities. It transforms a general-purpose LLM into a domain expert by providing a "Thinking Framework" and standardized procedures.

These skills are essentially **system prompt extensions** that can be used in:
- **Agentic IDEs**: Such as Cursor, Windsurf, or custom internal tools.
- **Rules Files**: By importing content into `.cursorrules` or similar configuration files.
- **Context Injection**: By referencing the `SKILL.md` directly in your chat context.

Each skill typically contains:
- **`SKILL.md`**: The core definition file containing the prompt engineering, workflow steps, and operational constraints.
- **`resources/`**: Templates, checklists, and reference documents used by the skill.
- **`scripts/`**: (Optional) Python or Bash scripts for automation tasks.

## 📂 Available Skills

### 1. [DeFi Smart Contract Audit](./smart-contract-audit)
**Description**: A rigorous, adversarial audit process specialized for DeFi protocols (DEX, Lending, Vaults, etc.).
- **Version**: 2.0.0
- **Key Features**:
    - **3-Phase Workflow**: Automated Scan -> Manual Deep Dive -> Defense/Verdict.
    - **DeFi-Specific Checks**: Tailored logic for AMMs, Flash Loans, and Token Standards.
    - **Test Fixture Generation**: Automatically creates Foundry test bases for PoC development.
    - **Invariant Verification**: Uses mathematical invariants to validate or refute findings.

### 2. [Sun Frontend Engineering Workflow](./sun-frontend)
**Description**: A structured engineering workflow skill for React + TypeScript frontend development, enabling AI agents and developers to execute tasks with standardized planning, development governance, self-testing, and risk assessment.

- **Version**: 1.0.0
- **Key Features**:
    - **3-Phase Engineering Workflow**: Planning → Development → Self-Test to ensure structured frontend task execution.
    - **React + TypeScript Governance**: Enforces engineering best practices including component architecture, hooks rules, state management discipline, and API/UI separation.
    - **Scenario-Based Self Testing**: Uses a scenario matrix to simulate QA flows, covering edge cases, negative cases, and state transitions.
    - **Structured Risk Assessment**: Integrates a PR Gate review process with standardized risk levels (Low / Medium / High).
    - **Engineering Documentation Templates**: Includes technical planning, self-test report, and PR Gate review templates for traceable engineering processes.

## 🚀 How to Use

### For Agentic IDEs (Antigravity, Cursor etc.)
1.  **Import as Rule**: Copy the content of the `SKILL.md` file (or key sections) into your project's `.cursorrules` file or simply `@mention` the file in your chat.
2.  **Prompting**: Ask the agent to "Follow the workflow defined in `SKILL.md` to audit this project."

### For Custom Agents
1.  **Context Loading**: Load the `SKILL.md` content into the agent's system prompt or context window when the relevant task is requested.
2.  **Execution**: Ensure the agent has access to the toolset required by the skill (e.g., file reading, terminal access).

## 🛠️ Contributing

To add a new skill to this repository:

1.  Create a new directory: `mkdir my-new-skill`.
2.  Add a `SKILL.md` file with the required YAML frontmatter:
    ```yaml
    ---
    name: My New Skill
    description: A brief description of what this skill does.
    version: "1.0.0"
    ---
    ```
3.  Define your workflow instructions in standard Markdown.
4.  (Optional) Add a `resources/` folder for any templates your skill needs.

---
