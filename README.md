## Niccolò · `@NickAme03`

I build the layer between an AI agent and the things it is allowed to do.

### Two systems, one problem seen from both ends

**[claude-second-brain](https://github.com/NickAme03/claude-second-brain)**
A connected skill ecosystem for Claude Code. Most skill repos are catalogs:
isolated prompts you have to remember to invoke. This one is a system. Every
skill declares what comes after it, honesty is enforced by hard prohibitions
rather than by tone, and the graph that links them is maintained by one of its
own nodes. MIT, [live site](https://nickame03.github.io/claude-second-brain/).

**[PlutoGate](https://github.com/NickAme03/Pact)**
Permissions as contract clauses, not machine settings. An orchestrator that
turns a goal into verifiable task contracts and submits every agent action to
approval against that contract: default-deny, with an audit trail. An action the
contract does not foresee writes nothing. Today the repository is the design and
a full statement of what does not work yet; extracting the running gate is the
open milestone.

The two connect. The first writes the contract, the second enforces it.

### How I work in public

I read the code before I have an opinion about it. Most of what I contribute
starts as a trace through someone else's read path, becomes a question the
maintainer is the only person able to answer, and only then becomes a patch.

Recent work has been on instrumentation that reports confidently while measuring
the wrong thing: telemetry whose detectors cannot fire, and usage accounting that
drifts because it recomputes from files that get rewritten underneath it.

### Elsewhere

[path-of-gods](https://github.com/NickAme03/path-of-gods), a 2D action-platformer
in LÖVE2D/Lua. Different problem, same habit of building the system before the
content.
