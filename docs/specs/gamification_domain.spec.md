---
vitalia_spec_version: 2.1.0-sdo-draft
domain: "Gamification"
medical_gate_level: "L1"
requires_hitl_deploy: false
contains_phi: false
---

# Spec: Gamification Domain (Legacy Schema Reverse SDD)

**Projeto**: Vitalia Kit v2 (SDO Sandbox)
**Data**: 06-06-2026
**Autor**: Vitalia Archaeologist
**Status**: DRAFT

---

## Visão Geral

> Sistema de engajamento do aplicativo, fornecendo mecânicas de pontuação, emblemas (badges) de conquista e progressão por níveis para incentivar a aderência aos planos de wellness e comportamentos positivos.

---

## Requisitos Funcionais

- RF-01: O sistema deve permitir a concessão de badges e conquistas aos usuários (`gamification_userbadge`, `gamification_badge`).
- RF-02: O sistema deve registrar transações de pontos XP na carteira virtual (`gamification_pointtransaction`), vinculadas a eventos e referências.
- RF-03: O sistema deve manter a trilha de progressão de níveis globais, determinando quanto XP é exigido por nível (`gamification_gamificationlevel`).

## Requisitos Não-Funcionais

- RNF-01: **Performance & Escalabilidade:** A tabela `gamification_pointtransaction` terá alta volumetria (inserts constantes). É recomendável indexação otimizada no `user_id` e no `created_at` (como observado nas FKs do schema).
- RNF-02: **Isolamento Não-Clínico:** Pontos não influenciam decisões médicas. Falhas ou manipulações neste domínio não impactam a segurança do paciente.

## Histórias de Usuário

```
Como Paciente Engajado,
quero ser recompensado com XP e Badges ao concluir meus treinos resistidos e plano de bem-estar,
para me sentir motivado a manter meu "streak" e autonomia.

Como Designer de Engajamento,
quero cadastrar novos Badges e triggers associados,
para criar eventos sazonais na plataforma.
```

## Critérios de Aceite

- [ ] A transação de pontos (`gamification_pointtransaction`) deve impedir valores de XP negativo (exceto para transações de resgate com tipo explícito de débito).
- [ ] Todo badge ganho deve ter um timestamp (`earned_at`) obrigatório e evitar duplicação (único por badge/usuário).

## Escopo Negativo (Fora do Escopo)

- Loja de prêmios ou infraestrutura para resgate de produtos reais (monetização).

---

## Operational Constraints

- **CI/CD L1**: Domínio totalmente livre de impacto médico ou financeiro real. Pode utilizar Continuous Deployment direto, sujeito apenas aos testes funcionais automatizados (Unit/Integration).
