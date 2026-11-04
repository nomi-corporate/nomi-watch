# Backlog inicial do smartwatch

## SPIKE-W01 — Verificar viabilidade do RM22

Tipo: investigação técnica, não HU. Prioridade P0 para decidir o escopo do relógio.

- Identificar fabricante, versão de firmware e aplicativo acompanhante.
- Encontrar documentação, SDK e condições de distribuição, se existirem.
- Verificar espelhamento de notificações de aplicativos próprios.
- Verificar instalação de apps, botões acionáveis e acesso ao áudio, sem presumir suporte.
- Registrar evidências e uma matriz suportado/não suportado/não confirmado.
- Concluir com decisão: integração limitada a notificações, desenvolvimento próprio viável ou aparelho incompatível.

## HU-013 — Consultar lembretes no relógio

Como estudante, quero visualizar lembretes acadêmicos no relógio para acompanhar compromissos sem abrir o celular.

Entrega: condicionada à SPIKE-W01. Estado: Bloqueada por viabilidade técnica.

Critérios de aceite propostos:

- Exibir título e horário do compromisso em dispositivo validado.
- Documentar a dependência de celular, Bluetooth e aplicativo acompanhante.
- Respeitar preferências e permissões de notificações do usuário.
- Validar atualização e comportamento offline no aparelho real.

Dependências: HU-007 e SPIKE-W01.

## HU-014 — Capturar compromisso por voz no relógio

Como estudante, quero registrar um compromisso por voz no relógio para capturá-lo rapidamente durante a rotina.

Entrega: futura e condicionada a acesso documentado ao microfone e comunicação com o celular/servidor. Estado: Bloqueada por viabilidade técnica.

Critérios de aceite propostos:

- Permitir iniciar e interromper captura apenas se o dispositivo oferecer acesso suportado.
- Encaminhar áudio ou transcrição ao fluxo do Assistant.
- Permitir revisão e confirmação no dispositivo suportado ou no celular antes de salvar.
- Apresentar erro e alternativa no celular quando houver falha.
- Não armazenar como confirmado um comando ainda pendente de revisão.

Dependências: HU-005, HU-006 e SPIKE-W01.
