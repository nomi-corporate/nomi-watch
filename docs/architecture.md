# Arquitetura

Cliente Wear OS Kotlin/Compose. Organizar funcionalidades em presentation, casos de uso e adapters conforme forem implementados. Backend usa APIs compartilhadas; o relógio não acessa bancos. Comunicação Data Layer fica isolada do cliente HTTP.

A base contém somente a tela inicial. Login, persistência segura, cache, voz, push e sincronização ainda não estão implementados.
