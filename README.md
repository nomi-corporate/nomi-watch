# Nomi Watch

Cliente nativo Kotlin + Jetpack Compose Material 3 para Wear OS. A tela inicial apresenta a identidade lilás; agenda, login, voz e comunicação com celular serão implementados nas próximas entregas.

## Requisitos

- JDK 17.
- Android SDK Platform 37 e Build Tools 36.0.0.
- Aparelho ou emulador Wear OS com API 30 ou superior.
- Gradle 9.3.1 pelo wrapper versionado. AGP 9.1.1 com Kotlin integrado.

Configure `ANDROID_HOME` ou `local.properties` (não versionado) com `sdk.dir`.

```sh
./gradlew assembleDebug lintDebug
./gradlew installDebug
```

No Windows, use `gradlew.bat`. A instalação exige um dispositivo autorizado no ADB. O APK debug fica em `app/build/outputs/apk/debug/` e também é artefato do CI. A compilação não comprova interação em relógio físico.

## Integração

O app usa `com.nomi.app`, o mesmo identificador Android do mobile. Data Layer exige assinatura compatível e configuração específica dos dois aplicativos, ainda não implementada. O relógio acessará APIs HTTPS com sessão própria; handoff e atualização local usarão o protocolo da TEC-003.

Nenhuma credencial, conta AWS, chave de assinatura ou segredo de IA fica no repositório. Wear OS é o alvo atual; o RM22/Micro OS 10 deixa de definir o escopo. Apple Watch terá cliente e adapter próprios, usando contratos de backend compartilhados.
