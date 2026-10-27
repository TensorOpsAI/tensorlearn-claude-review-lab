# Approved artifacts
INTENT-1: Notify only after a doctor has reviewed AND released the result.
SPEC-PRIVACY: Do not log clinical payloads; log event IDs and delivery state only.
SPEC-CACHE: Cache each customer status for 30 seconds to respect the 50 requests/second upstream limit.
PLAN-1: Change notification presentation only. Preserve current release gate, cache and log redaction. Verify boundary conditions.
