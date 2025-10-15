---
description: Execute development based on approved specification
---

# Development Request

Implement the following feature:

{{PROMPT}}

Please follow this process:

1. **Load Context**: Review CLAUDE-activeContext.md and CLAUDE-patterns.md to understand current project state and established patterns.

2. **Verify Specification**: Ensure there's a clear spec to work from. If not, suggest using /plan first.

3. **Use fullstack-dev Agent**: Delegate the implementation to the fullstack-dev agent with complete context including:
   - Current spec and requirements
   - Architectural patterns from memory bank
   - Technology stack: Django/DRF + Vue 3 + MySQL + Docker Compose
   - Need for tests and documentation

4. **Update Memory Bank**: After implementation, use memory-bank-synchronizer to:
   - Update CLAUDE-activeContext.md with what was built
   - Record new patterns in CLAUDE-patterns.md
   - Log any decisions in CLAUDE-decisions.md

5. **Verification**: Ensure the implementation matches the spec and follows project patterns.
