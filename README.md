# Vitalia SDO Sandbox

Bem-vindo ao **Vitalia SDO Sandbox**, o ambiente isolado de testes e transmutação arquitetural (Spec-Driven Operations). 

Este repositório contém a infraestrutura legada do banco de dados (dump PostgreSQL) e as novas Especificações de Domínio (Specs) extraídas via **Reverse SDD** (Spec-Driven Development) utilizando o **Vitalia Spec Kit**.

## 🎯 Objetivo

A principal meta desta sandbox é realizar a engenharia reversa de um schema legado monolítico de mais de 3.400 linhas (`legacy_schema.sql`), desmembrando-o em domínios modulares e mapeando rigorosamente o risco de cada tabela por meio da [Constituição de Medical Gates](https://vitalia.network) (L1 a L5).

## 🗂️ Arquitetura de Domínios (Reverse SDD)

Através da leitura integral do esquema (incluindo chaves estrangeiras, índices e metadados de encriptação nativa), extraímos as seguintes *Specs* formais para o `/docs/specs`:

1. **[Medical Domain (L5)](docs/specs/medical_domain.spec.md)**
   - Risco Clínico Extremo (Zero-Trust Data Probing).
   - Gerencia prescrição de exercícios, contraindicações de movimento, avaliações físicas e criptografia nativa de PHI at rest.
2. **[Social Domain (L3)](docs/specs/social_domain.spec.md)**
   - Risco Moderado.
   - Trata receitas de família, conexões de apoio mútuo, e tagueamento rigoroso de alérgenos sob supervisão de um perfil profissional.
3. **[Core & Auth Domain (L2)](docs/specs/core_auth_domain.spec.md)**
   - Risco Organizacional e Privacidade (PII).
   - Unifica as permissões (RBAC) e logs de auditoria transversais para todos os usuários (pacientes e profissionais).
4. **[Gamification Domain (L1)](docs/specs/gamification_domain.spec.md)**
   - Risco Clínico Nulo.
   - Gerencia a carteira de XP, níveis globais e concessão de badges de conquistas na plataforma.

## 🛠️ Tecnologias e Kit

- **Vitalia Spec Kit v2**: Orquestrador estrito de IA para geração e manutenção de especificações médicas seguras. Utiliza a abordagem de *Human-in-the-Loop* (HITL) e travas de segurança em domínios clínicos.
- **SQL / PostgreSQL**: Arquivo `legacy_schema.sql` contendo restrições complexas (Foreign Keys, Constraints) que revelam os fluxos de dados do sistema legado.

## 🚀 Como Contribuir

Todo o fluxo de desenvolvimento neste repositório é mediado por especificações rigorosas. Nenhuma linha de código deve ser escrita sem que uma Spec `.spec.md` tenha sido previamente aprovada pelo Medical Gate.

1. Inicie a sessão com `/vitalia-session-start`.
2. Proponha ou revise Specs usando o template oficial em `.specify/templates/`.
3. Para domínios de saúde, evoque `/vitalia-medical-gate` ou `/vitalia-science-review` antes da implementação.
4. Finalize o turno com `/vitalia-session-end` e mantenha a sincronia de times com `/vitalia-session-consolidate`.

---

*Repositório gerenciado via SDD Assistido. Vitalia Archaeologist Agent.*
