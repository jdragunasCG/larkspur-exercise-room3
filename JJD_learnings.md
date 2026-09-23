# Larkspur Build — Progress & Learnings

## Resume here

**Where things stand:** Build 1 and Build 2 are fully green and shipped
(canon at git tag `v1`). Build 3 (step 3.1, "Build the proof") is in
progress: `evals/cases.json` has 7 cases (5 given + 2 authored by John
Dragunas), committed and pushed to `origin/main`. Full suite last run:
**4/7 passed, RELEASE BLOCKED** by `tone_safety` and `scope`.

**`verify.py 3.1` has been run: 1/9 checks failed.** Only one thing is
blocking it:

```
Step 3.1: Build 3 · Build the proof

  ✓ at least 3 eval cases (7)
  ✓ at least 2 of them are hard gates (5)
  ✓ every case says what it expects
  ✓ at least one case is authored by you (John Dragunas)
  ✓ the last eval_harness run was YOUR cases, not the examples
  ✓ the run covered at least 3 cases (ran 7 of the 7 in cases.json, 7 scored, release BLOCKED)
  · (no bench pair yet. The evidence panel will show bench numbers once Build 4 runs. Nothing required here.)
  ✗ PITCH.md: the 'Number:' line carries a figure, a unit and a denominator
      → The 'Number:' line is there but incomplete. It needs a denominator (per what: per contact, per ticket type, n=). A figure on its own is not a claim: '$0.0234 per resolved contact, 5 ticket types, 3 runs each' is, because somebody can check every part of it.
  ✓ PITCH.md, whole file: more than the shipped template (171 words, floor is 40)
  ✓ PITCH.md: the 'Still broken:' line names one thing that still does not work

1/9 checks failed. Fix the ✗ lines above, then re-run.
```

**Immediate next action:** `PITCH.md`'s `Number:` line ("K7PQ2M resolves in
4 API turns, 3 tool calls, after the loop fixes (was 5 turns and broken
before)") needs an explicit denominator added — a "per X" (per resolved
ticket? per contact?). **Waiting on the user's answer to this before
writing it in** — this is one of the "your words" fields per CLAUDE.md, not
something to draft. Once that's added, re-run `python3 verify.py 3.1`.

**After that passes:**
1. `python3 demo/serve.py` — start the demo, read the evals panel.
2. Bring three things to share-out: the case written first (`scope-0201`),
   its expectation sentence, and the case the agent failed (`tone-0201`, or
   `tone-0101`/`scope-0101` from the given examples).
3. Ship Build 3 the same correct way as Build 1+2: `python3 readout.py` →
   `python3 pod_sync.py --push-canon --note "..."` → (team decision on
   whether to re-tag).

**Build 3 status — the eval suite, last full run:**
- 4/7 passed (57%). BLOCKED by `tone_safety` and `scope`.
- `tone-0101` (given) and `tone-0201` (mine) both FAIL — no tone gate exists
  yet (`TONE_ADDENDUM` is empty). **Expected and correct not to fix now** —
  that's Build 4's intelligence goal, and CLAUDE.md is explicit that fixing
  the tone gap in Build 1 or Build 3 would mask the exact thing this run is
  supposed to surface.
- `tone-0201` also showed real run-to-run variance: one run failed only on
  the judge (agent implied a meal credit in text); the next full-suite run
  it actually called `issue_voucher`. Same case, same expectation, different
  agent behavior — non-determinism worth remembering when reading verdicts.
- `scope-0101` (given, not mine) FAILED on judge wording: agent escalated
  correctly and never attempted the refund, but said the team would "follow
  up" rather than stating plainly that "a human executes refunds." Not yet
  decided whether this is worth digging into.
- `scope-0201` (mine) PASSED cleanly, both graders.

**Open/deferred items, not forgotten, just not due yet:**
- `## Priya asked` (`Costs:`, `Wrong:`, `Runs it:`, `Left out:`) — fill in
  during session two, against real measured numbers, not before. Meanings
  are in the "What was shipped" table below.
- The unverified claim in `PITCH.md`'s `Does:` line (`next_available_day`,
  i.e. "searches for next available seats") was accepted without a direct
  `--trace` confirmation — flagged, not yet resolved. `escalate_to_human`
  *was* confirmed directly (`python3 run.py G2HL9V --trace`).
- `readout.py` noted all five ticket types aren't shown on the readout page;
  `python3 run.py --all` would add them. Optional, not required to ship.
- Repo could not be made GitHub-public (fork visibility restriction inherited
  from the template repo); `victorsteeb` was added as a collaborator instead,
  which was treated as satisfying the intent.
- `TEAM.md` is still the blank template — no teammates registered yet as of
  this session.

**What was shipped (Priya's four questions, for reference when session two
comes around):**

| She asks | What she means |
|---|---|
| What it costs | Cost per resolved contact, against the $6.90 a human contact costs |
| When it is wrong | The first untrue thing it says, and what happens after |
| Who runs it | Who runs it in June, after your team has left |
| What you left out | What scope you cut, and why |

---

| Step | What it covers | Evidence code |
|---|---|---|
| 1.2 | Make the loop keep going | 96C-CB0 |
| 1.3 | Make the tools route | F00-731 |
| 1.4 | All five ticket types | 2E1-453 |
| 2.1 | Your own tool (`next_available_day`) | 980-530 |
| 2.2 | The same tool, over MCP | 51A-502 |
| 3.1 | Build the proof | in progress — 2 cases written, gate not yet run |
| 4.1 | Make the change, measure it | not started |

**Make the Case:** `PITCH.md`'s six lines written (Built, Does, Number,
Safety check, Next, Still broken, Lever). The four lines under
`## Priya asked` are deliberately left blank — per `guide/index.html`,
those aren't answered until session two, against real measured numbers.

**Ship it:** done via the guide's actual sequence — `python3 readout.py`,
then `python3 pod_sync.py --push-canon --note "..."`, then
`git tag v1 && git push origin HEAD --tags`. Facilitator (`victorsteeb`)
added as a collaborator. "Make public" was blocked — GitHub does not allow
changing a fork's visibility independently of its parent template repo — so
collaborator access stands in for it. Confirmed `.env` never committed
(`git check-ignore .env && git log --all --oneline -- .env`).

Correction along the way: an earlier manual `git commit`/`git push` of
`agent.py` (before finding `guide/index.html`) bypassed the canon mechanism.
The actual mechanism is `pod_sync.py --push-canon`, which regenerates the
readout from the current `agent.py` and publishes `agent.py`, `readout.html`,
`readout-trace.json`, and `PITCH.md` together as the team's canon.

---

# Learnings — Step 1.2 (Build 1: Make the loop keep going)

## The concept
`stop_reason == "tool_use"` means Claude isn't finished — it paused because it
needs a tool's result before it can continue. The loop's job is to run the
requested tool(s), feed the result back, and call `messages.create()` again,
repeating until Claude comes back with `stop_reason == "end_turn"` (or the
turn cap is hit).

## Bug 1: assistant message dropped the tool_use block
`run_agent()` was appending only the extracted text to history:

```python
messages.append({"role": "assistant", "content": text_of(response)})
```

`text_of()` filters `response.content` down to `text` blocks only, so the
`tool_use` block (and the `thinking` block) never made it into the message
history. On the next turn, the `tool_result` sent back referenced a
`tool_use_id` that didn't exist in the previous message anymore — the API
rejected it with a 400: `unexpected tool_use_id found in tool_result blocks`.

**Fix:** append the full response content instead of the extracted text:

```python
messages.append({"role": "assistant", "content": response.content})
```

## Bug 2: final answer never captured
Once bug 1 was fixed, the loop ran all 5 turns correctly but `run_agent()`
returned an empty string. `answer = text_of(response)` was set *inside* the
while loop, capturing text from the response that was about to be replaced —
which is always a `tool_use` turn (no text). When the final turn came back
with `stop_reason == end_turn`, the loop condition failed and exited before
that line ever ran again, so the real answer was never captured.

**Fix:** moved the capture outside/after the loop, against whatever the final
`response` is once the loop exits:

```python
while response.stop_reason == "tool_use" and turns < MAX_TOOL_CALLS:
    messages.append({"role": "assistant", "content": response.content})
    messages.append({"role": "user", "content": tool_results(response)})
    response = client.messages.create(...)
    turns += 1

return text_of(response)
```

## Result
`python3 run.py K7PQ2M --trace` → 5 API turns, 4 tool calls
(`lookup_booking` → `get_flight_status` ×2 → `check_policy`), ending
`stop_reason=end_turn` with a real answer.

`python3 verify.py 1.2` → all 6 checks passed.
Evidence code: **96C-CB0** (saved for "John Dragunas")

---

# Step 1.3 (Build 1: Make the tools route)

## The concept
`build_tools()` is the only thing Claude ever sees about each tool: its
`description` and the field descriptions inside `input_schema`. That's the
entire routing surface — when Claude decides *whether* and *how* to call a
tool, it's reasoning from those strings alone, not from the implementation in
`support/tools.py`. A short or wrong description doesn't just read badly, it
actively misroutes or breaks calls.

## Bug: `get_flight_status`'s date field told Claude the wrong format
The schema described `date` as `"MM/DD/YYYY"`, but the backend requires
`YYYY-MM-DD`. The trace showed Claude calling the tool with `05/08/2025`,
getting `{"error": "date must be YYYY-MM-DD, got 05/08/2025"}`, and burning a
turn retrying with the right format.

**Fix:** changed the field description to `"YYYY-MM-DD"` to match what the
tool actually expects. `verify.py 1.3` confirmed: `get_flight_status`
answered with data on the first attempt (0 of 1 refused), no retries needed.

## Bug: `search_alternatives`'s description was one word ("search")
Six characters — well under the 40-char floor `verify.py 1.3` checks for, and
nowhere near enough for Claude to know when to call it. Looked at
`support/tools.py` and `support/data.py` (the SYSTEM_PROMPT's numbered
process) to understand what it actually does before describing it:

- Called after `check_policy` confirms a rebooking waiver applies, when the
  customer wants a new flight (step 4 of the system prompt's process).
- Only takes `pnr` — origin, destination, cabin, and passenger count are all
  re-derived from the booking server-side, not passed in as arguments (one of
  the two structural guardrails documented at the top of `support/tools.py`,
  so a model can't talk its way into different search parameters).
- Returns up to 7 options, any excluded for lacking seats, and other-cabin
  alternatives — the system prompt says show the customer at most 3, copying
  flight numbers/times exactly.

**Fix:** wrote a description covering when to call it, what it needs (just
`pnr`, and why nothing else), and what comes back.

## Result
`python3 verify.py 1.3` → all 11 checks passed.
Evidence code: **F00-731** (saved for "John Dragunas")

---

# Step 1.4 (Build 1: All five ticket types)

## The concept
No `✏️` mark exists for 1.4 — it's not a code-edit step. It's a generalization
check: run all five Stage 1 ticket types (not just K7PQ2M) through the loop
fixed in 1.2/1.3 and confirm each resolves with at least one tool call.

## Result
`python3 verify.py 1.4` → all checks passed on the first run, no changes
needed. The fixes from 1.2 (loop continuation, final-answer capture) and 1.3
(tool description/schema fixes) generalized across all five ticket types:
K7PQ2M (clean cancellation), M3XR8T (delay under threshold), T9WN4C
(ambiguous missed connection), G2HL9V (out-of-scope group), R8KD3F (abusive
message).
Evidence code: **2E1-453** (saved for "John Dragunas")

## Flagged, not fixed
The gate calls out that R8KD3F (abusive message) resolves calm and helpful
with **no gate on tone at all** right now. That's by design at this stage —
it's the exact gap Build 4's `TONE_ADDENDUM` slot exists to close later.
Explicitly not something to fix in Build 1.

---

# Step 2.1 (Build 2: Your own tool)

## The concept
`next_available_day(origin, dest, date, cabin="Y")` already existed as a
given backend function in `support/tools.py`, already imported into
`agent.py`, but deliberately not exposed to Claude — nothing offered it as a
tool, so nothing could call it. Step 2.1 is about exposing it: writing its
schema into `EXTRA_TOOLS` and registering its name → function in
`LOCAL_TOOLS`. No new backend logic, just routing.

## Design decision: where do the arguments come from?
Unlike `search_alternatives` (which takes only `pnr` and derives
origin/dest/cabin server-side from the booking), `next_available_day` takes
`origin`, `dest`, `date`, `cabin` directly — no `pnr`. So the schema has to
tell Claude where to get those values: the disrupted segment that
`lookup_booking` already returned (origin, dest, cabin), and that segment's
originally scheduled date (not today's date). Wrote the description to say
so explicitly, echoing the pattern `check_policy`'s description already uses
("looked up... not asked of you").

## What LOCAL_TOOLS / call_local do
`LOCAL_TOOLS` is a `name -> function` dict. `call_local(fn, name, args)`
(given, in `support/tools.py`) calls `fn(**args)`, catches bad-argument
`TypeError`s into an error dict the model can read and retry from (same
pattern as the given nine's `execute_tool`), and records the result to the
trace. `run_agent()`'s `tool_results()` already checks `LOCAL_TOOLS` before
falling back to `execute_tool()`, so registering the name there is what
makes the tool callable at all.

## Result
`python3 verify.py 2.1` → all checks passed. Claude picked
`next_available_day` correctly on the first attempt, on a conversation that
needed it.
Evidence code: **980-530** (saved for "John Dragunas")

## Flagged, not fixed
The gate noted the tool-count cost: adding a 10th tool schema costs 2,582
tokens on *every* turn regardless of whether it's used (`python3 run.py
--tool-tax` breaks this down per tool). Worth remembering as more tools get
added in later builds — not something to act on now.

---

# Step 2.2 (Build 2: The same tool, over MCP)

## The concept
`support/mcp_server.py` (given) already hosts `next_available_day` (and
`fare_rules`) as MCP tools — same name, description, input schema shape a
hand-written Anthropic tool would have. Step 2.2 is about pulling those
schemas from the server via `mcp_client.tools()` instead of hand-writing
them, and letting `mcp_client.call_remote()` execute them instead of
`call_local()`.

## Bug: the same tool registered twice
Running `verify.py 2.2` right after 2.1 failed one check: `next_available_day`
was registered in both `LOCAL_TOOLS` (from 2.1) and discoverable via the MCP
server. `run_agent()`'s dispatch order is
`mcp_client.tool_names -> LOCAL_TOOLS -> execute_tool`, so with both
registered, which one actually ran was a race depending on dispatch order,
not a deliberate choice.

**Fix:**
- Emptied `EXTRA_TOOLS` / `LOCAL_TOOLS` back out (the 2.1 local
  `next_available_day` entry is superseded, not needed once it's served over
  MCP).
- Changed `tool_list()` from `build_tools() + EXTRA_TOOLS` to
  `build_tools() + EXTRA_TOOLS + mcp_client.tools()`.

## Result
`python3 run.py K7PQ2M --trace` → `tools=11` sent (9 given + fare_rules +
next_available_day from MCP, EXTRA_TOOLS empty).
`python3 verify.py 2.2` → all checks passed, no duplicate registration,
MCP-discovered tool fired correctly on the team's probe.
Evidence code: **51A-502** (saved for "John Dragunas")

## Flagged, not fixed
Schema token cost climbed further: 2,582 tokens/turn (2.1, one local tool) ->
2,973 tokens/turn now (MCP server also exposes `fare_rules`, which came along
for free once `mcp_client.tools()` was wired in). Tool cost is driven by what
the server offers, not by the transport (local vs. MCP) — worth remembering,
not something to act on now.
