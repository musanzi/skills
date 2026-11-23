# Skills

These are the agent skills I use with Claude Code to take work from idea to merged pull request. Each folder holds one skill: a `SKILL.md` file that Claude loads when the skill runs, plus any reference files it points to.

The stack these skills assume is NestJS on the backend and Angular on the frontend, with issues tracked on GitHub.

## Install

1. Clone the repo into `~/.agents/skills`:

	```sh
	git clone https://github.com/musanzi/skills ~/.agents/skills
	```

2. Link each skill into `~/.claude/skills`, where Claude Code looks for personal skills:

	```sh
	mkdir -p ~/.claude/skills
	for dir in ~/.agents/skills/*/; do
		name=$(basename "$dir")
		[ -e ~/.claude/skills/"$name" ] || ln -s "$dir" ~/.claude/skills/"$name"
	done
	```

3. Restart Claude Code. The skills appear without a prefix, such as `/grilling` and `/workflow`.

4. In each project, run `/setup-skills` once. It records the issue tracker, the triage labels, and the location of the domain docs that the other skills read.

To update the skills on another machine, run `git pull` in `~/.agents/skills`. The links pick up the change.

## Start with `/workflow`

`/workflow` is the map. It says which skill to run at each step and when to clear or compact the context between steps. Read [`workflow/SKILL.md`](workflow/SKILL.md) for the full flow.

The main path for a feature:

1. `/grill-with-docs` interviews you until the idea is sharp, and records terms in `GLOSSARY.md` and decisions as ADRs.
2. `/prototype` answers a design question with throwaway code, if conversation alone can't settle it.
3. `/to-spec` turns the conversation into a spec on the issue tracker.
4. `/to-tickets` splits the spec into tickets, each listing the tickets that block it.
5. `/implement` builds one ticket, or `/implement-spec` builds every ticket with parallel subagents. Both build test-first through `/tdd`.
6. `/spec-review` checks the diff against the coding standards and against the spec.
7. `/pr` writes the pull request body.
8. `/retro` looks back on the session and suggests changes to the setup, not the code.

For a small change, skip steps 3 and 4 and run `/implement` in the same session.

## Coding rules

`/spec-review` checks these rules before any other standard, and treats a breach as a hard violation:

- No comments in code. Only tool directives such as `// eslint-disable-next-line` and `// @ts-expect-error` are allowed.
- One CQRS class per file. Each command, query, event, and handler gets its own file named after the class, such as `create-stand.command.ts` and `create-stand.handler.ts`.

## Skill list

A skill marked **manual** runs only when you type its name. Claude starts the others on its own when the task fits their description.

### Plan

| Skill | Runs | What it does |
| --- | --- | --- |
| `workflow` | manual | Says which skill fits the situation and lists the stack skills to load. |
| `grilling` | auto | The interview loop that the other grilling skills build on: questions in rounds, each with a recommended answer. |
| `grill-me` | manual | Interviews you about a plan or design, and saves nothing. |
| `grill-with-docs` | manual | Interviews you about a plan in a repo, and updates `GLOSSARY.md` and the ADRs as it goes. |
| `wayfinder` | manual | Plans work too large for one session as a map of decision tickets on the tracker. |
| `to-questionnaire` | manual | Writes a questionnaire for someone else when they hold the answer you need. |
| `research` | auto | Reads primary sources in the background and saves cited findings as a Markdown file. |
| `prototype` | auto | Builds throwaway code to answer one design question. |

### Specify

| Skill | Runs | What it does |
| --- | --- | --- |
| `to-spec` | manual | Turns the current conversation into a spec on the issue tracker. |
| `to-tickets` | manual | Splits a spec into tickets, each listing the tickets that block it. |
| `triage` | manual | Moves incoming issues and pull requests through triage labels and writes briefs an agent can act on. |
| `setup-skills` | manual | Configures a repo's issue tracker, triage labels, and domain docs for the other skills. |

### Build

| Skill | Runs | What it does |
| --- | --- | --- |
| `implement` | manual | Builds one ticket or spec test-first, then runs `/spec-review`. |
| `implement-spec` | manual | Builds every ticket in a spec with parallel subagents on one integration branch. |
| `tdd` | auto | Builds one behavior at a time: a failing test, then the code that passes it. |
| `diagnosing-bugs` | auto | Reproduces a hard bug with one failing command before it fixes anything. |
| `wizard` | auto | Writes an interactive bash script for steps only a person can do, such as creating credentials. |

### Stack

| Skill | Runs | What it does |
| --- | --- | --- |
| `nestjs` | auto | Covers NestJS project setup, layout, and modules. |
| `nestjs-feature` | auto | Adds or changes HTTP endpoints, validation, and error responses. |
| `nestjs-data` | auto | Adds persistence with Drizzle or TypeORM, including migrations and transactions. |
| `nestjs-auth` | auto | Adds sign-in, sessions, tokens, API keys, and permissions. |
| `nestjs-production` | auto | Adds config, logging, health checks, rate limits, OpenAPI, and graceful shutdown. |
| `nestjs-testing` | auto | Writes unit, service, and end-to-end tests with `@nestjs/testing`. |
| `angular-developer` | auto | Writes Angular code and answers architecture questions. |
| `impeccable` | auto | Designs, critiques, and polishes frontend interfaces. |

### Review and ship

| Skill | Runs | What it does |
| --- | --- | --- |
| `spec-review` | auto | Reviews a diff against the coding standards and against the spec, in two parallel subagents. |
| `pr` | auto | Writes a pull request body. |
| `retro` | manual | Reviews a session and suggests changes to checks, standards, and tooling. |

### Codebase health

| Skill | Runs | What it does |
| --- | --- | --- |
| `architect-codebase` | manual | Finds modules worth making deeper and reports them as an HTML page. |
| `codebase-design` | auto | Defines the words used to design modules: interface, depth, seam, and adapter. |
| `domain-modeling` | auto | Keeps `GLOSSARY.md` and the ADRs accurate. |

### Writing

| Skill | Runs | What it does |
| --- | --- | --- |
| `writing-for-agents` | auto | Covers how to write skills, `AGENTS.md`, and `CLAUDE.md`. |
| `technical-writing` | manual | Applies a technical-writing standard to docs, pull requests, and commits. |
| `unslop` | manual | Removes AI writing patterns from prose. |

### Tooling

| Skill | Runs | What it does |
| --- | --- | --- |
| `git-guardrails-claude-code` | auto | Adds Claude Code hooks that block destructive git commands. |
| `setup-pre-commit` | auto | Adds Husky pre-commit hooks for formatting, type checks, and tests. |
| `find-skills` | auto | Finds and installs skills from the open skills ecosystem. |

## Sources and license

Most of the planning, build, and review skills started as a copy of [mattpocock/skills](https://github.com/mattpocock/skills) and have since been renamed and changed. `angular-developer` comes from [angular/skills](https://github.com/angular/skills), `impeccable` from [pbakaus/impeccable](https://github.com/pbakaus/impeccable), and `find-skills` from [vercel-labs/skills](https://github.com/vercel-labs/skills).

The repo is under the MIT license. See [`LICENSE`](LICENSE).
