# Previsão da Carga Máxima Diária do Paraná e Relação entre Clima, Demanda e Interrupções no Fornecimento

**Disciplina:** Ciência de Dados — UTFPR  
**Professor:** Prof. Dr. Eduardo Pena  
**Integrantes:** Kalil Nege (RA 2409020); [demais integrantes]  
**Documento da Proposta (Google Docs):** [Acessar Documento](https://docs.google.com/document/d/19r2d1naF1CqVDHZCL37abtJKSbo9_XD1qQugRQb1pfg/edit)  
**Proposta em PDF (1–2 páginas):** [`docs/proposta_etapa1.pdf`](docs/proposta_etapa1.pdf)

---

## 1. Visão Geral do Projeto

Este projeto investiga o comportamento da demanda e do fornecimento de energia elétrica no Paraná a partir de duas frentes:

1. **Frente Preditiva (P1):** Previsão da carga máxima diária (MW) do subsistema Paraná com antecedência de um dia (horizonte D+1), comparando modelos supervisionados e baselines temporais sob três cenários de informação climática.
2. **Frente Explicativa/Associativa (P2):** Análise da relação entre extremos meteorológicos, picos de consumo e frequência/duração de interrupções no fornecimento da Copel Distribuição, avaliando se esse padrão mudou após a privatização (agosto de 2023).

---

## 2. Perguntas de Pesquisa e Hipóteses

### Perguntas de Pesquisa
- **P1:** É possível prever a carga máxima diária do Paraná em D+1 com erro menor que baselines históricos, utilizando variáveis de clima e calendário?
- **P2:** Dias de clima extremo ou de alta demanda concentram mais interrupções na rede de distribuição? Essa relação se alterou após agosto de 2023?

### Hipóteses
- **H1:** A carga máxima diária tem relação não linear em formato de "U" com a temperatura (mínimo em temperaturas amenas e aumentos tanto no frio quanto no calor).
- **H2:** Feriados, dias pontes e recesso de fim de ano reduzem significativamente a carga máxima diária em relação a dias úteis comparáveis.
- **H3:** Chuva intensa e rajadas fortes de vento concentram maior volume de interrupções e de consumidores afetados.
- **H4 (Exploratória):** A sensibilidade das interrupções a extremos climáticos difere antes e depois de agosto de 2023 (análise associativa, controlando por fatores regulatórios e sazonais).

---

## 3. Fontes de Dados

- **ONS:** Carga verificada semi-horária por área de carga (`PR`), 2016–2026.
- **Open-Meteo ERA5:** Reanálise climática diária e horária para Curitiba, Londrina, Maringá, Cascavel e Ponta Grossa.
- **Open-Meteo Historical Forecast:** Hindcast de previsões numéricas operacionais (a partir de 2021) para o cenário de previsão D+1.
- **ANEEL:** Microdados de interrupções por ocorrência (2017–2025) e indicadores DEC/FEC por conjunto elétrico (2010–2025) na área de concessão da Copel.
- **ONS CIPER:** Indicador de interrupções na rede de transmissão do Paraná.
- **Feriados:** Biblioteca `holidays` complementada com Carnaval, Corpus Christi, pontes e recesso de fim de ano.

---

## 4. Estrutura do Repositório

```text
├── README.md
├── .gitignore
├── docs/
│   ├── proposta_etapa1.pdf
│   ├── proposta_etapa1.md
│   ├── proposta_etapa1.tex
│   └── especificacao_projeto.pdf
├── data/
│   ├── raw/
│   └── processed/
├── notebooks/
└── src/
    ├── data/
    ├── features/
    ├── models/
    └── utils/
```

---

## 5. Etapas de Desenvolvimento

- **Etapa 1:** Definição do problema e proposta de projeto.
- **Etapa 2:** Coleta, limpeza e integração dos dados (base final em Parquet).
- **Etapa 3:** Análise exploratória, consultas SQL e testes de hipóteses.
- **Etapa 4:** Modelagem preditiva, validação temporal e avaliação de erros.
- **Etapa 5:** Relatório técnico, apresentação e repositório final.
