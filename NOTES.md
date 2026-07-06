# Fix Notes

## 1. Explain one fix: The double-booking bug (Bug 1)

**What was wrong:**
The `dates_overlap()` function only checked if the new booking's start date fell within an existing booking's range:
```python
return start_b <= start_a <= end_b
```

This misses many overlaps. For example, if an existing booking is Jan 10–15, and you try to book Jan 5–12, the function wouldn't detect the overlap because start_a (5) is not between start_b (10) and end_b (15).

**How the fix addresses it:**
The corrected function properly checks if *any* part of the two ranges overlap:
```python
return start_a <= end_b and end_a >= start_b
```

This ensures no double-bookings are possible by testing both directions: does range A end after B starts, AND does A start before B ends?

## 2. Show the failure: Concrete example the original code wrongly allowed

**Original code allowed:** Booking Canon DSLR from Jan 5 to Jan 12 when Jan 10–15 was already booked (overlaps on Jan 10–12 but wasn't blocked).

**Fixed code now blocks this:** The proper overlap check rejects this booking correctly.

## 3. AI use

I used Claude (Copilot) to help brainstorm the overlap logic and verify the fix. I confirmed the output was correct by:
- Manually tracing through test cases (existing Jan 10–15, test booking Jan 5–12)
- Checking that the logic `start_a <= end_b and end_a >= start_b` covers all overlap scenarios
- Testing the app locally with the Canon DSLR booking to ensure overlaps are now blocked
- Verifying that non-overlapping bookings (e.g., Jan 5–9 or Jan 16–20) still work
