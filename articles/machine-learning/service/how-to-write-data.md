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
ms.openlocfilehash: 4a2af832fda8a85ee8a4aba395a8f436172153ed
ms.sourcegitcommit: a08d1236f737915817815da299984461cc2ab07e
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 11/26/2018
ms.locfileid: "52308567"
---
# <a name="write-data-using-the-azure-machine-learning-data-prep-sdk"></a>Escrever os dados com o SDK de Prep de dados do Azure Machine Learning

Neste artigo, vai aprender diferentes métodos para escrever dados com o SDK do Azure Machine Learning Data Prep. Dados de saída podem ser escritos em qualquer um fluxo de dados e gravações são adicionadas como passos para o fluxo de dados resultante e são executadas sempre que é o fluxo de dados. Dados são gravados para vários ficheiros de partição para permitir que escritas paralelas.

Uma vez que existem sem limites de quantos escrever passos existem num pipeline, pode adicionar facilmente passos adicionais de escrita para obter os resultados intermediários para resolução de problemas ou para outros pipelines.

Sempre que executar uma etapa de escrita, ocorre uma solicitação completa dos dados no fluxo de dados. Por exemplo, um fluxo de dados com três passos de escrita irá ler e processar todos os registos no conjunto de dados três vezes.

## <a name="supported-data-types-and-location"></a>Tipos de dados suportados e localização

São suportados os seguintes formatos de arquivo
-   Ficheiros delimitados (CSV, TSV, etc.)
-   Ficheiros parquet

Utilizar o [SDK de python de preparação de dados do Azure Machine Learning](https://aka.ms/data-prep-sdk), pode escrever dados para:
+ um sistema de ficheiros local
+ Armazenamento de Blobs do Azure
+ Armazenamento do Azure Data Lake

## <a name="spark-considerations"></a>Considerações sobre o Spark

Ao executar um fluxo de dados no Spark, deve escrever para uma pasta vazia. Tentar executar uma gravação para um resulta de pasta existente numa falha. Certificar-se de que sua pasta de destino está vazia ou utilizar uma localização de destino diferente para cada execução, ou a gravação irá falhar.

## <a name="monitoring-write-operations"></a>Monitorização de operações de escrita

Para sua comodidade, um arquivo de sentinel com êxito o nome é gerado quando uma gravação for concluída. Sua presença ajuda a identificar quando uma gravação de intermediária for concluído sem ter de esperar por todo o pipeline concluir.

## <a name="example-write-code"></a>Código de escrita de exemplo

Neste exemplo, comece por carregar os dados num fluxo de dados. Reutilizar esses dados com diferentes formatos.

```python
import azureml.dataprep as dprep
t = dprep.smart_read_file('./data/fixed_width_file.txt')
t = t.to_number('Column3')
t.head(10)
```

Exemplo de saída:
|   |  Column1 |    Column2 | Column3 | Column4  |Column5   | Column6 | Column7 | Column8 | Column9 |
| -------- |  -------- | -------- | -------- |  -------- |  -------- |  -------- |  -------- |  -------- |  -------- |
| 0 |   10000.0 |   99999.0 |   Nenhuma|       NO|     NO  |   ENRS    |NaN    |   NaN |   NaN|    
|   1|      10003.0 |   99999.0 |   Nenhuma|       NO|     NO  |   ENSO|       NaN|        NaN |NaN|   
|   2|  10010.0|    99999.0|    Nenhuma|   NO| JN| ENJA|   70933.0|    -8667.0 |90,0|
|3| 10013.0|    99999.0|    Nenhuma|   NO| NO| |   NaN|    NaN|    NaN|
|4| 10014.0|    99999.0|    Nenhuma|   NO| NO| ENSO|   59783.0|    5350.0| 500.0|
|5| 10015.0|    99999.0|    Nenhuma|   NO| NO| ENBL|   61383.0|    5867.0| 3270.0|
|6| 10016.0 |99999.0|   Nenhuma|   NO| NO|     |64850.0|   11233.0|    140.0|
|7| 10017.0|    99999.0|    Nenhuma|   NO| NO| ENFR|   59933.0|    2417.0| 480.0|
|8| 10020.0|    99999.0|    Nenhuma|   NO| SV|     |80050.0|   16250.0|    80.0|
|9| 10030.0|    99999.0|    Nenhuma|   NO| SV|     |77000.0|   15500.0|    120.0|

### <a name="delimited-file-example"></a>Exemplo de ficheiro delimitado

O seguinte código utiliza o `write_to_csv` função para escrever dados num arquivo delimitado.

```python
# Create a new data flow using `write_to_csv` 
write_t = t.write_to_csv(directory_path=dprep.LocalFileOutput('./test_out/'))

# Run the data flow to begin the write operation.
write_t.run_local()

written_files = dprep.read_csv('./test_out/part-*')
written_files.head(10)
```

Exemplo de saída:
|   |  Column1 |    Column2 | Column3 | Column4  |Column5   | Column6 | Column7 | Column8 | Column9 |
| -------- |  -------- | -------- | -------- |  -------- |  -------- |  -------- |  -------- |  -------- |  -------- |
| 0 |   10000.0 |   99999.0 |   ERRO |       NO|     NO  |   ENRS    |ERRO    |   ERRO |   ERRO|    
|   1|      10003.0 |   99999.0 |   ERRO |       NO|     NO  |   ENSO|       ERRO|        ERRO |ERRO|   
|   2|  10010.0|    99999.0|    ERRO |   NO| JN| ENJA|   70933.0|    -8667.0 |90,0|
|3| 10013.0|    99999.0|    ERRO |   NO| NO| |   ERRO|    ERRO|    ERRO|
|4| 10014.0|    99999.0|    ERRO |   NO| NO| ENSO|   59783.0|    5350.0| 500.0|
|5| 10015.0|    99999.0|    ERRO |   NO| NO| ENBL|   61383.0|    5867.0| 3270.0|
|6| 10016.0 |99999.0|   ERRO |   NO| NO|     |64850.0|   11233.0|    140.0|
|7| 10017.0|    99999.0|    ERRO |   NO| NO| ENFR|   59933.0|    2417.0| 480.0|
|8| 10020.0|    99999.0|    ERRO |   NO| SV|     |80050.0|   16250.0|    80.0|
|9| 10030.0|    99999.0|    ERRO |   NO| SV|     |77000.0|   15500.0|    120.0|

Na saída do anterior, vários erros aparecem nas colunas numéricas por causa de números que não foram analisados corretamente. Quando escritos para CSV, os valores nulos são substituídos com a cadeia de caracteres "ERROR" por predefinição.

Adicione os parâmetros como parte da sua escrita chamarem e especifique uma cadeia de caracteres a utilizar para representar valores nulos.

```python
write_t = t.write_to_csv(directory_path=dprep.LocalFileOutput('./test_out/'), 
                         error='BadData',
                         na='NA')
write_t.run_local()
written_files = dprep.read_csv('./test_out/part-*')
written_files.head(10)
```

O código anterior produz esta saída:
|   |  Column1 |    Column2 | Column3 | Column4  |Column5   | Column6 | Column7 | Column8 | Column9 |
| -------- |  -------- | -------- | -------- |  -------- |  -------- |  -------- |  -------- |  -------- |  -------- |
| 0 |   10000.0 |   99999.0 |   BadData |       NO|     NO  |   ENRS    |BadData    |   BadData |   BadData|    
|   1|      10003.0 |   99999.0 |   BadData |       NO|     NO  |   ENSO|       BadData|        BadData |BadData|   
|   2|  10010.0|    99999.0|    BadData |   NO| JN| ENJA|   70933.0|    -8667.0 |90,0|
|3| 10013.0|    99999.0|    BadData |   NO| NO| |   BadData|    BadData|    BadData|
|4| 10014.0|    99999.0|    BadData |   NO| NO| ENSO|   59783.0|    5350.0| 500.0|
|5| 10015.0|    99999.0|    BadData |   NO| NO| ENBL|   61383.0|    5867.0| 3270.0|
|6| 10016.0 |99999.0|   BadData |   NO| NO|     |64850.0|   11233.0|    140.0|
|7| 10017.0|    99999.0|    BadData |   NO| NO| ENFR|   59933.0|    2417.0| 480.0|
|8| 10020.0|    99999.0|    BadData |   NO| SV|     |80050.0|   16250.0|    80.0|
|9| 10030.0|    99999.0|    BadData |   NO| SV|     |77000.0|   15500.0|    120.0|

### <a name="parquet-file-example"></a>Exemplo de ficheiro parquet

Semelhante à `write_to_csv`, o `write_to_parquet` função devolve um novo fluxo de dados com um passo de Parquet, que é executado quando as execuções de fluxo de dados de escrita.

```python
write_parquet_t = t.write_to_parquet(directory_path=dprep.LocalFileOutput('./test_parquet_out/'),
error='MiscreantData')
```

Execute o fluxo de dados para iniciar a operação de escrita.

```python
write_parquet_t.run_local()

written_parquet_files = dprep.read_parquet_file('./test_parquet_out/part-*')
written_parquet_files.head(10)
```

O código anterior produz esta saída:
|   |  Column1 |    Column2 | Column3 | Column4  |Column5   | Column6 | Column7 | Column8 | Column9 |
| -------- |  -------- | -------- | -------- |  -------- |  -------- |  -------- |  -------- |  -------- |  -------- |
| 0 |   10000.0 |   99999.0 |   MiscreantData |       NO|     NO  |   ENRS    |MiscreantData    |   MiscreantData |   MiscreantData|    
|   1|      10003.0 |   99999.0 |   MiscreantData |       NO|     NO  |   ENSO|       MiscreantData|        MiscreantData |MiscreantData|   
|   2|  10010.0|    99999.0|    MiscreantData |   NO| JN| ENJA|   70933.0|    -8667.0 |90,0|
|3| 10013.0|    99999.0|    MiscreantData |   NO| NO| |   MiscreantData|    MiscreantData|    MiscreantData|
|4| 10014.0|    99999.0|    MiscreantData |   NO| NO| ENSO|   59783.0|    5350.0| 500.0|
|5| 10015.0|    99999.0|    MiscreantData |   NO| NO| ENBL|   61383.0|    5867.0| 3270.0|
|6| 10016.0 |99999.0|   MiscreantData |   NO| NO|     |64850.0|   11233.0|    140.0|
|7| 10017.0|    99999.0|    MiscreantData |   NO| NO| ENFR|   59933.0|    2417.0| 480.0|
|8| 10020.0|    99999.0|    MiscreantData |   NO| SV|     |80050.0|   16250.0|    80.0|
|9| 10030.0|    99999.0|    MiscreantData |   NO| SV|     |77000.0|   15500.0|    120.0|
