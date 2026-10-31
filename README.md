# conText para Claude

Plugin de Claude de **conText**, el tutor socrático de [AIdeaText](https://context.aideatext.ai)
para estudiantes de secundaria.

El estudiante comparte su material (una foto del cuaderno, un PDF, un texto o la foto de un objeto
cotidiano) y Claude conversa con una pregunta a la vez, sin hacerle la tarea. Mientras conversan,
arma un grafo de ideas que distingue tres orígenes:

| Color | Origen | Significa |
|---|---|---|
| Gris pizarra `#4e5a68` | `material` | Estaba en lo que subiste |
| **Azul** `#0b7ec5` | `estudiante` | **Lo pensaste tú** |
| Rojo `#ae0025` | `tutor` | Lo introdujo el tutor |

Pide "mi grafo" para verlo, o "mis conexiones" para ver solo lo que pensaste tú.

## Contenido

- [`.claude-plugin/plugin.json`](.claude-plugin/plugin.json): manifiesto del plugin.
- [`skills/tutor-socratico/`](skills/tutor-socratico/SKILL.md): el método de conText como skill.
- [`.claude-plugin/icon.png`](.claude-plugin/icon.png): icono del plugin.

La app web conText vive en su propio repositorio y está desplegada en https://context.aideatext.ai.

## Instalar en Claude Code

```
/plugin marketplace add aideatext/aideatext-context4Claude
/plugin install context-tutor@aideatext
```

## Licencia

Apache 2.0. Ver [LICENSE](LICENSE).
