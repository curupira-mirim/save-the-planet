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

```bash
git clone git@github.com:curupira-mirim/save-the-planet.git
cd save-the-planet

for skill in skills/*; do
  name="$(basename "$skill")"
  ln -s "$PWD/$skill" "$HOME/.codex/skills/$name"
done
```

Se uma skill com o mesmo nome já existir, revise e substitua-a conscientemente em vez de sobrescrever o diretório.

O arquivo `AGENTS.md` é um exemplo de roteador global. Mescle suas regras com `~/.codex/AGENTS.md` existente em vez de sobrescrevê-lo sem revisão.

## Uso

Use uma skill explicitamente com `$nome-da-skill` ou deixe o Codex selecioná-la conforme a descrição e as regras do `AGENTS.md`.

### Atalhos de terminal

Os atalhos abaixo pressupõem que `codex-skill` e os aliases estejam configurados no shell:

- `cskills`: lista as skills locais.
- `cseo "..." `: inicia o Codex com `landing-discovery`.
- `cgitinit "git@github.com:ORGANIZACAO/REPOSITORIO.git"`: prepara e publica o primeiro push de um projeto local. Execute dentro do diretório do projeto.
- `cpush`: abre o fluxo de commit/push. Ele lê a tarefa e o diff, escolhe uma mensagem específica, valida o escopo e faz o push normal. Não faz force push nem inclui arquivos fora do contexto.
- `cagents`: mostra sessões/agentes disponíveis no daemon do Codex.

Para tornar os aliases disponíveis no terminal atual após configurá-los:

```bash
source ~/.bashrc
```

### Uso sem atalhos

```text
$landing-discovery
$github-repo-bootstrap
$commit-and-push
```

Não inclua tokens, chaves, arquivos .env ou credenciais neste repositório.
