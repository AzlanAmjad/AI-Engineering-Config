---
name: context7-mcp
description: This skill should be used when the user asks about libraries, frameworks, API references, or needs code examples. Activates for setup questions, code generation involving libraries, or mentions of specific frameworks like React, Vue, Next.js, Prisma, Supabase, etc.
---

When the user asks about libraries, frameworks, or needs code examples, use Context7 to fetch current documentation instead of relying on training data.

## When to Use This Skill

Activate this skill when the user:

- Asks setup or configuration questions ("How do I configure a Next.js proxy?")
- Requests code involving libraries ("Write a Prisma query for...")
- Needs API references ("What are the Supabase auth methods?")
- Mentions specific frameworks (React, Vue, Svelte, Express, Tailwind, etc.)

Skip external lookups for language fundamentals or purely local changes fully determined by the repository.

## How to Fetch Documentation

Discover the available Context7 tools using the host's tool discovery mechanism; tool names may have a server prefix. If the tools are unavailable, report that explicitly and consult official documentation using available tools. Never invent a successful lookup.

### Step 1: Resolve the Library ID

If the user supplies a library ID in `/owner/repo` or `/owner/repo/version` form, use it directly and skip resolution.

Otherwise call `resolve-library-id` with:

- `libraryName`: The library name extracted from the user's question
- `query`: What to look up in the library's documentation (improves relevance ranking)

### Step 2: Select the Best Match

From the resolution results, choose based on:

- Exact or closest name match to the query
- Higher benchmark scores indicate better documentation quality
- If the user mentioned a version (e.g., "React 19"), prefer version-specific IDs

### Step 3: Fetch the Documentation

Call `query-docs` with:

- `libraryId`: The selected Context7 library ID (e.g., `/vercel/next.js`)
- `query`: What to look up in the library's documentation, scoped to a single concept

If the user's question spans multiple distinct concepts (e.g. routing and auth and caching), make a separate `query-docs` call per concept with the same library ID, unless the question is about how the concepts interact. Combined queries dilute ranking and return shallow results for each topic.

### Step 4: Use the Documentation

Incorporate the fetched documentation into your response:

- Answer the user's question using current, accurate information
- Include relevant code examples from the docs
- Cite sources and the library version when relevant
- If no reliable source is found, state the limitation, distinguish assumptions from verified facts, and give a validation step

## Guidelines

- **Be specific**: Describe what to look up in the library's documentation, but keep each query to a single concept
- **One topic per query**: Split multi-topic questions into separate `query-docs` calls. Resolve the library ID once, then query per concept, unless the question is about how the concepts interact
- **Version awareness**: When users mention versions ("Next.js 16", "React 19"), use version-specific IDs if available from the resolution step
- **Prefer official sources**: When multiple matches exist, prefer official/primary packages over community forks
- **Bound calls**: At most 3 resolution calls and 3 documentation calls per user question, including calls made under Context7 instructions
- **Protect private data**: Queries must contain only minimal public API names and generic task descriptions, never secrets, private code, or confidential context
