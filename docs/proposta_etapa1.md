# Previsão da Carga Máxima Diária do Paraná e Relação entre Clima, Demanda e Interrupções no Fornecimento

**Integrantes:** Kalil Nege (RA 2409020); [demais integrantes] | **UTFPR — Ciência de Dados** (Prof. Dr. Eduardo Pena)  
**Git:** https://github.com/kalil03/previsao-carga-pr.git  
**Proposta:** Etapa 1 — Definição do Problema (Máximo 2 páginas)

---

## 1. Contexto, Motivação e Perguntas de Pesquisa

Desde a privatização da Copel (agosto/2023), intensificou-se o debate público sobre falhas no fornecimento no Paraná. A previsão de carga é central no planejamento eletroenergético, regida por clima e calendário. O projeto divide-se em: **(i) Preditiva:** prever a carga máxima diária em D+1; **(ii) Explicativa:** avaliar a relação entre extremos climáticos/demanda e interrupções, comparando antes e após a privatização.

- **P1 (Preditiva):** É possível prever a carga máxima diária do subsistema Paraná em D+1 com erro menor que baselines históricos, integrando clima e calendário?
- **P2 (Explicativa):** Dias de clima extremo ou alta demanda concentram mais interrupções? Esse padrão mudou após agosto de 2023?

---

## 2. Hipóteses Testáveis

- **H1 (Demanda x Temperatura):** A carga tem relação não linear ("U") com a temperatura: mínima em clima ameno (~18°C–22°C) e picos no calor (refrigeração) e no frio (aquecimento).
- **H2 (Efeito Calendário):** Feriados, pontes e fim de ano reduzem significativamente a carga máxima diária frente a dias úteis equivalentes, controlando por temperatura e mês.
- **H3 (Clima x Falhas):** Chuva intensa e rajadas de vento elevam o volume diário de interrupções e de consumidores afetados.
- **H4 (Pós-Privatização — Exploratória):** A sensibilidade das interrupções a extremos climáticos difere antes e após agosto de 2023 *(estudo estritamente associativo, sem inferência causal)*.

---

## 3. Recorte, Granularidade e Dados Selecionados

- **Variável-Alvo e Granularidade:** Carga máxima diária (MW) da área `PR`, agregada dos dados semi-horários do ONS. Granularidade diária alinhada ao horizonte operacional e aos ciclos climáticos.
- **Horizonte (P1):** D+1. Previsão gerada no final do dia D usando estritamente dados conhecidos até então, evitando *data leakage*.

| Fonte | Período | Granularidade | Variáveis e Aplicação |
|---|---|---|---|
| **ONS (Carga PR)** | 2016–2026 | Semi-horária | Carga verificada área `PR` (máx. diário). Tratamento das quebras MMGD (29/04/2023) e não supervisionadas (02/03/2021). |
| **Open-Meteo ERA5** | 2016–pres. | Horária/Diária | Temp. (máx/mín/méd), chuva, vento e umidade para Curitiba, Londrina, Maringá, Cascavel e Ponta Grossa (média ponderada). |
| **Open-Meteo NWP** | 2021–pres. | Diária | Hindcast operacional (ECMWF/GFS) para simular o cenário operacional real em D+1. |
| **ANEEL Distribuição** | 2017–2025 | Evento/Mensal | Microdados de ocorrências na área Copel (causa, duração, afetados) e DEC/FEC consolidados por conjunto elétrico. |
| **ONS CIPER & Feriados** | 2016–2026 | Diária/Mensal | Indicador de perturbações na transmissão do PR; feriados nacionais/estaduais (`holidays`), Carnaval, pontes e recesso. |

---

## 4. Planejamento da Previsão (P1) e Análise Associativa (P2)

### Cenários de Informação (P1)
- **Cenário (a) — Sem Clima:** Lags de carga (D, D-1, D-7, médias móveis 7 e 14 dias) e calendário.
- **Cenário (b) — Clima D (Persistência):** Variáveis climáticas observadas no dia D (defasagem de 1 dia; conservador).
- **Cenário (c) — Previsão NWP D+1:** Previsões meteorológicas hindcast (cenário operacional realista, avaliado desde 2021).

### Baselines, Modelos e Validação
- **Baselines:** B1 (mesmo dia da semana anterior, D-7) e B2 (média móvel dos últimos 4 mesmos dias da semana).
- **Modelos:** Regressão Linear com termo quadrático de temperatura, Random Forest, LightGBM e TabPFN (amostra temporal).
- **Validação e Métricas:** Validação cruzada com janela expansiva (*expanding window*); anos 2025–2026 reservados como teste final fora da amostra. Métricas: MAE, RMSE e MAPE, com teste de Diebold-Mariano contra baselines.

### Modelagem de Interrupções (P2) e Limitações
- **Modelos P2:** Regressão de contagem (Poisson/Binomial Negativa) com controles temporais e termos de interação entre extremos climáticos (percentil 95) e o período pós-agosto/2023.
- **Limitações Declaradas:** Caráter estritamente observacional/associativo (sem causalidade; controle das quebras regulatórias ANEEL REN 1.000/2021 e metodológicas ONS 2021/2023); representatividade das 5 cidades como aproximação populacional da concessão.
