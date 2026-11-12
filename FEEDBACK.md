# Client review: Larkspur disruption-care agent

**To:** Room 12  
**From:** Larkspur client review agent, on behalf of Priya Raghavan  
**Re:** the disruption-care agent you walked us through in our last session  
**Generated:** 2026-11-12 09:44

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

Larkspur pointed a review agent at your repository. It read the
code. It did not run your agent, and the only file it changed is this one. Each
item below names the file and the line it is about.

**1. agent.py fare(): the function opens a hard-coded /Users/amalantony/Desktop path and ignores its topic argument.**

fare(topic) calls Path("/Users/amalantony/Desktop/larkspur-exercise/data/americas/fare_rules_excerpt.md").read_text() and returns the whole file for every topic. The path exists on one laptop. On any other machine, call_local will return an error dict to the model. The topic enum-style examples in the fare_rules schema ('hotel', 'meal', 'goodwill') change nothing about what comes back.

Run python3 run.py K7PQ2M --trace from a fresh clone on a different machine and paste the fare_rules tool_result line.

**2. agent.py search_alternatives: the description is 217 characters and asks for 'PNR,flight_status, and date', but the schema takes only pnr.**

The description says "search for alternative flights for a given PNR,flight_status, and date". The input_schema lists properties {"pnr": {"type": "string"}} and nothing else. The description never says when to call the tool or that it follows get_flight_status. Claude is told to supply two inputs it has no field for.

Run python3 run.py --show-tools and paste the search_alternatives entry next to a --trace of a cancelled-flight booking showing the call it produced.

**3. readout-trace.json: the committed run used 3 tool calls and never reached search_alternatives, hold_seat, or any care tool.**

The trace shows 4 API turns, tools called in order lookup_booking, get_flight_status, check_policy, 12776 tokens in, 1186 out, 10.5s. Prompt caching shows 0 read, 0 written. The run stops after the policy lookup, and fare_rules is not among the calls.

Run python3 run.py --all --trace and paste the totals footer so the booking shapes beyond this one are on record.

**4. agent.py fare_rules schema: the description routes by 'hotels, fare families... goodwill policies' while check_policy already returns hotel and goodwill.**

The fare_rules description (374 characters) covers hotels, meals, delays, cancellations, diversions, and goodwill. check_policy's description (445 characters) already says it resolves 'meal/hotel/ground care, goodwill eligibility and cap'. Nothing in the diff tells Claude which tool wins on an overlapping question, and the fare_rules string runs its sentences together with no spaces between them.

Run python3 run.py --tool-tax and paste the output showing what the 10-tool list costs per turn, then a --trace of a hotel question showing which of the two tools Claude called.

**5. Nothing in the repo yet shows the loop, TONE_ADDENDUM, or the extra tool changing an outcome, so a model name has nothing to be compared against.**

TONE_ADDENDUM is still 0 characters, there is no evals/cases.json, and readout.html banks only gates 1.2, 1.3, 1.4. The 12776 input tokens in one run include a 10-schema tool list and a system prompt sent unchanged every turn, and cache_control appears nowhere in agent.py. Those costs and the empty tone section belong to the build, and a larger model would carry both unchanged.

Run python3 bench.py --label before --stage 1 --runs 3, make one change, run it again as --label after, and paste the python3 bench.py --compare before after output.

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

- `agent.py (256 lines)`
- `PITCH.md`
- `TEAM.md`
- `readout-trace.json`
- `readout.html (evidence block)`
- given files that differ from the shipped pack: `verify.py`, `eval_harness.py`, `bench.py`, `readout.py`, `pod_sync.py`, `run.py`, `support/__init__.py`, `support/client.py`, `support/data.py`, `support/tools.py`, `support/trace.py`, `support/offline.py`, `support/mock_backend.py`

Reviewer: `claude-sonnet-5-5`. Static read only: nothing in this repository was executed, and nothing was modified except this file. Larkspur Airlines is a fictional training scenario. Confidential, do not distribute.
