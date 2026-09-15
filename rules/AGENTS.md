---
trigger: model_decision
description: "Ativar ao discutir arquitetura, criar ou revisar Technical Design Docs"
---

# Governança de Arquitetura e Design Docs

Ao atuar no domínio de arquitetura e Technical Design Docs, siga rigorosamente as diretrizes corporativas abaixo:

## 1. Critérios de Aplicabilidade

Design Docs são mandatórios para projetos com:

- Impacto cross-squad e dependências entre múltiplos microsserviços.
- Quebra de contratos de API ou eventos.
- Novos componentes de nuvem (bancos, filas) ou adoção de vendors.
- Processamento e armazenamento de dados sensíveis ou pessoais.

## 2. Modelagem C4 Container (Nível 2)

- **Obrigatório**: Todo design doc deve conter diagrama C4 Container (Nível 2).
- **Puro e Agnóstico**: Nós do C4 representam Topologia de Software e Runtime. É expressamente proibido colocar URLs de repositório Git, nomes de squads ou responsáveis físicos dentro dos nós do C4.
- **Expansão Mandatória**: Sistemas cujo código/arquitetura interna será modificado devem ser expandidos usando blocos `subgraph`. Sistemas periféricos de parceiros são marcados como `[External System]`.
- **Formatação de Nós**: `[Person]`, `[Software System]`, `[External System]`, `[Container: Tecnologia]`.
- **Protocolos nas Relações**: Toda seta deve especificar a ação e o protocolo/formato: `Origem -->|"Ação (Protocolo: REST, gRPC, Kafka, etc)"| Destino`.

## 3. Diagramas de Sequência (`sequenceDiagram`)

- `autonumber` é mandatório.
- Deve cobrir cenários felizes e tratamento de exceções usando blocos condicionais (`alt / else` e `opt`).
- Qualificação semântica clara (atores humanos como `actor`, sistemas/componentes como `participant`).

## 4. Segurança, Privacidade e LGPD

- Se fluxos financeiros ou de transações de alto risco estiverem presentes, a arquitetura deve prever revisão explícita de AppSec.
- Caso haja armazenamento de novos dados pessoais, o time de Governança de Dados / Privacidade (DPO) deve ser acionado de acordo com as políticas da organização.
