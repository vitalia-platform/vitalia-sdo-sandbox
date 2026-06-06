---
vitalia_spec_version: 2.1.0-sdo-draft
domain: "Social"
medical_gate_level: "L3"
requires_hitl_deploy: false
contains_phi: false
---

# Spec: Social Domain (Legacy Schema Reverse SDD)

**Projeto**: Vitalia Kit v2 (SDO Sandbox)
**Data**: 06-06-2026
**Autor**: Vitalia Archaeologist
**Status**: DRAFT

---

## Visão Geral

> Gerencia a comunidade, nutrição familiar e redes de apoio mútuo, englobando receitas familiares, conexões de suporte ("Care Connections") e mapeamento de alergias alimentares (auto-declaradas e detectadas nas receitas).

---

## Requisitos Funcionais

- RF-01: O sistema deve permitir que usuários registrem e compartilhem receitas de família (`social_familyrecipe`), capturando ingredientes e valor emocional.
- RF-02: O sistema deve rastrear e classificar alérgenos (`social_allergen`), mapeando-os nas receitas submetidas (`social_familyrecipe_detected_allergens`).
- RF-03: O sistema deve suportar uma rede social de cuidado (`social_careconnection`), vinculando um "Supporter" a um "Participant".
- RF-04: O sistema deve exigir a validação de receitas por um profissional de saúde antes de serem exibidas na comunidade (`validated_by_id`).

## Requisitos Não-Funcionais

- RNF-01: **Validação de Conteúdo (L3):** Qualquer receita familiar postada publicamente não tem peso de "prescrição clínica" e deve ter um disclaimer. Porém, exige chancela de um `core_professionalprofile` (conforme Foreign Key `validated_by_id_5ab60e05_fk_core_prof`) para checagem mínima de alergias e segurança nutricional.
- RNF-02: **Integridade Comunitária:** Conexões de cuidado (`social_careconnection`) devem ser bidirecionais apenas mediante consentimento.

## Histórias de Usuário

```
Como Membro da Família (Participant),
quero compartilhar uma receita que tem valor emocional,
para inspirar outras pessoas a seguirem dietas mais afetivas e saudáveis.

Como Nutricionista (Profissional),
quero validar as receitas enviadas pela comunidade,
para assegurar que potenciais alérgenos estejam devidamente tagueados.
```

## Critérios de Aceite

- [ ] A tabela `social_familyrecipe` deve preencher obrigatoriamente a FK `validated_by_id` se a flag `is_public` for ativada (apenas para receitas publicadas na comunidade).
- [ ] Qualquer alérgeno tagueado na receita deve existir previamente em `social_allergen` (Foreign Key enforce).

## Escopo Negativo (Fora do Escopo)

- Avaliação de alergias em laboratório (exames médicos ocorrem no Domínio Medical L5).
- Interface de feed de rede social rica (foco estrito na modelagem dos dados).

---

## Operational Constraints

- **CI/CD L3**: Embora as alergias sejam relevantes à saúde, trata-se de auto-declaração para receitas. Releases do esquema Social podem passar por esteira automatizada CI/CD padrão, dispensando deploy HITL médico.
