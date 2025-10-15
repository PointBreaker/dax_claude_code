# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## AI Guidance

* To save main context space, for code searches, inspections, troubleshooting or analysis, use code-searcher subagent where appropriate - giving the subagent full context background for the task(s) you assign it.
* After receiving tool results, carefully reflect on their quality and determine optimal next steps before proceeding. Use your thinking to plan and iterate based on this new information, and then take the best next action.
* For maximum efficiency, whenever you need to perform multiple independent operations, invoke all relevant tools simultaneously rather than sequentially.
* Before you finish, please verify your solution
* Do what has been asked; nothing more, nothing less.
* NEVER create files unless they're absolutely necessary for achieving your goal.
* ALWAYS prefer editing an existing file to creating a new one.
* NEVER proactively create documentation files (*.md) or README files. Only create documentation files if explicitly requested by the User.
* When you update or modify core context files, also update markdown documentation and memory bank
* When asked to commit changes, exclude CLAUDE.md and CLAUDE-*.md referenced memory bank system files from any commits. Never delete these files.

## Memory Bank System

This project uses a structured memory bank system with specialized context files. Always check these files for relevant information before starting work:

### Core Context Files

* **CLAUDE-activeContext.md** - Current session state, goals, and progress (if exists)
* **CLAUDE-patterns.md** - Established code patterns and conventions (if exists)
* **CLAUDE-decisions.md** - Architecture decisions and rationale (if exists)
* **CLAUDE-troubleshooting.md** - Common issues and proven solutions (if exists)
* **CLAUDE-config-variables.md** - Configuration variables reference (if exists)
* **CLAUDE-temp.md** - Temporary scratch pad (only read when referenced)

**Important:** Always reference the active context file first to understand what's currently being worked on and maintain session continuity.

### Memory Bank System Backups

When asked to backup Memory Bank System files, you will copy the core context files above and @.claude settings directory to directory @/path/to/backup-directory. If files already exist in the backup directory, you will overwrite them.

## Agent Orchestration Commands

This project uses a multi-agent orchestration system defined in `.claude/commands.json`. The system coordinates specialized agents to handle different aspects of development workflow.

### Available Commands

#### `/plan` - Spec-Driven Planning
Convert user prompts into clear, actionable specifications with automatic routing.

**Workflow:**
1. **spec-driven-planner** reflects on prompt clarity and completeness
2. If insufficient → asks user for missing info
3. If sufficient → **doc-analysis** gathers context from docs/
4. **spec-driven-planner** generates full spec with citations
5. **memory-bank-synchronizer** records spec and decisions
6. Present spec and downstream prompts to user

**Use when:** Starting a new feature, planning major changes, or need structured requirements.

**Example:**
```
/plan "We need a simple inventory app for small shops"
```

#### `/develop` - Execute Development
Implement features based on approved specifications.

**Workflow:**
1. **memory-bank-synchronizer** loads current spec and context
2. **fullstack-dev** implements backend, frontend, data, and ops
3. **memory-bank-synchronizer** records implementation and patterns
4. Present implementation to user

**Use when:** You have an approved spec and ready to implement.

**Example:**
```
/develop "Implement the inventory management spec"
```

#### `/debug` - Debug and Fix Issues
Systematic debugging with context awareness and documentation cross-checking.

**Workflow:**
1. **memory-bank-synchronizer** loads context and troubleshooting history
2. **spec-driven-planner** triages issue and checks for missing info
3. If insufficient → asks user for reproduction steps
4. **doc-analysis** cross-checks against documented flows
5. **spec-driven-planner** generates bugfix report and patch plan
6. **fullstack-dev** applies fixes with tests
7. **memory-bank-synchronizer** records solution to troubleshooting memory

**Use when:** Fixing bugs, addressing regressions, or investigating issues.

**Example:**
```
/debug "Checkout page throws 500 when submitting large carts"
```

#### `/analyze` - Documentation and Code Analysis
Extract knowledge from both documentation and code for comprehensive understanding.

**Workflow:**
1. **doc-analysis** analyzes ./project_docs for insights and context
2. If documentation has gaps → **code-searcher** extracts from codebase
3. **memory-bank-synchronizer** stores extracted knowledge
4. Present analysis to user

**Use when:** Need to understand system, extract API specs, find undocumented features, or build knowledge base.

**Example:**
```
/analyze "How does the file classification system work?"
# → doc-analysis provides context from docs
# → code-searcher finds actual implementation
# → Combined comprehensive answer
```

#### `/quick-dev` - Quick Development
Fast-track simple changes without full spec process.

**Workflow:**
1. **memory-bank-synchronizer** loads current context and patterns
2. **fullstack-dev** implements directly with context awareness
3. **memory-bank-synchronizer** records changes
4. Present implementation to user

**Use when:** Making small, straightforward changes that don't need full planning.

**Example:**
```
/quick-dev "Add email validation to user registration form"
```

### Agent Roles

- **spec-driven-planner**: Planning, reflection, spec generation, triage, patch planning
- **doc-analysis**: Documentation layer expert - analyzes ./project_docs for architectural insights and design context
- **code-searcher**: Code layer expert - locates implementations, performs forensic analysis, fills documentation gaps
- **fullstack-dev**: Backend/frontend/database implementation, ops configuration
- **memory-bank-synchronizer**: Context management, pattern tracking, decision logging

### Two-Layer Analysis Approach

This system uses a **dual-layer analysis** for comprehensive understanding:

**Documentation Layer** (doc-analysis):
- Analyzes ./project_docs directory (6 core files)
- Provides architectural context and design rationale
- Explains "why" behind decisions
- Identifies what's documented

**Code Layer** (code-searcher):
- Analyzes actual source code
- Provides implementation details
- Shows "how" things work
- Fills gaps where docs are silent

**Collaboration Pattern**:
```
User Question → doc-analysis (context) → code-searcher (implementation) → Complete Answer
```

### Routing Logic

The system automatically routes between agents based on workflow state:
- **ask_user**: Pauses workflow to request missing information
- **present_to_user**: Displays results and awaits approval
- **complete**: Finishes workflow and presents final results

### Command Selection Guide

```
User request type → Recommended command

"I want to build..." → /plan (uses doc-analysis + code-searcher)
"Implement the spec for..." → /develop
"This is broken..." → /debug (uses doc guidance + code forensics)
"How does X work?" → /analyze (extracts from docs + code)
"What's not documented?" → /analyze (identifies gaps, extracts from code)
"Quick fix: add..." → /quick-dev
```

### Agent Collaboration Examples

**Example 1: Understanding a Feature**
```
/analyze "How does authentication work?"
→ doc-analysis: Explains auth architecture from 01-ARCHITECTURE.md
→ code-searcher: Finds actual auth implementation
→ Result: Complete understanding (design + implementation)
```

**Example 2: Debugging with Context**
```
/debug "Login fails with 401 error"
→ doc-analysis: Provides auth flow from docs
→ code-searcher: Locates auth middleware, finds missing token validation
→ Result: Root cause identified with fix
```

**Example 3: Planning with Existing Patterns**
```
/plan "Add rate limiting to API"
→ doc-analysis: Checks architecture docs (not found)
→ code-searcher: Finds existing middleware patterns
→ Result: Spec following existing patterns + doc update suggestion
```

## Project Overview



## ALWAYS START WITH THESE COMMANDS FOR COMMON TASKS

**Task: "List/summarize all files and directories"**

```bash
fd . -t f           # Lists ALL files recursively (FASTEST)
# OR
rg --files          # Lists files (respects .gitignore)
```

**Task: "Search for content in files"**

```bash
rg "search_term"    # Search everywhere (FASTEST)
```

**Task: "Find files by name"**

```bash
fd "filename"       # Find by name pattern (FASTEST)
```

### Directory/File Exploration

```bash
# FIRST CHOICE - List all files/dirs recursively:
fd . -t f           # All files (fastest)
fd . -t d           # All directories
rg --files          # All files (respects .gitignore)

# For current directory only:
ls -la              # OK for single directory view
```

### BANNED - Never Use These Slow Tools

* ❌ `tree` - NOT INSTALLED, use `fd` instead
* ❌ `find` - use `fd` or `rg --files`
* ❌ `grep` or `grep -r` - use `rg` instead
* ❌ `ls -R` - use `rg --files` or `fd`
* ❌ `cat file | grep` - use `rg pattern file`

### Use These Faster Tools Instead

```bash
# ripgrep (rg) - content search 
rg "search_term"                # Search in all files
rg -i "case_insensitive"        # Case-insensitive
rg "pattern" -t py              # Only Python files
rg "pattern" -g "*.md"          # Only Markdown
rg -1 "pattern"                 # Filenames with matches
rg -c "pattern"                 # Count matches per file
rg -n "pattern"                 # Show line numbers 
rg -A 3 -B 3 "error"            # Context lines
rg " (TODO| FIXME | HACK)"      # Multiple patterns

# ripgrep (rg) - file listing 
rg --files                      # List files (respects •gitignore)
rg --files | rg "pattern"       # Find files by name 
rg --files -t md                # Only Markdown files 

# fd - file finding 
fd -e js                        # All •js files (fast find) 
fd -x command {}                # Exec per-file 
fd -e md -x ls -la {}           # Example with ls 

# jq - JSON processing 
jq. data.json                   # Pretty-print 
jq -r .name file.json           # Extract field 
jq '.id = 0' x.json             # Modify field
```

### Search Strategy

1. Start broad, then narrow: `rg "partial" | rg "specific"`
2. Filter by type early: `rg -t python "def function_name"`
3. Batch patterns: `rg "(pattern1|pattern2|pattern3)"`
4. Limit scope: `rg "pattern" src/`

### INSTANT DECISION TREE

```
User asks to "list/show/summarize/explore files"?
  → USE: fd . -t f  (fastest, shows all files)
  → OR: rg --files  (respects .gitignore)

User asks to "search/grep/find text content"?
  → USE: rg "pattern"  (NOT grep!)

User asks to "find file/directory by name"?
  → USE: fd "name"  (NOT find!)

User asks for "directory structure/tree"?
  → USE: fd . -t d  (directories) + fd . -t f  (files)
  → NEVER: tree (not installed!)

Need just current directory?
  → USE: ls -la  (OK for single dir)
```
