# Choveu mais ou menos que o normal na minha cidade?

**Problema:** num ano de El Niño forte, a chuva sai do padrão — mas "choveu pouco" só significa algo quando você compara com o histórico da sua região.
**Dado:** INMET, Dados Históricos (estações automáticas) — https://portal.inmet.gov.br/dadoshistoricos
**Resultado:** um gráfico de barras com a chuva de cada mês de 2026 ao lado da média dos anos anteriores, para a estação que você escolher.

## Opção 1 — Google Colab (para todo mundo)

1. Abra o arquivo `colab/chuva_vs_media.ipynb` no Google Colab (Arquivo → Fazer upload de notebook, ou pelo link do GitHub).
2. Rode as células na ordem (Shift + Enter). Na primeira, o Colab pede para conectar o Google Drive: aceite.
3. Os resultados ficam salvos em `Meu Drive/cases-agro/chuva-vs-media`.
4. Para usar com a sua cidade, troque o código da estação (`ESTACAO`) na célula **"Configure aqui"**.

## Opção 2 — Databricks (para quem quer escalar)

1. Crie uma conta na Databricks Free Edition.
2. Workspace → Import → envie `databricks/chuva_vs_media.ipynb`.
3. Se o download direto da internet não funcionar no seu workspace, baixe os `.zip` dos anos no link do INMET e envie para o Volume (Catalog → cases_agro → brutos → Upload), e rode a partir da célula 2.
4. Rode com o compute serverless.

## O que muda de um para o outro (e por quê)

| | Colab | Databricks |
|---|---|---|
| Biblioteca principal | pandas / matplotlib | PySpark (DataFrame distribuído) + pandas só no parse e no gráfico |
| Onde o dado fica | Google Drive | Unity Catalog (Volume + tabelas Delta bronze/silver/gold) |
| Escala | O que cabe na memória de uma máquina | Muitas máquinas em paralelo; fácil rodar várias estações |
| Para repetir todo mês | Rodar de novo na mão | Agendar como Job |
| Rastreabilidade | Arquivo no Drive | Delta versiona cada atualização (dá para auditar) |

**Quando usar cada um:** para olhar a sua cidade uma vez, o Colab resolve em 10 minutos. Quando você quer acompanhar **dezenas de estações todo mês**, de forma governada e automática, é o caso do Databricks — mesma lógica, pronta para virar Job.

> **Nota sobre a "média":** nos dois notebooks, a média é a das estações automáticas nos anos escolhidos (padrão desde 2016), **não** é a Normal Climatológica de 30 anos do INMET. Quanto mais anos, mais perto da Normal. Os meses incompletos de 2026 não entram na comparação.
