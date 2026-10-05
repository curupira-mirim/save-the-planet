# save-the-planet

Coleção portátil de skills e instruções globais usadas pelo Codex.

## Incluído

- `landing-discovery`: SEO técnico, AEO/GEO, descoberta e indexabilidade para landing pages.
- `github-repo-bootstrap`: conexão segura de um projeto local a um repositório GitHub novo e primeiro push.
- `commit-and-push`: commits focados, sem segredos ou alterações fora do escopo atual.
- `AGENTS.md`: roteador global que seleciona essas skills quando aplicáveis.

Cada skill inclui seu `SKILL.md` e `agents/openai.yaml`.

## Instalação local

Clone este repositório e copie ou crie links simbólicos das pastas em `skills/` para `~/.codex/skills/`. Antes de substituir uma skill existente, revise o diff.

O arquivo `AGENTS.md` é um exemplo de roteador global. Mescle suas regras com `~/.codex/AGENTS.md` existente em vez de sobrescrevê-lo sem revisão.

## Uso

Use uma skill explicitamente com `$nome-da-skill` ou deixe o Codex selecioná-la conforme a descrição e as regras do `AGENTS.md`.

Não inclua tokens, chaves, arquivos .env ou credenciais neste repositório.
