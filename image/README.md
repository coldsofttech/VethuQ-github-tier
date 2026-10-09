# Worker image

Planned (#142): `Dockerfile` for a single public `linux/amd64` image containing the worker,
core, entitlements, all add-on wheels (gated at runtime by the licence) and baked-in models.

Published to GHCR as a public package (new GHCR packages default to private; the one-time
visibility change is documented with the image). Tags plus the digest are recorded in the
compatibility table in the root README. Supply chain (signing, SBOM, provenance, scanning) is
tracked in #144; model profiles in #261.
