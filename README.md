# conText for Claude

Claude plugin for **conText**, the Socratic tutor by [AIdeaText](https://context.aideatext.ai).

You share your own material (notes, a PDF, a text, a diagram, or a photo of an everyday object)
and Claude talks it through with you one question at a time, without doing the work for you. As the
conversation goes on, it builds a graph of ideas that tells apart three origins:

| Color | Origin | Meaning |
|---|---|---|
| Slate gray `#4e5a68` | `material` | It was in what you shared |
| **Blue** `#0b7ec5` | `persona` | **You thought of it** |
| Red `#ae0025` | `tutor` | The tutor brought it in |

Ask for "my graph" to see it, or "my connections" to see only what you came up with.

## Who it's for

By default, **adults (18 or older) at any educational level**: undergraduate and graduate students,
teachers, researchers, professionals, and self-learners. The tutor adapts its language for a minor
only when the person says so explicitly.

## Contents

- [`.claude-plugin/plugin.json`](.claude-plugin/plugin.json): plugin manifest.
- [`skills/tutor-socratico/`](skills/tutor-socratico/SKILL.md): the conText method as a skill.
- [`.claude-plugin/icon.png`](.claude-plugin/icon.png): plugin icon.

The conText web app lives in its own repository and is deployed at https://context.aideatext.ai.

## Install in Claude Code

```
/plugin marketplace add aideatext/aideatext-context4Claude
/plugin install context-tutor@aideatext
```

## License

Apache 2.0. See [LICENSE](LICENSE).
