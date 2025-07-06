---
description: Guidelines and best practices for creating effective .clinerules to guide Cline's behavior, knowledge, and workflows.
author: JD
version: 1.1
tags: ["meta", "guideline", "clinerules", "documentation", "best-practices"]
globs: ["clinerules/**/*.md"] # This rule is relevant when writing or editing any .clinerule
---

# Writing Effective .clinerules

 This guide outlines best practices for creating effective and understandable Cline rules.

## Getting Started: The Basics

Cline rules are Markdown files (`.md`) that you create to guide Cline's behavior. They can be placed in two primary locations:

*   **Global Rules**: For rules that apply across all projects, place them in your global Cline rules directory, typically `~/Documents/Cline/Rules/`.
*   **Project-Specific Rules**: For rules specific to a particular project, create a `.clinerules/` directory at the root of your project and place the Markdown files there.

Name your files using `kebab-case` (e.g., `my-new-rule.md`).

## Core Principles for All ClineRules

*   **Clear Objective**: Every rule should have a well-defined purpose. State this objective clearly at the beginning of the rule, ideally in the frontmatter `description` and reinforced in the introductory text
    *   *Example*: A rule for research tasks might start with an "Objective" section. This document's objective is stated in its frontmatter `description` and introduction
*   **Structured Content**: Use Markdown effectively to structure your rule
    *   **Headings and Subheadings**: Organize content logically using `#`, `##`, `###`, etc
    *   **Lists**: Use bulleted (`-`) lists for steps, criteria, or key points
    *   **Code Blocks**: Use fenced code blocks (```) for code examples, commands, or structured data. Specify the language for syntax highlighting (e.g., ```typescript ... ```)
    *   **Emphasis**: Use **bold** and *italics* to highlight important terms or instructions
*   **Clarity and Precision**: Write in a clear, unambiguous manner. Avoid jargon where possible, or explain it if necessary. If the rule is meant to guide AI behavior, precision is paramount
*   **Modularity**: Each rule should ideally focus on a specific topic, tool, workflow, or area of knowledge. This makes rules easier to manage, understand, and update

## Frontmatter for Metadata

Use YAML frontmatter at the beginning of your rule file to provide metadata. This helps Cline (and humans) understand the rule's context and applicability.

```yaml
---
description: A brief explanation of what this rule is for.
author: Your Name/Handle
version: 1.0
# Globs can specify file patterns where this rule is particularly relevant.
# Cline might use this to prioritize or activate rules.
globs: ["**/*.js", "**/*.ts", "specific-config.json"]
# Tags can help categorize rules.
tags: ["coding-guideline", "documentation", "workflow", "supabase"]
---

# Rule Title
... rest of the rule content ...
```

*   **`description`**: A concise summary of the rule's purpose (as used in this document)
*   **`globs`**: An array of file patterns indicating relevance
*   **Other metadata**: Include `author`, `version`, `tags` as appropriate (see this document's frontmatter for an example)

## Rule Scopes: Global vs. Project-Specific

Cline rules can be applied at different scopes to provide flexibility and context-awareness:

*   **Global Rules**: These rules are stored in your global Cline rules directory (e.g., `~/Documents/Cline/Rules/`). They apply to all tasks and projects you work on with Cline. Use global rules for general guidelines, common workflows, or universal behavioral instructions.
*   **Project-Specific Rules**: These rules are stored in a `.clinerules/` directory at the root of a specific project. They only apply when you are working within that particular project. Use project-specific rules for project-specific coding standards, architecture guidelines, or unique development workflows.

When both global and project-specific rules exist, project-specific rules generally take precedence for conflicting instructions, allowing for fine-grained control within a project.

## Types of ClineRules and Their Structure

ClineRules can serve various purposes. Tailor the structure and content to the type of rule you're writing.

### Informational / Documentation Rules
Provide comprehensive information about a system, architecture, or technology. This document is an example of an informational rule.
*   **Key Elements**:
    *   Overview and project goals
    *   Detailed explanations of components, concepts, or processes
    *   Diagrams (e.g., Mermaid.js) to visualize systems
    *   Code snippets or configuration examples
    *   Definitions of key terms
*   **Example**: A rule describing a project's architecture, or a guide for creating presentations. This document is an example of an informational rule.

### Process / Workflow Rules
Define a sequence of steps for Cline or the user to follow to achieve a specific outcome.
*   **Key Elements**:
    *   A clear start and end point
    *   Numbered steps for sequential actions
    *   Decision points with clear options (e.g., "If X, then Y, else Z")
    *   Specification of tools to be used at each step (e.g., `use_mcp_tool`, `write_to_file`)
    *   Expected inputs and outputs for each step
    *   Notes on dependencies or prerequisites
*   **Example**: A rule for conducting research, or a protocol for MCP development.

### Behavioral / Instructional Rules (for Guiding AI)
These rules directly instruct Cline on how it should behave, process information, or generate responses, especially in specific contexts.
*   **Key Elements**:
    *   **Explicit Instructions**: Use imperative verbs (MUST, SHOULD, DO NOT, NEVER, ALWAYS)
    *   **Critical Warnings**: Use formatting (bold, ALL CAPS, emojis like 🚨, ⚠️, ✅, ❌) to draw attention to critical instructions or prohibitions
    *   **Positive and Negative Examples**: Show correct and incorrect ways of doing things (e.g., code patterns to use vs. avoid)
    *   **Triggers and Conditions**: Define when the rule or specific instructions within it should be activated
    *   **Verification Steps**: Include "thinking" blocks or checklists for the AI to verify its actions against the rule's constraints
    *   **Context Management**: Define how Cline should manage context, memory, or state if relevant
*   **Example**: A rule for Next.js and Supabase integration, or a rule for managing Cline's memory.

### Meta-Rules
Rules that define how Cline manages or improves its own rules or processes.
*   **Key Elements**:
    *   Triggers for the meta-process
    *   Steps involved in the meta-process (e.g., reflection, suggesting improvements)
    *   User interaction points (e.g., asking for confirmation)
*   **Example**: A rule for self-improving Cline.

## Language and Formatting for AI Guidance

When writing rules intended to directly steer Cline's AI behavior, certain conventions are highly effective:

*   **Be Directive**:
    *   Use **MUST** for absolute requirements
    *   Use **SHOULD** for strong recommendations
    *   Use **MAY** for optional actions
    *   Use **MUST NOT** or **NEVER** for absolute prohibitions
    *   Use **SHOULD NOT** for strong discouragement
*   **Highlight Critical Information**:
    *   Use "🚨 CRITICAL INSTRUCTIONS FOR AI LANGUAGE MODELS 🚨" and "❌ NEVER GENERATE THIS CODE" / "✅ ALWAYS GENERATE THIS EXACT PATTERN"
    *   Use "⚠️ CRITICAL: DO NOT USE attempt_completion BEFORE TESTING ⚠️" and "BLOCKER ⛔️"
*   **Provide Concrete Examples**:
    *   Show exact code snippets, commands, or output formats
    *   For code generation, clearly distinguish between desired and undesired patterns
*   **Define AI's "Thought Process"**:
    *   The `<thinking> ... </thinking>` block is a good way to make the AI "pause and check" its understanding or state before proceeding
    *   "AI MODEL VERIFICATION STEPS" serve a similar purpose
*   **Specify Tool Usage**:
    *   If Cline needs to use a specific tool (e.g., `attempt_completion`, `replace_in_file`, `use_mcp_tool`), explicitly state it and provide any necessary parameters or context for that tool

## Content Best Practices

*   **Start Broad, Then Narrow**: Begin with a general overview or objective, then delve into specifics
*   **Use Analogies or Scenarios**: If explaining a complex concept, an analogy or a use-case scenario can be helpful
*   **Define Terminology**: If your rule introduces specific terms or acronyms, define them
*   **Anticipate Questions**: Try to think about what questions a user (or Cline itself) might have and address them proactively
*   **Keep it Updated**: As systems or processes change, ensure the relevant `.clinerules` are updated to reflect those changes. A rule for self-improving Cline encourages this.

## Referencing Other Rules

If your rule builds upon or relates to another rule, feel free to reference it by its filename. This helps create a connected knowledge base.
