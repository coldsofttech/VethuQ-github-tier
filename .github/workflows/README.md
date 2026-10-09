# Workflows

Planned (not yet built):

- `ocr.yml` (#137): reusable workflow, `workflow_call` only. Pulls the pinned image by digest,
  downloads the encrypted payload, runs the worker, uploads the encrypted result (also on
  failure or timeout) with 1-day retention. Logs carry no file names or OCR text.
- `build-image.yml` (#143): builds the worker image from PyPI wheels on dispatch or when a
  dependent release is published, runs the smoke test, publishes to GHCR. Guarded with
  `if: github.repository == 'coldsofttech/vethuq-github-tier'` so it is inert in template copies.

Only workflows belong in this folder (GitHub resolves reusable workflows from here).
