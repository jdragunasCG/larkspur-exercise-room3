# PITCH.md

Six lines and a lever. Your words. The last two are scored.

Built: We built a disruption-care agent for Larkspur that resolves cancellations, delays and diversions utilizing APIs, MCPs and Tools.
Does: It checks flights, searches for next available seats, resolves entitlements and escalates as needed.
Number: Caching the system prompt and tool list cut model cost at Larkspur's volume from $771/week to $293/week (13,700 chats/week) — a 62% reduction — measured on Stage 1's 5 ticket types, 3 runs each.
Safety check: check_policy and search_alternatives re-derive fare_family, loyalty_tier, and overnight status from the booking every call — they don't take them as arguments, so the model can't be talked into a wrong entitlement.
Next: Fix the abusive message tone gap first, validate with automated tests, then add instrumentation to improve trace, audit and overall observability leveraging OpenTelemetry (or similar framework).
Still broken: R8KD3F (abusive message) resolves calm and helpful with no tone gate at all right now. "Rebooks" (from Does:) is unverified past the offer stage in every trace run so far.
Lever: cost

## Priya asked

Costs:
Wrong:
Runs it:
Left out:
