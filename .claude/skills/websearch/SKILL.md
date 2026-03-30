---
name: websearch
description: >
  Web search superpowers — search the web, fetch URLs, research topics, find documentation,
  and compile summarized findings. Use when the user wants to look something up online,
  research a topic, find the latest info, or fetch a webpage.
  Triggers on: "search the web", "look up", "find online", "websearch", "/websearch",
  "what is the latest", "google", "search for", "fetch this URL", "research".
---

# Web Search Superpowers

Use WebSearch and WebFetch to find information on the web, research topics, fetch pages,
and deliver clear summarized answers.

## Workflow

Make a todo list and work through each step.

### 1. Understand the Query

Read the user's request and identify:
- The core question or topic
- Whether a direct URL was provided (use WebFetch) or a topic to search (use WebSearch)
- Whether they need a quick answer, deep research, or multiple sources

### 2. Search or Fetch

**For topic searches** — use WebSearch:
```
WebSearch: "<specific query>"
```
Tips:
- Use precise, targeted queries (e.g. `"python asyncio tutorial 2024"` not just `"python"`)
- If the first query returns poor results, rephrase and try again
- For broad topics, run 2–3 searches with different angles

**For direct URLs** — use WebFetch:
```
WebFetch: <url>
```
Tips:
- Fetch the exact URL the user provides
- If a page is too large, focus on the relevant section

**For deep research** — combine both:
1. WebSearch to find the best sources
2. WebFetch on the top 2–3 results for full content

### 3. Synthesize Results

After gathering results:
- Extract the key facts, answers, or data relevant to the user's question
- Cross-reference multiple sources if doing research
- Note the source URLs for attribution
- Identify any conflicting information and flag it

### 4. Deliver the Answer

Present findings clearly:
- **Lead with the direct answer** (don't bury it)
- Use bullet points or headers for multi-part answers
- Include source URLs so the user can verify
- If information may be outdated (cutoff), note that and mention the search date
- For code/docs: include relevant snippets or examples found

**Example output format:**
```
## Answer

<Direct answer here>

## Details

- <Key point 1> (source: <url>)
- <Key point 2> (source: <url>)

## Sources
- <url 1>
- <url 2>
```

### 5. Follow-up

If the results were incomplete or the user seems to need more:
- Offer to search deeper or with different terms
- Suggest related queries they might find useful

## Wrap up

Confirm what was searched/fetched and give the user a clean, well-sourced answer.
If research spanned multiple searches, briefly summarize the approach taken.
