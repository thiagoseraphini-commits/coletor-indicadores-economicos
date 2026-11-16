# Coletor de Indicadores Econômicos

Script em Python que busca diariamente **Selic, CDI, IPCA e dólar (PTAX)** na API do Banco Central (SGS), grava os dados no **PostgreSQL** e gera uma **planilha Excel formatada** com a variação do dia e do mês.

Primeiro projeto do meu plano de estudos para atuar em mesa de operações (Python, dados e renda fixa).

## Objetivos

- [ ] Coleta via API com tratamento de erros
- [ ] Tabelas modeladas no PostgreSQL
- [ ] Relatório Excel gerado automaticamente
- [ ] Execução agendada com cron
- [x] Repositório no GitHub com README

## Fonte dos dados

API pública do SGS (Sistema Gerenciador de Séries Temporais) do Banco Central:

```
https://api.bcb.gov.br/dados/serie/bcdata.sgs.{codigo}/dados?formato=json&dataInicial=dd/MM/aaaa&dataFinal=dd/MM/aaaa
```

| Indicador | Código SGS | Periodicidade |
|---|---|---|
| Selic (taxa diária) | 11 | diária |
| CDI (taxa diária) | 12 | diária |
| IPCA (variação mensal) | 433 | mensal |
| Dólar PTAX (venda) | 1 | diária |

> Confirme os códigos no [portal do SGS](https://www3.bcb.gov.br/sgspub) antes de usar.

## Estrutura

```
.
├── src/coletor/      # código: coleta, banco, relatório
├── sql/              # scripts de criação das tabelas
├── relatorios/       # planilhas geradas (ignoradas pelo git)
├── tests/            # testes
├── .env.example      # modelo de configuração
└── requirements.txt
```

## Como rodar

```bash
python -m venv .venv
source .venv/bin/activate        # Windows: .venv\Scripts\activate
pip install -r requirements.txt
cp .env.example .env             # preencha com os dados do seu PostgreSQL
psql -f sql/schema.sql           # cria as tabelas
python -m coletor                # executa a coleta e gera o relatório
```

## Agendamento (cron)

Exemplo para rodar de segunda a sexta às 19h:

```
0 19 * * 1-5 cd /caminho/do/projeto && .venv/bin/python -m coletor >> logs/coletor.log 2>&1
```

## Tecnologias

Python · requests · pandas · PostgreSQL (psycopg) · openpyxl · cron
