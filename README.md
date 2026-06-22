# zeroclaw-docs-translations

Translated gettext (`.po`) catalogues for the [ZeroClaw](https://github.com/zeroclaw-labs/zeroclaw) documentation book (mdBook).

This repository is consumed as a git submodule by the main `zeroclaw-labs/zeroclaw` repo at `docs/book/po`. It holds one `.po` file per non-English locale:

- `es.po` — Spanish
- `fr.po` — French
- `ja.po` — Japanese
- `zh-CN.po` — Simplified Chinese

English is the gettext **source** language (extracted from `docs/book/src/**` in the main repo), so there is no `en.po`. The `messages.pot` template and `*.failures.log` files are regenerated build artifacts and are intentionally not tracked here.

## How it is used

The main repo builds the translated docs with mdBook + `mdbook-i18n-helpers`. The submodule mounts at `docs/book/po`, so `mdbook build` and `cargo mdbook sync` find these catalogues with no path overrides.

The common Rust dev loop (`cargo build`, `cargo test`, `cargo clippy`) does **not** depend on this submodule. It is only required for building the documentation book and for release/CI docs jobs.

## Cloning with translations

```sh
git clone --recurse-submodules https://github.com/zeroclaw-labs/zeroclaw
# or, in an existing clone:
git submodule update --init --recursive
```

## Versioning

This repo is tagged `v{version}` to mirror each `zeroclaw` release. The main repo pins its submodule gitlink to the matching tag at release time via `scripts/release/bump-version.sh`. Between releases the main repo may track `main`.

## Editing translations

Translations are regenerated from the main repo with:

```sh
cargo mdbook sync --model-provider <name>
```

which runs `msgmerge`/`msginit` against the freshly extracted `messages.pot` and AI-fills missing entries. The resulting `.po` changes are committed here, and the main repo's gitlink is advanced to include them.
