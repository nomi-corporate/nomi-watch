# Integração e entrega — nomi-watch

## Escopo e rastreabilidade

Esta entrega estrutura a parte de CI da [TEC-007 — CI/CD](https://github.com/orgs/nomi-corporate/projects/1/views/1?pane=issue&itemId=292993590). Ela não conclui o CD nem a TEC inteira.

Branch principal: `main`. Cada alteração entra por branch e PR associado à HU/TEC, com responsável, parte entregue, dependências e evidências. Uma HU que atravessa repositórios só termina após validar a entrega completa.

Os mantenedores designados são `gabrielwilliandev` e `Lucas200507`. Autorizações excepcionais e proteções são configuradas no GitHub; este documento não as altera. Conferir as regras efetivas antes de integrar. Não falsificar checks nem desabilitar proteções para fazer o pipeline passar.

## Disparos e segurança

- PRs destinados a `main`, pushes em `main` e execução manual em Actions. A execução manual passa a aparecer quando o workflow estiver na branch principal.
- Não há filtro por caminho: uma alteração documental também publica os checks obrigatórios, evitando estados Expected indefinidos.
- Novos commits cancelam execuções anteriores do mesmo PR. Execuções já iniciadas da principal não são canceladas automaticamente; a fila de concurrency pode substituir execuções ainda pendentes.
- Token com `contents: read`; checkout sem persistir credenciais. Nenhum job recebe credenciais AWS ou publica em registry, loja ou hospedagem.
- Actions fixadas por SHA. Para atualizar, conferir a origem e o commit da versão desejada, atualizar os pins e executar os checks.
- O job `Workflow lint` valida todos os YAMLs com actionlint 1.7.12, cujo binário é conferido por SHA-256.
- Não usar `pull_request_target` para executar código de PR. Runners são hospedados no GitHub; não executar contribuições externas em máquina pessoal com segredos.

## Pipeline atual

Workflow `Wear OS CI`:
- `Android build and lint`: JDK 17, Android SDK 37.0, Build Tools 36.0.0 e Gradle Wrapper versionado; executa assembleDebug, lintDebug e testDebugUnitTest.
- `Workflow lint`: valida os workflows.

O job `Android build and lint` mantém seu nome. Configurar sua obrigatoriedade e a de `Workflow lint` separadamente no GitHub; a presença do workflow não cria proteção de branch.

A tarefa testDebugUnitTest pode indicar NO-SOURCE: ainda não há testes locais. Isso não equivale a suíte aprovada. Testes instrumentados e emulador não fazem parte desta execução.

O cache Gradle é somente leitura em PRs e pode ser atualizado por execuções confiáveis da principal. Não inserir segredos ou chaves de assinatura no cache.

## Pacotes gerados

- APK debug e build.json em `nomi-watch-debug-<SHA>-<tentativa>`, retenção de 7 dias.
- Relatórios de lint e de testes disponíveis, inclusive após falha, em artefato separado de relatórios por 7 dias.

O APK é para testes, com assinatura debug. Ele não é release, não é publicado em loja e não prova funcionamento no relógio. Celular e relógio precisam de pacote e assinatura compatíveis para Data Layer; não presumir que chaves debug independentes dos runners sejam compatíveis.

## Próxima etapa de entrega Wear OS

Validar instalação e interação em emulador/dispositivo, adicionar testes de comportamento, definir assinatura/distribuição e conferir compatibilidade com o celular. Guardar chaves fora do Git e revisar quem pode distribuir. Não implementar publicação de loja ou integração Apple Watch implicitamente.

## Identidade e artefatos

Os nomes incluem o SHA testado e a tentativa da execução. Em PR, `github.sha` identifica o merge de teste do GitHub; não confundir esse valor com o último commit da branch de origem. `build.json` registra repositório, SHA, execução e tentativa.

Artefatos servem para inspeção e reaproveitamento técnico. Um artefato de PR não é uma release confiável para implantação. Para promover, usar uma execução bem-sucedida de push da branch principal, conferir origem, SHA e ambiente. A retenção curta não é armazenamento permanente de releases.

A indisponibilidade do upload de um artefato obrigatório falha o job, mesmo que o build tenha passado. Se atingir a cota de artefatos, revisar armazenamento e retenção; não mascarar a falha.

## Referências

- [Workflow syntax](https://docs.github.com/en/actions/reference/workflows-and-actions/workflow-syntax)
- [Artefatos de workflow](https://docs.github.com/en/actions/tutorials/store-and-share-data)
