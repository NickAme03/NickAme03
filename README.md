## Niccolò · `@NickAme03`

I look for the places where tools for AI coding agents report confidently and measure the wrong thing: tokens counted twice, a model priced at zero, a test suite that has never run on Windows. Then I measure it on real data and take it to the maintainer.

### Other people's repositories

Tools that read agent transcripts mostly run their tests on Linux. The paths
inside those transcripts are the one thing that changes shape on Windows, so I
went looking there. Fork, add a workflow with an ubuntu and windows matrix, run
the suite, compare. The ubuntu leg is the control: without it a Windows failure
tells you nothing, because you cannot separate a broken platform from a broken
suite.

[junhoyeo/tokscale](https://github.com/junhoyeo/tokscale) is the one worth
citing. 947 tests passed and 12 failed on Windows, against 2461 passed and none
failed on Ubuntu, with the suite stopping at the first binary and never reaching
the end. Three of the twelve were in the program itself, not in its tests.

All three shipped fixed in v4.9.0. The maintainer then put the windows leg in
himself and ran it with `--no-fail-fast`, which reached the nine binaries that
had never executed on Windows at all: 34 failures, not 12, and 33 of them
predated the report, spanning six root-cause groups. Two of those groups looked
like Windows product defects and turned out not to be: the fixtures built their
JSON by interpolating a filesystem path, which parses on Linux and not on
Windows, and the parse error was swallowed into an empty result. I verified the
product side separately against a real Windows install.

The suite is green there now, and the windows leg is a hard gate: a Windows
failure blocks a merge, in a project that had never executed a single test on
Windows before the report.

That same false claim was still sitting in the project's own CI workflow, where
it is what a future reader consults before touching the leg. I opened a PR
against it; it conflicted with a change that landed hours later, so the
maintainer rebased my correction onto his own tree and merged it as
[20e2932](https://github.com/junhoyeo/tokscale/commit/20e29322a4c3bca2f155e1a8d1ab35c581309019),
co-authored.
[The run](https://github.com/NickAme03/tokscale/actions/runs/30696683117),
[the report](https://github.com/junhoyeo/tokscale/issues/997),
[what it uncovered](https://github.com/junhoyeo/tokscale/issues/1014).

Before that, [five merged PRs on
splitrail](https://github.com/Piebald-AI/splitrail/pulls?q=is%3Apr+author%3ANickAme03+is%3Amerged).
Streaming snapshots were summed instead of deduplicated, so subagent transcripts
came out 50 to 60% heavier in input and cache tokens, dollar cost included
([#220](https://github.com/Piebald-AI/splitrail/issues/220)). Claude Opus 5 was
missing from the price table, so all of its usage cost $0
([#221](https://github.com/Piebald-AI/splitrail/issues/221)). And the checks
workflow never ran `cargo test` while the release workflow shipped a signed
Windows binary; it now runs on Linux and Windows
([#231](https://github.com/Piebald-AI/splitrail/issues/231)).

[claude-night-market #656](https://github.com/athola/claude-night-market/pull/656),
merged: a report generator where a swallowed error produced the same empty
output as a clean pass.

[Claude-Code-Usage-Monitor
#238](https://github.com/Maciek-roboblog/Claude-Code-Usage-Monitor/issues/238),
where six of the nine failures were tests asserting on a POSIX API that the
program had already worked around.

[phuryn/claude-usage](https://github.com/phuryn/claude-usage) came back clean,
eight green jobs out of eight. That one is in the record too, with the same
detail as the others.

### Evaluations

[eval-diff](https://github.com/NickAme03/eval-diff) asks whether a code-review
finding describes a defect the diff introduced or one that was already there.
The gold comes from running a check on base and head, never from labelling by
hand, and the cases nobody can decide are dropped. A positional baseline (0.40)
was written down before any model ran: a result counts only if it beats it.

### Client work

[autoscuolaspagnoletto.it](https://www.autoscuolaspagnoletto.it/), a driving
school in Milan. I audited the old WordPress site against six local
competitors, then replaced it with a static site of nine pages, live 15 days
after the audit. I am now writing its Python enrolment service. The code is
private.

### Elsewhere

[path-of-gods](https://github.com/NickAme03/path-of-gods), a 2D action-platformer
in LÖVE2D/Lua, graded 29/30 as a university exam. Different problem, same habit
of building the system before the content.

Earlier I built two things around agent permissions.
[claude-second-brain](https://github.com/NickAme03/claude-second-brain) is a set
of Claude Code skills where every skill declares what comes after it.
[PlutoGate](https://github.com/NickAme03/PlutoGate) treats permissions as
contract clauses; today the repository is the design and a full statement of
what does not work yet.
