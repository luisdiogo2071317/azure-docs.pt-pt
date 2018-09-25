---
title: Escrever dados com o Azure Machine Learning dados Prep SDK - Python
description: Saiba mais sobre a escrita de dados com o SDK do Azure Machine Learning Data Prep. Pode escrever os dados em qualquer ponto num fluxo de dados e aos ficheiros em qualquer um dos nossos localizações suportadas (sistema de arquivos local, armazenamento de Blobs do Azure e armazenamento do Azure Data Lake).
services: machine-learning
ms.service: machine-learning
ms.component: core
ms.topic: conceptual
ms.author: cforbe
author: cforbe
manager: cgronlun
ms.reviewer: jmartens
ms.date: 09/24/2018
ms.openlocfilehash: da5823473b7f69fa0a6c65d5ea7319bfd2e92720
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/24/2018
ms.locfileid: "46946770"
---
# <a name="write-data-with-the-azure-machine-learning-data-prep-sdk"></a>Escrever dados com o SDK de preparação de dados do Azure Machine Learning
Pode escrever os dados em qualquer ponto num fluxo de dados. Essas escritas são adicionadas como passos para o fluxo de dados resultante e são executadas sempre que é o fluxo de dados. Uma vez que existem sem limites de quantos escrever passos existem num pipeline, é fácil escrever os resultados intermediários para resolução de problemas ou a ser escolhidas por outros pipelines. É importante observar que a execução de cada passo de escrita resulta numa solicitação completa dos dados no fluxo de dados. Por exemplo, um fluxo de dados com três passos de escrita irá ler e processar todos os registos no conjunto de dados três vezes.

## <a name="writing-to-files"></a>Gravar em arquivos
Com o [SDK do Azure Machine Learning Data Prep](https://docs.microsoft.com/python/api/overview/azure/dataprep?view=azure-dataprep-py), pode escrever dados em ficheiros em qualquer um dos nossos localizações suportadas (sistema de arquivos local, armazenamento de Blobs do Azure e armazenamento do Azure Data Lake). Dados são gravados para vários ficheiros de partição para permitir que escritas paralelas. Quando uma gravação for concluída, um arquivo de sentinel com êxito o nome também é gerado. Isto ajuda a identificar quando uma gravação de intermediária for concluído sem ter de esperar por todo o pipeline concluir.

Ao executar um fluxo de dados no Spark, deve escrever para uma pasta vazia. a tentar executar uma gravação para uma pasta existente irá falhar. Certificar-se de que sua pasta de destino está vazia ou utilizar uma localização de destino diferente para cada execução, ou a gravação irá falhar.

O SDK de Prep de dados do Azure Machine Learning suporta os seguintes formatos de arquivo:
-   Ficheiros delimitados (CSV, TSV, etc.)
-   Ficheiros parquet

Neste exemplo, comece por carregar os dados num fluxo de dados. Podemos irá reutilizar esses dados com diferentes formatos.

```

import azureml.dataprep as dprep
t = dprep.smart_read_file('./data/fixed_width_file.txt')
t = t.to_number('Column3')
t.head(10)
```   

|   |  Column1 |    Column2 | Column3 | Column4  |Column5   | Column6 | Column7 | Column8 | Column9 |
| -------- |  -------- | -------- | -------- |  -------- |  -------- |  -------- |  -------- |  -------- |  -------- |
| 0 |   10000.0 |   99999.0 |   Nenhuma|       NÃO|     NÃO  |   ENRS    |NaN    |   NaN |   NaN|    
|   1|      10003.0 |   99999.0 |   Nenhuma|       NÃO|     NÃO  |   ENSO|       NaN|        NaN |NaN|   
|   2|  10010.0|    99999.0|    Nenhuma|   NÃO| JN| ENJA|   70933.0|    -8667.0 |90,0|
|3| 10013.0|    99999.0|    Nenhuma|   NÃO| NÃO| |   NaN|    NaN|    NaN|
|4| 10014.0|    99999.0|    Nenhuma|   NÃO| NÃO| ENSO|   59783.0|    5350.0| 500.0|
|5| 10015.0|    99999.0|    Nenhuma|   NÃO| NÃO| ENBL|   61383.0|    5867.0| 3270.0|
|6| 10016.0 |99999.0|   Nenhuma|   NÃO| NÃO|     |64850.0|   11233.0|    140.0|
|7| 10017.0|    99999.0|    Nenhuma|   NÃO| NÃO| ENFR|   59933.0|    2417.0| 480.0|
|8| 10020.0|    99999.0|    Nenhuma|   NÃO| SV|     |80050.0|   16250.0|    80.0|
|9| 10030.0|    99999.0|    Nenhuma|   NÃO| SV|     |77000.0|   15500.0|    120.0|

## <a name="delimited-files"></a>Ficheiros delimitados
A linha abaixo cria um novo fluxo de dados com um passo de escrita, mas a gravação real ainda não tenha ocorrido. Quando os dados de execuções de fluxo, irá executar a operação de escrita.

```
write_t = t.write_to_csv(directory_path=dprep.LocalFileOutput('./test_out/'))
```
Agora, execute o fluxo de dados, que executa a operação de escrita.
```
write_t.run_local()

written_files = dprep.read_csv('./test_out/part-*')
written_files.head(10)
```
|   |  Column1 |    Column2 | Column3 | Column4  |Column5   | Column6 | Column7 | Column8 | Column9 |
| -------- |  -------- | -------- | -------- |  -------- |  -------- |  -------- |  -------- |  -------- |  -------- |
| 0 |   10000.0 |   99999.0 |   ERRO |       NÃO|     NÃO  |   ENRS    |ERRO    |   ERRO |   ERRO|    
|   1|      10003.0 |   99999.0 |   ERRO |       NÃO|     NÃO  |   ENSO|       ERRO|        ERRO |ERRO|   
|   2|  10010.0|    99999.0|    ERRO |   NÃO| JN| ENJA|   70933.0|    -8667.0 |90,0|
|3| 10013.0|    99999.0|    ERRO |   NÃO| NÃO| |   ERRO|    ERRO|    ERRO|
|4| 10014.0|    99999.0|    ERRO |   NÃO| NÃO| ENSO|   59783.0|    5350.0| 500.0|
|5| 10015.0|    99999.0|    ERRO |   NÃO| NÃO| ENBL|   61383.0|    5867.0| 3270.0|
|6| 10016.0 |99999.0|   ERRO |   NÃO| NÃO|     |64850.0|   11233.0|    140.0|
|7| 10017.0|    99999.0|    ERRO |   NÃO| NÃO| ENFR|   59933.0|    2417.0| 480.0|
|8| 10020.0|    99999.0|    ERRO |   NÃO| SV|     |80050.0|   16250.0|    80.0|
|9| 10030.0|    99999.0|    ERRO |   NÃO| SV|     |77000.0|   15500.0|    120.0|

Os dados escritos contém vários erros nas colunas numéricas por causa de números que não foram analisados corretamente. Quando escritos para CSV, estes valores nulos são substituídos com a cadeia de caracteres "ERROR" por predefinição. Pode adicionar parâmetros como parte da sua escrita chamarem e especifique uma cadeia de caracteres a utilizar para representar valores nulos.

```
write_t = t.write_to_csv(directory_path=dprep.LocalFileOutput('./test_out/'), 
                         error='BadData',
                         na='NA')
write_t.run_local()
written_files = dprep.read_csv('./test_out/part-*')
written_files.head(10)
```
|   |  Column1 |    Column2 | Column3 | Column4  |Column5   | Column6 | Column7 | Column8 | Column9 |
| -------- |  -------- | -------- | -------- |  -------- |  -------- |  -------- |  -------- |  -------- |  -------- |
| 0 |   10000.0 |   99999.0 |   BadData |       NÃO|     NÃO  |   ENRS    |BadData    |   BadData |   BadData|    
|   1|      10003.0 |   99999.0 |   BadData |       NÃO|     NÃO  |   ENSO|       BadData|        BadData |BadData|   
|   2|  10010.0|    99999.0|    BadData |   NÃO| JN| ENJA|   70933.0|    -8667.0 |90,0|
|3| 10013.0|    99999.0|    BadData |   NÃO| NÃO| |   BadData|    BadData|    BadData|
|4| 10014.0|    99999.0|    BadData |   NÃO| NÃO| ENSO|   59783.0|    5350.0| 500.0|
|5| 10015.0|    99999.0|    BadData |   NÃO| NÃO| ENBL|   61383.0|    5867.0| 3270.0|
|6| 10016.0 |99999.0|   BadData |   NÃO| NÃO|     |64850.0|   11233.0|    140.0|
|7| 10017.0|    99999.0|    BadData |   NÃO| NÃO| ENFR|   59933.0|    2417.0| 480.0|
|8| 10020.0|    99999.0|    BadData |   NÃO| SV|     |80050.0|   16250.0|    80.0|
|9| 10030.0|    99999.0|    BadData |   NÃO| SV|     |77000.0|   15500.0|    120.0|
## <a name="parquet-files"></a>Ficheiros parquet

 Da mesma forma para o `write_to_csv` função acima, `write_to_parquet` retorna um novo fluxo de dados com um passo de Parquet que será executado quando as execuções de fluxo de dados de escrita.

```
write_parquet_t = t.write_to_parquet(directory_path=dprep.LocalFileOutput('./test_parquet_out/'),
error='MiscreantData')
```
 Agora, podemos executar o fluxo de dados, que executa a operação de escrita.

```
write_parquet_t.run_local()

written_parquet_files = dprep.read_parquet_file('./test_parquet_out/part-*')
written_parquet_files.head(10)
```
|   |  Column1 |    Column2 | Column3 | Column4  |Column5   | Column6 | Column7 | Column8 | Column9 |
| -------- |  -------- | -------- | -------- |  -------- |  -------- |  -------- |  -------- |  -------- |  -------- |
| 0 |   10000.0 |   99999.0 |   MiscreantData |       NÃO|     NÃO  |   ENRS    |MiscreantData    |   MiscreantData |   MiscreantData|    
|   1|      10003.0 |   99999.0 |   MiscreantData |       NÃO|     NÃO  |   ENSO|       MiscreantData|        MiscreantData |MiscreantData|   
|   2|  10010.0|    99999.0|    MiscreantData |   NÃO| JN| ENJA|   70933.0|    -8667.0 |90,0|
|3| 10013.0|    99999.0|    MiscreantData |   NÃO| NÃO| |   MiscreantData|    MiscreantData|    MiscreantData|
|4| 10014.0|    99999.0|    MiscreantData |   NÃO| NÃO| ENSO|   59783.0|    5350.0| 500.0|
|5| 10015.0|    99999.0|    MiscreantData |   NÃO| NÃO| ENBL|   61383.0|    5867.0| 3270.0|
|6| 10016.0 |99999.0|   MiscreantData |   NÃO| NÃO|     |64850.0|   11233.0|    140.0|
|7| 10017.0|    99999.0|    MiscreantData |   NÃO| NÃO| ENFR|   59933.0|    2417.0| 480.0|
|8| 10020.0|    99999.0|    MiscreantData |   NÃO| SV|     |80050.0|   16250.0|    80.0|
|9| 10030.0|    99999.0|    MiscreantData |   NÃO| SV|     |77000.0|   15500.0|    120.0|
