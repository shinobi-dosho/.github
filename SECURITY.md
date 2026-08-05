# Security Policy

Organisation-wide default for the shinobi-dosho projects. Repositories
that carry their own `SECURITY.md` — with the threat model specific to
what they do — take precedence over this one.

## Supported versions

Security fixes are applied to the latest release of a project only; there
are no long-term-support branches. Several projects here are pre-1.0, and
a pre-1.0 release stream gets fixes on its newest `0.x` only.

## Reporting a vulnerability

**Please do not report security issues in public GitHub issues.**

Report vulnerabilities privately by email to **sphemakh@gmail.com**, or
via GitHub's [private vulnerability reporting][ghsa] on the affected
repository where it is enabled. Include enough detail to reproduce —
affected version, the operation you ran, the inputs involved, and the
impact you observed.

We aim to acknowledge reports within a reasonable time, work with you on
a fix, and credit you in the release notes if you'd like.

[ghsa]: https://docs.github.com/en/code-security/security-advisories/guidance-on-reporting-and-writing-information-about-vulnerabilities/privately-reporting-a-security-vulnerability

## Security posture

These projects run on local data and, in the pipeline case, on whatever
containers and clusters you point them at. The guarantees that hold
across the organisation:

- **No `eval`/`exec` of content the tools read.** A cab definition, a
  Measurement Set, a FITS header and a JSON summary are all *data*.
  Cab YAML in particular can come from anywhere, so it is never
  `eval`/`exec`-ed and its code-carrying flavours are refused rather than
  run.
- **No shell.** Where a command is executed, it goes out as a list-form
  `subprocess` argv — never `shell=True`, never string interpolation into
  a shell. Compiled cluster-offload scripts embed exec-form argv only.
- **Untrusted input stays untrusted.** Reading someone else's MS, FITS
  file or cab library should not be able to execute their code. That is a
  property we intend to keep; a way around it is a security issue.
- **Writes are explicit.** Operations that would overwrite existing data
  require an explicit `overwrite`/`force`, rather than clobbering by
  default.

Where a project deliberately executes something you give it — TaQL in
`msutils` is the clear case — its own `SECURITY.md` says so and explains
the handling. Treat those strings the way you would treat SQL: do not
build one by concatenating untrusted input.

If you find a way around any of these guarantees, it's a security issue —
please report it as above.
