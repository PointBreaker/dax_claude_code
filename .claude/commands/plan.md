---
description: Convert user prompt into clear spec with reflection and agent collaboration
---

# Planning Request

I need help planning the following:

{{PROMPT}}

Please follow this process:

1. **Reflect on Requirements**: Analyze the request for clarity, completeness, and feasibility. If critical information is missing (actors, flows, data models, constraints), ask targeted questions before proceeding.

2. **Gather Documentation Context**: Use the doc-analysis agent to extract relevant architectural context, design patterns, and constraints from ./project_docs directory.

3. **Analyze Existing Implementation**: Use the code-searcher agent to find similar implementations, integration points, and existing patterns in the codebase.

4. **Generate Comprehensive Spec**: Create a detailed specification including:
   - Overview and goals
   - Personas and user stories
   - Data models and API design
   - Frontend and backend implementation plans
   - Security, performance, and deployment considerations
   - Risks and mitigations

5. **Record to Memory Bank**: Use memory-bank-synchronizer to record the spec and key decisions to CLAUDE-activeContext.md and CLAUDE-decisions.md.

Present the final spec with clear next steps for implementation.
