---
name: windows-store-msix
description: >-
  Plan and implement MSIX packaging and Microsoft Store submission for the
  existing Windows PyInstaller GUI build. Use when working on MSIX, AppxManifest,
  Partner Center, Windows Store, MakeAppx, or Store CI packaging.
---

# Windows Store / MSIX

## Goal

Ship a Store-ready MSIX (or msixbundle) built from the current Windows PyInstaller output, then submit via Partner Center. Keep `CIBMangoTree_windows.zip` for GitHub Releases.

## Repo facts

- Windows build: `.github/workflows/build_gui.yml` matrix entry `Windows` / `windows-2022`
- Artifact today: onedir under `dist/CIBMangoTree` → zipped as `CIBMangoTree_windows`
- No MSIX / Store upload automation exists yet
- macOS already has signing/notarization patterns for secrets — mirror the secrets approach, not the Apple tooling

## Phased workflow

### Phase 0 (no Partner Center login)

1. Research with **microsoft-learn** MCP (`microsoft_docs_search` / `microsoft_docs_fetch` / `microsoft_code_sample_search`)
2. Draft `AppxManifest.xml` placeholders (Name, Publisher, Version, capabilities)
3. Design CI step after PyInstaller: MakeAppx (or equivalent) → upload `.msix` artifact
4. On a Windows VM: sideload a test MSIX from local `dist/CIBMangoTree`, run `--noop`

### Phase 1 (login available)

1. Align manifest Publisher/Identity with Partner Center exactly
2. Obtain Store upload / signing material per Microsoft docs for the account type
3. Store secrets in GitHub Actions (never commit)

### Phase 2

1. Implement CI MSIX production
2. Manual Partner Center submit first; automate later if needed
3. Fix certification failures (capabilities, privacy URL, publisher mismatch, launch crashes)

## Done when

- CI produces versioned MSIX/msixbundle
- Package installs and launches on Windows
- Listing submitted / published (or remediation list owned)

## Do not

- Rewrite the app into a different UI stack for Store access
- Remove the Windows zip release artifact without an explicit ask
- Commit Partner Center credentials or `.pfx` files
