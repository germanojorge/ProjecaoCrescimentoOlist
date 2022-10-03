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

-   **steambrcorpus.zip:** Corpus com mais de 2 milhões de comentários em Português Brasileiro de jogos na Steam, extraídos de 10 mil jogos que tiveram seu nome e gênero anotados manualmente.


-   **reviews_filtradas.zip:** Contém mais de 230 mil comentários em Português Brasileiro retirados do site steam.com, após serem filtrados aqueles possuíam *3 votos ou mais*. Os comentários foram classificados e agrupados em 10 gêneros diferentes. Depois disso, foram dividos ao meio para que uma metade *(part_50)* fosse utilizada no treinamento dos vetores de documento (doc2vec) e a outra *(rest_part_50)* para o treino e teste do algoritmo. Dessa forma, neste arquivo .zip há um total de 10 diretórios cujos nomes se referem aos gêneros obtidos e que contêm 2 arquivos, *e.g.,* *Racing_json_part_50* e *Racing_json_rest_part_50*. Além disso, há também um diretório que contém as duas partes de todos os gêneros combinados.

-   **meu_doc2vec:** Modelo de vetores de documentos doc2vec (Le e Mikolov, 2014) com 1000 dimensões já treinado utilizando metade do conjunto de dados. Trata-se de uma representação das sentenças dos comentários através de vetores que permite com que sentenças com significados semelhantes possuam representações semelhantes. Para uma explicação mais detalhada, recomenda-se a leitura de Lau e Balwdwin (2016).

 -  **meu_lda:** Modelo de Latent Dirichlet Allocation (LDA) já treinado. Um modelo estatístico para a descoberta de tópicos abstratos. Blei DM, Ng AY, Jordan MI (2003)

 -  **LIWC2007_Portugues_win (1).dic:** Dicionário que contém palavras que revelam determinados sentimentos e opiniões (Balage Filho et al., 2013, Pennebaker et al., 2001)
 - **predUtil_h05_Racing.ipynb:** Script do código em formato de python notebook para a criação do algoritmo. Será utilizado como exemplo no tutorial.
 
 
 
 
 
