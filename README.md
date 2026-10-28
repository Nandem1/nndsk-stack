# nndsk-stack

Skills reutilizables para agentes de código. Cada carpeta en `skills/` se instala por separado.

## Skills

| Skill | Uso |
| --- | --- |
| [living-architecture](skills/living-architecture/SKILL.md) | Documentación de arquitectura viva con Markdown y Mermaid. |

## Instalar

Requiere Node.js 22.20+.

```bash
npx skills add Nandem1/nndsk-stack --skill living-architecture -g -a cursor -a claude-code -a codex -a opencode -y
```

Quita `-g` para instalar en un solo proyecto. Para explorar las disponibles:

```bash
npx skills add Nandem1/nndsk-stack --list
```

## Usar

`Usa living-architecture en modo BOOTSTRAP para documentar este repositorio.`

Modos: `BOOTSTRAP` (crear mapa), `MAINTAIN` (mantenerlo), `REVIEW` (revisar drift), `AUDIT` (detectar drift).

Para exigir mantenimiento habitual durante la implementación, añade el [contrato](skills/living-architecture/references/repo-contract.md) al `AGENTS.md` del proyecto.

Código y migraciones son la fuente operacional. La documentación es el mapa mental.
