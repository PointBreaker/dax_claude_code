---
description: Analyze documentation and code to extract comprehensive knowledge
---

# Analysis Request

Analyze the following:

{{PROMPT}}

Please use the two-layer analysis approach:

1. **Documentation Layer (doc-analysis agent)**:
   - Extract insights from ./project_docs directory
   - Provide architectural context and design rationale
   - Explain "why" behind decisions
   - Identify what's documented vs. what's missing

2. **Code Layer (code-searcher agent)**:
   - Analyze actual implementation in source code
   - Show "how" things work in practice
   - Find patterns and integration points
   - Fill gaps where documentation is silent

3. **Synthesize Findings**:
   - Combine documentation context with implementation reality
   - Highlight discrepancies between docs and code
   - Provide comprehensive answer with citations
   - Suggest documentation updates if needed

4. **Store Knowledge**: Use memory-bank-synchronizer to record extracted insights to appropriate memory bank files.

Present findings with clear references to both documentation files (with sections) and source code (with file paths and line numbers).
