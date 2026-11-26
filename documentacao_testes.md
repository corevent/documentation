# Corevent — Documentação de Testes

Anexo da [documentação principal](README.md) com o registro dos testes realizados e seus resultados, elaborado para a disciplina de Qualidade e Testes de Software.

| Equipe | Versão do documento | Última atualização |
| --- | --- | --- |
| Corevent | 1.3 | 25/11/2026 |

# 1. Estratégia de testes

| Componente | Níveis de teste | Ferramentas | Ambiente |
| --- | --- | --- | --- |
| API (backend) | Unitário, sistema (HTTP/E2E) e verificação estática de código | Jest, Supertest, NestJS Testing, ts-jest e GitHub Actions | Staging / local com PostgreSQL |
| Aplicação Web | Unitário, integração, sistema (HTTP) e E2E | Jest, Vitest, Testing Library, MSW, Supertest, Cypress e GitHub Actions | Staging |
| Mobile — Participantes | Unitário, integração e E2E | Flutter Test, Integration Test, Mocktail e GitHub Actions | Staging |
| Mobile — Equipes | Unitário e integração | Flutter Test e GitHub Actions | Staging |

# 2. Casos de teste

| ID | Componente | Requisito | Nível | Descrição | Resultado esperado | Automatizado |
| --- | --- | --- | --- | --- | --- | --- |
| CT-API-001 | API | RF 1 / RNF 2 | Sistema | Cadastrar com dados válidos e rejeitar senha fraca, e-mail duplicado e documento inválido | Responder 201 no cadastro válido e 400 nos casos inválidos | Sim — `test/t01-register-valid.e2e-spec.ts` a `test/t04-register-invalid-document.e2e-spec.ts` |
| CT-API-002 | API | RF 2 / RNF 2 | Sistema | Autenticar com credenciais válidas e inválidas | Retornar access e refresh tokens no login válido e 401 sem tokens no inválido | Sim — `test/t05-login-valid.e2e-spec.ts` e `test/t06-login-invalid.e2e-spec.ts` |
| CT-API-003 | API | RF 5 / RF 6 / RNF 2 | Sistema | Atualizar perfil e recusar troca de senha com senha atual incorreta | Persistir o nome atualizado e responder 400 na senha atual inválida | Sim — `test/t07-update-profile.e2e-spec.ts` e `test/t08-change-password-wrong-current.e2e-spec.ts` |
| CT-API-004 | API | RF 3 / RF 4 / RF 10 / RNF 2 | Sistema | Criar e editar eventos próprios, consultar detalhes e bloquear criação sem autenticação ou edição alheia | Criar/listar/editar o próprio evento, retornar detalhes antes da compra e responder 401/400 nos acessos indevidos | Sim — `test/t09-create-event.e2e-spec.ts` a `test/t13-edit-other-organizer-event.e2e-spec.ts` |
| CT-API-005 | API | RF 7 / RF 12 / RNF 2 | Sistema | Convidar colaborador, bloquear convite sem permissão e aceitar convite | Criar convite pendente, responder 403 a não organizador e vincular o staff após o aceite | Sim — `test/t14-invite-staff.e2e-spec.ts` a `test/t16-accept-staff-invitation.e2e-spec.ts` |
| CT-API-006 | API | RF 8 / RF 9 / RNF 2 | Sistema | Criar tipo de ingresso, comprar gratuito/pago, impedir oversell e duplicidade e proteger pedido alheio | Ajustar estoque, marcar gratuito como pago, criar checkout pendente e responder 400/403 nos casos inválidos | Sim — `test/t17-create-ticket-type.e2e-spec.ts` a `test/t23-view-other-user-order.e2e-spec.ts` |
| CT-API-007 | API | RF 8 / RNF 2 | Sistema | Confirmar pagamento via webhook e garantir idempotência (validação de assinatura temporariamente desabilitada no sandbox) | Marcar order como paga uma vez e decrementar estoque uma vez; T26 permanece skipped enquanto o sandbox omite `x-authenticity-token` | Sim — `test/t24-webhook-mark-paid.e2e-spec.ts`, `test/t25-webhook-idempotency.e2e-spec.ts`, `test/t38-webhook-concurrent-idempotency.e2e-spec.ts` (T26 skipped) |
| CT-API-008 | API | RF 13 / RF 15 / RF 16 / RF 18 / RNF 2 | Sistema | Realizar check-in válido, impedir duplicidade/concorrência, order não paga, QR inválido, evento errado e acessos sem permissão | Aceitar exatamente um check-in válido e responder 409/400/401/403/404 nos demais cenários | Sim — `test/t27-checkin-valid.e2e-spec.ts` a `test/t30-checkin-forbidden.e2e-spec.ts`, `test/t37-checkin-concurrent.e2e-spec.ts`, `test/t39-checkin-unauthenticated.e2e-spec.ts`, `test/t40-checkin-other-event-staff.e2e-spec.ts` e `test/t55-checkin-invalid-qr-and-wrong-event.e2e-spec.ts` |
| CT-API-009 | API | RNF 3 | Sistema | Expor health check com banco/mail/storage disponíveis ou indisponíveis e contrato JSON estruturado | Responder 200 com status saudável, 503 com dependência fora e JSON com status, versão, uptime e dependências | Sim — `test/t31-health-check.e2e-spec.ts`, `test/t36-health-database-down.e2e-spec.ts`, `test/t44-health-structured-json.e2e-spec.ts` e `test/t57-health-dependency-down.e2e-spec.ts` |
| CT-API-010 | API | RNF 2 | Sistema | Aplicar rate limit em recuperação de e-mail, login e webhook público | Responder 429 após exceder o limite de cada rota | Sim — `test/t32-rate-limit-forgot-password.e2e-spec.ts`, `test/t41-rate-limit-login.e2e-spec.ts` e `test/t46-webhook-rate-limit.e2e-spec.ts` |
| CT-API-011 | API | RNF 1 / RNF 2 | Sistema | Validar paginação, rejeitar campos não permitidos e padronizar erro de validação | Retornar meta de paginação coerente e body 400 consistente do ValidationPipe | Sim — `test/t33-paginated-listing.e2e-spec.ts`, `test/t34-forbid-non-whitelisted.e2e-spec.ts` e `test/t43-validation-error-contract.e2e-spec.ts` |
| CT-API-012 | API | RNF 1 | Sistema | Disponibilizar documentação OpenAPI/Swagger com caminhos e códigos críticos | Servir UI/JSON da documentação e descrever auth, orders, check-in, webhook e status codes | Sim — `test/t35-api-documentation.e2e-spec.ts` e `test/t42-api-docs-critical-paths.e2e-spec.ts` |
| CT-API-013 | API | RNF 2 | Sistema | Enviar headers de segurança (Helmet) e responder preflight CORS | Incluir headers padrão do navegador e `Access-Control-Allow-Origin` no preflight | Sim — `test/t45-security-headers.e2e-spec.ts` e `test/t48-cors-enabled.e2e-spec.ts` |
| CT-API-014 | API | RNF 1 | Verificação | Verificar limite de linhas dos services críticos (orders, tickets e events) | Manter cada service com no máximo 200 linhas | Sim — `test/t47-service-max-lines.e2e-spec.ts` |
| CT-API-015 | API | RF 8 / RNF 2 | Sistema | Reservar estoque no pedido pago, não emitir tickets antes do pagamento e proteger o último ingresso | Decrementar estoque na reserva, emitir tickets só após PAID e rejeitar o segundo comprador do último item | Sim — `test/t49-paid-order-stock-before-payment.e2e-spec.ts` a `test/t51-last-ticket-double-payment.e2e-spec.ts` |
| CT-API-016 | API | RF 3 / RF 4 / RF 5 | Sistema | Exigir timezone em datas de evento e gerenciar upload de avatar/banner com URL assinada | Responder 400 sem timezone; autenticar, validar tipo e titularidade e assinar a leitura após o upload | Sim — `test/t52-strict-event-dates.e2e-spec.ts` e `test/t53-image-upload.e2e-spec.ts` |
| CT-API-017 | API | RF 1 / RF 5 / RNF 2 | Unitário | Validar datas estritas, storage com URL assinada, seed de política etária e limpeza E2E | Aceitar apenas datas válidas, assinar chaves internas e preservar regras de seed/cleanup | Sim — `src/**/*.spec.ts` |
| CT-API-018 | API | RF 8 / RF 9 / RF 14 / RNF 2 | Sistema | Listar participantes e detalhes, consultar ingressos por evento, aplicar regras de elegibilidade e cancelar evento com pedidos | Retornar participantes/detalhes ao organizador, 403/404 nos acessos indevidos, rejeitar underage/mix free-paid e marcar pending/paid no cancelamento | Sim — `test/t54-event-participants.e2e-spec.ts`, `test/t56-my-tickets-by-event.e2e-spec.ts`, `test/t58-purchase-eligibility-rules.e2e-spec.ts` e `test/t59-cancel-event-with-orders.e2e-spec.ts` |
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

| Data | Caso de teste | Versão | Resultado obtido | Status | Evidência | Defeito |
| --- | --- | --- | --- | --- | --- | --- |
| 02/11/2026 | CT-API-001 a CT-API-015 — E2E com cobertura (`pnpm test:e2e:cov`) | `d8db7cd` / suíte T01–T51 | 39 testes aprovados e 15 reprovados (54 casos); cobertura 5923/7178 linhas | Parcial | Execução local — relatório `coverage/` | DEF-API-001 a DEF-API-008 |
| 02/11/2026 | CT-API-007 / CT-API-008 / CT-API-009 / CT-API-010 / CT-API-013 / CT-API-014 / CT-API-015 — falhas da 1ª rodada | `d8db7cd` | Webhook sem assinatura (200), health texto plano, Helmet/CORS ausentes, sem 429 no webhook, services > 200 linhas, tickets antes do pagamento e contrato de check-in | Falhou | Execução local — `pnpm test:e2e` | DEF-API-001 a DEF-API-008 |
| 17/11/2026 | CT-API-017 — unitários (Jest) | `7aff240` | 36 testes aprovados (6 suítes) | Passou | Execução local — `pnpm test` | — |
| 17/11/2026 | CT-API-001 a CT-API-016 — E2E regressão (`pnpm test:e2e`) | `7aff240` | 56 testes aprovados e 7 reprovados (63 casos; 48 suítes OK, 5 suítes com falha) | Parcial | Execução local — `pnpm test:e2e` (~401 s) | DEF-API-005, DEF-API-009, DEF-API-010 |
| 17/11/2026 | CT-API-005 — aceitar convite de colaborador | `7aff240` | Aceite retornou 201; o teste e o contrato esperavam 200 | Falhou | `test/t16-accept-staff-invitation.e2e-spec.ts` | DEF-API-009 |
| 17/11/2026 | CT-API-009 — health com banco disponível / indisponível | `7aff240` | T31 ainda exige texto plano `OK` (API já devolve JSON); T36 recebeu 200 ao simular banco fora | Falhou | `test/t31-health-check.e2e-spec.ts` e `test/t36-health-database-down.e2e-spec.ts` | DEF-API-010 |
| 17/11/2026 | CT-API-012 — documentação crítica (status codes) | `7aff240` | Descrição 401 do webhook no OpenAPI diverge do texto esperado pelo teste | Falhou | `test/t42-api-docs-critical-paths.e2e-spec.ts` | — |
| 17/11/2026 | CT-API-014 — limite de linhas por service | `7aff240` | `orders` 523, `tickets` 389 e `events` 419 linhas (limite 200) | Falhou | `test/t47-service-max-lines.e2e-spec.ts` | DEF-API-005 |
| 25/11/2026 | CT-API-001 a CT-API-018 — E2E com cobertura (`pnpm test:e2e:cov`) | local / suíte T01–T59 | 73 testes aprovados e 2 skipped (T26 sandbox); 59 suítes OK; cobertura 8339/10384 linhas (≈80,30%) | Passou | Execução local — relatório `coverage/` | — |
| 25/11/2026 | CT-API-005 / CT-API-009 / CT-API-012 / CT-API-014 — regressão dos defeitos abertos | local | Accept 200; health JSON + 503; Swagger webhook alinhado; services ≤200 linhas após fatiamento | Passou | `test/t16`, `t31`, `t36`, `t42`, `t47` | DEF-API-005, DEF-API-009, DEF-API-010 |
| 25/11/2026 | CT-API-008 / CT-API-009 / CT-API-018 — gaps críticos de cobertura | local | Participantes, check-in QR/evento, health mail/storage, elegibilidade e cancelamento com pedidos | Passou | `test/t54` a `test/t59` | — |
| 15/11/2026 | CT-WEB-002 — unitários no CI | `3cc8e7e30a` | Teste de conversão de datas reprovado no runner do GitHub Actions (fuso UTC) | Falhou | GitHub Actions | DEF-WEB-003 |
| 17/11/2026 | CT-WEB-001 a CT-WEB-005 — unitários (Jest) | `2d68bb77d5` | 61 testes aprovados (5 suítes) | Passou | Execução local — `pnpm test:unit` | — |
| 17/11/2026 | CT-WEB-006 a CT-WEB-010 — integração (Vitest) | `2d68bb77d5` | 25 testes aprovados (5 arquivos) | Passou | Execução local — `pnpm test:integration` | — |
| 17/11/2026 | CT-WEB-011 — sistema (Supertest) | `2d68bb77d5` | 13 testes aprovados sobre o build de produção | Passou | Execução local — `pnpm test:api` | — |
| 17/11/2026 | CT-WEB-012 a CT-WEB-016 — E2E (Cypress, Chrome) | `2d68bb77d5` | 14 cenários aprovados (5 specs) | Passou | Execução local — `pnpm test:e2e` | — |
| 15/11/2026 | CT-MOB-001 a CT-MOB-005 — suítes locais e integração controlada | `1fb7a38d16` | 243 testes unitários/widgets/contratos e 12 de integração Android aprovados | Passou | [Run Tests](https://github.com/corevent/corevent_app/actions/runs/43732465235) | — |
| 13/10/2026 | CT-MOB-006 — E2E | `ea3a5bf158` | 10 cenários com API real aprovados | Passou | [Run Staging E2E](https://github.com/corevent/corevent_app/actions/runs/38429129623) | — |
| 15/11/2026 | CT-MOB-007 a CT-MOB-012 — suítes de equipes | `8642a8076c` | 109 testes aprovados | Passou | [Run Release APK](https://github.com/corevent/corevent_work_app/actions/runs/43794936441) | — |

# 4. Defeitos encontrados

| ID | Descrição | Componente | Caso de teste | Severidade | Status | Data de correção |
| --- | --- | --- | --- | --- | --- | --- |
| DEF-API-001 | Webhook PagBank processava notificações sem validar assinatura/token de autenticidade | API | CT-API-007 | Alta | Corrigido — [PR #32](https://github.com/corevent/api-corevent/pull/32) (`f1b9f77`) | 17/11/2026 |
| DEF-API-002 | Health check retornava texto plano (`OK`) em vez de JSON estruturado com status, versão, uptime e dependências | API | CT-API-009 | Média | Corrigido — commit `5025481` | 22/09/2026 |
| DEF-API-003 | Headers de segurança do Helmet ausentes nas respostas HTTP | API | CT-API-013 | Média | Corrigido — [PR #32](https://github.com/corevent/api-corevent/pull/32) (`f1b9f77`) | 17/11/2026 |
| DEF-API-004 | Endpoint público de webhook sem rate limit (11ª chamada ainda retornava 200) | API | CT-API-010 | Alta | Corrigido — [PR #32](https://github.com/corevent/api-corevent/pull/32) (`f1b9f77`) | 17/11/2026 |
| DEF-API-005 | Services críticos (`orders`, `tickets`, `events`) acima do limite de 200 linhas definido nas regras do projeto | API | CT-API-014 | Baixa | Corrigido — fatiamento em services menores (facade ≤200 linhas) | 25/11/2026 |
| DEF-API-006 | Bootstrap sem CORS habilitado, falhando o preflight do navegador | API | CT-API-013 | Alta | Corrigido — commit `84dd5c2` / [PR #32](https://github.com/corevent/api-corevent/pull/32) | 15/11/2026 |
| DEF-API-007 | Pedido pago criava tickets `PENDING` antes da confirmação do pagamento, gerando ingressos órfãos | API | CT-API-015 | Alta | Corrigido — commit `b630bf1` | 23/09/2026 |
| DEF-API-008 | Check-in respondia 201 enquanto a documentação e o contrato dos testes exigiam 200 | API | CT-API-008 | Baixa | Corrigido — [PR #32](https://github.com/corevent/api-corevent/pull/32) (`f1b9f77`) | 17/11/2026 |
| DEF-API-009 | Aceite de convite de colaborador responde 201 em vez de 200 | API | CT-API-005 | Baixa | Corrigido — `@HttpCode(200)` em accept/reject | 25/11/2026 |
| DEF-API-010 | Health com banco indisponível continua respondendo 200 (esperado ≥ 500 na suíte) | API | CT-API-009 | Média | Corrigido — resposta 503 quando `status !== OK` | 25/11/2026 |
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

## Levantamento dos testes

| Componente | Planejados | Executados | Aprovados | Reprovados | Defeitos encontrados | Linhas de código |
| --- | --- | --- | --- | --- | --- | --- |
| API (backend) | 75 | 73 | 73 | 0 | 10 | 10384 |
| Aplicação Web | 113 | 113 | 113 | 0 | 3 | 5.361 |
| Mobile — Participantes: suítes locais e integração controlada | 255 | 255 | 255 | 0 | 7 | 12.946 |
| Mobile — Participantes: E2E de 13/10 | 10 | 10 | 10 | 0 | — | — |
| Mobile — Equipes | 109 | 109 | 109 | 0 | 2 | 8.823 |

## Indicadores de qualidade

| Indicador | Cálculo | API | Web | Mobile — Participantes | Mobile — Equipes |
| --- | --- | --- | --- | --- | --- |
| Taxa de execução (TER) | (casos executados ÷ casos planejados) × 100 | 73 ÷ 75 × 100 ≈ **97,33%** (2 skipped: T26 sandbox) | 113 ÷ 113 × 100 = **100%** | 255 ÷ 255 × 100 = **100%**; E2E: 10 ÷ 10 × 100 = **100%** | 109 ÷ 109 × 100 = **100%** |
| Taxa de aprovação | (casos aprovados ÷ casos executados) × 100 | 73 ÷ 73 × 100 = **100%** | 113 ÷ 113 × 100 = **100%** | 255 ÷ 255 × 100 = **100%**; E2E: 10 ÷ 10 × 100 = **100%** | 109 ÷ 109 × 100 = **100%** |
| Densidade de defeitos (DD) | defeitos encontrados ÷ linhas de código | 10 ÷ 10384 ≈ **0,000963** (≈ **0,96 por KLOC**) | 3 ÷ 5.361 ≈ **0,000560** (**0,56 por KLOC**) | 7 ÷ 12.946 ≈ **0,000541** (**0,54 por KLOC**) | 2 ÷ 8.823 ≈ **0,000227** (**0,23 por KLOC**) |
| Cobertura de testes | (linhas cobertas ÷ linhas medidas no relatório) × 100 | 8339 ÷ 10384 × 100 ≈ **80,30%** | 692 ÷ 1.413 × 100 ≈ **48,97%** (unitários + integração) | 4.540 ÷ 5.266 × 100 ≈ **86,21%** | 1.845 ÷ 2.829 × 100 ≈ **65,22%** |
| Defeitos corrigidos | nº de defeitos corrigidos ÷ nº de defeitos encontrados | 10 ÷ 10 = **100%** | 3 ÷ 3 = **100%** | 7 ÷ 7 = **100%** | 2 ÷ 2 = **100%** |
| Tempo médio de correção (MTTR) | soma dos tempos de correção ÷ defeitos corrigidos | **≈ 62 dias** (média histórica dos 7 primeiros; DEF-API-005/009/010 corrigidos em 25/11/2026) | **A informar** (média da equipe) | **1h11min — 71 minutos** (média informada pela equipe) | **1h02min — 62 minutos** (média informada pela equipe) |

# 6. Considerações finais

Os testes contemplaram os quatro componentes do Corevent: API, aplicação Web e os aplicativos mobile de participantes e de equipes, com casos automatizados, rastreados aos requisitos e executados localmente e no GitHub Actions. A Web e os aplicativos mobile mantêm 100% de execução e aprovação, com todos os defeitos corrigidos. A API, que concentra as regras de negócio, os pagamentos e o credenciamento, evoluiu de ≈72,22% de aprovação na primeira rodada para **100%** dos 73 casos executados em 25/11/2026 (2 skipped no sandbox: T26), com cobertura ≈**80,30%** e os 10 defeitos registrados corrigidos.

Os testes cumpriram seu papel de revelar falhas antes do uso real, como as de segurança na API, de robustez na Web e de sessão e distribuição no mobile. Como próximos passos, destacam-se a reativação da validação de assinatura do webhook PagBank fora do sandbox, o aumento da cobertura da Web (≈48,97%) e do aplicativo de equipes (≈65,22%) e a padronização da medição do tempo médio de correção entre os componentes.
