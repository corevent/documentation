# Corevent — Documentação de Testes

Anexo da [documentação principal](README.md) com o registro dos testes realizados e seus resultados, elaborado para a disciplina de Qualidade e Testes de Software.

> ✏️ Os blocos marcados com ✏️ são instruções de preenchimento e podem ser removidos. Duplique as linhas das tabelas conforme necessário.

| Equipe | Versão do documento | Última atualização |
| --- | --- | --- |
|  |  |  |

# 1. Estratégia de testes

> ✏️ Indique, para cada componente, os níveis de teste aplicados (unitário, integração, sistema/E2E, aceitação), as ferramentas utilizadas e o ambiente onde os testes são executados (os ambientes estão descritos na seção [11. Aplicação](README.md#11-aplicação) do README).

| Componente | Níveis de teste | Ferramentas | Ambiente |
| --- | --- | --- | --- |
| API (backend) |  |  |  |
| Aplicação Web | Unitário, integração, sistema (HTTP) e E2E | Jest, Vitest, Testing Library, MSW, Supertest, Cypress e GitHub Actions | Staging |
| Mobile — Participantes | Unitário, integração e E2E | Flutter Test, Integration Test, Mocktail e GitHub Actions | Staging |
| Mobile — Equipes | Unitário e integração | Flutter Test e GitHub Actions | Staging |

# 2. Casos de teste

> ✏️ Liste os casos de teste usando o padrão de ID `CT-<COMPONENTE>-<NÚMERO>` (API, WEB, MOB). Relacione cada caso a um requisito do [README](README.md#2-requisitos) (ex.: RF 1, RNF 2). Para testes automatizados, informe o arquivo ou a suíte correspondente.

| ID | Componente | Requisito | Nível | Descrição | Resultado esperado | Automatizado |
| --- | --- | --- | --- | --- | --- | --- |
| CT-API-001 |  |  |  |  |  |  |
| CT-WEB-001 | Web | RF 1 / RNF 2 | Unitário | Validar CPF, CNPJ, e-mail, telefone, chave Pix, datas e regras de senha | Rejeitar dados inválidos com as mesmas regras da API | Sim — `tests/unit/validators.test.ts` |
| CT-WEB-002 | Web | RF 3 / RF 5 | Unitário | Aplicar máscaras e formatar datas, moeda e números | Formatar valores no padrão da interface e converter datas para ISO sem deslocamento de fuso | Sim — `tests/unit/format.test.ts` |
| CT-WEB-003 | Web | RNF 1 | Unitário | Interpretar e traduzir os erros retornados pela API | Exibir mensagens em português e mensagens genéricas para falhas de rede, servidor e tempo esgotado | Sim — `tests/unit/errors.test.ts` |
| CT-WEB-004 | Web | RF 14 / RNF 2 | Unitário | Gerar o CSV de participantes | Escapar células e neutralizar fórmulas de planilha | Sim — `tests/unit/csv.test.ts` |
| CT-WEB-005 | Web | RF 4 / RF 19 | Unitário | Calcular vendas, ocupação e receita a partir dos lotes e descrever o local do evento | Apresentar valores corretos, sem vendas negativas, para eventos presenciais, híbridos e online | Sim — `tests/unit/events.test.ts` |
| CT-WEB-006 | Web | RF 2 / RNF 2 | Integração | Autenticar, restaurar sessão, realizar logout e proteger rotas | Abrir o painel após o login, revogar o refresh token no logout e bloquear páginas da conta para visitantes | Sim — `tests/integration/auth.test.tsx` |
| CT-WEB-007 | Web | RF 2 / RNF 2 | Integração | Renovar sessão em requisições concorrentes e tratar falhas do cliente HTTP | Compartilhar uma única renovação, encerrar a sessão expirada e tratar tempo esgotado e respostas inesperadas | Sim — `tests/integration/api-client.test.ts` |
| CT-WEB-008 | Web | RF 1 / RNF 2 | Integração | Cadastrar conta com verificação de e-mail | Bloquear CPF inválido, criar a conta somente após o código correto e autenticar o usuário | Sim — `tests/integration/register.test.tsx` |
| CT-WEB-009 | Web | RF 3 / RF 4 / RF 19 | Integração | Consultar indicadores, filtrar, publicar e excluir eventos no painel | Calcular os indicadores, confirmar as ações, exigir dados de recebimento e permitir nova tentativa após falha | Sim — `tests/integration/dashboard.test.tsx` |
| CT-WEB-010 | Web | RNF 1 | Integração | Fechar modais com dados não salvos | Solicitar confirmação antes de descartar as informações digitadas | Sim — `tests/integration/modal-close.test.tsx` |
| CT-WEB-011 | Web | RNF 2 / RNF 3 | Sistema | Servir o build de produção (página inicial, rotas do SPA e arquivos estáticos) | Responder às rotas com os tipos corretos, usar a URL de produção da API e não expor *source maps* nem arquivos `.env` | Sim — `tests/api/site.test.ts` |
| CT-WEB-012 | Web | RF 2 / RF 19 | E2E | Executar o fluxo crítico do PI: entrar e acessar o painel | Concluir o login e exibir o painel do organizador | Sim — `cypress/e2e/teste_pi.cy.js` |
| CT-WEB-013 | Web | RF 2 / RNF 2 / RNF 4 | E2E | Autenticar com credenciais inválidas, restaurar sessão, tratar sessão revogada e entrar pelo celular | Exibir os erros traduzidos, preservar a sessão válida e solicitar novo login após a expiração | Sim — `cypress/e2e/auth.cy.ts` |
| CT-WEB-014 | Web | RF 3 / RF 4 / RNF 4 | E2E | Criar eventos online e presenciais com lotes, publicar e salvar como rascunho | Exigir endereço completo, confirmar o descarte e publicar também pelo layout mobile | Sim — `cypress/e2e/create-event.cy.ts` |
| CT-WEB-015 | Web | RF 4 / RF 14 / RF 19 | E2E | Gerenciar lotes, exportar participantes e cancelar evento publicado | Adicionar o lote, gerar o CSV sem fórmulas e cancelar somente após confirmação | Sim — `cypress/e2e/manage-events.cy.ts` |
| CT-WEB-016 | Web | RF 5 / RF 6 | E2E | Consultar e atualizar dados da conta e alterar senha | Salvar o contato atualizado e recusar a troca de senha com a senha atual incorreta | Sim — `cypress/e2e/account.cy.ts` |
| CT-MOB-001 | Participantes | RF 1 / RNF 2 | Unitário | Validar CPF, CNPJ e senha | Rejeitar documentos inválidos e senhas fora das regras | Sim — `test/auth_validation_test.dart` |
| CT-MOB-002 | Participantes | RF 2 / RNF 2 | Integração | Autenticar, restaurar sessão e realizar logout | Preservar a sessão válida e encerrar o acesso após logout | Sim — `integration_test/controlled/app_test.dart` |
| CT-MOB-003 | Participantes | RF 5 | Integração | Editar dados pessoais e alterar senha | Salvar alterações válidas e apresentar o resultado | Sim — `integration_test/controlled/profile_test.dart` |
| CT-MOB-004 | Participantes | RF 10 | Integração | Buscar eventos e abrir a prévia | Exibir o evento e permitir nova tentativa após falha | Sim — `integration_test/controlled/discovery_test.dart` |
| CT-MOB-005 | Participantes | RF 8 / RF 9 | Integração | Consultar pedido pendente e confirmar pagamento | Liberar o QR somente após confirmação de pagamento | Sim — `integration_test/controlled/app_test.dart` |
| CT-MOB-006 | Participantes | RF 2 / RF 5 / RF 8 / RF 9 / RF 10 | E2E | Executar autenticação, consultas, edição de perfil e compra gratuita com API real | Concluir os fluxos e refletir os dados retornados pela API | Sim — `integration_test/e2e/all_test.dart` |
| CT-MOB-007 | Equipes | RF 1 / RNF 2 | Unitário | Validar dados de autenticação | Rejeitar informações inválidas | Sim — `test/auth_validation_test.dart` |
| CT-MOB-008 | Equipes | RF 2 / RNF 2 | Unitário | Renovar sessão em requisições concorrentes | Compartilhar a renovação e tratar falhas de autenticação | Sim — `test/auth_interceptor_test.dart` e `test/auth_refresh_test.dart` |
| CT-MOB-009 | Equipes | RF 5 / RF 6 | Integração | Consultar e atualizar perfil e senha | Utilizar as rotas e os campos esperados pela API | Sim — `test/profile_api_contract_test.dart` |
| CT-MOB-010 | Equipes | RF 7 / RF 12 | Integração | Consultar, aceitar e recusar convites | Interpretar os convites e respeitar a permissão retornada | Sim — `test/invitations_api_contract_test.dart` |
| CT-MOB-011 | Equipes | RF 14 | Integração | Consultar listas paginadas de participantes | Interpretar páginas e encerrar a paginação corretamente | Sim — `test/paginated_api_contract_test.dart` |
| CT-MOB-012 | Equipes | RF 13 / RF 15 / RF 16 / RF 17 / RF 18 | Unitário | Validar QR, permissões e estados do credenciamento | Impedir duplicidade, pausar diante de falhas e respeitar o acesso do colaborador | Sim — `test/checkin_test.dart` e `test/staff_access_test.dart` |

# 3. Execução e resultados

> ✏️ Registre cada execução dos casos de teste. Status: **Passou**, **Falhou** ou **Bloqueado**. Toda falha deve apontar o defeito correspondente na seção 4. Evidências (capturas de tela, relatórios de cobertura, logs) podem ser salvas em `assets/` e referenciadas pelo link.

| Data | Caso de teste | Versão | Resultado obtido | Status | Evidência | Defeito |
| --- | --- | --- | --- | --- | --- | --- |
| 15/11/2026 | CT-WEB-002 — unitários no CI | `3cc8e7e30a` | Teste de conversão de datas reprovado no runner do GitHub Actions (fuso UTC) | Falhou | GitHub Actions | DEF-WEB-003 |
| 17/11/2026 | CT-WEB-001 a CT-WEB-005 — unitários (Jest) | `2d68bb77d5` | 61 testes aprovados (5 suítes) | Passou | Execução local — `pnpm test:unit` | — |
| 17/11/2026 | CT-WEB-006 a CT-WEB-010 — integração (Vitest) | `2d68bb77d5` | 25 testes aprovados (5 arquivos) | Passou | Execução local — `pnpm test:integration` | — |
| 17/11/2026 | CT-WEB-011 — sistema (Supertest) | `2d68bb77d5` | 13 testes aprovados sobre o build de produção | Passou | Execução local — `pnpm test:api` | — |
| 17/11/2026 | CT-WEB-012 a CT-WEB-016 — E2E (Cypress, Chrome) | `2d68bb77d5` | 14 cenários aprovados (5 specs) | Passou | Execução local — `pnpm test:e2e` | — |
| 15/11/2026 | CT-MOB-001 a CT-MOB-005 — suítes locais e integração controlada | `1fb7a38d16` | 243 testes unitários/widgets/contratos e 12 de integração Android aprovados | Passou | [Run Tests](https://github.com/corevent/corevent_app/actions/runs/43732465235) | — |
| 13/10/2026 | CT-MOB-006 — E2E | `ea3a5bf158` | 10 cenários com API real aprovados | Passou | [Run Staging E2E](https://github.com/corevent/corevent_app/actions/runs/38429129623) | — |
| 15/11/2026 | CT-MOB-007 a CT-MOB-012 — suítes de equipes | `8642a8076c` | 109 testes aprovados | Passou | [Run Release APK](https://github.com/corevent/corevent_work_app/actions/runs/43794936441) | — |

# 4. Defeitos encontrados

> ✏️ Registre os defeitos identificados nos testes ou em revisões. Severidade: **Crítica**, **Alta**, **Média** ou **Baixa**. Status: **Aberto**, **Em correção** ou **Corrigido**.

| ID | Descrição | Componente | Caso de teste | Severidade | Status | Data de correção |
| --- | --- | --- | --- | --- | --- | --- |
| DEF-001 |  |  |  |  |  |  |
| DEF-WEB-001 | Tempo esgotado não reconhecido quando a exceção vem de outro contexto (`instanceof DOMException`), exibindo erro genérico nas requisições e no envio de imagens | Web | CT-WEB-007 | Média | Corrigido — commit `3cc8e7e` | 15/11/2026 |
| DEF-WEB-002 | Diálogo de confirmação sem nome acessível (`aria-labelledby`) | Web | CT-WEB-009 | Baixa | Corrigido — commit `3cc8e7e` | 15/11/2026 |
| DEF-WEB-003 | Fuso horário dos testes unitários definido tarde demais, quebrando a conversão de datas no CI (UTC) | Web | CT-WEB-002 | Baixa | Corrigido — commit `2d68bb7` | 16/11/2026 |
| DEF-MOB-001 | Encerramento indevido da sessão após falha temporária no refresh | Participantes | CT-MOB-002 | Alta | Corrigido — [PR #19](https://github.com/corevent/corevent_app/pull/19) | 12/10/2026 |
| DEF-MOB-002 | Validação de senha divergente das regras da API | Participantes | CT-MOB-001 | Média | Corrigido — [PR #19](https://github.com/corevent/corevent_app/pull/19) | 12/10/2026 |
| DEF-MOB-003 | Ausência da permissão de Internet no manifesto principal Android | Participantes | CT-MOB-006 | Alta | Corrigido — [PR #19](https://github.com/corevent/corevent_app/pull/19) | 12/10/2026 |
| DEF-MOB-004 | Cores incorretas nas telas de autenticação | Participantes | Revisão visual | Baixa | Corrigido — [PR #26](https://github.com/corevent/corevent_app/pull/26) | 21/10/2026 |
| DEF-MOB-005 | Instalação de staging sem identificação visual distinta de produção | Participantes | Verificação da instalação | Baixa | Corrigido — [PR #30](https://github.com/corevent/corevent_app/pull/30) | 02/11/2026 |
| DEF-MOB-006 | Falha na compilação release devido ao registro de plugins de teste | Participantes | Pipeline de distribuição | Alta | Corrigido — [PR #33](https://github.com/corevent/corevent_app/pull/33) | 02/11/2026 |
| DEF-MOB-007 | Falha no upload de APK por exigência de listagem do bucket | Participantes | Pipeline de distribuição | Alta | Corrigido — [PR #33](https://github.com/corevent/corevent_app/pull/33) | 02/11/2026 |
| DEF-MOB-008 | Instalação de staging sem identificação visual distinta de produção | Equipes | Verificação da instalação | Baixa | Corrigido — [PR #3](https://github.com/corevent/corevent_work_app/pull/3) | 02/11/2026 |
| DEF-MOB-009 | Falha no upload de APK com acesso restrito ao bucket | Equipes | Pipeline de distribuição | Alta | Corrigido — [PR #5](https://github.com/corevent/corevent_work_app/pull/5) | 02/11/2026 |

# 5. Métricas

> ✏️ Calcule os indicadores a partir das seções 2, 3 e 4 ao final de cada ciclo de testes.

## Levantamento dos testes

| Componente | Planejados | Executados | Aprovados | Reprovados | Defeitos encontrados | Linhas de código |
| --- | --- | --- | --- | --- | --- | --- |
| API (backend) | 54 | 54 | 39 | 15 | 8 | 7178 |
| Aplicação Web | 113 | 113 | 113 | 0 | 3 | 5.361 |
| Mobile — Participantes: suítes locais e integração controlada | 255 | 255 | 255 | 0 | 7 | 12.946 |
| Mobile — Participantes: E2E de 13/10 | 10 | 10 | 10 | 0 | — | — |
| Mobile — Equipes | 109 | 109 | 109 | 0 | 2 | 8.823 |
## Indicadores de qualidade

| Indicador | Cálculo | API | Web | Mobile — Participantes | Mobile — Equipes |
| --- | --- | --- | --- | --- | --- |
| Taxa de execução (TER) | (casos executados ÷ casos planejados) × 100 | 54 ÷ 54 × 100 = **100%** | 113 ÷ 113 × 100 = **100%** | 255 ÷ 255 × 100 = **100%**; E2E: 10 ÷ 10 × 100 = **100%** | 109 ÷ 109 × 100 = **100%** |
| Taxa de aprovação | (casos aprovados ÷ casos executados) × 100 | 39 ÷ 54 × 100 ≈ **72,22%** | 113 ÷ 113 × 100 = **100%** | 255 ÷ 255 × 100 = **100%**; E2E: 10 ÷ 10 × 100 = **100%** | 109 ÷ 109 × 100 = **100%** |
| Densidade de defeitos (DD) | defeitos encontrados ÷ linhas de código | 8 ÷ 7178 ≈ **0,0011** (≈ 1,11 por KLOC) | 3 ÷ 5.361 ≈ **0,000560** (**0,56 por KLOC**) | 7 ÷ 12.946 ≈ **0,000541** (**0,54 por KLOC**) | 2 ÷ 8.823 ≈ **0,000227** (**0,23 por KLOC**) |
| Cobertura de testes | (linhas cobertas ÷ linhas medidas no relatório) × 100 | 5923 ÷ 7178 × 100 ≈ **82,52%** | 692 ÷ 1.413 × 100 ≈ **48,97%** (unitários + integração) | 4.540 ÷ 5.266 × 100 ≈ **86,21%** | 1.845 ÷ 2.829 × 100 ≈ **65,22%** |
| Defeitos corrigidos | nº de defeitos corrigidos ÷ nº de defeitos encontrados |  | 3 ÷ 3 = **100%** | 7 ÷ 7 = **100%** | 2 ÷ 2 = **100%** |
| Tempo médio de correção (MTTR) | soma dos tempos de correção ÷ defeitos corrigidos |  | **A informar** (média da equipe) | **1h11min — 71 minutos** (média informada pela equipe) | **1h02min — 62 minutos** (média informada pela equipe) |

# 6. Considerações finais

> ✏️ Resuma os resultados obtidos, os principais problemas encontrados e as melhorias previstas para os próximos ciclos.

A preencher.
