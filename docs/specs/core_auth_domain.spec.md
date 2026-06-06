---
vitalia_spec_version: 2.1.0-sdo-draft
domain: "CoreAuth"
medical_gate_level: "L2"
requires_hitl_deploy: false
contains_phi: false
---

# Spec: Core & Auth Domain (Legacy Schema Reverse SDD)

**Projeto**: Vitalia Kit v2 (SDO Sandbox)
**Data**: 06-06-2026
**Autor**: Vitalia Archaeologist
**Status**: DRAFT

---

## Visão Geral

> Centraliza o controle de acesso, perfis de usuários (pacientes, profissionais), times, organizações e infraestrutura base de logs (consentimento e auditoria) unificando Django Auth com Core Vitalia.

---

## Requisitos Funcionais

- RF-01: O sistema deve autenticar usuários e gerenciar permissões baseadas em roles (RBAC) via Django Auth e Core Roles.
- RF-02: O sistema deve diferenciar perfis de Participante (`core_participantprofile`) e Profissional (`core_professionalprofile`).
- RF-03: O sistema deve gerenciar o escopo organizacional e de times para segregação multi-tenant.
- RF-04: O sistema deve registrar e validar consentimentos granulares dos usuários na tabela `core_consentlog` e permissões temporárias em `core_dataaccessgrant`.

## Requisitos Não-Funcionais

- RNF-01: **Auditoria Universal:** Qualquer operação de criação/alteração/deleção nos dados essenciais ou clínicos deve gerar uma entrada rastreável em `core_auditlog`.
- RNF-02: **Proteção de PII (L2):** Dados como CPF e telefone na tabela `core_userprofile` devem obedecer a mascaramento na interface e restrição de acesso interno.

## Histórias de Usuário

```
Como Administrador do Sistema,
quero organizar profissionais e pacientes em times e organizações específicas,
para isolar o acesso a dados demográficos apenas para os autorizados.

Como Paciente,
quero conceder ou revogar consentimento de acesso aos meus dados,
para garantir minha privacidade e controle (via Data Access Grant).
```

## Critérios de Aceite

- [ ] Toda delegação de acesso entre times deve gerar logs no `core_dataaccessgrant`.
- [ ] A tabela de PII `core_userprofile` (onde reside o CPF, restrição `core_userprofile_cpf_key`) deve possuir políticas de Row Level Security garantindo que apenas o dono ou perfis profissionais autorizados possam acessá-la.

## Escopo Negativo (Fora do Escopo)

- Não contempla a interface de login ou recuperação de senhas, focando apenas no core de persistência.
- Lógica de Single Sign-On (SSO) externa.

---

## Operational Constraints

- **CI/CD L2**: Alterações não exigem bloqueio médico (Gate), mas requerem revisão de segurança (Security Review) devido à exposição do RBAC.
- **Auditoria de Migrações**: Qualquer modificação que expanda PII deve ser acompanhada de uma atualização da Política de Privacidade.
