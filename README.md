# Observatório Pantanal

## Instalação do PostgreSQL e PGAdmin 4.

Primeiramente, baixe o PostgreSQL e mantenha a instalação _default_ do programa. Segue o link para o [Download PostgreSQL](https://www.enterprisedb.com/downloads/postgres-postgresql-downloads).

<p align="center">
  <img src="https://user-images.githubusercontent.com/58231791/197791198-ac02747c-fc13-48bf-bd3b-35881581279c.png" width="720">
<p>
  
Seguindo com o processo, inicie a instalação do _Stack Builder_. No momento do __menu de extensões__, selecione a opção "PostGIS 3.3 Bundle for PostgreSQL 15 (64 bit)". 

<p align="center">
  <img src="https://user-images.githubusercontent.com/58231791/197800152-652cad07-b43e-413e-8d79-81b2d6aaadce.png" width="420">
<p>

Para saber mais sobre o manual aprofundado do PostGis acesse [PostGis WorkShop](https://docs.google.com/presentation/d/1qYXdeCIymLl32uoAHvAPrp1r-hK-_4Z8InG7sHEo6vc/edit?usp=sharing).

</p>

## Configuração o PGAdmin 4 e definindo o ambiente

### Criação e configuração do Servidor

Entre no aplicativo do PGAdmin 4, utilizando a senha definida pelo usuário. Inicialmente, cria-se um _server_ ao qual irá conter o banco de dados.

<p align="center">
<img src="https://user-images.githubusercontent.com/58231791/198715495-e133441c-8667-4ecf-834b-d6bdad35031a.png" width="720">
<p>

Insira o nome do host que irá hospedar o servidor (no caso utilizaremos __localhost__). Além da senha estabelecidade e a porta, no nosso caso:
> 5432

<p align="center">
<img src="https://user-images.githubusercontent.com/58231791/198717192-a057b5cd-0e81-419a-8537-bbd922e2390f.png" width="420">
<p>

### Criação e configuração de um banco de dados

Na página do _PGAdmin 4_, cria-se um ambiente para importar o banco de dados. Inseri-se um nome para o __dataset__.

<p align="center">
  <img src="https://user-images.githubusercontent.com/58231791/198725669-a280c8d9-6583-484e-b3c6-5115352518d7.png" width="720">
<p>

 Após gerar o ambiente, roda-se o SQL Querry, para isso utiliza-se a ferramenta: _Querry Tool_. Segue os comandos para inserir e executar no terminal Querry:

<p align="center">
  <img src="https://user-images.githubusercontent.com/58231791/198725983-881c5e91-9fcf-4b1f-b329-62b5a4a5cb84.png" width="720">
<p>

```
CREATE EXTENSION postgis;
```

```
SELECT postgis_full_version();
```

__OBS__: Caso o programa aponte um problema como _Utility file not found. Please correct the Binary Path in the Preferences dialog postgres_. Sugere-se encontrar o caminho da __bin__ do PostgreSQL e copie-o.

> Arquivos de Programa >> PostgreSQL >> 15 >> bin

No aplicativo do pgAdmin 4, selecione na aba superior _Files_ e clique em _Preferences_. Encontre a área _Path_ e selecione _Binay paths_.
Baseado na versão do seu PostgreSQL, altere o caminho da __Database Server__.

<p align="center">
<img src="https://user-images.githubusercontent.com/58231791/198731094-36c1a230-c3cc-498f-a556-fabdf9600b97.png" width="720">
<p>
</p>
 
## Importando Spatial Data (Dados Geográficos) para o PostGis

### Configurando o terminal do OSGeo4W

Ao utilizar o [QGIS e OSGeo4W](https://www.e-education.psu.edu/geog489/node/2294) como uma ferramenta de visualição e manipulação de formatos. Temos o comando __ogr2ogr__ capaz de converter os arquivos shapefile (disponibilizados pelo professor Hudson) para conseguir manipula-los dentro do Banco de Dados. 
  
Inicia-se o _OSGeo4W Shell_, conectamos  _OSGeo4W Shell_ com o _PostGIS_ e suas tabelas. Através do seguinte comando no terminal:
  
```
ogrinfo PG:"host=localhost port=5432 user='postgres' password='PASSWORD' dbname='observatorio_pantanal'"
```
> INFO: Open of `PG:host=localhost port=5432 user='postgres' password='PASSWORD' dbname='observatorio_pantanal'' using driver `PostgreSQL' successful.

### Inserindo o dataset no banco de dados

Assim, para inserir as informações do __dataset__ para as tabelas do _PostGIS_, deve-se acessar o diretório que os arquivos estão armazenados. Como, por exemplo, no nosso caso:

```
cd C:\Users\...\database_observatorio 
```

Em seguida, aplica-se o seguinte código no terminal:

```
ogr2ogr -f "PostgreSQL" PG:"host=localhost user=postgres dbname=observatorio_pantanal password=PASSWORD" -nlt POLYGON inferencia_out_2021.shp       
```

```
ogr2ogr -f "PostgreSQL" PG:"host=localhost user=postgres dbname=observatorio_pantanal password=PASSWORD" -nlt POLYGON output.shp
```

Mais informações sobre o comando, segue a [Documentação do OGR2OGR](https://gdal.org/programs/ogr2ogr.html).

### Visualização dos polígonos no mapa

Desse modo, ao abrir o _pgAdmin 4_, observa-se que na aba do ícone __Tables__ encontra-se as tabelas referentes aos valores disponibilizados pelo __shapefile__.

<p align="center">
<img src="https://user-images.githubusercontent.com/58231791/199906376-d6dd5480-450b-430d-9791-b29fbad574e7.png" width="320">
<p>

Possibilitando assim, uma visualização bruta do __dataset__. Clique em _View Data_ e depois, ao lado de __wkb geometry__, selecione a figura: _View all geometries in the column_. Dessa maneira, possibilita verificar os polígonos.
  
<p align="center">
<img src="https://user-images.githubusercontent.com/58231791/199906408-4f028722-1980-4f88-850f-cc117405107b.png" width="720">
<p>

## Funções do PostGIS e manipulação do shapefiles
  
### [ST_Transform](https://postgis.net/docs/ST_Transform.html) - Pontos em um sistema de referência. 

Função responsável por gerar novos pontos/polígonos com suas coordenadas em outros sistemas de referência espacial. Basicamente, as geometrias em um mapa (sistema de referência).

 ```
ST_Transform(wkb_geometry, 4326)
```
  
<p align="center">
<img src="" width="720">
<p>
 
Por convenção dos alunos, utilizou-se o seguinte valor de sistema de referência:
> 4326

__OBS__: Caso deseje utilizar outro valor de referência, utilize o seguinte comando:
 ```
 ST_SRID()
```  

### [ST_AsGeoJSON](https://postgis.net/docs/ST_AsGeoJSON.html)
  
Função responsável por retornar os pontos/polígonos como uma "geometria" GeoJSON. 

```
ST_AsGeoJSON(ST_Transform(wkb_geometry, 4326), 6) 
```
  
<p align="center">
<img src="" width="720">
<p>

O parâmetro **"6"** é associado como maneira de reduzir o número máximo de casas decimais. Recomenda-se para o sistema de referência **4326**, o valor **6**.

### [ST_Contains](http://postgis.net/docs/manual-1.4/ST_Contains.html) e [ST_MakeEnvelop](https://postgis.net/docs/ST_MakeEnvelope.html) - Seleção de pontos em área específica. 

Na realização das atividades do **Observatório Pantanal**, como forma de selecionar polígonos em uma determinada precisão espacial, nossa solução são as funções *ST_Contais* e *ST_MakeEnvelope*. 
  
A função *ST_Contais* retorna um booleano verdadeiro se e somente se nenhum ponto/polígono de B estiver no exterior de A, e pelo menos um ponto/polígonos do interior de B estiver no interior de A. 

Enquanto a função *ST_MakeEnvelope* gera um polígono retangular a partir dos valores mínimo e máximo para X e Y. Os valores de entrada devem estar no sistema de referência espacial especificado pelo SRID. Se nenhum SRID for especificado, o sistema de referência espacial desconhecido (SRID 0) será usado.

Seguindo a área da aplicação, utiliza-se  da função *ST_MakeEnvelope* para especificar a área desejada. Desse modo, a função *ST_Contais*, verifica os polígonos que estão totalmente contidos na área determinada, sempre mantendo o mesmo sistema de referência.
  
```
SELECT ST_AsGeoJSON(ST_Transform(wkb_geometry, 4326), 6) FROM inferencia_out_2021 
WHERE ST_Contains(ST_MakeEnvelope(-58,-22,-57,-21, 4326), ST_Transform(wkb_geometry, 4326))
```
<p align="center">
<img src="" width="720">
<p>
  
## API para o PostGIS 
  
### Receber parâmetros externos

