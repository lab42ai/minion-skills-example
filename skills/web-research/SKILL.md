---
name: web-research
description: Search the web for current information on any topic, synthesize results from multiple sources, and present a clear summary.
metadata: {"minion":{"emoji":"🔍"}}
---

# Web Research

Searches the web for up-to-date information on a given topic, reads relevant pages, and delivers a concise, sourced summary.

## Trigger Phrases

- "Search for [topic]"
- "What's the latest on [topic]?"
- "Research [topic]"
- "Find me [something] in [year]"
- "Top [X] [things] right now"
- "What are the best [things] this month?"
- "Compare [X] vs [Y]"

## Workflow

### Step 1: Understand the Query

Extract from the user's message:
- **Topic** — what they want to know about
- **Scope** — how broad or narrow (top 5 vs deep dive)
- **Recency** — do they want current info (this month, this year, today)?

### Step 2: Search the Web

Use `web_search` to find relevant, recent results:

```
web_search: [topic] [year/month if recency matters]
```

Run 2-3 searches with different angles if the topic is broad. For example:
- "top AI tools released March 2026"
- "best new AI tools 2026 review"
- "AI tool launches this month"

### Step 3: Read Top Sources

Pick the 3-5 most relevant results and fetch their content:

```
web_fetch: [url]
```

Prioritize:
- Recent articles (this month/week over older)
- Reputable sources (known tech sites, official announcements)
- Diverse perspectives (not all from the same site)

### Step 4: Synthesize and Present

Combine findings into a clean, structured response:

```
🔍 [Topic] — [Date]

1. **[Item/Finding]**
   [2-3 sentence summary]
   Source: [site name]

2. **[Item/Finding]**
   [2-3 sentence summary]
   Source: [site name]

3. **[Item/Finding]**
   [2-3 sentence summary]
   Source: [site name]

...

💡 Key Takeaway: [One sentence insight from the research]
```

### Step 5: Offer Follow-Up

After presenting results, offer to go deeper:

```
Want me to:
- Dig deeper into any of these?
- Compare specific options?
- Save this research to your workspace?
```

## Response Format Guidelines

- Lead with the answer, not the process
- Keep each item to 2-3 sentences max
- Always cite the source (site name, not full URL)
- Include dates where relevant to prove recency
- If results are thin or outdated, say so honestly
- For comparisons, use a simple table format

## Important Notes

- Always include the current date in the header so the user can see the info is fresh
- If `web_search` returns no useful results, try rephrasing before giving up
- Do not fabricate results — only report what you actually found
- Keep the total response concise — aim for 5-10 items max unless the user asks for more
- For "vs" comparisons, search for both sides separately to avoid biased results
