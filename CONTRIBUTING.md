# Contributing to shinobi-dosho

Thanks for your interest in contributing. This is the organisation-wide
default: it applies to any repository here that does not carry its own
`CONTRIBUTING.md`, and where a repository does have one, **that file
wins**. Read it first — it will tell you what the project is for and what
is deliberately out of scope, which is most of what a good contribution
needs to know.

Everyone taking part is expected to follow the
[Code of Conduct](CODE_OF_CONDUCT.md).

## Scope and philosophy

Our core philosophy: **avoid unnecessary complexity like the plague**.

Every project here has a scope it stays inside. `msutils` does everyday
Measurement Set operations and does not calibrate or image;
`stimela-ninja` runs recipes and does not reinvent control flow; `dosho`
describes tools declaratively and executes nothing at load time. A
feature that would widen the scope is out of scope, however useful — the
right home for it is usually a different project, sometimes a new one.

Each repository's `AGENTS.md` carries the design rationale and the
conventions review comments are drawn from. Read it before changing
anything structural. The organisation-wide baseline is in
[`AGENTS.md`](AGENTS.md).

## Ways to contribute

- **Report bugs** via the repository's issue tracker. A report that says
  what you ran, what the input looked like, and what happened instead is
  worth more than a traceback alone — most bugs here are "the input was
  shaped in a way the code did not expect", and the shape is the part we
  cannot guess. Security issues go to `SECURITY.md`, never to a public
  issue.
- **Fix a bug**, with a regression test that fails without the fix.
- **Improve documentation** under `docs/`, or the docstrings that feed
  the API reference. Documentation that has gone stale against the code
  is a bug like any other.
- **Sizable change or addition** — open an issue to discuss it first.
  That is especially true for anything that adds a dependency or widens
  the scope above. Aligning before you write the code is much cheaper
  than realigning after.

## Development setup

The projects use [uv](https://docs.astral.sh/uv/):

```bash
git clone https://github.com/shinobi-dosho/<repo>.git
cd <repo>
uv sync --group dev
uv run pytest
uv run ruff check .

# enable the repo's pre-commit hook (once per clone)
git config core.hooksPath .githooks
```

Enabling the hook is the only setup step that is not uv's job — git will
not let a repository turn on an executable hook by itself, which is why
the `git config` is manual; skip it and you simply get no hook.
`.githooks/pre-commit` is a tracked shell script (no `pre-commit`
framework, no separate pinned tool universe) that runs `ruff check` and
`ruff format --check` through `uv run` when a commit touches Python, so
it agrees with CI's lint job by construction. A format failure is fixed
with `uv run ruff format <file> && git add <file>`; the hook never
rewrites files behind your back. `git commit --no-verify` bypasses it.

CI runs `uv sync --locked`, so a dependency change means committing the
updated `uv.lock`.

## Commits and pull requests

Write the commit message for the person who will read it in a year
without the surrounding conversation: what changed, what it deviates from
and why, and what a reviewer should not assume held still.

Keep a pull request to one coherent change. Its description is review
material — say what changed, why, and what to check. If you used a coding
assistant, the provenance belongs in a commit trailer and **not** in the
PR description; see
[*Attribution: commit trailers yes, PR trailers no*](AGENTS.md#attribution-commit-trailers-yes-pr-trailers-no).

Before opening a PR, run the tests and the linter, and check that the
docs still describe what the code now does.

## Licensing

Repositories here are MIT, GPL-2.0 or GPL-3.0 — check the `LICENSE` in
the one you are contributing to. By contributing you agree your
contribution is licensed under that repository's terms.
