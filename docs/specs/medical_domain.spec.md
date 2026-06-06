---
vitalia_spec_version: 2.1.0-sdo-draft
domain: "Medical"
medical_gate_level: "L5"
requires_hitl_deploy: true
contains_phi: true
---

# Spec: Medical Domain (Legacy Schema Reverse SDD)

**Projeto**: Vitalia Kit v2 (SDO Sandbox)
**Data**: 06-06-2026
**Autor**: Vitalia Archaeologist
**Status**: DRAFT

---

## Visão Geral

> Define a estrutura central de dados médicos legada, cobrindo anatomia (ossos, articulações, músculos), avaliações físicas, exames médicos, rotinas de exercícios, e regras críticas de contraindicação de movimento que afetam diretamente o risco clínico do paciente.

---

## Requisitos Funcionais

- RF-01: O sistema deve permitir o armazenamento seguro e o vínculo de estruturas anatômicas (ossos, articulações, músculos e suas fixações).
- RF-02: O sistema deve permitir o registro de avaliações físicas e exames médicos de usuários (PHI).
- RF-03: O sistema deve gerenciar atividades prescritas, treinos resistidos e planos de wellness (incluindo metadados algorítmicos em `medical_wellnessplan.ai_generation_metadata`).
- RF-04: O sistema deve impor regras de contraindicação de movimento associadas a patologias (avaliando `severity` e `risk_description`) para evitar danos à saúde do paciente.

## Requisitos Não-Funcionais

- RNF-01: **Segurança (PHI):** A tabela `medical_physicalevaluation` já prevê campos nativos de criptografia (`_measurements_data_encrypted`, `_results_data_encrypted`). O sistema deve garantir que esses dados e exames (`medicalexam.results_structured`) sejam manipulados via túneis seguros e permaneçam encriptados at rest.
- RNF-02: **Auditoria L5:** Alterações nas tabelas de contraindicações de movimento e atividades prescritas devem gerar trilhas de auditoria imutáveis (conforme `core_auditlog`).

## Histórias de Usuário

```
Como Profissional de Saúde,
quero registrar contraindicações de movimento para uma patologia específica,
para que o algoritmo do sistema não prescreva exercícios de risco que prejudiquem o paciente.

Como Paciente/Usuário,
quero visualizar meu plano de wellness e atividades prescritas,
para que eu possa executar os treinos com segurança e acompanhamento.
```

## Critérios de Aceite

- [ ] A arquitetura deve isolar as tabelas críticas L5 (contraindicações, prescrições) em esquemas com políticas de RLS (Row-Level Security) estritas.
- [ ] Nenhum acesso direto a `medical_medicalexam` deve ser permitido sem consentimento explícito prévio no `core_consentlog`.

## Escopo Negativo (Fora do Escopo)

- Não inclui o motor algorítmico de geração dinâmica de treinos neste documento (apenas o esquema de persistência e validação primária).
- Não inclui a interface gráfica de visualização de modelos 3D anatômicos.

---

## Medical Constraints

> Definidos pelo `/medical-gate` (Simulação de Análise SDO)
> Nível de risco: L5
> Gate I aprovado por: HITL Pendente

| ID | Constraint | Evidência | Nível |
|---|---|---|---|
| MC-GLOBAL-002 | Prevenção de Risco (Contraindicações) | Tabelas `medical_movementcontraindication` e `medical_pathology` devem bloquear regras de prescrição caso haja match com o usuário. | A |

### Restrições de Publicação (Gate II)

- [ ] Disclaimer educacional presente em toda exibição ao usuário
- [ ] Aprovação de profissional de saúde obrigatória para alteração de contraindicações.
- [ ] Conteúdo de prescrição exige chancela de um profissional certificado.

---

## Notas de Implementação

> A tabela `medical_movementcontraindication` atua como o core blocker clínico deste domínio. Qualquer falha na leitura dessa tabela pode gerar prescrições lesivas. O design do repositório de exercícios (`medical_exercise`, `medical_muscleaction`) deve considerar joints e patologias intrinsecamente acopladas a regras de segurança.

---

## Operational Constraints

- **CI/CD para L5**: Toda PR que modificar o esquema do domínio Medical ou suas lógicas de acesso requer, compulsoriamente, aprovações de especialistas (Vitalia Science Review e Security).
- **HITL Deploy**: A diretriz `requires_hitl_deploy: true` obriga que o release para a base de produção só possa ocorrer após a assinatura manual no painel de CI (Vitalia Medical Gate). Nenhum deploy contínuo automatizado é permitido para este nível de risco.
