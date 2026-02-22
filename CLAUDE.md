# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What This Is

This is a Homebrew tap (`homebrew-tap`). Taps are third-party formula repositories consumed by the `brew` command. Users install it with:

```
brew tap arturo/tap
```

and then install formulas with `brew install jarfernandez/tap/<formula>` or the short form `brew install <formula>` (if there's no naming conflict with other taps).

## Repository Structure

- `Formula/` — Ruby formula files (`.rb`), one per installable package
- `Casks/` — Ruby cask files (`.rb`) for macOS GUI applications (if applicable)

Formula filenames must be lowercase and match the formula name (e.g., `Formula/mytool.rb`).

## Common Commands

### Working with formulas locally

```sh
# Install a formula from local source
brew install --build-from-source Formula/<name>.rb

# Run a formula's defined tests
brew test <name>

# Audit a new formula for style/correctness (use --new-formula for brand-new ones)
brew audit --new-formula Formula/<name>.rb
brew audit Formula/<name>.rb

# Lint a formula file
brew style Formula/<name>.rb

# Uninstall for a clean reinstall
brew uninstall <name>
```

### Updating formula versions/checksums

```sh
# Compute SHA256 for a new source tarball
curl -L <url> | shasum -a 256
```

## Formula Anatomy

Formulas are Ruby classes inheriting from `Formula`. Key DSL elements:

- `url` — source tarball URL
- `sha256` — checksum of the tarball
- `version` — explicit version (if not derivable from URL)
- `depends_on` — runtime or build dependencies
- `def install` — build and install steps using `bin.install`, `system`, etc.
- `def test` — a lightweight smoke test block run by `brew test`
- `bottle` — pre-built binary blocks (managed by Homebrew CI; omit when authoring new formulas)

## Homebrew Formula Conventions

- Use `stable do ... end` and `head do ... end` blocks when supporting both stable and HEAD installs.
- Prefer `bin.install` over manual `cp`; use `lib`, `share`, `etc`, `var` helpers appropriately.
- Dependencies declared as `:build` are only needed at build time; `:test` only at test time.
- Formula class names must be CamelCase matching the filename (e.g., `mytool.rb` → `class Mytool < Formula`).
- Run `brew audit` and `brew style` before committing to catch common issues.
