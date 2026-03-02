---
name: session-handoff
description: "Document current session, update memory, and generate handoff prompt for next session. Use when approaching context limits to prepare seamless session transitions."
risk: safe
---

# Session Handoff

Manage session transitions when approaching context limits by documenting progress, updating memory, and creating a handoff prompt.

## When to Use This Skill

Use this skill when:
- Context usage approaches 150K tokens (~75% of limit)
- Ending a significant work session
- Need to transition to a new session without losing progress
- Want to document accomplishments and prepare for continuation. For example when finishing a task 4 out of 6 wanting to document progress and prepare for next session.


**Recommended:** Invoke at ~150K tokens, not at 200K limit (leaves buffer for handoff process)

## 🔧 SKILL EXECUTION INSTRUCTIONS

**When this skill is invoked, follow these steps:**

### Step 1: Path Detection & Configuration

**Before starting the handoff workflow:**

1. **Auto-detect current environment:**
   - Use `pwd` command to get current working directory
   - Check for project memory at: `~/.claude/projects/[project-hash]/memory/MEMORY.md`
   - Detect if there's a specific project directory in the current path
   - Get current date for folder organization (YYYY-MM format)

2. **Ask about file location with smart suggestions:**
   ```
   I've detected you're working in: [detected path]

   Where would you like me to save your session files?

   Option 1 (Recommended): [current working directory]/sessions/YYYY-MM/
   Option 2: ~/Documents/sessions/YYYY-MM/
   Option 3: Current directory [current working directory]/
   Option 4: Custom path (specify your own)

   What works best for you?
   ```

3. **Create organized folder structure:**
   - Base directory structure:
     ```
     [base-path]/
     ├── sessions/YYYY-MM/        # Monthly session organization
     ├── architecture/            # Design and architecture docs
     ├── guides/                  # Reference guides
     ├── planning/                # Planning and next steps
     └── backups/                 # Backup files
     ```
   - Use `mkdir -p` to create necessary directories
   - Sessions auto-organized by month

4. **Remember the choice:**
   - Note the chosen base path for this session
   - Auto-append `/sessions/YYYY-MM/` for session files
   - Confirm: "Session files will be saved to [chosen path]/sessions/YYYY-MM/"

### Step 2: Session Handoff Workflow

**After path configuration, proceed with:**

## What This Skill Does

When you run `/session-handoff`, I will:

### 1. Document Current Session 📝
Create a timestamped session summary with conversation log:
- What was accomplished
- Current status and blockers
- Files created/modified
- Next steps and priorities
- Full conversation timestamp log

**Output:** `[chosen path]/sessions/YYYY-MM/SESSION_[DATE]_[TOPIC].md`

### 2. Update Memory 🧠
Update your persistent memory file:
- Current status checkboxes
- Key documentation references
- "When starting new session" instructions
- Last updated timestamp
- Link to latest session file

**Output:** Updates `~/.claude/projects/[project-hash]/memory/MEMORY.md`

Note: The project hash is auto-detected from your current working directory path. Claude Code creates this automatically.

### 3. Generate Handoff Prompt 🚀
Create a ready-to-paste prompt for next session:
- Documents to read
- Current status summary
- Immediate next action
- Important context

**Output:** `[chosen path]/HANDOFF_PROMPT.md`

### 4. Create Conversation Log 📋
Save a timestamped conversation log (for reference only, not loaded into context):
- Session start/end time
- Key conversation points
- Decisions made
- Commands executed

**Output:** `[chosen path]/sessions/YYYY-MM/CONVERSATION_LOG_[DATE]_[TIME].md`
**Note:** This file is for your reference only. Next session loads ONLY the lean SESSION summary.

### 5. Generate Task List (Optional) ✅
If requested, create a structured task list:
- Immediate next actions (prioritized)
- Testing checklist
- Future work items
- Dependencies and blockers

**Output:** `[chosen path]/planning/NEXT_TASKS.md` (overwrites previous)
**Note:** This is OPTIONAL - only created if you want task tracking. Keeps handoff quick otherwise.

## How to Use

Simply invoke the command:

```
/session-handoff
```

Or ask naturally:
- "Document this session for handoff"
- "Prepare for new session"
- "We're approaching context limits, prepare session transition"

## What I'll Ask You

I'll ask 4 quick questions to create accurate documentation:

1. **Main focus/topic of this session?**
   - Example: "Phase 3 - Created PROJECT_CONTEXT.md files"

2. **Approximate session duration?**
   - Example: "2 hours"

3. **Any blockers or issues?**
   - Example: "None, ready to test" or "Routing skill has wrong paths"

4. **Immediate next action?**
   - Example: "Follow PHASE_3_ROUTING_TEST_GUIDE.md to test routing"

5. **Create task list? (Optional)**
   - Say "yes" or "with tasks" to generate structured NEXT_TASKS.md
   - Say "no" or skip for quick handoff (default)

**Quick Modes:**
- "use smart defaults" - I'll infer everything from context
- "quick handoff" - Skip task list, just create essential files
- "with tasks" - Include detailed task breakdown

## Task List Format (Optional)

If you request task generation, I'll create `planning/NEXT_TASKS.md`:

```markdown
# Next Tasks - [Project Name]
**Generated:** YYYY-MM-DD HH:MM
**From Session:** SESSION_YYYY-MM-DD_[TOPIC].md

## 🎯 Immediate Actions (Do First)
- [ ] [Specific task 1]
- [ ] [Specific task 2]
- [ ] [Specific task 3]

## ✅ Testing Checklist
- [ ] [Test scenario 1]
- [ ] [Test scenario 2]
- [ ] [Test scenario 3]

## 🔮 Future Work (Later)
- [ ] [Future enhancement 1]
- [ ] [Future enhancement 2]

## 🚧 Blockers & Dependencies
- [ ] [Blocker or dependency to resolve]

## 📝 Notes
- [Any important context for these tasks]
```

**Benefits:**
- ✅ Structured task breakdown
- ✅ Prioritized by urgency
- ✅ Clear testing checklist
- ✅ Tracks blockers
- ✅ Keeps handoff prompt lean (tasks in separate file)

## Session Documentation Format

The SESSION file I create will include:

```markdown
# Session: [Topic]
**Date:** YYYY-MM-DD
**Start Time:** HH:MM
**End Time:** HH:MM
**Duration:** ~X hours
**Status:** Complete/In Progress/Blocked
**Working Directory:** [path where session took place]

## Accomplishments
- What was built/completed
- What was fixed
- What was learned

## Current Status
- What's working
- What's ready
- What's pending
- What's blocked
- What needs fixing

## Files Created/Modified
- List of changed files (auto-detected via git status or ls -lt)

## Conversation Log
- Key discussion points
- Decisions made
- Commands executed
- Problems solved

## Next Steps
- Immediate and urgent actions
- Testing needed
- Future work

## Important Notes
- Key decisions
- Context for next session
```

## Handoff Prompt Format

The prompt I generate will look like:

```markdown
I'm continuing work on [PROJECT].

Please read:
1. [full path]/SESSION_[DATE]_[TOPIC].md
   - Latest accomplishments and status
   - Next steps and priorities

Current Status: [Brief 1-2 sentence summary]

Next Action: [Specific task to start with]

Working Directory: [path to cd into]

Important Context: [Any critical notes]

---
Note: Conversation log saved to sessions/YYYY-MM/CONVERSATION_LOG_[DATE]_[TIME].md (for reference only, not loaded)
```

## Starting Your Next Session

After running `/session-handoff`:

1. **End current session** (context wipe)
2. **Start new Claude Code session**
3. **Open** `HANDOFF_PROMPT.md` from your chosen base path
4. **Paste the prompt** into new session
5. **I read referenced docs** and we continue seamlessly!

## Smart Defaults

If you don't provide details, I'll:
- Auto-detect current working directory (via `pwd`)
- Suggest intelligent path options based on project structure
- Analyze recent conversation to infer session focus
- Check file system for recently modified files (via `git status` or `ls -lt`)
- Look at task list (if tasks were used)
- Extract conversation timestamps and key points
- Generate reasonable defaults
- Ask you to confirm before writing

## File Naming & Organization Convention

**Folder Structure:**
- Sessions organized by month: `sessions/YYYY-MM/`
- Auto-creates folder for current month
- Keeps sessions organized chronologically

**Session Files:**
- Location: `sessions/YYYY-MM/SESSION_YYYY-MM-DD_[TOPIC].md`
- Topic: Derived from session focus (kebab-case)
- Examples:
  - `sessions/2026-02/SESSION_2026-02-12_PHASE_3_COMPLETE.md`
  - `sessions/2026-02/SESSION_2026-02-13_ROUTING_TESTS.md`
  - `sessions/2026-03/SESSION_2026-03-01_NEW_FEATURE.md`

**Conversation Logs:**
- Location: `sessions/YYYY-MM/CONVERSATION_LOG_YYYY-MM-DD_HH-MM.md`
- Timestamped with session start time
- Examples:
  - `sessions/2026-02/CONVERSATION_LOG_2026-02-12_14-30.md`
  - `sessions/2026-02/CONVERSATION_LOG_2026-02-13_09-15.md`

**Handoff Prompt:**
- Location: `HANDOFF_PROMPT.md` (always at root, overwrites previous)
- Latest prompt is always ready to use
- Easy to find for next session startup

## Example Workflow

```
User: /session-handoff

Me: I've detected you're working in: /home/user/projects/my-project

    Where would you like me to save your session files?

    Option 1 (Recommended): /home/user/projects/my-project/sessions/2026-03/
    Option 2: ~/Documents/sessions/2026-03/
    Option 3: Current directory /home/user/projects/my-project/
    Option 4: Custom path (specify your own)

    What works best for you?

You: Option 1

Me: Session files will be saved to /home/user/projects/my-project/sessions/2026-03/

    Creating organized folder structure...

    I'll help you prepare for session handoff. A few quick questions:
    1. What was the main focus/topic of this session?

You: Phase 3 - created PROJECT_CONTEXT files and routing skills

Me: 2. Approximately how long was this session?

You: About 2 hours

Me: 3. Any blockers or issues to note?

You: No blockers, ready to test routing

Me: 4. What's the immediate next action for the next session?

You: Follow PHASE_3_ROUTING_TEST_GUIDE.md

Me: ✅ Session Handoff Complete!

    📂 Folder Structure:
    - Created: sessions/2026-03/
    - Ready: architecture/, guides/, planning/, backups/

    📝 Created:
    - sessions/2026-03/SESSION_2026-03-01_PHASE_3.md
    - sessions/2026-03/CONVERSATION_LOG_2026-03-01_14-30.md
    - HANDOFF_PROMPT.md (at root)

    🧠 Updated:
    - ~/.claude/projects/[project-hash]/memory/MEMORY.md

    🎯 Your handoff prompt:
    ──────────────────────────────────
    I'm continuing work on my-project.

    Please read:
    1. /home/user/projects/my-project/sessions/2026-03/SESSION_2026-03-01_PHASE_3.md
       - Latest accomplishments and status
       - Next steps and priorities

    Current Status: Phase 3 complete - all PROJECT_CONTEXT.md files
    created, routing skills configured, ready for testing.

    Next Action: Test complete routing chain following
    PHASE_3_ROUTING_TEST_GUIDE.md

    Working Directory: /home/user/projects/my-project

    ---
    Note: Conversation log saved to sessions/2026-03/CONVERSATION_LOG_2026-03-01_14-30.md (for reference only)
    ──────────────────────────────────

    📋 Saved to HANDOFF_PROMPT.md

    When you start your next session, paste this prompt!
```

## Tips for Effective Handoffs

1. **Invoke early:** When you hit ~150K tokens, not 200K
2. **Be specific:** Clear next action = faster next session startup
3. **Note blockers:** Help future you avoid rabbit holes
4. **Test before handoff:** Don't handoff mid-debugging
5. **Choose project-local paths:** Save sessions in project directories for better organization
6. **Use from anywhere:** The skill auto-detects your location and suggests appropriate paths
7. **Folders auto-organize:** Sessions saved by month, keeps everything clean and browsable
8. **Archive old sessions:** Move old session folders to `backups/` to keep current work focused
9. **Task list when complex:** Use task planning for multi-step work, skip for simple continuations
10. **Keep context lean:** Only SESSION summary loads in next session, conversation logs are for reference

## Integration with Existing Workflow

This skill complements:
- **MEMORY.md:** Auto-loaded, provides quick context
- **SESSION files:** Deep dive into specific work sessions
- **Architecture docs:** Reference material
- **Test guides:** Step-by-step procedures

**Complete Workflow:**
```
Session Start → Work → Approaching Limit → /session-handoff → New Session
     ↑                                                              ↓
     └──────────────────────────────────────────────────────────────┘
                    (with handoff prompt)
```

## What Gets Saved Where

| File | Location | Purpose |
|------|----------|---------|
| Session Doc | `[base]/sessions/YYYY-MM/SESSION_*.md` | Full session details (LOADED in next session) |
| Conversation Log | `[base]/sessions/YYYY-MM/CONVERSATION_LOG_*.md` | Detailed transcript (reference only, NOT loaded) |
| Task List | `[base]/planning/NEXT_TASKS.md` | Structured todos (optional, overwrites previous) |
| Memory | `~/.claude/projects/[project-hash]/memory/MEMORY.md` | Persistent context (auto-detected, LOADED) |
| Handoff Prompt | `[base]/HANDOFF_PROMPT.md` | Next session starter (always at root) |

**Folder Structure Created:**
```
[base-path]/
├── HANDOFF_PROMPT.md           # Always at root for easy access
├── sessions/
│   ├── 2026-03/                # Auto-organized by month
│   │   ├── SESSION_2026-03-01_TOPIC.md         (LOADED in next session)
│   │   └── CONVERSATION_LOG_2026-03-01_14-30.md (reference only)
│   └── 2026-02/
├── architecture/               # Design docs
├── guides/                     # Reference guides
├── planning/                   # Planning docs
│   └── NEXT_TASKS.md          # Task list (optional, updated each handoff)
└── backups/                    # Backup files
```

**Context Loading (Lean & Focused):**
- ✅ LOADED: SESSION summary, MEMORY.md
- ❌ NOT LOADED: CONVERSATION_LOG (too verbose)
- ✅ OPTIONAL: NEXT_TASKS.md (if you want structured task tracking)

**Path Options:**
- Project-specific: `[project-dir]/` with auto-organization (Recommended)
- Global: `~/Documents/sessions/` with folders
- Current directory: Wherever you invoked the skill
- Custom: Any path you specify

## Installation

Copy the `skills/session-handoff/` directory to your Claude Code skills folder:

```bash
cp -r skills/session-handoff ~/.claude/skills/
```

Then invoke it in Claude Code:

```
/session-handoff
```

## Benefits

✅ **Never lose progress** when context resets
✅ **Fast startup** for new sessions
✅ **Complete documentation** of all work
✅ **Persistent memory** across sessions
✅ **Clear next actions** for continuity
✅ **Works from any directory** - portable and flexible
✅ **Smart path detection** - suggests best save location
✅ **Conversation logs** - full timestamped transcript (reference only)
✅ **Lean context loading** - only SESSION summary loaded, not verbose logs
✅ **Auto-detects project structure** - intelligent defaults
✅ **Organized folders** - sessions by month, clean structure
✅ **Scalable** - grows with your project without clutter
✅ **Optional task planning** - structured todos when you need them
✅ **Quick by default** - task list is optional, keeps handoff fast

---

**Version:** 2.2.0
**Created:** 2026-02-12
**Updated:** 2026-02-15 (Added lean context, optional task planning, conversation logs for reference)
**Risk:** Safe (only reads/writes documentation files)
