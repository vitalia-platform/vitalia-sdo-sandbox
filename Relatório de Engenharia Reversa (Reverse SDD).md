# Relatório de Engenharia Reversa (Reverse SDD)

A missão do **Vitalia Archaeologist** foi concluída com sucesso. Analisamos o schema legado completo de um dump PostgreSQL (~3.400 linhas) e transmutamos seus metadados implícitos em Especificações de Domínio (Specs) estruturadas, seguindo os rigorosos portões médicos da constituição do Vitalia Kit (L1 a L5).

## Domínios Mapeados

Durante nossa incursão aos artefatos antigos do banco de dados, estabelecemos o seguinte mapeamento tático:

| Domínio | Spec Link | Nível Clínico (Gate) | Risco Primário Identificado |
| :--- | :--- | :---: | :--- |
| **Medical** | [medical_domain.spec.md](file:///home/andre/projetos/assistidos/vitalia-sdo-sandbox/docs/specs/medical_domain.spec.md) | **L5** | Contraindicações de Movimento e Dados Estruturados em JSONB. |
| **Social** | [social_domain.spec.md](file:///home/andre/projetos/assistidos/vitalia-sdo-sandbox/docs/specs/social_domain.spec.md) | **L3** | Identificação de Alergias auto-declaradas em Receitas de Família. |
| **Core & Auth** | [core_auth_domain.spec.md](file:///home/andre/projetos/assistidos/vitalia-sdo-sandbox/docs/specs/core_auth_domain.spec.md) | **L2** | PII Sensível e RBAC (Auditorias e Delegações de Acesso). |
| **Gamification** | [gamification_domain.spec.md](file:///home/andre/projetos/assistidos/vitalia-sdo-sandbox/docs/specs/gamification_domain.spec.md) | **L1** | Engajamento de usuário (Badges e XP). |

## Descobertas Arquiteturais Críticas

> [!CAUTION]
> **Túneis de Criptografia**: As tabelas de `medical_physicalevaluation` evidenciaram que dados nativos em colunas como `_measurements_data_encrypted` são inerentes à arquitetura L5, comprovando a obrigatoriedade de transporte seguro (PHI).

> [!NOTE]
> **O Hub Profissional**: A análise da janela deslizante (Sliding Window) sobre as *Foreign Keys* revelou que o `core_professionalprofile` age como validador transversal (validação de `social_familyrecipe`, gerência em `medical_wellnessplan`). Ele é o pilar de verificação humana do sistema legado.

> [!TIP]
> **Metadados de IA Primitivos**: O esquema prevê o campo `ai_generation_metadata jsonb` no `medical_wellnessplan`, indicando que a infraestrutura fundacional da Vitalia para IA generativa em planos de exercícios já estava parcialmente sedimentada.

## Status da Operação

As especificações DRAFT formatadas com frontmatter SDO encontram-se injetadas em `/docs/specs`. A fase exploratória inicial está oficialmente documentada e versionável. O ambiente de sandbox SDO está pronto para transição das features de design em implementações governadas pelos gates da Constituição.
