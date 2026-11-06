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
| Aplicação Web |  |  |  |
| Aplicativo mobile |  |  |  |

# 2. Casos de teste

> ✏️ Liste os casos de teste usando o padrão de ID `CT-<COMPONENTE>-<NÚMERO>` (API, WEB, MOB). Relacione cada caso a um requisito do [README](README.md#2-requisitos) (ex.: RF 1, RNF 2). Para testes automatizados, informe o arquivo ou a suíte correspondente.

| ID | Componente | Requisito | Nível | Descrição | Resultado esperado | Automatizado |
| --- | --- | --- | --- | --- | --- | --- |
| CT-API-001 |  |  |  |  |  |  |

# 3. Execução e resultados

> ✏️ Registre cada execução dos casos de teste. Status: **Passou**, **Falhou** ou **Bloqueado**. Toda falha deve apontar o defeito correspondente na seção 4. Evidências (capturas de tela, relatórios de cobertura, logs) podem ser salvas em `assets/` e referenciadas pelo link.

| Data | Caso de teste | Versão | Resultado obtido | Status | Evidência | Defeito |
| --- | --- | --- | --- | --- | --- | --- |
|  |  |  |  |  |  |  |

# 4. Defeitos encontrados

> ✏️ Registre os defeitos identificados nos testes ou em revisões. Severidade: **Crítica**, **Alta**, **Média** ou **Baixa**. Status: **Aberto**, **Em correção** ou **Corrigido**.

| ID | Descrição | Componente | Caso de teste | Severidade | Status | Data de correção |
| --- | --- | --- | --- | --- | --- | --- |
| DEF-001 |  |  |  |  |  |  |

# 5. Métricas

> ✏️ Calcule os indicadores a partir das seções 2, 3 e 4 ao final de cada ciclo de testes.

## Levantamento dos testes

| Componente | Planejados | Executados | Aprovados | Reprovados | Defeitos encontrados | Linhas de código |
| --- | --- | --- | --- | --- | --- | --- |
| API (backend) | 54 | 54 | 39 | 15 | 8 | 7178 |
| Aplicação Web | 100 | 100 | 100 | 0 | 2 | 5361 |
| Aplicativo mobile | 240 | 240 | 238 | 2 | 2 | 2016 |
## Indicadores de qualidade

| Indicador | Cálculo | API | Web | Mobile |
| --- | --- | --- | --- | --- |
| Taxa de execução (TER) | (casos executados ÷ casos planejados) × 100 | 54 ÷ 54 × 100 = **100%** | 100 / 100 x 100 = **100%** | 240 ÷ 240 × 100 = **100%** |
| Taxa de aprovação | (casos aprovados ÷ casos executados) × 100 | 39 ÷ 54 × 100 ≈ **72,22%** | 100 / 100 * 100 = **100%** | 238 ÷ 240 × 100 ≈ **99,17%** |
| Densidade de defeitos (DD) | defeitos encontrados ÷ linhas de código | 8 ÷ 7178 ≈ **0,0011** (≈ 1,11 por KLOC) | 2 / 5361 ≃ **0,00037** (≃ 0,37 por KLOC) | 2 ÷ 2016 ≈ **0,00099** (≈ 0,99 por KLOC) |
| Cobertura de testes | (linhas testadas ÷ linhas de código) × 100 | 5923 ÷ 7178 × 100 ≈ **82,52%** | 4546 / 2469 x 100 ≃ **54,31%** | 1116 ÷ 2016 × 100 ≈ **55,36%** |
| Defeitos corrigidos | nº de defeitos corrigidos ÷ nº de defeitos encontrados |  |  |  |
| Tempo médio de correção (MTTR) | soma dos tempos de correção ÷ defeitos corrigidos |  |  |  |

Evidências: [levantamento dos testes](<evidencias/Screenshot From 2026-11-02 14-06-29.png>), [indicadores da API](<evidencias/Screenshot From 2026-11-02 14-06-37.png>) e [indicadores do mobile](<evidencias/Screenshot From 2026-11-02 14-06-42.png>).

# 6. Considerações finais

> ✏️ Resuma os resultados obtidos, os principais problemas encontrados e as melhorias previstas para os próximos ciclos.

A preencher.
