## Obtenção dos Dados do Prêmio do Seguro Rural (PSR)

Os dados sobre seguro rural foram obtidos junto ao site Dados Abertos do Governo Federal pelo link https://dados.gov.br/dataset/sisser3/ (SISSER - Sistema de Subvenção Econômica ao Prêmio do Seguro Rural).  O SISSER é utilizado na operacionalização do Prêmio do Seguro Rural (PSR), através de troca de informações entre o MAPA e as seguradoras habilitadas no programa. Conforme o MAPA "Ao contratar uma apólice de seguro rural o produtor (pessoa fisica ou juridica) pode minimizar suas perdas ao recuperar o capital investido na sua lavoura. O Programa de Subvenção ao Prêmio do Seguro Rural (PSR) oferece ao agricultor a oportunidade de segurar sua produção com custo reduzido por meio de auxílio financeiro do governo federal." A subvenção econômica concedida pelo Ministério da Agricultura pode ser pleiteada por qualquer pessoa física ou jurídica que cultive ou produza espécies contempladas pelo Programa e permite ainda, a complementação dos valores por subvenções concedidas por estados e municípios. Para contratar o seguro rural, o produtor deve procurar uma seguradora habilitada pelo Ministério da Agricultura no Programa de Subvenção. Caso o produtor já tenha cobertura do Proagro ou Proagro Mais para uma lavoura, o mesmo não será beneficiado pelo PSR na mesma área.

Layout do arquivo csv

- NM_RAZAO_SOCIAL
- CD_PROCESSO_SUSEP
- NR_PROPOSTA
- ID_PROPOSTA
- DT_PROPOSTA
- DT_INICIO_VIGENCIA
- DT_FIM_VIGENCIA
- NM_SEGURADO
- NR_DOCUMENTO_SEGURADO
- NM_MUNICIPIO_PROPRIEDADE
- SG_UF_PROPRIEDADE
- LATITUDE
- NR_GRAU_LAT
- NR_MIN_LAT
- NR_SEG_LAT
- LONGITUDE
- NR_GRAU_LONG
- NR_MIN_LONG
- NR_SEG_LONG
- NM_CLASSIF_PRODUTO
- NM_CULTURA_GLOBAL
- NR_AREA_TOTAL
- NR_ANIMAL
- NR_PRODUTIVIDADE_ESTIMADA
- NR_PRODUTIVIDADE_SEGURADA
- NivelDeCobertura
- VL_LIMITE_GARANTIA
- VL_PREMIO_LIQUIDO
- PE_TAXA
- VL_SUBVENCAO_FEDERAL
- NR_APOLICE
- DT_APOLICE
- ANO_APOLICE
- CD_GEOCMU
- VALOR_INDENIZACAO
- EVENTO_PREPONDERANTE


Observação dos Dados
O conjunto de dados é formado de 2 arquivos (csv) em que o primeiro contempla os anos de 2006 a 2016 e o segundo conjunto vai de 2017 até 06/2021. Para efeitos práticos juntamos os arquivos onde observou-se que o valor total de registros é de 1146420, dos quais 113675 apresentam ocorrências com "VALOR_DE_INDENIZAÇÃO" e "EVENTO_PREPONDERANTE" que são alvos da observação.

Observação por evento preponderante
Observou-se a seguinte distribuição de eventos:

| Evento |Quantidade |
|------|----------|
|GRANIZO| 42269|
|SECA| 41695|
|GEADA| 17322|
|CHUVA EXCESSIVA| 8205|
|VENTOS FORTES/FRIOS| 1327|
|MORTE| 1154|
|INUNDAÇÃO/TROMBA D´ÁGUA| 1025|
|INCÊNDIO| 192|
|QUEDA DE PARREIRAL| 173|
|DEMAIS CAUSAS| 118|
|VARIAÇÃO EXCESSIVA DE TEMPERATURA| 106|
|VARIAÇÃO DE PREÇO| 52|
|PERDA DE QUALIDADE| 19|
|RAIO| 17|
|REPLANTIO |1|
|TOTAL|113675|

Observação por cultura
Observou-se a seguinte distribuição por culturas:

|Cultura|Ocorrência|
|-------|----------|
|Soja                 |30726|
|Uva                  |21328|
|Trigo                |18656|
|Milho 2ª safra       |15973|
|Maçã                 | 7726|
|Milho 1ª safra       | 3432|
|Cebola               | 2426|
|Ameixa               | 1853|
|Pêssego              | 1799|
|Tomate               | 1785|
|Arroz                | 1510|
|Caqui                | 1433|
|Pecuário             | 1172|
|Café                 | 1043|
|Cevada               |  587|
|Cana-de-açúcar       |  349|
|Feijão               |  279|
|Alho                 |  245|
|Feijão 1ª safra      |  175|
|Nectarina            |  129|
|Pêra                 |  119|
|Pimentão             |  118|
|Canola               |  110|
|Floresta             |  110|
|Goiaba               |   98|
|Tangerina            |   84|
|Sorgo                |   61|
|Aveia                |   43|
|Kiwi                 |   39|
|Melancia             |   38|
|Algodão              |   37|
|Amendoim             |   33|
|Batata               |   30|
|Chuchu               |   24|
|Figo                 |   16|
|Repolho              |   10|
|Laranja              |    9|
|Banana               |    8|
|Alface               |    7|
|Beterraba            |    7|
|Mandioca             |    6|
|Morango              |    6|
|Berinjela            |    6|
|Abobrinha            |    5|
|Vagem                |    4|
|Atemoia              |    4|
|Pepino               |    4|
|Melão                |    3|
|Abóbora              |    2|
|Triticale            |    2|
|Cenoura              |    2|
|Lichia               |    1|
|Abacaxi              |    1|
|Couve-flor           |    1|
|Manga                |    1|
|TOTAL|113675|

Observação por UF

Observou-se a seguinte distribuição de eventos por UF:

|Uf|Ocorrências|
|--|-----------|
|PR            |       43027|
|RS            |       34310|
|SP            |       12105|
|SC            |       11851|
|MS            |        3660|
|GO            |        3348|
|MG            |        2960|
|MT            |         729|
|BA            |         695|
|PI            |         297|
|TO            |         199|
|DF            |         141|
|MA            |         140|
|SE            |          79|
|ES            |          78|
|PE            |          12|
|AL            |           9|
|PA            |           9|
|AM            |           8|
|RO            |           6|
|CE            |           5|
|PB            |           4|
|RJ            |           3|
|TOTAL|113675|

Para o estado do Rio Grande do Sul quando a cultura é soja, observou-se:

|Evento |Quantidades|
|-------|-----------|
|SECA                          |       1183|
|CHUVA EXCESSIVA               |        746|
|INUNDAÇÃO/TROMBA D´ÁGUA       |        188|
|GRANIZO                       |        160|
|GEADA                         |         10|
|INCÊNDIO                      |          8|
|VENTOS FORTES/FRIOS           |          7|
|VARIAÇÃO EXCESSIVA DE TEMPERATURA |      1|