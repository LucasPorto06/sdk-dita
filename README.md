# Dita Audit SDK

SDK Android para vistoria veicular com IA.

## Integração

### 1. Adicionar repositório

No `settings.gradle` do seu projeto:

```groovy
dependencyResolutionManagement {
    repositories {
        google()
        mavenCentral()
        maven { url 'https://raw.githubusercontent.com/LucasPorto06/sdk-dita/main' }
    }
}
```

### 2. Adicionar dependência

No `build.gradle` do módulo (app):

```groovy
dependencies {
    implementation 'com.dita:audit-sdk:1.0.0'
}
```

### 3. Uso básico

```java
// Inicializar (uma vez, ex: no Application.onCreate)
DitaAudit.init(new DitaConfig.Builder().build());

// Iniciar vistoria
DitaAudit.getInstance().startInspection(activity,
    new InspectionData.Builder()
        .plate("ABC1D23")
        .chassis("9BWZZZ377VT004251")
        .vehicle("Onix 2024")
        .store("Loja Centro")
        .inspector("João Silva")
        .extra("contrato", "CT-12345")       // campos extras opcionais
        .extra("filial", "SP-001")
        .build(),
    new DitaCallback() {
        @Override
        public void onResult(AuditResult result) {
            result.isApproved();          // aprovado?
            result.getStatus();           // APPROVED, REJECTED, etc.
            result.getScore();            // 0-100
            result.getApprovedItems();    // itens detectados
            result.getRejectedItems();    // itens não detectados
            result.getBlockingReasons();  // motivos de reprovação
        }

        @Override
        public void onError(DitaError error) {
            error.getCode();
            error.getMessage();
        }

        @Override
        public void onProgress(String step, int progress) {
            // step: CREATING_INSPECTION, RECORDING_VIDEO_1, etc.
        }
    });
```

## Requisitos

- **minSdk**: 24 (Android 7.0)
- **targetSdk**: 34
- **Java**: 17

## Changelog

### 1.0.0
- Vistoria com gravação de 2 vídeos (carro fechado + aberto)
- Detecção de peças em tempo real (YOLO on-device)
- Upload com retry automático
- Resultado com score, itens detectados e motivos de reprovação
- Validação anti-fraude (FSM)
- Verificação de estado do veículo (aberto/fechado)
- Campos extras customizáveis
