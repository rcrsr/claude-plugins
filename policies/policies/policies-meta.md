# Meta Policies

Standards for policy documents that get injected into agent prompts.

## Table of Contents

- [Overview](#meta1-overview)
- [Document Structure](#meta2-document-structure)
  - [Required Elements](#meta21-required-elements)
  - [Section Numbering](#meta22-section-numbering)
  - [Table of Contents](#meta23-table-of-contents)
- [Writing for Prompts](#meta3-writing-for-prompts)
  - [Be Explicit](#meta31-be-explicit)
  - [Lead with Context](#meta32-lead-with-context)
  - [Show Correct and Incorrect](#meta33-show-correct-and-incorrect)
  - [Use Tables for Rules](#meta34-use-tables-for-rules)
- [Section Design](#meta4-section-design)
  - [Size Limits](#meta41-size-limits)
  - [Consolidation Triggers](#meta42-consolidation-triggers)
- [Cross-References](#meta5-cross-references)
- [Maintenance](#meta6-maintenance)

## {§META.1} Overview

Policy sections are injected into agent prompts via the `## Required Policies` mechanism. Effective policies are explicit, actionable, and concise—every word consumes tokens and competes for attention.

**Core Principles:**

| Principle | Why It Matters |
| --------- | -------------- |
| Be explicit, not implicit | Vague instructions reduce compliance |
| Lead with context | Agents need to know WHY before WHAT |
| Show correct AND incorrect | Contrast clarifies boundaries |
| Keep sections under 50 lines | Long sections dilute impact |

**Prefix Convention:**

Each policy uses a unique prefix (e.g., `§AUTH`, `§API`) to identify its domain. Prefixes must be 2-6 uppercase characters.

## {§META.2} Document Structure

### {§META.2.1} Required Elements

Every policy document must include these five elements in order:

1. **Title** — `# Domain Name Policies`
2. **Introduction** — One sentence stating what this policy governs
3. **Table of Contents** — Linked section hierarchy
4. **Sections** — Content with `{§PREFIX.X}` headers
5. **End marker** — `{§END}` on its own line at document end

**Template:**

```markdown
# Domain Policies

Governs [specific behavior] for [specific context].

## Table of Contents

- [Section Name](#prefix1-section-name)

## {§PREFIX.1} Section Name

Content here.

{§END}
```

### {§META.2.2} Section Numbering

Wrap section identifiers in curly braces for parsing:

| Level | Format | Example |
| ----- | ------ | ------- |
| Major section | `{§PREFIX.X}` | `{§AUTH.1}` |
| Subsection | `{§PREFIX.X.Y}` | `{§AUTH.1.2}` |
| Sub-subsection | `{§PREFIX.X.Y.Z}` | Avoid—max 2 levels |

Numbers are sequential within each level. Never skip numbers.

### {§META.2.3} Table of Contents

Anchor format removes the `{§}` wrapper, dots, and lowercases everything:

| Header | Anchor |
| ------ | ------ |
| `## {§AUTH.1} Token Flow` | `#auth1-token-flow` |
| `### {§AUTH.1.2} Validation` | `#auth12-validation` |

Update the TOC whenever sections change. Broken links waste agent reasoning.

## {§META.3} Writing for Prompts

### {§META.3.1} Be Explicit

Vague instructions reduce compliance. Replace qualitative language with specific thresholds.

| Incorrect | Correct |
| --------- | ------- |
| "Keep examples brief" | "Examples must be under 10 lines" |
| "Consider security" | "Reject requests missing auth tokens" |
| "Use appropriate error handling" | "Catch exceptions at API boundaries; log with request ID" |
| "Follow best practices" | [Delete—provides no actionable guidance] |

Every rule must pass the **actionability test**: Can an agent determine unambiguously whether it complied?

### {§META.3.2} Lead with Context

Agents comply better when they understand WHY a rule exists. State context before rules.

**Incorrect** (rule without context):
```markdown
## {§API.3} Rate Limiting

Never exceed 100 requests per minute.
```

**Correct** (context then rule):
```markdown
## {§API.3} Rate Limiting

The external API enforces 100 req/min limits and blocks violators for 1 hour.

**Rule:** Batch requests and add 50ms delays between calls. Never exceed 80 req/min to maintain buffer.
```

### {§META.3.3} Show Correct and Incorrect

Examples that show only the correct approach leave the boundaries ambiguous. Always pair correct with incorrect.

**Format:**

```markdown
**Correct:** [what to do]
[code or prose example]

**Incorrect:** [what NOT to do]
[code or prose example]
[optional: why this fails]
```

**Example:**

```markdown
**Correct:** Validate before processing
​```python
if not request.user_id:
    raise ValidationError("user_id required")
process(request)
​```

**Incorrect:** Process then check
​```python
result = process(request)  # May corrupt data
if not request.user_id:
    rollback(result)  # Too late—side effects occurred
​```
```

### {§META.3.4} Use Tables for Rules

Tables encode rules more precisely than prose and consume fewer tokens.

**Incorrect** (prose):
```markdown
When the response status is in the 200 range, process normally. If it's a
4xx error, log it and return a user-friendly message. For 5xx errors,
retry up to 3 times with exponential backoff before failing.
```

**Correct** (table):
```markdown
| Status | Action |
| ------ | ------ |
| 2xx | Process response |
| 4xx | Log error; return user message |
| 5xx | Retry 3x with exponential backoff; then fail |
```

## {§META.4} Section Design

### {§META.4.1} Size Limits

Long sections dilute compliance. Agents weight earlier content more heavily—bury important rules in 200 lines and they get missed.

| Element | Limit | If Exceeded |
| ------- | ----- | ----------- |
| Policy document | 500 lines | Split into multiple policies |
| Major section | 50 lines | Split into subsections |
| Code example | 10 lines | Show pattern only; omit boilerplate |
| Subsections per section | 6 | Consolidate related items |

### {§META.4.2} Consolidation Triggers

Consolidate when any of these occur:

1. Section exceeds 50 lines
2. More than 6 subsections in one section
3. Same concept appears in multiple sections
4. Examples exceed 10 lines
5. Prose can be replaced with a table

**Techniques:**

- Merge related subsections under one header
- Convert prose comparisons to tables
- Replace full implementations with 5-line patterns
- Reference other policies instead of duplicating

## {§META.5} Cross-References

Reference other policies using `§PREFIX.X` notation instead of duplicating content.

**Format:**
```markdown
Apply retry logic per §API.3 before returning errors.
```

**Rules:**

1. Reference the most specific section (§API.3.2 not §API)
2. Verify the referenced section exists before committing
3. Never duplicate content across policies—reference instead

## {§META.6} Maintenance

**Review triggers:**

- Document exceeds 500 lines
- Section exceeds 50 lines
- Users report confusion or non-compliance
- Referenced section was renamed or removed

**Streamlining process:**

1. Count lines per section
2. Identify sections over 50 lines
3. Convert prose to tables where possible
4. Merge sections under 10 lines into parents
5. Update TOC to match changes
6. Verify all cross-references resolve

**Deprecation format:**

```markdown
> **Deprecated:** Moved to §NEW.X. Remove by YYYY-MM-DD.
```

{§END}
