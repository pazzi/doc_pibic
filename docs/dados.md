## Dados Pluviométricos do Rio Grande do Sul

### Dados obtidos através da ANA - Agência Nacional de Águas

A ANA - Agência Nacional de Águas mantém informações sobre estações fluviométricas e pluviométricas em um portal que pode ser acessado pelo endereço [http://www.snirh.gov.br/hidroweb/apresentacao](http://www.snirh.gov.br/hidroweb/apresentacao). O portal permite acesso a dados da "Rede Hidrometeorológica Nacional" que é composta de 4641 pontos 1 de monitoramento no país, onde 1874 estações monitoram parâmetros fluviais e 2767 monitoram as chuvas.

#### Obtenção dos dados das estações do Rio Grande do Sul ANA

Para obter-se os dados das estações pluviométricas do Rio Grande Sul digita-se o trecho a seguir em um navegador com acesso à Internet:
https://www.snirh.gov.br/hidroweb/rest/api/dadosHistoricos?estadoCodigo=24&tipoEstacao=P&size=1326&page=0, em resposta são apresentados dados em formato json de 1000 estações ao qual deverá ser salvo em um arquivo com formato json <nome>.json. 

Para obter as estações restantes basta colocar o valor 1 ao invés de 0 na mesma requisição feita no navegador, assim:  https://www.snirh.gov.br/hidroweb/rest/api/dadosHistoricos?estadoCodigo=24&tipoEstacao=P&size=1326&page=1. Em resposta obtem-se os dados das 326 estações restantes em formato json que também devem ser salvos em um arquivo de extenção json (<nome>.json).

Apps a obtenção dos arquivos json, pretende-se uní-los e ao mesmo tempo deixá-los em formato csv. Supondo-se que os dados mencionados acima tenham sido salvos com os nome dados.json e dados2.json, executaremos o script Python abaixo com a finalidade de transformar os dois arquivos json em um único arquivo com formato csv (chamaremos de dados.csv).


```
import pandas as pd
import json

with open('data.json') as f:
    jsonstr = json.load(f)

df1 = pd.io.json.json_normalize(jsonstr['content'])

with open('data2.json') as f:
    jsonstr = json.load(f)

df2 = pd.io.json.json_normalize(jsonstr['content'])

df=pd.concat([df1, df2])
df.set_index('id', inplace=True)
df.to_csv('data.csv', sep=';', index='False')
```
De posse do arquivo formato csv (dados.csv) pretendemos extrair os códigos das estações para autilizá-los no software QGIS juntamente com o plugin ANA Data Aquisition. Conforme  "a ferramenta ANA Data Acquisition realiza o download automático dos dados de várias estações pluviométricas e fluviométricas disponibilizados pela Agência Nacional de Águas (ANA). Foi idealizada para auxiliar na aplicação do Modelo de Grandes Bacias (MGB), atualmente disponível no software Quantum GIS (QGIS), desenvolvido pelo grupo de pesquisa Hidrologia de Grande Escala (HGE), do Instituto de Pesquisas Hidráulicas (IPH), da Universidade Federal do Rio Grande do Sul (UFRGS)."

Para a extração dos códigos das estações utilizaremos o awk 

```
awk -F";" {print $1}\'' dados.csv > estacoes.txt
```

Como o plugin processa no máximo 200 estações por vêz, reagrupou-se as estações em arquivos com no máximo 200 estações executou-se o comando:
```
split --lines=200 estacoes.txt 
```
Assim os arquivos com as estações forma processados pelo QGIS juntamente com o plugin ANA Data Acquisition. O processamento do plugin gerou 1326 arquivos texto sem delimitações e formatados por espaços com o seguinte layout:

| dia | mês | ano | valor |
|-----|-----|-----|-------|
|  1  | 1   | 1970|2.000000|
|2     |1   |1970 |0.000000|
|3     |1   |1970 |0.000000|
|4     |1   |1970 |13.500000|

Os arquivos de todas as estações foram agrupadas em um diretório único onde substituiu-se os espaços em branco entre os campos pelo delimitador ponto e vírgula utilizando-se os comandos:

```
sed -i 's/[[:blank:]]\+/;/g' *.txt 
sed -i 's/.//' *.txt
```

Verificou-se que 812 arquivos de estações que tinham o tamanho 390372 bytes registravam as datas corretamente, porém com o valor -1.000000. Considerou-se estes arquivos inválidos para o estudo e assim foram separados dos demais arquivos. Para separar os arquivos utilizou-se:

```find . -type f -size '390372c' -exec mv '{}' ../invalidos \;``` 

De posse dos arquivos que não estão totalmente com dados inválidos, passou-se ao processamento para consolidá-los em um único arquivo recuperando-se o número de estação. O número da estação é a denominação de cada um dos arquivos. Para este serviço usou-se o script abaixo que obtem o nome do arquivo e grava este número com os demais dados (dia, mês, ano e valor).

```
#!/bin/bash
dir_path="./saida_hidro/geral"
x=`ls $dir_path`

for item in $x 
do
        while read linha
        do
                estacao=`echo $item | cut -c1-8`
                uf="RS"
                sep=";"
                gravar="${estacao}${sep}${linha}"
                echo "${gravar}" >> consolidado_ANA_${uf}.csv 
        done < $dir_path/$item
done
```

Observe-se que a execucação do script pressupõem que os arquivos de dados a serem processados encontram-se no diretório atual apontado pela variável dir_path na linha 2 do script e neste mesmo local será gerado o arquivo final com a denominacao "consolidado_ANA_<uf>S.csv" e terá o seguinte layout:

| estação | dia | mês | ano | valor |
|---------|-----|-----|-----|-------|
|02751005|6|1|1970|0.000000|
|02751005|7|1|1970|8.000000|
|02751005|8|1|1970|1.000000|
|02751005|9|1|1970|3.700000|
|02751005|10|1|1970|0.100000|

A primeira coluna é o número da estação, a segunda será o dia, a terceira o mês, na quarta coluna irá conter o ano e a quinta o valor da preciptação. Esse valor é dado em milímetros por dia. As falhas são representadas pelo valor “-1”.
Foi verificada a quantidade de leituras com falhas no arquivo consolidade através do comando:

```
awk -F";" '$5 ~ "-1.000000" ' consolidado_ANA_RS|wc -l
```

O total de linha de informações no arquivo consolidado é de 9574792. O total de linhas que tem falhas na leitura é de 5566865. Ou seja 58,14% dos dados do arquivo foram gravados com falhas.

### Dados obtidos através do INMET - Instituto Nacional de Meteorologia

O INMET - Instituto Nacional de Meteorologia mantém informações sobre estações pluviométricas em uma página do seu portal que pode ser acessado pelo endereço [https://portal.inmet.gov.br/dadoshistoricos](https://portal.inmet.gov.br/dadoshistoricos). A página permite acesso a dados meteorologiocos de estações entre os anos 2000 até os dias atuais. Obtendo-se os arquivos dos anos, observou-se a inclusão de novas estações a medida que os anos passam, dessa forma no ano 2000 haviam 4 estações e no ano 2020 haviam 554 estações. Para o estado do Rio Grande do Sul em 2020 haviam 44 estações instaladas. A ordem de entrada das estações desse estado da federação, o ano de início de operação e a quantidade de registros coletados estão informados a seguir:

| Cidade | ano | quantidades |
|--------|-----|-------------|
|PORTO ALEGRE|2000|177753|
|RIO GRANDE|2001|167664|
|SANTA MARIA|2001|167424|
|SANTANA DO LIVRAMENTO|2001|167520|
|SANTO AUGUSTO|2001|167208|
|ALEGRETE|2006|125016|
|BENTO GONCALVES|2006|123480|
|CACAPAVA DO SUL|2006|127368|
|CAMAQUA|2006|123216|
|ERECHIM|2006|123624|
|PASSO FUNDO|2006|123576|
|RIO PARDO|2006|124968|
|SANTA ROSA|2006|123864|
|SAO JOSE DOS AUSENTES|2006|124344|
|TORRES|2006|127872|
|URUGUAIANA|2006|125016|
|BAGE|2007|122664|
|CANGUCU|2007|122184|
|CRUZALTA|2007|119136|
|JAGUARAO|2007|122544|
|LAGOA VERMELHA|2007|121296|
|QUARAI|2007|115800|
|FREDERICO WESTPHALEN|2007|114408|
|SAO BORJA|2007|117912|
|SAO GABRIEL|2007|118152|
|SAO LUIZ GONZAGA|2007|117816|
|MOSTARDAS|2008|112296|
|PALMEIRAS DAS MISSOES|2008|112632|
|CANELA|2008|108336|
|SANTA VITORIA DO PALMAR|2008|112368|
|SOLEDADE|2008|112536|
|TRAMANDAI|2008|112344|
|VACARIA|2008|111192|
|SANTIAGO|2009|104376|
|IBIRUBA|2012|70584|
|TEUTONIA|2012|72264|
|CAMPOBOM|2013|62160|
|SAO VICENTE DO SUL|2016|41544|
|SERAFIN CORREA|2016|41664|
|TUPANCIRETA|2016|38496|
|CAMBARA DO SUL|2017|35976|
|ENCRUZILHADA DO SUL|2018|24552|
|CAMPO DO LEAO PELOTAS|2019|12792|

#### Obtenção dos dados das estações do Rio Grande do Sul INMET

Após a extração dos dados pelo portal das estações por ano através de transferência (download), obtem-se arquivos em formato zip que foram extraídos em um diretório temporário. Uma particularidade a ser tratada é que uma parte da informação que necessitamos encontra-se no nome dos arquivos a outra são os dados do arquivo. Dessa forma o arquivo "INMET_S_RS_A826_ALEGRETE_01-01-2007_A_31-12-2007.CSV" obtido nos site do INMET contém informaçoes da estação A826 do município de Alegrete no período que inicia-se em 01-01-2007 e vai até 31-12-2007.

Criou-se também um novo diretório que irá receber os dados que pretende-se analisar cujo nome foi RS_INMET e executou-se a instrução:
```
find ./temporario/ -name INMET_S_RS_*.CSV -exec cp {} ./RS_INMET \;
```
A instrução irá mover todos os arquivos começando com INMET_S_RS_ e terminado por CSV para o diretório RS_INMET. Posteriormente executou-se o script abaixo que irá consolidar os dados em um arquivo denominado consolidado.csv.

```
#bin/bash
dir_path="./RS_INMET"
x=`ls $dir_path`

for item in $x 
do
        while read linha
        do
                estacao=`echo $item | cut -f4 -d'_'`
                cidade=`echo $item | cut -f5 -d'_'`
                sep=";"
                gravar="${estacao}${sep}${cidade}${sep}${linha}"
                echo "${gravar}" >> ./csv/consolidado.csv  
        done < $dir_path/$item
done
```

O script tem por finalidade gravar no arquivo consolidado.csv os dados das estações acrescidos do nome da estação e da cidade onde situa a estação que estão no nome do arquivo processado.

O arquivo consolidado.csv gerado tem campos separados pelo caracter ";" e tem o seguinte layout:
    1. Nome da estação;
    2. Município;
    3. Data no formato ano-mês-dia (aaaa-mm-dd);
    4. Hora no formato hh:mm;
    5. Precipitação total (mm);
    6. Pressão atmosférica (mB);
    7. Pressão atmosférica máxima na hora anterior (mB);
    8. Pressão atmosférica mínima na hora anterior (mB);
    9. Radiação Global (Kj/m²);
    10. Temperatura do ar - Bulbo Seco (°C);
    11. Temperatura do ponto de orvalho(°C);
    12. Temperatura máxima na hora anterior (°C);
    13. Temperatura mínima na hora anterior (°C);
    14. Temperatura orvalho máxima na hora anterior (°C);
    15. Temperatura orvalho mínima na hora anterior (°C);
    16. Umidade relativa máxima na hora anterior (%);
    17. Umidade relativa mínima na hora anterior (%);
    18. Umidade relativa do ar (%);
    19. Vento direção horária (gr) (° (gr));
    20. Vento rajada máxima (m/s);
    21. Vento velocidade horária (m/s);

Exemplo de uma linha do arquivo consolidado:

*A803;SANTAMARIA;2002-01-02;12:00;0;995,6;995,6;995,3;1990;24,9;18,5;25;23,7;19,3;18,5;73;68;68;262;5,5;2,1*

O arquivo consolidado contém 4791686 linhas com informações. Dessas 238303 apresentaram o campo relativo a "Precipitação Total" com valor -9999 que são caracterizados como leituras com falhas. Assim o arquivo consolidado apresenta 95,02% de dados íntegros.

Como obteve-se a quantidade de linhas com dados com falhas do arquivo consolidado:

```
awk -F";" '$6 ~ "-9999" ' tudo.csv|wc -l
```

As localizações das estações do Inmet no estado do Rio Grande do SUL podem ser vistas no mapa do link: [https://parabolicamara.com.br/pibic/mapInmetRS.html](https://parabolicamara.com.br/pibic/mapInmetRS.html).

