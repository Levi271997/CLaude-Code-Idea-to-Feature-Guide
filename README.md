
# Claude Code Workflow Guide

## From idea to shipped feature

A step-by-step process for brainstorming, researching, planning, designing and building with Claude Code, with the command and a ready-to-copy prompt for each step. Replace text in `<angle brackets>` with your own.

**The short version:** `/clear` → brainstorm (interview me) → research (web + Explore agents) → spec doc → `/plan` → prototype / Figma → build step by step (`/run`) → `/code-review` + `/security-review` → PR

---

## 0. Set up the session

Goal: start each idea with a clean, well-informed session.

| Command | What it does |
|---|---|
| `/init` | Creates a `CLAUDE.md` describing the project. Re-run only if the project has changed a lot. |
| `/clear` | Starts a fresh conversation. Use it for each new idea so old context doesn't leak in. |
| `/model` | Picks the model. Use the strongest one for brainstorming and planning. |
| `/context` | Shows how much of the context window you've used. |

**PROMPT**

```
I'm starting a new idea. Before anything else, read CLAUDE.md and AGENTS.md
and give me a 5-line summary of this project's stack and conventions.
```

---

## 1. Brainstorm the idea

Goal: understand the problem and explore options. Stay in normal mode; this is a conversation, not code.

**CLAUDE INTERVIEWS YOU**

```
I have an idea: <one-paragraph description>.
Don't write any code. Interview me with questions, one round at a time,
until you understand the problem, the users, and what success looks like.
Then summarize the idea back to me in a short brief.
```

**WIDEN THE OPTIONS**

```
Give me 5 different ways to approach this idea, from simplest MVP to most
ambitious. For each: what it does, effort (S/M/L), and the main risk.
Recommend one and explain why.
```

**CHALLENGE IT**

```
Act as a skeptical senior engineer and a skeptical customer.
What's weak, risky, or unnecessary in this idea? What would you cut?
```

---

## 2. Research

Goal: learn what already exists, both outside and inside your codebase.

### A. Outside research (web)

**COMPETITORS AND REUSABLE TOOLS**

```
Research how existing products solve <problem>. Find 3–5 competitors or
open-source projects, what they do well, what users complain about, and
any libraries/APIs we could reuse. Cite sources.
```

**CURRENT BEST PRACTICE**

```
What's the current best practice (2026) for <e.g. multi-tenant auth in
Next.js with Supabase>? Check official docs, not old blog posts.
```

### B. Research inside the codebase

For broad searches, ask for subagents so the main conversation stays clean.

**PROMPT**

```
Use Explore agents to find everything in this codebase related to <topic,
e.g. leads, admin pages, Supabase tables>. Report which files and patterns
already exist that this new feature should reuse. Don't change anything.
```

### C. Framework docs

This repo uses a newer Next.js than most training data, so check the bundled docs.

**PROMPT**

```
Read the relevant guide in node_modules/next/dist/docs/ for <routing /
server actions / caching> and tell me what's different from older
Next.js that affects this feature.
```

---

## 3. Write the spec

Goal: turn the brainstorm and research into a document you can share and come back to.

**OPTION 1: A SHARED DOC (EDITABLE AND COMMENTABLE)**

```
Write a product spec doc for this idea: problem, target users, goals,
non-goals, user stories, MVP scope vs later, data model sketch, open
questions, and risks. Base it on our brainstorm and research above.
```

**OPTION 2: A FILE IN THE REPO**

```
Save the spec as docs/specs/<feature-name>.md so it's versioned with the code.
```

**SAVE KEY DECISIONS FOR FUTURE SESSIONS**

```
Remember that for <feature>, we decided <decision> because <reason>.
```

---

## 4. Plan the implementation with `/plan`

Goal: agree on the approach before any code changes. In plan mode Claude can read code but can't change it.

**PROMPT**

```
/plan
Using the spec in docs/specs/<feature-name>.md, design the implementation.
Include: files to create/modify, database/Supabase changes, API/server
actions, UI components, order of work in small shippable steps, testing
approach, and anything risky or irreversible (migrations, prod data).
```

**REFINE BEFORE APPROVING**

```
Split step 3 into smaller steps.
What's the rollback if the migration fails?
Which parts can be built and tested independently?
```

> **Second opinion:** "Use a Plan agent to propose an alternative architecture and compare it with yours." You can also toggle plan mode with **Shift+Tab** in the terminal, or with the mode selector in VS Code.

---

## 5. Design

Goal: see and click the idea before building it.

| Goal | How |
|---|---|
| Quick clickable prototype | "Build an interactive HTML prototype of the `<screen>` as an artifact so I can click through it." |
| Design in Figma | "Create a Figma design for the `<screen>` using our design system." (Figma connector) |
| Figma to code | "Implement this Figma frame as a React component: `<figma.com link>`" |
| Template-based page | `/wireframe-assembly`, Digitalfeet's template system |
| Diagrams | "Draw a diagram of the user flow / data flow for this feature." |

**PROMPT**

```
Before we code, mock up the 3 main screens from the plan as a clickable
prototype. Match the styling of the existing admin pages.
```

---

## 6. Develop

Goal: build in small, verifiable steps.

**OPTIONAL: TURN THE PLAN INTO TICKETS**

```
Create ClickUp tasks for each step of the approved plan in list <list name>.
```

**BUILD ONE STEP AT A TIME**

```
Implement step 1 of the plan only. Stay on branch levi. Stop when it's
done and tell me how to verify it.
```

| Command | Use |
|---|---|
| `/run` | Starts the app and checks that the change works. |
| `/compact` | Shrinks a long conversation, keeping the key points. |
| `/resume` | Continues an earlier session. |
| `/end-of-day` | Saves the day's context and pushes, so you can pick up on another machine. |

---

## 7. Review before you merge

Goal: catch bugs and security issues before they reach production.

| Command | What it checks |
|---|---|
| `/code-review` | Bugs in your changes. `/code-review high` looks more broadly. |
| `/security-review` | Security problems on the branch: auth, injection, exposed secrets. |
| `/simplify` | Cleanup and simplification, not bugs. |

**PROMPT**

```
Commit this work on levi and open a PR to main with a clear description.
```

> **Remember:** `main` deploys to production. Work on `levi` and merge only after review.

---

*Claude Code workflow guide · webcalculator-v2 · October 2026*
