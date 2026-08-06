# .github

Organisation-level defaults for [**shinobi-dosho**](https://github.com/shinobi-dosho).

Nothing here is a project. GitHub reads this repository for two separate things:

- [`profile/README.md`](profile/README.md) — the landing page rendered at
  [github.com/shinobi-dosho](https://github.com/shinobi-dosho). It is *only* that
  page; it has no effect on any other repository.
- The files at the root — [`AGENTS.md`](AGENTS.md),
  [`CONTRIBUTING.md`](CONTRIBUTING.md), [`SECURITY.md`](SECURITY.md),
  [`CODE_OF_CONDUCT.md`](CODE_OF_CONDUCT.md) — which GitHub falls back to for any
  repository in the organisation that does not carry its own copy.

Every current project ships its own versions, so in practice the root files are
the baseline for new repositories and the canonical statement the per-repo copies
point at. Each one says explicitly that the repository's own file wins on
conflict, so nothing here can silently override a project's narrower rules.

## Starting a new project

Use [**starter-pack**](https://github.com/shinobi-dosho/starter-pack) rather than
copying files out of here. It carries these conventions already wired into a
working repository — packaging, CI, docs, release workflow and the pre-commit
hook — so a new project starts green instead of accumulating them one PR at a
time.

## The projects

| | |
| --- | --- |
| [stimela-ninja](https://github.com/shinobi-dosho/stimela-ninja) | Stimela 3.0 — the pipeline framework |
| [dosho](https://github.com/shinobi-dosho/dosho) | The native cab library |
| [msutils](https://github.com/shinobi-dosho/msutils) | Everyday Measurement Set operations |
| [fitstoolz](https://github.com/shinobi-dosho/fitstoolz) | FITS data with named axes |
| [simms](https://github.com/shinobi-dosho/simms) | Interferometer and sky-model simulation |
