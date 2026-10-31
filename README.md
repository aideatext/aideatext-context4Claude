# conText para Claude

Plugin de Claude de **conText**, el tutor socrático de [AIdeaText](https://context.aideatext.ai).

La persona comparte su material (apuntes, un PDF, un texto, un diagrama o la foto de un objeto
cotidiano) y Claude conversa con una pregunta a la vez, sin hacerle el trabajo. Mientras conversan,
arma un grafo de ideas que distingue tres orígenes:

| Color | Origen | Significa |
|---|---|---|
| Gris pizarra `#4e5a68` | `material` | Estaba en lo que compartiste |
| **Azul** `#0b7ec5` | `persona` | **Lo pensaste tú** |
| Rojo `#ae0025` | `tutor` | Lo introdujo el tutor |

Pide "mi grafo" para verlo, o "mis conexiones" para ver solo lo que pensaste tú.

## Para quién es

Por defecto, **personas adultas (18 años o más) de cualquier nivel educativo**: universitarios,
posgrado, docentes, investigadores, profesionales y autodidactas. El tutor adapta el lenguaje a un
menor de edad solo si la persona lo indica explícitamente.

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
