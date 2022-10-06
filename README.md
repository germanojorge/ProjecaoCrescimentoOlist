Projeção de Crescimento Olist.
================
Germano Antonio Zani Jorge

Instituto de Ciências Matemáticas e Computação, Universidade de São Paulo, Brasil.

e-mail: germano.jorge@alumni.usp.br

Objetivo:
---------

>Prever o desempenho de vendas da empresa Olist para clientes B2B nos próximos 12 meses.


Ferramentas Utilizadas:
---------
- Python
- Excel
- Regressão Linear
- Média móvel


Explicação dos arquivos:
------------------------

-   **olist_trafego_pago_e_organico:** Contém dados fictícios que simulam uma extração de informações do Google Analytics da empresa Olist, como seu investimento de Marketing em tráfego pago (Facebook, Google Search, Google Display) e tráfego orgânico (Email e Outros).

-   **leads e vendas olist csv:** Conjunto de dados fornecido pelo Kaggle que possui 2 arquivos em .csv com a quantidade de negócios fechados da empresa no primeiro e a quantidade de leads no segundo.

 -  **olist python:** Notebook python com código para ler, formatar e convertar os dados de leads e vendas em .csv para excel.

 -   **leads e vendas (formatado com python):** Arquivo em excel contendo a formatação dos dados anteriormente em .csv.

 -  **olist_final:** Arquivo em excel para prever a projeção de crescimento da empresa Olist. Possui leads do tráfego pago e orgânico e o desempenho de vendas.
 
 Tutorial da projeção de crescimento:
------------------------------------------
A Olist é uma loja de departamentos brasileira que conecta pequenos negócios. Os mercadores podem vender seus produtos através da loja Olist e utilizar sua logística para enviá-los aos clientes. Neste projeto investigaremos o funil de marketing da empresa com o intuito de criar uma projeção de crescimento de vendas para esses mercadores nos próximos 12 meses. O conjunto de dados e contexto da empresa podem ser encontrados em: https://www.kaggle.com/datasets/olistbr/marketing-funnel-olist

 
### 1. Entendimento do negócio:
De acordo com as informações da empresa contidas no Kaggle, o funil de marketing pode ser divido nas seguintes etapas:

-   **Etapa 1:** O cliente entra no site da empresa para se cadastrar.

-   **Etapa 2:** Há um contato do Representante de Desenvolvimento de Vendas (SDR) para confirmar a informação do cliente e agendar uma consulta de negócios.

-  **Etapa 3:** O Representante de Vendas (SR) realiza a consulta e fecha ou não o negócio.


> Contudo, devemos pressupor que o cliente passa por uma etapa anterior à etapa 1, que o leva a entrar no site da empresa, isto é, trata-se do canal de tráfego/marketing da empresa que o levou a conhecê-la. Este tipo de tráfego pode ser pago, realizado pela empresa em investimentos para pesquisas do Google (Google Search e Google Display), ou por meio de propagandas pelo Facebook (Facebook ads), por exemplo. Já o tráfego orgânico geralmente é feito com blogs, emails e outros. Chamaremos esta de Etapa 0. Assim: 

  -  **Etapa 0:** O cliente passa por um tráfego, pago ou orgânico, que o leva até o site da empresa.
  
  
### 2. Conjuntos de dados 
 Extrairemos os seguintes dados das respectivas etapas:
 
 - **Dados das Etapa 0 e 1**: https://github.com/germanojorge/ProjecaoCrescimentoOlist/blob/main/olist_trafego_pago_e_organico.xlsx Informações extraídas através do Google Analytics. Neste projeto utilizaremos dados fictícios. As métricas são: valor do investimento, CPC (custo por click), clicks, usuários, sessões e taxa de conversão para o Google Search, Google Display, Facebook, Email e Outros.
 
 - **Dados das Etapas 2 e 3**: https://github.com/germanojorge/ProjecaoCrescimentoOlist/tree/main/leads%20e%20vendas%20olist%20csv Taxa de conversão de ligações e fechamento de negócios. Usaremos o conjunto de dados fornecido pelo Kaggle que contém informações como quantidade de leads, data de fechamento, id de representante de vendas. Calcularemos a taxa de conversão.
----------------------------------------------
Dessa forma, o processo do negócio e seus dados podem ser resumidos através da figura a seguir:
![funil](https://github.com/germanojorge/ProjecaoCrescimentoOlist/blob/main/public/funilmarketing.png)


### 3. Limpeza e normalização dos dados
- Primeiro, faça o download da pasta **leads e vendas olist csv**
- Em seguida, abra o google colaboratory (https://colab.research.google.com/)

Começamos por importar as bibliotecas. O pandas nos ajudará na leitura e manipulação dos dados, enquanto o datetime é utilizado para alteração no formato da data.

``` python
import pandas as pd
import datetime as dt
``` 
Em seguida, vamos fazer o upload dos arquivos para o google colab:
``` python
from google.colab import files
uploaded = files.upload()
``` 
Procure por eles no seu diretório e adicione-os.

-----------------------------------------------

Essas duas linhas a seguir vão fazer com que o pandas leia seu arquivo em csv e o converta em um dataframe.
``` python
leads = pd.read_csv("olist_marketing_qualified_leads_dataset.csv")
deals = pd.read_csv("olist_closed_deals_dataset.csv")
```
----------------------------------------

Quer dar uma olhada como ficou? É só digitar:

```python
leads.head()
```
![leads](https://github.com/germanojorge/ProjecaoCrescimentoOlist/blob/main/public/pythonleadshead.JPG)

```python
deals.head()
```
![deals](https://github.com/germanojorge/ProjecaoCrescimentoOlist/blob/main/public/pythondealshead.JPG)

-----------------------------------------------

Agora fazemos uma cópia para preservar os originais.

```python
qualified_leads = leads.copy()
closed_deals = deals.copy()
```

Depois, vamos renomear as colunas "mql_id, ar_id,sdr_id" para nomes mais fáceis de reconhecer:

```python
qualified_leads = qualified_leads.rename(columns={'mql_id': 'Marketing Lead id', 'origin': 'origem'})
closed_deals = closed_deals.rename(columns={'mql_id': 'Marketing Lead id', 'ar_id': 'Sales Representative','sdr_id':'Sales Devevelopment id'})
```

Hora de juntar as duas tabelas em uma só, nosso tabelão:

```python
tabelao=pd.merge(qualified_leads, closed_deals, on='Marketing Lead id', how='left')
```
------------------------

Quase pronto! falta só colocar as datas "first_contact_date" e "won_date" em mês, pra isso vamos usar datetime como dt e chamar seu método "to_period('M')": 

```python
tabelao['month_year_first_contact'] = pd.to_datetime(tabelao['first_contact_date']).dt.to_period('M')
tabelao['month_year_won_date'] = pd.to_datetime(tabelao['won_date']).dt.to_period('M')
```
------------------------

Agora é só exportar no formato de um arquivo excel, com a função "to_excel".

```python
tabelao_excel = tabelao.to_excel("leads e vendas.xlsx")
files.download(tabelao_excel)
```
