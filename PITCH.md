# PITCH.md

Six lines and a lever. Your words. The last two are scored.

Built: We built a disruption-care agent for Larkspur that resolves cancellations, delays and diversions utilizing APIs, MCPs and Tools.
Does: It checks flights, searches for next available seats, resolves entitlements and escalates as needed.
Number: K7PQ2M resolves in 4 API turns, 3 tool calls, after the loop fixes (was 5 turns and broken before).
Safety check: check_policy and search_alternatives re-derive fare_family, loyalty_tier, and overnight status from the booking every call — they don't take them as arguments, so the model can't be talked into a wrong entitlement.
Next: Fix the abusive message tone gap first, validate with automated tests, then add instrumentation to improve trace, audit and overall observability leveraging OpenTelemetry (or similar framework).
Still broken: R8KD3F (abusive message) resolves calm and helpful with no tone gate at all right now. "Rebooks" (from Does:) is unverified past the offer stage in every trace run so far.
Lever: intelligence

## Priya asked

Costs:
Wrong:
Runs it:
Left out:
