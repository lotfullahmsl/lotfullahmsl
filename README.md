<!--
GitHub Profile — Lotfullah Muslimwal (@lotfullahmsl)
Design goals: dark • professional • animated • signal‑driven • readable at a glance
This is a special profile README (repo name = username).
-->

<!-- Header: animated wave banner -->
<p align="center">
	<img src="https://capsule-render.vercel.app/api?type=waving&height=140&color=0f172a&text=Lotfullah%20Muslimwal&fontColor=ffffff&fontAlign=40&fontAlignY=34" alt="Header wave" />
</p>

<!-- Animated type intro -->
<p align="left">
	<img src="https://readme-typing-svg.demolab.com?font=Fira+Code&size=22&pause=1200&color=22D3EE&width=720&lines=Systems+mindset+%E2%80%94+Clarity+%E2%86%92+Performance;Small+units+compose+into+reliable+systems;Measure+learning+rate+and+leverage" alt="Typing intro" />
</p>

<!-- Quick badges (dark accents) -->
<p align="left">
	<a href="https://github.com/lotfullahmsl?tab=followers" title="Followers">
		<img src="https://img.shields.io/github/followers/lotfullahmsl?style=flat&color=0f172a" alt="GitHub followers" />
	</a>
	<a href="https://github.com/lotfullahmsl" title="Stars">
		<img src="https://img.shields.io/github/stars/lotfullahmsl?affiliations=OWNER%2CCOLLABORATOR&style=flat&color=0f172a" alt="GitHub stars" />
	</a>
	<img src="https://komarev.com/ghpvc/?username=lotfullahmsl&label=views&color=404040&style=flat" alt="Profile views" />
</p>

<img src="https://capsule-render.vercel.app/api?type=rect&height=2&color=0f172a&section=header" alt="divider" />

## About

- Systems‑minded software engineer with a focus on fundamentals and clarity.
- I build small, composable units that scale into reliable systems.
- I prefer explainable code, explicit invariants, and boring reliability over novelty.
- I optimize for learning rate, leverage, and user outcomes.

## Operating Principles

- Signal over noise — measure outcomes, not aesthetics.
- Simplicity scales — remove accidental complexity before adding capability.
- Correctness first — invariants explicit, tests close to logic.
- Mechanical sympathy — choose data and boundaries that fit the problem.
- Write to explain — make intent obvious in code and docs.
- Document decisions — context beats folklore; ADRs when decisions matter.
- Prefer composition over inheritance — small parts, stable interfaces.
- Fail fast at boundaries — validate, sanitize, observe.
- Bias to automation — tests, CI, and reproducible environments.
- Maintain momentum — consistent, incremental improvement beats sporadic heroics.

## Now / Next

- Deepening CS foundations (systems, algorithms, distributed trade‑offs)
- Building durable tools that compose into larger capabilities
- Practicing deliberate refactoring and incremental design
- Capturing reasoning in lightweight docs and ADRs

---

## Expertise

- Languages: Python, TypeScript/JavaScript
- Backend: Node.js, FastAPI/Flask, REST/gRPC fundamentals
- Frontend: React, minimal state, typed APIs
- Systems: Docker, Linux, shell, basic k8s ergonomics
- Data: modeling, migrations, indexing, cache strategies
- Testing: property‑based where useful, fast unit tests near logic
- Observability: logs/metrics/traces with simple SLOs

---

## Signals (Dark)

<!-- Activity stats (dark) -->
<p align="left">
	<img
		src="https://github-readme-stats.vercel.app/api?username=lotfullahmsl&show_icons=true&hide_title=true&hide_border=true&rank_icon=github&include_all_commits=true&theme=github_dark&cache_seconds=7200"
		alt="Activity signal (dark)"
	/>
</p>

<!-- Languages (dark) -->
<p align="left">
	<img
		src="https://github-readme-stats.vercel.app/api/top-langs/?username=lotfullahmsl&layout=compact&langs_count=8&hide_border=true&theme=github_dark&cache_seconds=7200"
		alt="Language mix (dark)"
	/>
</p>

<!-- Streak (dark) -->
<p align="left">
	<img
		src="https://streak-stats.demolab.com?user=lotfullahmsl&theme=github-dark-blue&hide_border=true"
		alt="Consistency streak (dark)"
	/>
</p>

<!-- Contribution graph (dark, collapsible) -->
<details>
	<summary>Contribution Graph (dark)</summary>
	<p>
		<img
			src="https://github-readme-activity-graph.vercel.app/graph?username=lotfullahmsl&theme=tokyo-night&hide_border=true&area=true"
			alt="Contribution graph (dark)"
		/>
	</p>
</details>

<!-- Locally generated contribution snake (dark/light) -->
<p>
	<picture>
		<source media="(prefers-color-scheme: dark)" srcset="./output/github-contribution-grid-snake-dark.svg" />
		<source media="(prefers-color-scheme: light)" srcset="./output/github-contribution-grid-snake.svg" />
		<img alt="Contribution snake" src="./output/github-contribution-grid-snake-dark.svg" />
	</picture>
</p>

<sub>Signals describe progress; they don’t define it. If any external card fails to load, local images (snake, summary cards) keep the page reliable.</sub>

---

## Selected Notes (Architecture)

<details open>
	<summary>Boundaries and data flow</summary>

	A pragmatic structure that keeps complexity where it belongs:

```text
app/
	domain/            # entities, invariants, domain services
	usecases/          # orchestration & policies
	adapters/          # db, http, queues (ports/adapters)
	entrypoints/       # cli, http handlers, workers
	support/           # config, logging, observability
```

Key rules:
- Domain is pure and framework‑agnostic.
- Use cases orchestrate; they don’t know transport or storage details.
- Adapters translate between external systems and domain/use cases.
- Entrypoints are thin; they glue transport to use cases.

</details>

<details>
	<summary>Data modeling and evolution</summary>

Start with reads and invariants, then design writes.

```sql
-- Users and sessions (minimal, indexed)
create table users (
	id bigserial primary key,
	email text unique not null,
	created_at timestamptz not null default now()
);

create table sessions (
	id bigserial primary key,
	user_id bigint not null references users(id),
	expires_at timestamptz not null,
	created_at timestamptz not null default now()
);

create index on sessions (user_id);
create index on sessions (expires_at);
```

Principles:
- Model for the questions you ask most frequently.
- Prefer explicit foreign keys; add indexes from first read profile.
- Use events (or change tables) for downstream async projections.

</details>

<details>
	<summary>Performance budgets</summary>

Think in budgets to prevent accidental slow paths.

```text
API p99 latency: 250 ms budget
	- auth (token check):         5–10 ms
	- validation:                 2–5 ms
	- primary query (indexed):   20–60 ms
	- compute/transform:         5–20 ms
	- serialization/network:    10–30 ms
Observability sampling: 5–10% traces on hot paths until stable.
```

Actions:
- Add tracing to boundary layers first.
- Measure, then refactor tight loops and allocations.
- Cache only with explicit invalidation signals.

</details>

<details>
	<summary>Fault tolerance patterns</summary>

Pragmatic patterns that keep systems calm under stress.

```text
• Circuit breakers at integration points
• Timeouts per call path; never rely on defaults
• Bounded queues and backpressure, avoid unbounded fan‑out
• Idempotency keys for retried writes
• Dead‑letter queues with clear retention policies
• Graceful degradation: partial features over total failure
```

</details>

---

## Toolbox

<p>
	<img alt="Python" title="Python" height="28" src="https://raw.githubusercontent.com/devicons/devicon/master/icons/python/python-original.svg" />
	<img alt="JavaScript" title="JavaScript" height="28" src="https://raw.githubusercontent.com/devicons/devicon/master/icons/javascript/javascript-original.svg" />
	<img alt="TypeScript" title="TypeScript" height="28" src="https://raw.githubusercontent.com/devicons/devicon/master/icons/typescript/typescript-original.svg" />
	<img alt="Node.js" title="Node.js" height="28" src="https://raw.githubusercontent.com/devicons/devicon/master/icons/nodejs/nodejs-original.svg" />
	<img alt="React" title="React" height="28" src="https://raw.githubusercontent.com/devicons/devicon/master/icons/react/react-original.svg" />
	<img alt="Docker" title="Docker" height="28" src="https://raw.githubusercontent.com/devicons/devicon/master/icons/docker/docker-original.svg" />
	<img alt="Linux" title="Linux" height="28" src="https://raw.githubusercontent.com/devicons/devicon/master/icons/linux/linux-original.svg" />
	<img alt="Git" title="Git" height="28" src="https://raw.githubusercontent.com/devicons/devicon/master/icons/git/git-original.svg" />
</p>

<sub>I pick tools for the problem, not the other way around.</sub>

---

## Summary Cards (Local, dark)

<details open>
	<summary>Profile details</summary>
	<p>
		<img src="./profile-summary-card-output/github_dark/0-profile-details.svg" alt="Profile details card" />
	</p>
</details>

<details>
	<summary>Repos • Commits • Languages</summary>
	<p>
		<img src="./profile-summary-card-output/github_dark/1-repos-per-language.svg" alt="Repos per language" />
		<img src="./profile-summary-card-output/github_dark/2-most-commit-language.svg" alt="Most commit language" />
		<img src="./profile-summary-card-output/github_dark/3-stats.svg" alt="Stats" />
		<img src="./profile-summary-card-output/github_dark/4-productive-time.svg" alt="Productive time" />
	</p>
</details>

---

## Reading & Writing

Recent interests: systems thinking, DDD lite, operational excellence, strategy.

Selected reads:
- Designing Data‑Intensive Applications — Kleppmann
- The Pragmatic Programmer — Hunt & Thomas
- A Philosophy of Software Design — Ousterhout
- Site Reliability Engineering — Beyer et al.

Notes and posts live off‑platform; I keep GitHub technical.

---

## Links

- Website: <a href="https://lotfullahmsl.com/" target="_blank">lotfullahmsl.com</a>
- LinkedIn: <a href="https://www.linkedin.com/in/lotfullahmsl/" target="_blank">/in/lotfullahmsl</a>

---

## Colophon

- Typing: <a href="https://github.com/DenverCoder1/readme-typing-svg" target="_blank">readme‑typing‑svg</a>
- Stats: <a href="https://github.com/anuraghazra/github-readme-stats" target="_blank">github‑readme‑stats</a>
- Streak: <a href="https://github.com/DenverCoder1/github-readme-streak-stats" target="_blank">streak‑stats</a>
- Activity graph: <a href="https://github.com/Ashutosh00710/github-readme-activity-graph" target="_blank">activity‑graph</a>
- Snake: <a href="https://github.com/Platane/snk" target="_blank">snk</a>
- Summary cards: <a href="https://github.com/vn7n24fzkq/github-profile-summary-cards" target="_blank">profile‑summary‑cards</a>

<p align="center">
	<img src="https://capsule-render.vercel.app/api?type=waving&height=120&color=0f172a&section=footer" alt="Footer wave" />
</p>

