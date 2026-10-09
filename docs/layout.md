# Layout and conventions

Agreed layout for `vethuq-github-tier`. Changes to it go through a pull request that updates this
file.

```
.github/workflows/
  ocr.yml              reusable workflow (workflow_call only)
  build-image.yml      image build + smoke test + publish
caller/
  vethuq-ocr.yml       canonical thin caller (source of truth for template and client)
image/
  Dockerfile           single linux/amd64 worker image
protocol/v1/
  manifest.schema.json
  result.schema.json
  examples/            documented examples
  fixtures/            synthetic payloads shared by client and worker tests
docs/
  layout.md            this file
```

## Rules

1. **Reusable workflows live in `.github/workflows/`.** GitHub only resolves `workflow_call`
   workflows from there. Nothing else goes in that folder except this repo's own build workflow.
2. **The caller is thin.** `caller/vethuq-ocr.yml` has `workflow_dispatch` as its only trigger,
   two plain inputs (job id, asset reference), minimal `permissions`, a concurrency group, and the
   reusable workflow pinned by tag or SHA. The client writes it into the user's repo and rewrites
   the pin on update, so the pin must stay on one clearly parseable line.
3. **Pins.** Third-party actions are avoided, or pinned to full commit SHAs. The image is
   referenced by digest.
4. **One protocol directory per major.** `protocol/v1/` is additive-only; breaking changes go to
   `protocol/v2/` and `v1` stays available. Client and worker refuse unknown major versions.
5. **Fixtures are synthetic and shared.** Client and worker tests consume the same files from
   `protocol/v1/fixtures/`.
6. **No secrets, no real user content** anywhere in the repo.

## Template repository behaviour

"Use this template" copies the default branch's files into the user's new repo. Consequences:

- Anything in this repo that could run in the user's repo must be inert there. Workflows other than
  the caller are guarded with `if: github.repository == 'coldsofttech/vethuq-github-tier'`, and
  `ocr.yml` is `workflow_call`-only, so it cannot be dispatched.
- The client's `setup` writes the pinned caller workflow itself and may prune files the
  runner repo does not need. A user's repo is therefore not expected to be identical to this one.
- If this proves awkward (for example the copied files confuse users), the fallback is to move the
  template into its own small repository and keep this one for the reusable workflow, image and
  protocol.
