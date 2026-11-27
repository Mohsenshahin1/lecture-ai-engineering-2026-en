# Lecture 06 answers

## Exercises

- Tool name in STEP 1 — glossary_search_glossary
- STEP 2 [tool call] — glossary_search_glossary({'query': 'homework levels'})
- STEP 3 [tool call] — fetch_fetch({'url': 'https://modelcontextprotocol.io/'})
- STEP 4 [tool call] — aws_aws___search_documentation({'topics': ['general'], 'search_phrase': 'Amazon S3 versioning'})
- Observations and questions — Each server's tools are prefixed by server name (glossary_, fetch_, aws_) to avoid collisions, and tool count grows automatically as servers are added to mcp_servers.json (1 → 2 → 7) with zero agent-side code changes, confirming the server decides what's published. More importantly, the model's behavior around the "end with Lecture NN" instruction was inconsistent across unrelated-content questions: for the fetch question (general MCP website content), it correctly avoided citing a lecture number and instead noted the question was outside course materials. But for the AWS question (STEP 4), it confabulated "(Lecture 1)" even though the entire answer came from aws_search_documentation with zero connection to any course lecture. This is a direct example of the lecture's point that most failures trace back to harness/instruction design rather than model capability: a instruction meant for course-content questions ("cite the lecture") got blindly applied to a tool result it was never designed for.


## Homework

- Level attempted (Basic / Implementation / Advanced) — Basic
- What you did — Added `server.tool()(tools.get_lecture_summary)` in `app/mcp_server.py` below the existing TODO comment, exposing the Lecture 03 function as a second tool on the glossary MCP server, without touching `app/mcp_agent.py` or any agent-side code.
- Results — `git status` showed only `app/mcp_server.py` (plus `app/mcp_servers.json`, already modified from the Steps 3–4 exercise) changed. `--list-tools` showed `glossary_get_lecture_summary` now present (8 tools total, up from 7). Asking "What will we learn in Lecture 02?" correctly triggered `[tool call] glossary_get_lecture_summary({'lecture': 2})` and returned an accurate, well-cited answer ending in "Lecture 02". `uv run pytest -q` passed all 21 tests.
- Observations and questions — Confirms the exercise's key point from the agent side too: the agent never had to be told a new tool exists — it re-discovers the server's tool list on every start. This also makes me want to test what happens if I ask about a lecture that genuinely isn't in the glossary (e.g. "Lecture 99") to see whether the model's behavior there matches the honest "I don't know" pattern from Lecture 01, or repeats the kind of confabulated citation seen with the AWS tool in the exercise.