# Selfie Segmentation

**MediaPipe Selfie Segmentation (person matting)** — Per-pixel foreground/subject matte from a single frame.

An **azphalt** AI-model plugin, packaged as a `.azp` (the azphalt analogue of a VS Code `.vsix`). It is
named for the *model*, not a single feature — the same model powers many tools, and it is **host-neutral**:
any azphalt host that understands its role can use it, not just one app. Install it from any host's
**Azphalt Storefront**.

## What it can do

- background removal / replacement
- portrait bokeh
- green-screen-free compositing
- the mask that seeds object removal

## Roles (host-neutral routing)

This plugin contributes the role(s): `subject-segmentation`. A host routes the model by role — it carries no
`targetApps`, so it is not tied to any single application.

**Example host — [Guillotine](https://github.com/HereLiesAz/Guillotine):** Desktop `segModelPath` — background removal, bokeh, and the object-removal mask.

## Model file(s)

- **`selfie_segmentation.onnx`** (role `subject-segmentation`) — [upstream](https://huggingface.co/onnx-community/mediapipe_selfie_segmentation/resolve/main/selfie_segmentation.onnx)

Model license: **Apache-2.0 (MediaPipe Selfie Segmentation, Google)**. This plugin's manifest/packaging is `Apache-2.0`.

## How it works — the VSCode Header Pattern

The `.azp` does **not** bundle the weights. The manifest declares each model as a *remote asset*
(`"path": ""` + `remoteUrl` + `checksum` + `byteSize`); the host downloads the weights on install and
verifies them against the pinned SHA-256 — exactly how a large VS Code extension fetches its language
server instead of shipping it inside the `.vsix`. `remoteUrl` points at this repo's own GitHub **Release**
asset (named the exact filename the host expects); the `release` workflow fetches the upstream model,
renames it, checksums it, and publishes it beside the packed `.azp`.

## Build / release

```sh
npm install && npm run build     # packs com.hereliesaz.azphalt.selfie-segmentation-1.0.0.azp
git tag v1.0.0 && git push --tags   # runs the release workflow: hosts the model + .azp
```
