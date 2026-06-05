# Conservative Parallel Dispatch

This note tracks a conservative implementation approach for live dispatch.

Rules:

- Keep sequential behavior available with a setting of 1.
- Cap concurrent in-flight requests at 2.
- Track pending request IDs instead of relying on a single last request ID.
- Process completed results one at a time to keep UI, stats, scripts, and strategy state deterministic.
- Treat unexpected or unknown result IDs as a safety stop.
- Release stale slots after a timeout so the client can recover from missing responses.
- Do not add adaptive probing, hidden threshold discovery, or block evasion logic.

Suggested environment variable:

```text
DICEBOT_MAX_PARALLEL_BETS=2
```

Set it to `1` to restore strictly sequential dispatch.
