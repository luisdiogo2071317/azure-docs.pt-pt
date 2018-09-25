---
title: Carregar dados com o Azure Machine Learning dados Prep SDK - Python
description: Saiba mais sobre a carregar dados com o SDK do Azure Machine Learning Data Prep. Pode carregar diferentes tipos de dados de entrada, especificar os tipos de ficheiro de dados e parâmetros ou utilizar a funcionalidade de leitura inteligente do SDK para detetar automaticamente o tipo de ficheiro.
services: machine-learning
ms.service: machine-learning
ms.component: core
ms.topic: conceptual
ms.author: cforbe
author: cforbe
manager: cgronlun
ms.reviewer: jmartens
ms.date: 09/24/2018
ms.openlocfilehash: 000870e3273799930f519ff32d6b072d8c2d1f10
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/24/2018
ms.locfileid: "46989690"
---
#<a name="load-data-with-the-azure-machine-learning-data-prep-sdk"></a>Carregar dados com o SDK de preparação de dados do Azure Machine Learning

O [SDK do Azure Machine Learning Data Prep](https://docs.microsoft.com/python/api/overview/azure/dataprep?view=azure-dataprep-py) permite-lhe carregar diferentes tipos de dados de entrada. Pode especificar o tipo de ficheiro de dados e seus parâmetros ou utilizar a funcionalidade de leitura inteligente do SDK para detetar automaticamente o tipo de um ficheiro.

## <a name="read-lines"></a>Ler linhas
Uma das formas mais simples para carregar dados é lê-lo a forma de linhas de texto.

```
dataflow = dprep.read_lines(path='./data/text_lines.txt')
dataflow.head(5)
```
||Linha|
|----|-----|
|0|Data \| \| temperatura mínima \| \| temperatura máximo|
|1|2015-07-1 \| \| -4.1 \| \| 10.0|
|2|2015-07-2 \| \| -0.8 \| \| 10.8|
|3|07 de 2015-3 \| \| -7.0 \| \| 10.5|
|4|4 de 07 de 2015 \| \| -5.5 \| \| 9.3|

Depois dos dados são ingeridos, pode recuperar um pandas DataFrame do conjunto de dados completo.

```
df = dataflow.to_pandas_dataframe()
df
```

||Linha|
|----|-----|
|0|Data\| \| temperatura mínima\| \| temperatura máximo|
|1|2015-07-1\| \| 4.1\| \| 10.0|
|2|2015-07-2\| \| 0,8\| \| 10.8|
|3|07 de 2015-3\| \| 7.0\| \| 10.5|
|4|4 de 07 de 2015\| \| 5.5\| \| 9.3|

## <a name="read-csv"></a>Leia o CSV
Ao ler arquivos delimitados, pode permitir que o tempo de execução subjacente inferir os parâmetros de análise (como um separador, codificação, se pretende utilizar cabeçalhos, etc.) em vez de fornecê-los. Neste exemplo, tente ler um ficheiro ao especificar apenas a respetiva localização. 

```
# SAS expires June 16th, 2019
dataflow = dprep.read_csv(path='https://dpreptestfiles.blob.core.windows.net/testfiles/read_csv_duplicate_headers.csv?st=2018-06-15T23%3A01%3A42Z&se=2019-06-16T23%3A01%3A00Z&sp=r&sv=2017-04-17&sr=b&sig=ugQQCmeC2eBamm6ynM7wnI%2BI3TTDTM6z9RPKj4a%2FU6g%3D')
dataflow.head(5)
```

| |stnam|fipst|leaid|leanm10|ncessch|MAM_MTH00numvalid_1011|
|-----|-------|---------| -------|------|-----|------|-----|
|0||stnam|fipst|leaid|leanm10|ncessch|MAM_MTH00numvalid_1011|
|1|ALABAMA|1|101710|CONDADO Hale|10171002158| |
|2|ALABAMA|1|101710|CONDADO Hale|10171002162| |
|3|ALABAMA|1|101710|CONDADO Hale|10171002156| |
|4|ALABAMA|1|101710|CONDADO Hale|10171000588|2|

É um dos parâmetros que pode especificar um número de linhas a ignorar nos ficheiros de que se estiver lendo. Utilize o seguinte código para filtrar a linha duplicada.
```
dataflow = dprep.read_csv(path='https://dpreptestfiles.blob.core.windows.net/testfiles/read_csv_duplicate_headers.csv',
                          skip_rows=1)
dataflow.head(5)
```

| |stnam|fipst|leaid|leanm10|ncessch|MAM_MTH00numvalid_1011|
|-----|-------|---------| -------|------|-----|------|-----|
|0|ALABAMA|1|101710|CONDADO Hale|10171002158|29|
|1|ALABAMA|1|101710|CONDADO Hale|10171002162|40 |
|2|ALABAMA|1|101710|CONDADO Hale|10171002156| 43|
|3|ALABAMA|1|101710|CONDADO Hale|10171000588|2|
|4|ALABAMA|1|101710|CONDADO Hale|10171000589|23 |

Em seguida, pode ver os tipos de dados das colunas.
```
dataflow.head(1).dtypes

stnam                     object
fipst                     object
leaid                     object
leanm10                   object
ncessch                   object
schnam10                  object
MAM_MTH00numvalid_1011    object
dtype: object
```
Infelizmente, todas as nossas colunas voltaram como cadeias de caracteres. Isto acontece porque, por predefinição, o SDK do Azure Machine Learning Data Prep não altera o tipo de dados. A origem de dados que está a ler é um arquivo de texto, para que o SDK lê todos os valores como cadeias de caracteres. No entanto, neste exemplo, o que queremos analisar as colunas numéricas como números. Para fazer isso, pode definir o parâmetro inference_arguments para current_culture.

```
dataflow = dprep.read_csv(path='https://dpreptestfiles.blob.core.windows.net/testfiles/read_csv_duplicate_headers.csv',
                          skip_rows=1,
                          inference_arguments=dprep.InferenceArguments.current_culture())
dataflow.head(1).dtypes

stnam                      object
fipst                     float64
leaid                     float64
leanm10                    object
ncessch                   float64
schnam10                   object
ALL_MTH00numvalid_1011    float64
dtype: object
```
Várias das colunas corretamente foram detetadas como números e seu tipo está definido como float64. Com a ingestão feito, pode recuperar um pandas DataFrame do conjunto de dados completo.

```
df = dataflow.to_pandas_dataframe()
df
```

| |stnam|leanm10|ncessch|MAM_MTH00numvalid_1011|
|-----|-------|---------| -------|------|-----|
|0|ALABAMA|CONDADO Hale|1.017100e + 10|49.0|
|1|ALABAMA|CONDADO Hale|1.017100e + 10|40.0|
|2|ALABAMA|CONDADO Hale|1.017100e + 10|43.0|
|3|ALABAMA|CONDADO Hale|1.017100e + 10|2.0|
|4|ALABAMA|CONDADO Hale|1.017100e + 10|23.0|

## <a name="read-excel"></a>Excel de leitura
O SDK de Prep de dados do Azure Machine Learning inclui um `read_excel` função para carregar ficheiros do Excel.
```
dataflow = dprep.read_excel(path='./data/excel.xlsx')
dataflow.head(5)
```
||Column1|Column2|Column3|Column4|Column5|Column6|Column7|Column8|
|------|------|------|-----|------|-----|-------|----|-----|
|0|Hoba|Iron, IVB|60000000.0|Encontrado|1920.0|http://www.lpi.usra.edu/meteor/metbull.php?cod... |-19.58333|17.91667|
|1|Cabo York|Iron, IIIAB|58200000.0|Encontrado|1818.0|http://www.lpi.usra.edu/meteor/metbull.php?cod... |76.13333|-64.93333|
|2|Campo del Cielo|Iron, IAB MG|50000000.0|Encontrado|1576.0|http://www.lpi.usra.edu/meteor/metbull.php?cod... |-27.46667|-60.58333|
|3|Canyon Diablo|Iron, IAB MG|30000000.0|Encontrado|1891.0|http://www.lpi.usra.edu/meteor/metbull.php?cod... |35.05000|-111.03333|
|4|Armanty|Iron, IIIE|28000000.0|Encontrado|1898.0|http://www.lpi.usra.edu/meteor/metbull.php?cod... |47.00000|88.00000|

Carregou a primeira planilha de ficheiro do Excel. Pode atingir o mesmo resultado ao especificar explicitamente o nome da folha que pretende carregar. Se pretender carregar a folha de cálculo do segundo em vez disso, pode fornecer seu nome como um argumento.
```
dataflow = dprep.read_excel(path='./data/excel.xlsx', sheet_name='Sheet2')
dataflow.head(5)
```

||Column1|Column2|Column3|Column4|Column5|Column6|Column7|Column8|
|------|------|------|-----|------|-----|-------|----|-----|
|0|Nenhuma|Nenhuma|Nenhuma|Nenhuma|Nenhuma|Nenhuma|Nenhuma|Nenhuma|Nenhuma|
|1|Nenhuma|Nenhuma|Nenhuma|Nenhuma|Nenhuma|Nenhuma|Nenhuma|Nenhuma|Nenhuma|
|2|Nenhuma|Nenhuma|Nenhuma|Nenhuma|Nenhuma|Nenhuma|Nenhuma|Nenhuma|Nenhuma|
|3|classificação|Cargo|Studio|Mundial|Nacionais / %|Column1|Coletividade / %|Column2|Ano ^|
|4|1|Avatar|Fox|2788|760.5|0.273|2027.5|0.727|2009 ^|5|

Como pode ver, a tabela na folha de segundo tinha cabeçalhos e três linhas vazias. Terá de modificar os argumentos da função em conformidade.
```
dataflow = dprep.read_excel(path='./data/excel.xlsx', sheet_name='Sheet2', use_header=True, skip_rows=3)
df = dataflow.to_pandas_dataframe()
df
```

||classificação|Cargo|Studio|Mundial|Nacionais / %|Column1|Coletividade / %|Column2|Ano ^|
|------|------|------|-----|------|-----|-------|----|-----|-----|
|0|1|Avatar|Fox|2788|760.5|0.273|2027.5|0.727|2009 ^|
|1|2|Titanic|Par.|2186.8|658.7|0.301|1528.1|0.699|1997 ^|
|2|3|Os Avengers do Marvel|BV|1518.6|623.4|0.41|895.2|0.59|2012|
|3|4|Harry Potter e o Deathly Hallows parte 2|WB|1341.5|381|0.284|960.5|0.716|2011|
|4|5|Congelado|BV|1274.2|400.7|0.314|873.5|0.686|2013|

## <a name="read-fixed-width-files"></a>Ler ficheiros de largura fixa
Para os ficheiros de largura fixa, pode especificar uma lista de desvios. A primeira coluna é sempre pressuposta para iniciar com offset 0.

```
dataflow = dprep.read_fwf('./data/fixed_width_file.txt', offsets=[7, 13, 43, 46, 52, 58, 65, 73])
dataflow.head(5)
```
||010000|99999|NORUEGA FALSAS|NÃO|NO_1|ENRS|Column7|Column8|Column9|
|------|------|------|-----|------|-----|-------|----|-----|----|
|0|010003|99999|NORUEGA FALSAS|NÃO|NÃO|ENSO||||
|1|010010|99999|JAN MAYEN|NÃO|JN|ENJA|+70933|-008667|+00090|
|2|010013|99999|ROST|NÃO|NÃO|||||
|3|010014|99999|SOERSTOKKEN|NÃO|NÃO|ENSO|+59783|+005350|+00500|
|4|010015|99999|BRINGELAND|NÃO|NÃO|ENBL|+61383|+005867|+03270|


Se não existirem não existem cabeçalhos nos arquivos, desejará tratar a primeira linha como dados. Passando `PromoteHeadersMode.NONE` para o argumento de palavra-chave do cabeçalho, pode evitar a detecção de cabeçalho e obter os dados corretos.

```
dataflow = dprep.read_fwf('./data/fixed_width_file.txt',
                          offsets=[7, 13, 43, 46, 52, 58, 65, 73],
                          header=dprep.PromoteHeadersMode.NONE)

df = dataflow.to_pandas_dataframe()
df
```
||Column1|Column2|Column3|Column4|Column5|Column6|Column7|Column8|Column9|
|------|------|------|-----|------|-----|-------|----|-----|----|
|0|010000|99999|NORUEGA FALSAS|NÃO|NO_1|ENRS|Column7|Column8|Column9|
|1|010003|99999|NORUEGA FALSAS|NÃO|NÃO|ENSO||||
|2|010010|99999|JAN MAYEN|NÃO|JN|ENJA|+70933|-008667|+00090|
|3|010013|99999|ROST|NÃO|NÃO|||||
|4|010014|99999|SOERSTOKKEN|NÃO|NÃO|ENSO|+59783|+005350|+00500|
|5|010015|99999|BRINGELAND|NÃO|NÃO|ENBL|+61383|+005867|+03270|

## <a name="read-sql"></a>Leitura de SQL
O SDK do Azure Machine Learning Data Prep também pode carregar dados do SQL servers. Atualmente, apenas o Microsoft SQL Server é suportado.
Para ler dados a partir de um servidor SQL, crie um objeto de origem de dados que contém as informações de ligação.

```
secret = dprep.register_secret("[SECRET-USERNAME]", "[SECRET-PASSWORD]")

ds = dprep.MSSQLDataSource(server_name="[SERVER-NAME]",
                           database_name="[DATABASE-NAME]",
                           user_name="[DATABASE-USERNAME]",
                           password=[DATABASE-PASSWORD])
```
Como pode ver, o parâmetro de palavra-passe do `MSSQLDataSource` aceita um objeto secreto. Pode obter um objeto de segredo de duas formas:
-   Registe o segredo e o respetivo valor com o motor de execução. 
-   Crie o segredo com apenas um ID (útil se o valor secreto já está registado no ambiente de execução).

Depois de criar um objeto de origem de dados, pode avançar para ler os dados.
```
dataflow = dprep.read_sql(ds, "SELECT top 100 * FROM [SalesLT].[Product]")
dataflow.head(5)
```

||ProductID|Nome|ProductNumber|Cor|StandardCost|ListPrice|Tamanho|Peso|ProductCategoryID|ProductModelID|SellStartDate|SellEndDate|DiscontinuedDate|ThumbNailPhoto|ThumbnailPhotoFileName|ROWGUID|ModifiedDate|
|-----------|-----------|-----------|-----------|-----------|-----------|-----------|-----------|-----------|-----------|-----------|-----------|-----------|-----------|-----------|-----------|-----------|-----------|
|0|680|Estrada hl - preto, 58|FR-R92B-58|Preto|1059.3100|1431.50|58|1016.04|18|6|2002-06-01 00:00:00 + 00:00|Nenhuma|Nenhuma|b'GIF89aP\x001\x00\xf7\x00\x00\x00\x00\x00\x80...|no_image_available_small.gif|43dd68d6-14a4-461f-9069-55309d90ea7e|2008-03-11 |0:01:36.827000 + 00:00|
|1|706|Estrada hl - vermelho, 58|FR-R92R-58|Vermelho|1059.3100|1431.50|58|1016.04|18|6|2002-06-01 00:00:00 + 00:00|Nenhuma|Nenhuma|b'GIF89aP\x001\x00\xf7\x00\x00\x00\x00\x00\x80...|no_image_available_small.gif|9540ff17-2712-4c90-a3d1-8ce5568b2462|2008-03-11 |10:01:36.827000 + 00:00|
|2|707|Capacete desportivo 100, vermelho|HL-U509-R|Vermelho|13.0863|34.99|Nenhuma|Nenhuma|35|33|2005-07-01 00:00:00 + 00:00|Nenhuma|Nenhuma|b'GIF89aP\x001\x00\xf7\x00\x00\x00\x00\x00\x80...|no_image_available_small.gif|2e1ef41a-c08a-4ff6-8ada-bde58b64a712|2008-03-11 |10:01:36.827000 + 00:00|
|3|708|Capacete desportivo 100, preto|HL-U509|Preto|13.0863|34.99|Nenhuma|Nenhuma|35|33|2005-07-01 00:00:00 + 00:00|Nenhuma|Nenhuma|b'GIF89aP\x001\x00\xf7\x00\x00\x00\x00\x00\x80...|no_image_available_small.gif|a25a44fb-c2de-4268-958f-110b8d7621e2|2008-03-11 |10:01:36.827000 + 00:00|
|4|709|Meias para bicicletas de montanha, M|ASSIM-B909-M|Branco|3.3963|9.50|M|Nenhuma|27|18|2005-07-01 00:00:00 + 00:00|2006-06-30 00:00:00 + 00:00|Nenhuma|b'GIF89aP\x001\x00\xf7\x00\x00\x00\x00\x00\x80...|no_image_available_small.gif|18f95f47-1540-4e02-8f1f-cc1bcb6828d0|2008-03-11 |10:01:36.827000 + 00:00|

```
df = dataflow.to_pandas_dataframe()
df.dtypes
```
```
ProductID                                     int64
Name                                         object
ProductNumber                                object
Color                                        object
StandardCost                                float64
ListPrice                                   float64
Size                                         object
Weight                                      float64
ProductCategoryID                             int64
ProductModelID                                int64
SellStartDate             datetime64[ns, UTC+00:00]
SellEndDate                                  object
DiscontinuedDate                             object
ThumbNailPhoto                               object
ThumbnailPhotoFileName                       object
rowguid                                      object
ModifiedDate              datetime64[ns, UTC+00:00]
dtype: object
```

## <a name="read-from-azure-data-lake-storage"></a>Ler a partir do armazenamento do Azure Data Lake
Existem duas formas do SDK podem adquirir o token de OAuth necessário para aceder ao armazenamento do Azure Data Lake:
-   Obter o token de acesso a partir de uma sessão de início de sessão recentes do logon do usuário da CLI do Azure
-   Utilizar um principal de serviço (SP) e um certificado como segredo

### <a name="use-an-access-token-from-a-recent-azure-cli-session"></a>Utilizar um token de acesso a partir de uma sessão de CLI do Azure recente
No seu computador local, execute o seguinte comando:

> [!NOTE] 
> Se a sua conta de utilizador é membro de mais do que um inquilino do Azure, tem de especificar o inquilino sob a forma de nome de anfitrião do URL do AAD.


```
az login
az account show --query tenantId
dataflow = read_csv(path = DataLakeDataSource(path='adl://dpreptestfiles.azuredatalakestore.net/farmers-markets.csv', tenant='microsoft.onmicrosoft.com')) head = dataflow.head(5) head
```
### <a name="create-a-service-principal-with-the-azure-cli"></a>Criar um principal de serviço com a CLI do Azure
Pode utilizar a CLI do Azure para criar um serviço principal e o certificado correspondente. Este principal de serviço específico é configurado como leitor, com seu escopo reduzido para apenas a conta de armazenamento do Azure Data Lake 'dpreptestfiles'
```
az account set --subscription "Data Wrangling development"
az ad sp create-for-rbac -n "SP-ADLS-dpreptestfiles" --create-cert --role reader --scopes /subscriptions/35f16a99-532a-4a47-9e93-00305f6c40f2/resourceGroups/dpreptestfiles/providers/Microsoft.DataLakeStore/accounts/dpreptestfiles
```
Este comando emite o `appId` e o caminho para o ficheiro de certificado (normalmente, na pasta raiz). O ficheiro. crt contém o certificado público e a chave privada no formato PEM.

Extrair o thumbprint:
```
openssl x509 -in adls-dpreptestfiles.crt -noout -fingerprint
```

##### <a name="configure-an-azure-data-lake-storage-account-for-the-service-principal"></a>Configurar uma conta de armazenamento do Azure Data Lake para o principal de serviço
Para configurar a ACL para o sistema de ficheiros de armazenamento do Azure Data Lake, utilize o objectId do utilizador ou, neste exemplo, o principal de serviço:
```
az ad sp show --id "8dd38f34-1fcb-4ff9-accd-7cd60b757174" --query objectId
```
##### <a name="configure-read-and-execute-access-for-the-azure-data-lake-storage-file-system"></a>Configurar a leitura e executar o acesso para o sistema de ficheiros de armazenamento do Azure Data Lake.
Uma vez que o modelo de ACL de HDFS subjacente não suporta a herança, terá de configurar individualmente a ACL para pastas e ficheiros.
```
az dls fs access set-entry --account dpreptestfiles --acl-spec "user:e37b9b1f-6a5e-4bee-9def-402b956f4e6f:r-x" --path /
az dls fs access set-entry --account dpreptestfiles --acl-spec "user:e37b9b1f-6a5e-4bee-9def-402b956f4e6f:r--" --path /farmers-markets.csv
```
```
certThumbprint = 'C2:08:9D:9E:D1:74:FC:EB:E9:7E:63:96:37:1C:13:88:5E:B9:2C:84'
certificate = ''
with open('./data/adls-dpreptestfiles.crt', 'rt', encoding='utf-8') as crtFile:
    certificate = crtFile.read()

servicePrincipalAppId = "8dd38f34-1fcb-4ff9-accd-7cd60b757174"
```
#### <a name="acquire-an-oauth-access-token"></a>Adquirir um token de acesso de OAuth
Utilize o `adal` pacote (por meio de: `pip install adal`) para criar um contexto de autenticação no inquilino MSFT e adquirir um token de acesso de OAuth. Para ADLS, o recurso no pedido de token tem de ser para 'https://datalake.azure.net', que é diferente da maioria dos outros recursos do Azure.

```
import adal
from azureml.dataprep.api.datasources import DataLakeDataSource

ctx = adal.AuthenticationContext('https://login.microsoftonline.com/microsoft.onmicrosoft.com')
token = ctx.acquire_token_with_client_certificate('https://datalake.azure.net/', servicePrincipalAppId, certificate, certThumbprint)
dataflow = dprep.read_csv(path = DataLakeDataSource(path='adl://dpreptestfiles.azuredatalakestore.net/farmers-markets.csv', accessToken=token['accessToken']))
dataflow.to_pandas_dataframe().head()
```

||FMID|MarketName|Site|Rua|city|Concelho|
|----|------|-----|----|----|----|----|
|0|1012063|Caledónia agricultores mercado associação - Danville|https://sites.google.com/site/caledoniafarmers... ||Danville|Caledónia|
|1|1011871|Stearns Homestead agricultores ' mercado|http://Stearnshomestead.com |6975 ridge estrada|Parma|Cuyahoga|
|2|1011878|Mercado de quilómetro 100|http://www.pfcmarkets.com |507 Harrison St|Kalamazoo|Kalamazoo|
|3|1009364|106 S. Main Street agricultores mercado|http://thetownofsixmile.wordpress.com/ |106 S. Main Street|Seis quilómetro|||
|4|1010691|10 de mercado de agricultores Steet Comunidade|http://agrimissouri.com/mo-grown/grodetail.php... |Rua 10th e Poplar|Lamar|Barton|

