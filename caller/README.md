# Caller template

Planned (#136): `vethuq-ocr.yml`, the thin caller that runs in the user's runner repo.

Requirements: `workflow_dispatch` only; plain inputs limited to the job id and the asset
reference; minimal `permissions`; a concurrency group; the reusable workflow pinned by tag or SHA
on a single parseable line (the client rewrites it on `vethuq github update`); the only secret
used is the master key.
