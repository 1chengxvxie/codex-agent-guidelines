# Codex Collaboration Guidelines

[简体中文](README.md) | [English](README.en.md)

A general-purpose `AGENTS.md` template for developers from beginner to advanced levels. It helps AI coding assistants verify important assumptions, explain engineering tradeoffs, keep changes focused, and complete tasks against observable acceptance criteria.

It supports individual development, team collaboration, and projects of different sizes. The guidelines cover high cohesion and low coupling, the smallest sufficient implementation, formal project documentation, trustworthy download sources, and artifact cleanup with explicit boundaries. Communication language, explanation depth, and tool choices adapt to the actual user and environment.

This repository combines organizational approaches and behavioral principles from several public repositories with practical collaboration preferences. It is an adaptable template, not an official standard or a substitute for code review, tests, or requirements acceptance.

## What It Helps With

- **Requirements and judgment:** Separate goals, facts, and assumptions. Verify key claims and explain the evidence and impact of problems so developers can understand the choice of solution.
- **First principles and adversarial review:** Start from goals, facts, constraints, and acceptance criteria. Seek counterexamples, failure paths, and hidden dependencies, including in the assistant's own proposals.
- **Engineering design:** Maintain high cohesion and low coupling, establish useful boundaries, and reduce unrequested features, configuration, and abstractions.
- **Scope of changes:** Trace each change to the task. Remove orphaned content caused by the current changes while preserving unrelated existing work.
- **Completion criteria:** Define observable acceptance criteria, specify checks for complex tasks, continue through delivery, and provide useful next steps.
- **Environment adaptation:** Adapt to the user's language and experience and the project's toolchain. Use trustworthy download sources and verify integrity.
- **Team documentation and cleanup:** Write documentation that stands on its own, keep examples independent of personal environments, and automatically remove only current-task temporary artifacts with confirmed provenance and purpose.

These guidelines aim to reduce the cost of mistakes in nontrivial tasks. Handle simple changes directly. Use reasonable, verifiable assumptions for low-risk implementation details; every change does not require a question or an extra design document.

## Repository Contents

| File | Purpose |
| --- | --- |
| [AGENTS.md](AGENTS.md) | Authoritative guidelines |
| [AGENTS.en.md](AGENTS.en.md) | English translation aligned with the authoritative guidelines |
| [README.md](README.md) | Chinese introduction, usage, and sources |
| [README.en.md](README.en.md) | English introduction, usage, and sources |
| [CLAUDE.md](CLAUDE.md) | Claude Code import entry point without a duplicate rules body |

This is an instruction template with no runtime dependencies. Installing a skill or plugin is unnecessary.

Both language versions use the same behavioral rules without restricting the communication language, developer experience, operating system, or download region. Codex does not automatically discover `AGENTS.en.md` by default: merge or copy its contents into `AGENTS.md` at the target location and install only one language version. If the translation is ambiguous, the authoritative guidelines take precedence.

## Usage

### Global Codex Instructions

Read [AGENTS.en.md](AGENTS.en.md) and adapt it to user preferences, project conventions, and the actual development environment.

Codex looks for global instructions in `~/.codex` by default, or in the directory specified by `CODEX_HOME`. If global instructions already exist, back them up and merge the rules rather than overwriting them.

The following PowerShell example installs the reviewed English version as `AGENTS.md`:

```powershell
$codexConfigDir = if ($env:CODEX_HOME) {
    $env:CODEX_HOME
} else {
    Join-Path $env:USERPROFILE '.codex'
}

$agentsPath = Join-Path $codexConfigDir 'AGENTS.md'
New-Item -ItemType Directory -Path $codexConfigDir -Force | Out-Null

if (Test-Path -LiteralPath $agentsPath) {
    throw 'AGENTS.md already exists; back it up and merge manually.'
}

Copy-Item -LiteralPath './AGENTS.en.md' -Destination $agentsPath
```

On macOS or Linux, use the same global location and copy or merge files with the actual shell. This template does not require PowerShell.

### Project Instructions

Merge the relevant rules into `AGENTS.md` at the project root. Document the actual architecture, run commands, acceptance criteria, and operational boundaries there. Subdirectories can provide more specific instructions.

Avoid duplicating the entire guidelines in both global and project files. Keep shared preferences global and project-specific differences in project files.

### Loading and Precedence

Codex builds its instruction chain at the start of a run. Global rules load first, followed by project and deeper-directory rules that supplement or override them. Each directory prefers `AGENTS.override.md`, otherwise `AGENTS.md`. Higher-priority system, platform, and safety constraints still apply.

After editing, verify the active instructions in a new run or session rather than assuming every client reloads an existing session automatically. Ask Codex to summarize the instructions it actually loaded, and check for `AGENTS.override.md` or a custom `CODEX_HOME`.

See the [official AGENTS.md documentation](https://learn.chatgpt.com/docs/agent-configuration/agents-md) for detailed behavior.

### Claude Code

The repository's `CLAUDE.md` imports the rules using `@AGENTS.md`. Keep both files together and verify that the client supports this import mechanism. To use the English version, first install its contents as the adjacent `AGENTS.md`. Check other tools' filenames and loading mechanisms separately.

## Ideas and Sources

| Source | Referenced ideas | Adaptation in this repository |
| --- | --- | --- |
| [multica-ai/andrej-karpathy-skills](https://github.com/multica-ai/andrej-karpathy-skills) | Think before coding, simplicity first, surgical changes, and goal-driven execution | Merge into judgment and implementation rules; handle ambiguity according to impact rather than enforcing absolute workflows |
| [richling98/karpathy-codex-skills](https://github.com/richling98/karpathy-codex-skills) | Codex packaging, applicability, and installation guidance | Separate agent rules from reader documentation and preserve existing configuration during installation |
| [zzusec/andrej-karpathy-skills-codex](https://github.com/zzusec/andrej-karpathy-skills-codex) | Publishing Codex-specific instruction files | Use the standard `AGENTS.md` filename and explain how to verify loading |
| [OpenAI: AGENTS.md](https://learn.chatgpt.com/docs/agent-configuration/agents-md) | Global and project instruction discovery and precedence | Check the usage instructions against the documentation |
| [OpenAI: Rethinking skills and prompts](https://developers.openai.com/blog/rethinking-skills-and-prompts-for-gpt-6-astra) | Reduce persistent context and clarify triggers and completion boundaries | Merge repeated concepts and scale workflows to the task |

The four coding principles come from community presentations of Karpathy-inspired guidance. The reference repositories do not imply endorsement by Andrej Karpathy or OpenAI. Cohesion and coupling, environment adaptation, formal documentation, and cleanup boundaries come from the maintainer's collaboration needs. The README uses independent wording rather than reproducing extended passages from other repositories.

## Customization and Contributions

Use concrete failure cases to improve rules: describe the behavior you want to change, why the current wording is insufficient, and how the proposed rule affects simple tasks. Prefer merging repeated requirements over adding unconditional approval, testing, or planning procedures.

Evaluate usefulness through fewer unrelated edits, clearer tradeoffs, completed acceptance checks, and clarification before decisions that affect results. There are currently no controlled cross-model or cross-project experiments; this repository does not claim universal improvements or the elimination of all instruction conflicts.

Treat `AGENTS.md` as the authoritative source. Update `AGENTS.en.md` after rule changes without introducing independent behavioral differences in the translation. Keep both READMEs aligned when installation instructions, audience, or sources change.

## Provenance and Permissions

This repository is publicly shared on GitHub and currently has no specified open-source license. Public visibility and permission for unrestricted redistribution are different matters. If a license is adopted, the repository's LICENSE will define its terms. Reference projects retain their own licenses.

The documentation was prepared with AI assistance under the maintainer's direction and is maintained by the maintainer.
