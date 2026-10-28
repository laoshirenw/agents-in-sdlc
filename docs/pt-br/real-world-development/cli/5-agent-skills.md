---
title: "Lição 5 - Personalizar e usar uma skill de verificações de qualidade"
description: "Explore a skill quality-checks existente, personalize o formato do relatório e use-a para validar a filtragem."
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

Escrever código envolve mais do que apenas programar. Já validamos manualmente o funcionamento do código e usamos arquivos de instruções para garantir que ele siga nossos padrões. E quanto aos testes, lint e outras partes da integração contínua (CI)?

Para essas tarefas, as **skills de agente** são ideais. Elas ajudam o Copilot a entender como executar operações desse tipo.

Nesta lição, você vai:

- explorar a skill `quality-checks` existente.
- personalizar o formato dos resultados.
- recarregar e executar a skill.

## Cenário

A Tailspin Toys usa a skill `quality-checks` para testes unitários, lint e verificações de tipos. A equipe quer aprimorar o relatório para facilitar a leitura dos resultados.

## Instruções, scripts e recursos

As skills de agente agrupam instruções reutilizáveis para tarefas, scripts executáveis e recursos de apoio que o agente carrega quando necessário. No mínimo, uma skill é uma pasta com um arquivo Markdown chamado `SKILL.md`. O Markdown contém frontmatter com nome e descrição, além de uma visão geral da função da skill e orientações sobre quando chamá-la. A pasta também pode conter subpastas com scripts e outros recursos.

> [!NOTE]
> Pastas e arquivos adicionais não são obrigatórios. A skill `quality-checks` do Tailspin Toys contém somente `SKILL.md`, pois usa os comandos existentes no projeto.

As skills podem ficar na pasta `.github/skills` do projeto para serem compartilhadas e reutilizadas pela equipe ou na pasta de skills do usuário, `~/.copilot/skills`.

## Explorar a skill

Vamos explorar a skill `quality-checks` que a equipe do Tailspin Toys criou para executar testes unitários, lint e verificações de tipos.

1. Retorne ao Codespace. No editor do Codespaces, abra `.github/skills/quality-checks/SKILL.md`.
2. Leia o `name` e a `description` no início. A descrição ajuda o Copilot a entender quando chamar a skill.
3. Leia as instruções e observe como orientam o processo de testes e lint.
4. Observe que a skill ainda não tem uma seção **Results output formatting**.

## Executar a skill antes da alteração

As skills podem ser chamadas diretamente no Copilot CLI ou por linguagem natural. Vamos pedir ao Copilot que execute as três verificações da skill.

1. Retorne à conversa de filtragem no modo Interactive.
2. Use o prompt:

   ```plaintext
   Run the quality-checks skill for unit tests, lint, and type checks.
   ```

3. Observe o relatório final.

## Personalizar o relatório

Vamos aprimorar o relatório para indicar o que foi executado, se houve sucesso e o que as ferramentas informaram.

1. Retorne a `.github/skills/quality-checks/SKILL.md`.
2. Adicione a seção a seguir ao final do arquivo:

   ```markdown
   ## Results output formatting

   Upon completion, report each command that ran and whether it passed, failed, or was blocked. Include test counts, durations, errors, warnings, and other metrics only when the tool reports them. Identify the next action for any failure or blocker, and never describe a skipped or incomplete check as passed.
   ```

3. O arquivo será salvo automaticamente.

## Executar a skill atualizada

O Copilot CLI pode recarregar skills editadas sem reiniciar a conversa.

1. Digite:

   ```plaintext
   /skills reload
   ```

2. Use o mesmo prompt:

   ```plaintext
   Run the quality-checks skill for unit tests, lint, and type checks.
   ```

3. Observe o relatório final e compare-o ao primeiro.

## Resumo e próximos passos

Você personalizou e usou uma skill de agente existente. A alteração será incluída no PR do recurso de filtragem. Em seguida, permita que o Copilot interaja diretamente com o site [por meio do servidor MCP do Playwright][next-lesson].

## Mais exemplos de skills

Estes exemplos da comunidade são referências, não tarefas adicionais:

- [Especificação de Agent Skills][skill-spec]
- [Fluxo de contribuição: `make-repo-contribution`][contribution-example]
- [Documentos de requisitos: `prd`][prd-example]
- [Diagramas e script de exportação incluído: `drawio`][drawio-example]
- [Testes no navegador: `webapp-testing`][browser-example]

[previous-lesson]: ../4-custom-instructions/
[next-lesson]: ../6-mcp-playwright/
[skill-spec]: https://agentskills.io/specification
[contribution-example]: https://github.com/github/awesome-copilot/tree/main/skills/make-repo-contribution
[prd-example]: https://github.com/github/awesome-copilot/tree/main/skills/prd
[drawio-example]: https://github.com/github/awesome-copilot/tree/main/skills/drawio
[browser-example]: https://github.com/github/awesome-copilot/tree/main/skills/webapp-testing
