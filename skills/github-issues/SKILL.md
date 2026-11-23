---
name: github-issues
description: Write or rewrite GitHub issues that are short, scannable and visual, using GitHub's own formatting (tables, task lists, callouts, folds, the right Mermaid type) and, when an issue needs more context, an HTML issue artifact attached with `gh issue artifact`. Use this whenever the user asks to open, draft, write, file, tidy up, restructure or "make nicer" a GitHub issue, proposal, RFC, bug report, feature request, meeting or community-call issue, or asks to attach a screenshot, explainer page or document to an issue, even if they don't say "skill" or "format". Not for PR bodies (use the PR skill) or for comments on an existing thread.
---

# Writing GitHub issues

Readers skim issues on a phone between meetings. A good issue gets its point across in ten seconds, and the detail is there if they want it. Everything below follows from that.

## Workflow

1. **Get the facts.** If you're editing an issue, read it first (`gh issue view <n> --json title,body,labels`) and save a copy of the body to the scratchpad, so the user can roll back.
2. **Draft the body** with the layout below. Write it to a file and use `--body-file`, because shell quoting mangles Markdown.
3. **Decide whether it needs an artifact** (see "Adding more context"). If it does, offer it. Don't add one unasked.
4. **Post it** with `gh issue create --body-file` or `gh issue edit <n> --body-file`. Creating or editing an issue is public, so make sure the user asked for it. Drafting doesn't need permission.
5. **Report back** in a few lines: the link and a small before/after table of what changed.

## Layout

```markdown
## What
One or two sentences. What changes, for whom.

(Optional table, if there are two or more things to compare.)

## Why
- 2–5 bullets. Each a concrete problem, not a feeling.

## Proposed changes        ← or "Steps to reproduce" / "Done when" for bugs and tasks
- [ ] One checkable item per line

> [!IMPORTANT]
> **Open questions: please reply below**
> - …

Related: #123
```

Adapt the headings to the kind of issue. A bug report needs "Steps to reproduce / Expected / Actual", not "Why". Keep **What** first whatever the type.

## GitHub formatting: pick by purpose

| Purpose | Use |
|---|---|
| Comparing two or more options or things | A table |
| Work to be done | A task list (`- [ ]`) |
| Something readers must act on (open questions, decisions) | `> [!IMPORTANT]` |
| Background or constraints that stay the same | `> [!NOTE]` |
| Risk or breaking change | `> [!WARNING]` |
| Long logs, specs, background | `<details><summary>Short label</summary> … </details>` |
| Pointing at code | A permalink with a commit SHA and line range (`blob/<sha>/path#L10-L20`). GitHub shows the snippet inline. |
| Big work with separate parts | Sub-issues instead of one long checklist |

See `references/github-markdown.md` for exact syntax and less common features.

## Diagrams: only when they earn their place

Add a Mermaid diagram only when it shows **flow, order, state or timing** that a table or list can't. A diagram that repeats a table is clutter. Ask: "Would a reader learn something from this they didn't get from the text?" If not, leave it out.

Pick the type that fits the content:

| Content | Mermaid type |
|---|---|
| Schedule, phases, a trial run | `timeline` (no dates needed) or `gantt` (real dates only) |
| Who calls whom, and in what order | `sequenceDiagram` |
| Lifecycle or status changes | `stateDiagram-v2` |
| Branching decisions | `flowchart`, as a last resort |

Don't make up dates for a `gantt`. If the start date is still open, use `timeline`.

## Adding more context: issue artifacts

GitHub issues can have **artifacts** (public preview, `gh` 2.103+): documents, HTML pages and links that sit in their own section on the issue, with version history.

| Need | Command |
|---|---|
| Visual explainer, comparison, mock-up, interactive diagram | `gh issue artifact create <n> page.html` |
| A long spec, research notes or plan | `gh issue artifact create <n> notes.md` (add `--type plan` for a plan) |
| A screenshot or GIF | `gh issue artifact create <n> notes.md --attach shot.png#alt text` (images go inside a Markdown artifact) |
| A link to a Slack thread, prototype or doc | `gh issue artifact create <n> https://… --name 'Label'` |
| Update an artifact (creates a new version) | `gh issue artifact edit <n> <artifact-no> --body-file page.html` |
| Roll back | `gh issue artifact edit <n> <artifact-no> --restore-version <v>` |

When to **offer** an HTML artifact: the issue is a proposal or decision with several moving parts, or it needs a visual that Markdown can't do well (layouts, side-by-side mock-ups, anything interactive). Skip it for bugs, small tasks and anything the body already covers.

Rules for HTML artifacts, and why:
- **Keep the issue body complete on its own.** HTML opens as source first and readers must choose Preview, which shows a warning. Many won't click, so essentials never live only in the artifact.
- **Make it self-contained.** Inline the CSS and images, and don't use CDNs or remote fonts. The preview is sandboxed, and external requests may be blocked or look suspicious.
- **Add little or no JavaScript.** It may not run in the sandbox, and HTML/CSS alone covers most explainers.
- Start from `assets/explainer-template.html`, which already follows these rules and works in light and dark mode.
- Mention the artifact in the body with one line, such as "📎 Visual overview attached as an artifact."

## Writing style

- Plain language and short sentences. No filler such as "This issue aims to…".
- Use concrete numbers where you have them ("about 8 agenda items in 45 minutes").
- Don't invent facts, dates, owners or numbers to fill a template. Put real unknowns under open questions.
- Don't @-mention anyone unless the user names them. Mentions notify real people.
