# Juros, spreads e inadimplência no crédito brasileiro (2011–2026)

Projeto final de **Análise Exploratória de Dados e Visualização** (FGV EMAp, 2026.2).

**Autores:** Abner Blota, Frederico Taveira e João Vitor Maia Prates

**Contribuições:**
-Abner Blota: Perguntas 6, 7, 8
-Frederico Taveira: Perguntas 3, 4, 5
-João Vitor Maia: Captação e Tratamento de Dados, Perguntas 1 e 2


## Sobre o projeto

Investigamos como o custo do crédito (spread) e a inadimplência evoluíram no Brasil entre 2011 e 2026, e como variam entre pessoas físicas e jurídicas, tipos de crédito, estados, faixas de renda e portes de empresa. Também olhamos como essas variáveis respondem às mudanças da taxa Selic, em tempo e em intensidade.

Usamos apenas dados públicos do Banco Central do Brasil. O trabalho é descritivo e exploratório: as análises mostram associações e padrões, não relações de causa e efeito.

## Perguntas

1. Como evoluiu a inadimplência de pessoas físicas e jurídicas desde 2011?
2. Quais modalidades de crédito têm mais inadimplência, e esse ranking é estável ao longo do tempo?
3. O spread de pessoas físicas é sistematicamente diferente do de pessoas jurídicas, e essa diferença mudou?
4. A inadimplência varia entre os estados?
5. Quanto tempo a inadimplência demora para reagir a mudanças na Selic, e isso depende da modalidade?
6. O spread sobe na mesma velocidade com que desce quando a Selic muda?
7. Como a inadimplência evolui entre faixas de renda e portes empresariais durante e após a pandemia?
8. Como a inadimplência por modalidade se comportou antes, durante e após a pandemia?

## Alguns resultados

Os números abaixo vêm das análises do notebook (detalhes, ressalvas e gráficos em `analise.ipynb` e `visualizacoes.ipynb`).

- **Inadimplência PF e PJ:** a PF chegou a 6,03% em agosto de 2026, o maior valor da série. Na pandemia as duas caíram (mínimo em dezembro de 2020: 2,89% PF e 1,24% PJ), ao contrário do que esperávamos.
- **Modalidades:** crédito caro e sem garantia lidera a inadimplência (rotativo do cartão 21,7%, cheque especial 12,1%), enquanto o financiamento habitacional fica em 1,3% a 1,6%. O ranking é muito estável ao longo dos anos.
- **Spread:** o spread médio da PF (24,55%) é bem maior que o da PJ (9,02%), sem tendência linear significativa na diferença.
- **Estados:** nos últimos 12 meses, a inadimplência da PF vai de 4,15% (SC) a 9,19% (TO), e a da PJ de 1,84% (RJ) a 8,29% (PB).
- **Selic:** a variação da Selic se associa positivamente à da inadimplência, com pico em cerca de 8 meses na PF e 14 meses na PJ, mas com correlações moderadas e sem teste de causalidade.

## Fontes de dados

| Dado | Fonte | Uso |
|---|---|---|
| Inadimplência PF e PJ (séries 21084 e 21083) | SGS, Banco Central (API `https://api.bcb.gov.br/dados/serie/bcdata.sgs.{código}/dados`) | Perguntas 1, 5 e 7 |
| Spread PF e PJ (séries 20785 e 20784) | SGS, Banco Central | Perguntas 3 e 6 |
| Taxa Selic meta (série 432) | SGS, Banco Central | Perguntas 5 e 6 |
| SCR.data (carteira de crédito por estado, cliente, porte e modalidade) | [Portal de Dados Abertos do Banco Central](https://dadosabertos.bcb.gov.br/dataset/scr_data) | Perguntas 2, 4, 5, 7 e 8 |
| Malha dos estados | IBGE | Mapas da Pergunta 4 |

## Estrutura do repositório

```
├── analise.ipynb          # notebook principal: dados, perguntas e análises
├── visualizacoes.ipynb    # resumo: pergunta, gráfico e resultado de cada questão
├── imagens/               # gráficos usados no resumo
├── data/
│   ├── sgs/               # séries do SGS salvas em CSV
│   ├── processed/         # tabelas resumidas da SCR.data (parquet)
│   ├── geo/               # malha dos estados (geojson)
│   └── raw/               # ZIPs da SCR.data (não versionados, ver abaixo)
```

## Como reproduzir

1. Clone o repositório e instale as dependências:

   ```bash
   pip install pandas numpy matplotlib scipy statsmodels geopandas pyarrow requests jupyter
   ```

2. Abra o `analise.ipynb` e execute as células em ordem.

Observações:

- As séries do SGS ficam salvas em `data/sgs/`. As células de download acessam a API do Banco Central, que às vezes é instável, e reescrevem esses CSVs.
- A SCR.data completa são ZIPs de cerca de 1,9 GB (um por ano, a partir de 2012), que **não estão no repositório**. As células que a processam só rodam se a tabela resumida não existir, e as tabelas resumidas (`data/processed/*.parquet`) já estão versionadas. Para regenerá-las, baixe os arquivos `scrdata_AAAA.zip` (por exemplo, `https://www.bcb.gov.br/pda/desig/scrdata_2025.zip`) e coloque em `data/raw/`. A página do conjunto de dados está em [dadosabertos.bcb.gov.br](https://dadosabertos.bcb.gov.br/dataset/scr_data).
- A malha dos estados é baixada pelo notebook se `data/geo/brasil_ufs.geojson` não existir.

## Limitações

A análise é descritiva. A inadimplência é medida como a proporção do saldo da carteira em atraso (acima de 90 dias), não como número de pessoas ou empresas inadimplentes. Os anos de 2012 e 2026 são parciais na SCR.data, e a seleção de produtos exclui os que não estão presentes em todo o período. As ressalvas de cada pergunta estão no notebook.
