# Recommended skills and plugins

This is the optional toolbox we reach for most often. It is a taste list, not a
requirement to load every skill on every task.

Skills are focused operating instructions. Plugins can bundle several skills, tools,
apps, and workflows behind one product-level capability. Names and distribution can
change, so inspect the current catalog and read the selected skill's own instructions
before acting.

## How to use this list

- Select the smallest set that covers the task.
- Invoke a skill when its domain is actually in scope, not merely because the project
  uses that technology somewhere.
- Let a plugin's router select its focused workflow instead of guessing which internal
  skill to call.
- Prefer current provider documentation over remembered APIs or copied setup.
- Inspect permissions and dependencies before connecting a plugin to an external app.
- If a recommendation is unavailable, discover its current equivalent rather than
  silently pretending it was used.

## At a glance

| Recommendation | Kind | Reach for it when |
| --- | --- | --- |
| Cloudflare | Standalone skill | Building or operating on the Cloudflare platform |
| React Best Practices | Standalone skill | Writing or reviewing React and Next.js for performance |
| EdgeCMS | Standalone skill | Managing EdgeCMS content, instances, integrations, or releases |
| Frontend Design | Plugin-provided skill | Giving a UI a deliberate, distinctive visual direction |
| Product Design | Plugin | Researching, auditing, exploring, cloning, or prototyping a product experience |
| ast-grep outline | Standalone skill | Mapping unfamiliar source structure before reading full files |
| ast-grep | Standalone skill | Searching for structural code patterns that text search cannot express |

## Platform and content

### Cloudflare

Installed name: `cloudflare`.

Use it for any Cloudflare development task: Workers, Pages, bindings, storage,
Durable Objects, Workflows, networking, security, AI, or infrastructure as code. Its
most important habit is retrieval-first work. Cloudflare APIs, limits, compatibility
flags, and configuration evolve quickly, so the current Cloudflare docs, installed
types, and Wrangler schema outrank memory.

Do not load it for a generic web task merely because the application happens to be
deployed on Cloudflare. Load it when the requested work touches Cloudflare behavior,
configuration, deployment, or architecture.

### EdgeCMS

Installed name: `edgecms`.

Use it for EdgeCMS instances, upgrades, translations, blocks, schemas, media, typed
locale snapshots, D1 migrations, CI, service bindings, custom admin routes, and SDK or
CLI integration.

Start by reading the project's `edgecms.config.json`, package scripts, documentation,
tests, and installed SDK version. Treat EdgeCMS as the content authority. Respect the
line between local synchronization and external mutation: pushing, publishing,
changing languages, applying schemas, or deleting keys can affect shared or live
state and needs the corresponding authorization.

## React and interface work

### React Best Practices

Installed name: `vercel-react-best-practices`.

Use it while writing, reviewing, or refactoring React and Next.js. It is primarily a
performance skill: eliminating async waterfalls, controlling bundle size, improving
server and client data flow, limiting rerenders, and choosing efficient rendering
patterns.

It is not a visual-design system. Pair it with Frontend Design when the task needs
both a strong interface and sound React implementation.

### Frontend Design

Installed name: `frontend-design:frontend-design` when supplied by the Frontend
Design plugin.

Use it when building a new UI or materially reshaping an existing one and the result
needs a specific visual identity. It pushes the work toward subject-grounded choices
in typography, palette, layout, motion, copy, and one defensible signature element,
with responsive and accessible execution.

This is the design skill for implementation. It should make the interface feel
intentional, but it does not replace product research, UX auditing, or visual concept
selection.

### Product Design

Kind: plugin with routed, focused skills.

Use Product Design when it is explicitly requested or when the main job is product
research, flow auditing, visual exploration, faithful source cloning, image-to-code,
prototype creation, design verification, or sharing a prototype.

Product Design is not the default for ordinary frontend implementation. For a routine
screen or component, use Frontend Design and React Best Practices. For a new product
direction or redesign without a selected visual target, let Product Design run the
exploration and selection workflow before implementation begins.

## Codebase exploration

### ast-grep outline

Installed name: `ast-grep-outline`.

Use it to get a cheap structural map of unfamiliar source: imports, exports,
top-level declarations, and direct members with line numbers. It is the bridge between
finding candidate files and reading their implementations.

A good exploration sequence is:

1. Use filenames or `rg` to find likely areas.
2. Outline candidate files or directories.
3. Open only the relevant ranges identified by the outline.
4. Escalate to structural search when the question requires it.

Outline is syntax-only. It does not resolve references, infer types, follow calls, or
replace compiler-backed analysis.

### ast-grep

Installed name: `ast-grep`.

Use it when the search depends on code structure rather than text: calls with a
particular argument shape, functions containing or missing a construct, patterns
inside a class or handler, or candidates for a structural refactor.

Do not reach for it when `rg` can answer the question clearly. For a complex search,
start with a minimal representative code example, prove the rule against that example,
then run it across the repository. Begin with a simple pattern and add AST kinds,
relationships, or composite rules only as needed.

## Useful combinations

### Everyday React UI

Use Frontend Design for visual direction and React Best Practices for implementation
quality. Add Product Design only when the task genuinely needs research, critique,
exploration, or a visual target chosen before coding.

### Product exploration followed by implementation

Use Product Design first to research, audit, ideate, or select a source. Once a target
exists, use Frontend Design and React Best Practices to implement it faithfully and
well.

### Cloudflare application with EdgeCMS

Use Cloudflare for runtime, bindings, storage, and deployment concerns. Use EdgeCMS
for content authority, synchronization, schemas, migrations, and releases. Add the
React and design skills only when interface work is part of the request.

### Unfamiliar codebase or structural refactor

Use ast-grep outline to map the relevant surface, then ast-grep only if the search or
rewrite depends on syntax structure. Add the domain skill after the responsible files
and boundaries are understood.

## Pointing an agent at this list

This document does not install, enable, or grant permissions to anything. It gives an
agent a shortlist to check against the skills and plugins currently available in its
environment.

For example:

```text
For this task, consult /path/to/aku-aku/recommended-skills-and-plugins.md.
Use only the recommendations that match the work and follow their current instructions.
```
