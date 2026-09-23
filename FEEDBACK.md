# Overnight review: Larkspur disruption-care agent

**To:** jdragunasCG_larkspur-exercise-room3  
**From:** Larkspur client review agent, on behalf of Priya Raghavan  
**Re:** the disruption-care agent you walked us through in our last session  
**Generated:** 2026-09-22 17:23

## Priya's note

> Our vendor says we should just be using your best model.
>
> Why aren't we?
>
> Priya Raghavan, Larkspur Airlines

She sent that before this session opened. She means it. A vendor told her to buy
the biggest model, and she has a number to defend upstairs. Her four questions from
day one are still open. Naming a model answers none of them.

## Still open from day one

| Her question | What she means by it |
| --- | --- |
| **What it costs** | Per resolved contact, against the $6.90 a human contact costs us. |
| **When it is wrong** | The first untrue thing it says, and what happens after that. |
| **Who runs it** | In June, after you have left. |
| **What you left out** | The scope you cut, and why. |

## What the review agent found

Overnight, Larkspur pointed a review agent at your repository. It read the
code. It did not run your agent, and the only file it changed is this one. Each
item below names the file and the line it is about.

**1. agent.py is byte-identical to the shipped template, so no build work has landed in the repo yet.**

The static scan confirms zero pencil-mark edits: TONE_ADDENDUM is still 0 characters, EXTRA_TOOLS is an empty list, and LOCAL_TOOLS has no executors. All nine tool schemas in build_tools() are the workshop's originals, not this team's.

Run git diff against the template tag and paste the output to confirm what, if anything, has changed since the scan.

**2. search_alternatives carries a description of exactly 6 characters: the string "search".**

Every other tool schema in build_tools() runs from 71 to 445 characters and spells out required fields, defaults, and when to call it. This one tells Claude nothing about what alternatives means, what inputs matter beyond pnr, or when to prefer it over check_policy. A larger model reading "search" still has to guess the same way a smaller one does, since the gap is in the schema text, not in model capability.

Run python3 run.py --show-tools and paste the printed search_alternatives entry to confirm the description in production matches this 6-character string.

**3. No readout-trace.json exists, so no run of this agent has ever been committed.**

The material states there is no committed wire run and no evals/cases.json in the repository. That means there is no evidence yet of how many turns a disruption case takes, whether the loop hits MAX_TOOL_CALLS at 8, or whether check_policy's policy_row_id citation requirement is actually honored in a real exchange.

Run python3 run.py K7PQ2M --trace and commit the resulting readout-trace.json.

**4. MAX_TOOL_CALLS is set to 8 with no measurement of how many turns a real disruption case needs.**

The comment on this line describes it as where a human takes over, but nothing in this repository shows a case that reaches turn 8, or one that finishes in 3. Whether 8 is generous or tight for a rebooking-plus-voucher flow is unmeasured, and that number would not move if a bigger model were dropped in behind the same schemas.

Run python3 run.py --all --trace and paste the per-case turn counts from the totals footer.

**5. PITCH.md is unchanged from the template, so no case for a model choice has been written down anywhere.**

Priya is asking why the team isn't using the vendor's suggested best model, and the only place that argument could live in this repository, PITCH.md, has nothing in it. There is also no evals/cases.json, so no eval result exists to compare one model's behavior against another's on these nine tools.

Run python3 bench.py --compare <a> <b> once two model runs exist and paste the comparison output into PITCH.md.

## Your four answers

The four lines under `## Priya asked` in your PITCH.md are still empty. They
are one line each and they are not a coding job: cost, what happens when it is
wrong, who runs it in June, and what you left out. Whoever on your side is not
editing agent.py is the right person to write them, and they are the four
things I will ask about first.

## Before our next meeting

> Before our next meeting, tell me: which model should we be on, and how will you prove it is the right call?
>
> Priya Raghavan, Larkspur Airlines

Bring two things. A recommendation, and the measurement behind it. If the model is
not the problem, say so, and bring the number that shows it.

## What this review read

- `agent.py (226 lines)`
- `PITCH.md (unchanged template)`
- `TEAM.md (unchanged template)`

Reviewer: `claude-sonnet-5`. Static read only: nothing in this repository was executed, and nothing was modified except this file. Larkspur Airlines is a fictional training scenario. Confidential, do not distribute.
