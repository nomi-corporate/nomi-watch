# Nomi Watch — instruções para agentes

Branch principal: `main`. Stack: Kotlin, Jetpack Compose e Wear OS. Usar JDK 17, Gradle Wrapper e versões Android declaradas no projeto.

## Antes de alterar
- Leia README.md, CONTRIBUTING.md, docs relevantes e .github/workflows. Verifique branch e alterações locais; preserve o trabalho dos colegas.
- Trabalhe em uma branch curta a partir da principal atualizada. Não reutilize uma branch de bootstrap já integrada para uma nova entrega.
- Relacione o trabalho a uma HU ou TEC existente, com critérios de aceite e dependências. Não invente IDs nem links.
- Contrato aprovado: [TEC-003 v0.2](https://github.com/nomi-corporate/nomi-academic/blob/master/docs/TEC-003-contratos-api-eventos-v1.md). Preserve compatibilidade; registre decisões que mudem o contrato.
- Código, modelos e workflows são evidência do que existe. Um documento aprovado não comprova implementação, deploy ou teste em dispositivo.

## HUs, tarefas e PRs
- Cada tarefa tem um responsável; uma HU com entregas em vários repositórios tem um coordenador para acompanhar o conjunto.
- Descreva no PR a HU/TEC, o problema resolvido, a parte entregue, dependências, validação e limitações. Use links completos para referências entre repositórios.
- Para cartões em rascunho no GitHub Project, referencie o link do cartão. Não converta rascunhos automaticamente.
- Use `Refs`/link simples para entregas parciais. Reserve `Closes` para issues inteiramente atendidas; não feche a HU por concluir apenas uma tarefa.
- A HU só está concluída quando todos os critérios de aceite, integrações e verificações aplicáveis estiverem atendidos.
- Mantenedores designados: `gabrielwilliandev` e `Lucas200507`. Não integrar, autoaprovar, usar bypass ou alterar proteções/acessos sem autorização para essa ação. As regras efetivas são as do GitHub; sinalize divergências com docs antigos.

## Regras comuns de implementação
- Aplicar SOLID com simplicidade: casos de uso claros e adapters nas fronteiras externas; evitar padrões e abstrações sem necessidade.
- Serviços acessam somente seus próprios dados; integração por APIs/eventos versionados. Não compartilhar entidades de domínio nem consultar tabelas de outro serviço.
- O Assistant gera propostas; o usuário confirma e o Academic revalida/persiste. Conteúdo gerado por IA é entrada não confiável.
- Identidade vem de token validado. Validar permissão sobre cada recurso e grupo; não confiar em proprietário enviado pelo cliente.
- Seguir idempotência, ETag/If-Match, erros e datas/fusos da TEC-003. Repetições não devem duplicar efeitos; prazos sem horário não viram meia-noite implicitamente.
- Não versionar segredos, tokens, áudio pessoal, .env, local.properties ou chaves de assinatura. Não registrar tokens ou conteúdo pessoal em logs.
- Não adicionar infraestrutura paga, deploy, provedor de IA ou publicação de app implicitamente. AWS e provedores externos exigem configuração por ambiente e escopo definido.
- Wear OS é o alvo atual; Apple Watch é futuro. Não criar um microsserviço por dispositivo.

## Verificação e comunicação
- Execute verificações proporcionais à alteração e os checks aplicáveis do workflow. Mudanças apenas documentais exigem revisão de consistência, links e diff; não builds completos.
- Ao implementar comportamento, cubra casos relevantes de sucesso, validação, autorização e falha. Não simule persistência ou entrega real como sucesso de produção.
- Informe o que foi validado e o que não foi. Build, mocks, bundle e liveness não equivalem a fluxo completo ou execução em aparelho.
- Explique mudanças e limitações em português; preserve nomes de código e contratos em inglês conforme as convenções existentes.

## Organização e experiência Wear OS
- Implementar consultas e comandos rápidos; fluxos longos continuam no celular. Tratar perda de rede, expiração de sessão e handoff explicitamente.
- Manter regras de negócio no backend; separar UI Compose, estado e adapters de rede/dispositivo.
- Projetar para telas pequenas e redondas, rolagem, alvos de toque e acessibilidade. Não comprimir a interface do telefone para caber no relógio.
- Respeitar orçamento de bateria: evitar polling contínuo, trabalho pesado na UI e captura de voz permanente.
- O relógio possui sessão própria e cache limitado ao usuário. Limpar dados e vínculos sensíveis na troca de conta/logout.
- Confirmar ações interpretadas por voz antes de enviá-las ao Academic. Preservar confirmationKey e idempotência no handoff para o celular.
- `com.nomi.app` deve permanecer compatível com o cliente Android do celular. Data Layer depende também de assinatura compatível; não confundir compilação com integração pronta.
- Apple Watch é extensão futura com cliente próprio. RM22/Micro OS 10 não é o alvo desta implementação.
- Não editar arquivos gerados em build/ nem versionar local.properties, SDKs, APKs ou chaves de assinatura.

## Verificações locais
Configure ANDROID_HOME ou local.properties e execute na raiz:

```sh
./gradlew --no-daemon assembleDebug lintDebug
```

No Windows, use `gradlew.bat --no-daemon assembleDebug lintDebug`.

- CI: `Android build and lint`. Não renomear o job sem coordenar a proteção de branch.
- Validar instalação/interação em emulador ou dispositivo Wear OS compatível quando a mudança afetar comportamento. `installDebug` exige dispositivo autorizado no ADB.
- Build e lint não comprovam voz, notificações, autenticação ou comunicação com o telefone.
- Respeitar as versões do wrapper e dos arquivos Gradle; não instalar uma versão global para contornar erro de compatibilidade.
