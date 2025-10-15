---
description: Debug and fix issues systematically with full context
---

# Debugging Request

Debug the following issue:

{{PROMPT}}

Please follow this systematic process:

1. **Load Troubleshooting Context**: Check CLAUDE-troubleshooting.md for similar past issues and solutions.

2. **Gather Issue Details**: If reproduction steps, environment info, or expected vs. actual behavior is unclear, ask for clarification.

3. **Use doc-analysis Agent**: Check ./project_docs (especially 05-DEBUGGING_GUIDE.md) for relevant debugging strategies and expected system behavior.

4. **Use code-searcher Agent**: Locate the problematic code, trace execution flow, and identify root cause with forensic precision.

5. **Develop Fix Plan**: Create a patch plan with:
   - Root cause analysis
   - Proposed fix with file paths and line numbers
   - Test cases to prevent regression
   - Rollback strategy if needed

6. **Apply Fix**: Either implement directly or use fullstack-dev agent for complex fixes.

7. **Record Solution**: Use memory-bank-synchronizer to add this issue and solution to CLAUDE-troubleshooting.md for future reference.
