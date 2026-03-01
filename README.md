# Claude Code Skills

A collection of custom [Claude Code skills](https://code.claude.com/docs/en/skills) for extending Claude's capabilities.

## What are skills?

Skills extend what Claude can do inside Claude Code. Each skill is a `SKILL.md` file with YAML frontmatter and markdown instructions. Once installed, you can invoke a skill directly with `/skill-name`, or Claude can load it automatically when relevant to your conversation.

Skills follow the [Agent Skills](https://agentskills.io) open standard and are compatible with any tool that supports it.

## Available skills

| Skill | Description |
|---|---|
| [`native-web`](skills/native-web/SKILL.md) | Guides building UI using native HTML, CSS, and browser APIs instead of component libraries. Activates when working on frontend UI, styling components, or reaching for ShadCN/Radix/MUI. |

## Installation

Skills can be installed at two levels:

- **Personal** (`~/.claude/skills/`) — available across all your projects
- **Project** (`.claude/skills/`) — available only in that project

### Install a single skill

Copy the skill directory to your desired location. For example, to install `native-web` as a personal skill:

```bash
mkdir -p ~/.claude/skills
cp -r skills/native-web ~/.claude/skills/
```

### Install all skills

```bash
mkdir -p ~/.claude/skills
cp -r skills/* ~/.claude/skills/
```

### Install for a specific project

```bash
mkdir -p /path/to/your/project/.claude/skills
cp -r skills/native-web /path/to/your/project/.claude/skills/
```

### Verify installation

In Claude Code, ask:

```
What skills are available?
```

Or invoke a skill directly by typing `/` followed by the skill name, e.g. `/native-web`.

## Usage

Once installed, skills work in two ways:

- **Automatic** — Claude loads the skill when your request matches its description
- **Manual** — invoke it directly with `/skill-name` (e.g. `/native-web`)

## Structure

Each skill is a directory with a required `SKILL.md` entrypoint:

```
skills/
└── skill-name/
    └── SKILL.md        # Instructions and frontmatter (required)
```

Supporting files (templates, examples, scripts) can be added to the directory and referenced from `SKILL.md`.

## Further reading

- [Skills documentation](https://code.claude.com/docs/en/skills)
- [Agent Skills open standard](https://agentskills.io)
