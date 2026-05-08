# agent-roles

Shared source of truth for agent persona, governance, and role files.

## Files

- `PERSONA.md`
- `GOVERNANCE.md`
- `TEAMLEAD.md`
- `IMPLEMENTOR.md`
- `VALIDATOR.md`
- `DESIGNER.md`

## Consumer Setup

In a consumer repo:

```bash
git remote add agent-roles <repo-url>
git fetch agent-roles
git subtree add --prefix=.agent/shared agent-roles main --squash
```

Before each session:

```bash
git fetch agent-roles
git subtree pull --prefix=.agent/shared agent-roles main --squash
```
