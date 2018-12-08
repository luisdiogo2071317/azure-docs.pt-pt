---
title: Transformar e de preparação de dados com o SDK de Python de preparação de dados
titleSuffix: Azure Machine Learning service
description: Saiba mais sobre a transformar e limpeza de dados com o SDK do Azure Machine Learning Data Prep. Utilize métodos de transformação para adicionar colunas, filtrar indesejadas linhas ou colunas e impute valores em falta.
services: machine-learning
ms.service: machine-learning
ms.component: core
ms.topic: conceptual
ms.author: cforbe
author: cforbe
manager: cgronlun
ms.reviewer: jmartens
ms.date: 12/04/2018
ms.custom: seodec18
ms.openlocfilehash: c734605cd67fcd2490aef0ddc6732dad112424b7
ms.sourcegitcommit: 9fb6f44dbdaf9002ac4f411781bf1bd25c191e26
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/08/2018
ms.locfileid: "53101794"
---
# <a name="transform-data-with-the-azure-machine-learning-data-prep-sdk"></a>Transformar dados com o SDK de preparação de dados do Azure Machine Learning

Neste artigo, vai aprender diferentes métodos de carregamento de dados com o [SDK do Azure Machine Learning Data Prep](https://aka.ms/data-prep-sdk). O SDK oferece funções tornam mais fácil adicionar colunas, filtrar indesejadas linhas ou colunas e impute valores em falta.

Atualmente, existem funções para as seguintes tarefas:

- [Adicionar coluna utilização de uma expressão](#column)
- [Impute valores em falta](#impute-missing-values)
- [Derivar coluna por exemplo](#derive-column-by-example)
- [Filtragem](#filtering)
- [Transformações de Python personalizadas](#custom-python-transforms)

## <a name="add-column-using-an-expression"></a>Adicionar coluna utilização de uma expressão

Inclui o SDK do Azure Machine Learning Data Prep `substring` expressões, pode utilizar para calcular um valor de colunas existentes e, em seguida, colocar esse valor numa nova coluna. Neste exemplo, carregar dados e tente adicionar colunas a esses dados de entrada.

```python
import azureml.dataprep as dprep

# loading data
dataflow = dprep.read_csv(path=r'data\crime0-10.csv')
dataflow.head(3)
```

||ID|Número de caso|Date|Bloquear|IUCR|Tipo de principal|Descrição|Descrição de localização|Prisão|Nacionais|...|Ward|Área de Comunidade|Código do FBI|Coordenada x|Coordenada Y|Ano|Atualizado em|Latitude|Longitude|Localização|
|-------|-------|-------|-------|-------|-------|-------|-------|-------|-------|-------|-------|-------|-------|-------|-------|-------|-------|-------|-------|-------|-------|
|0|10140490|HY329907|05/07/2015 50: 11:00 PM|050XX N NEWLAND AVE|0820|ROUBO|US $500 E, EM|RUA|false|false|...|41|10|06|1129230|1933315|2015|12/07/2015 |42:46 12:00 DE|41.973309466|-87.800174996|(41.973309466,-87.800174996)|
|1|10139776|HY329265|05/07/2015 30: 11:00 PM|011XX W MORSE AVE|0460|BATERIA|SIMPLES|RUA|false|true|...|49|1|08B|1167370|1946271|2015|12/07/2015 42:46 12:00 DE|42.008124017|-87.65955018|(42.008124017,-87.65955018)|
|2|10140270|HY329253|05/07/2015 20: 11:00 PM|121XX S FRONT AVE|0486|BATERIA|BATERIA NACIONAL SIMPLE|RUA|false|true|...|9|53|08B|||2015|12/07/2015 42:46 12:00 DE|


Utilize o `substring(start, length)` expressão para extrair o prefixo da coluna de número de caso e colocar essa cadeia de caracteres numa nova coluna, `Case Category`. Passando o `substring_expression` variável para o `expression` parâmetro cria uma nova coluna calculada que executa a expressão em cada registo.

```python
substring_expression = dprep.col('Case Number').substring(0, 2)
case_category = dataflow.add_column(new_column_name='Case Category',
                                    prior_column='Case Number',
                                    expression=substring_expression)
case_category.head(3)
```

||ID|Número de caso|Categoria de caso|Date|Bloquear|IUCR|Tipo de principal|Descrição|Descrição de localização|Prisão|...|Ward|Área de Comunidade|Código do FBI|Coordenada x|Coordenada Y|Ano|Atualizado em|Latitude|Longitude|Localização|
|-------|-------|-------|-------|-------|-------|-------|-------|-------|-------|-------|-------|-------|-------|-------|-------|-------|-------|-------|-------|-------|-------|------|
|0|10140490|HY329907|HY|05/07/2015 50: 11:00 PM|050XX N NEWLAND AVE|0820|ROUBO|US $500 E, EM|RUA|false|false|...|41|10|06|1129230|1933315|2015|12/07/2015 |42:46 12:00 DE|41.973309466|-87.800174996|(41.973309466,-87.800174996)|
|1|10139776|HY329265|HY|05/07/2015 30: 11:00 PM|011XX W MORSE AVE|0460|BATERIA|SIMPLES|RUA|false|true|...|49|1|08B|1167370|1946271|2015|12/07/2015 42:46 12:00 DE|42.008124017|-87.65955018|(42.008124017,-87.65955018)|
|2|10140270|HY329253|HY|05/07/2015 20: 11:00 PM|121XX S FRONT AVE|0486|BATERIA|BATERIA NACIONAL SIMPLE|RUA|false|true|...|9|53|08B|||2015|12/07/2015 42:46 12:00 DE|



Utilize o `substring(start)` expressão para extrair apenas o número da coluna de número de caso e criar uma nova coluna. Convertê-lo para um tipo de dados numéricos com o `to_number()` funcionar e passar o nome da coluna de cadeia de caracteres como um parâmetro.

```python
substring_expression2 = dprep.col('Case Number').substring(2)
case_id = dataflow.add_column(new_column_name='Case Id',
                              prior_column='Case Number',
                              expression=substring_expression2)
case_id = case_id.to_number('Case Id')
case_id.head(3)
```

||ID|Número de caso|Id de incidente|Date|Bloquear|IUCR|Tipo de principal|Descrição|Descrição de localização|Prisão|...|Ward|Área de Comunidade|Código do FBI|Coordenada x|Coordenada Y|Ano|Atualizado em|Latitude|Longitude|Localização|
|-------|-------|-------|-------|-------|-------|-------|-------|-------|-------|-------|-------|-------|-------|-------|-------|-------|-------|-------|-------|-------|-------|------|
|0|10140490|HY329907|329907.0|05/07/2015 50: 11:00 PM|050XX N NEWLAND AVE|0820|ROUBO|US $500 E, EM|RUA|false|false|...|41|10|06|1129230|1933315|2015|12/07/2015 |42:46 12:00 DE|41.973309466|-87.800174996|(41.973309466,-87.800174996)|
|1|10139776|HY329265|329265.0|05/07/2015 30: 11:00 PM|011XX W MORSE AVE|0460|BATERIA|SIMPLES|RUA|false|true|...|49|1|08B|1167370|1946271|2015|12/07/2015 42:46 12:00 DE|42.008124017|-87.65955018|(42.008124017,-87.65955018)|
|2|10140270|HY329253|329253.0|05/07/2015 20: 11:00 PM|121XX S FRONT AVE|0486|BATERIA|BATERIA NACIONAL SIMPLE|RUA|false|true|...|9|53|08B|||2015|12/07/2015 42:46 12:00 DE|

## <a name="impute-missing-values"></a>Impute valores em falta

O SDK pode impute valores em falta nas colunas especificadas. Neste exemplo, valores de latitude e longitude de carga e, em seguida, tente impute valores em falta nos dados de entrada.

```python
import azureml.dataprep as dprep

# loading input data
df = dprep.read_csv(r'data\crime0-10.csv')
df = df.keep_columns(['ID', 'Arrest', 'Latitude', 'Longitude'])
df = df.to_number(['Latitude', 'Longitude'])
df.head(5)
```

||ID|Prisão|Latitude|Longitude|
|-----|------|-----|------|-----|
|0|10140490|false|41.973309|-87.800175|
|1|10139776|false|42.008124|-87.659550|
|2|10140270|false|NaN|NaN|
|3|10139885|false|41.902152|-87.754883|
|4|10140379|false|41.885610|-87.657009|

O registo de terceiro está em falta valores de latitude e longitude. Para impute esses valores em falta, use `ImputeMissingValuesBuilder` para obter uma expressão fixa. Ele pode impute as colunas com qualquer um de um calculado `MIN`, `MAX`, `MEAN` valor, ou um `CUSTOM` valor. Quando `group_by_columns` for especificado, valores em falta vão ser imputed ao grupo com `MIN`, `MAX`, e `MEAN` calculado por grupo.

Verifique os `MEAN` valor de uso de coluna de latitude o `summarize()` função. Esta função aceita uma matriz de colunas no `group_by_columns` parâmetro para especificar o nível de agregação. O `summary_columns` parâmetro aceita um `SummaryColumnsValue` chamar. Esta chamada de função Especifica o nome da coluna atual, o novo nome de campo calculado e o `SummaryFunction` para executar.

```python
df_mean = df.summarize(group_by_columns=['Arrest'],
                       summary_columns=[dprep.SummaryColumnsValue(column_id='Latitude',
                                                                 summary_column_name='Latitude_MEAN',
                                                                 summary_function=dprep.SummaryFunction.MEAN)])
df_mean = df_mean.filter(dprep.col('Arrest') == 'false')
df_mean.head(1)
```

||Prisão|Latitude_MEAN|
|-----|-----|----|
|0|false|41.878961|

O `MEAN` valor de latitudes parece preciso, utilize o `ImputeColumnArguments` função para impute-lo. Esta função aceita um `column_id` cadeia de caracteres e um `ReplaceValueFunction` para especificar o tipo de impute. Para o valor de longitude em falta, impute-lo com 42 com base nos dados de conhecimento externos.

Impute passos podem ser encadeados num `ImputeMissingValuesBuilder` objeto, usando a função de construtor `impute_missing_values()`. O `impute_columns` parâmetro aceita uma matriz de `ImputeColumnArguments` objetos. Chamar o `learn()` funcionar para armazenar os passos de impute e, em seguida, aplicar a um objeto de fluxo de dados utilizando `to_dataflow()`.

```python
# impute with MEAN
impute_mean = dprep.ImputeColumnArguments(column_id='Latitude',
                                          impute_function=dprep.ReplaceValueFunction.MEAN)
# impute with custom value 42
impute_custom = dprep.ImputeColumnArguments(column_id='Longitude',
                                            custom_impute_value=42)
# get instance of ImputeMissingValuesBuilder
impute_builder = df.builders.impute_missing_values(impute_columns=[impute_mean, impute_custom],
                                                   group_by_columns=['Arrest'])
# call learn() to learn a fixed program to impute missing values
impute_builder.learn()
# call to_dataflow() to get a data flow with impute step added
df_imputed = impute_builder.to_dataflow()

# check impute result
df_imputed.head(5)
```

||ID|Prisão|Latitude|Longitude|
|-----|------|-----|------|-----|
|0|10140490|false|41.973309|-87.800175|
|1|10139776|false|42.008124|-87.659550|
|2|10140270|false|41.878961|42.000000|
|3|10139885|false|41.902152|-87.754883|
|4|10140379|false|41.885610|-87.657009|

Conforme mostrado no resultado acima, a latitude em falta foi imputed com o `MEAN` valor de `Arrest=='false'` grupo. A longitude em falta foi imputed com 42.

```python
imputed_longitude = df_imputed.to_pandas_dataframe()['Longitude'][2]
assert imputed_longitude == 42
```

## <a name="derive-column-by-example"></a>Derivar coluna por exemplo

Uma das ferramentas mais avançadas no SDK de Prep de dados do Azure Machine Learning é a capacidade de derivar de colunas com exemplos de resultados desejados. Isto permite-lhe dar um exemplo SDK para que ele pode gerar o código para alcançar a transformação pretendida.

```python
import azureml.dataprep as dprep
dataflow = dprep.read_csv(path='https://dpreptestfiles.blob.core.windows.net/testfiles/BostonWeather.csv')
dataflow.head(10)
```

||DATA|REPORTTPYE|HOURLYDRYBULBTEMPF|HOURLYRelativeHumidity|HOURLYWindSpeed|
|----|----|----|----|----|----|
|0|1/1/2015 0:54|FM-15|22|50|10|
|1|1/1/2015 1:00|FM-12|22|50|10|
|2|1/1/2015 1 minuto e 54|FM-15|22|50|10|
|3|1/1/2015 2 minutos e 54|FM-15|22|50|11|
|4|1/1/2015 3 minutos e 54|FM-15|24|46|13|
|5|1/1/2015 4:00|FM-12|24|46|13|
|6|1/1/2015 4 minutos e 54|FM-15|22|52|15|
|7|1/1/2015 5 minutos e 54|FM-15|23|48|17|
|8|1/1/2015 6 minutos e 54|FM-15|23|50|14|
|9|1/1/2015 7:00|FM-12|23|50|14|

Partem do princípio de que precisa de associar este ficheiro com um conjunto de dados em que a data e hora são em formato de "10 de Março de 2018 | 2AM -4AM '.

```python
builder = dataflow.builders.derive_column_by_example(source_columns=['DATE'], new_column_name='date_timerange')
builder.add_example(source_data=df.iloc[1], example_value='Jan 1, 2015 12AM-2AM')
builder.preview() 
```

||DATA|date_timerange|
|----|----|----|
|0|1/1/2015 0:54|1 de Janeiro de 2015 12AM -2AM|
|1|1/1/2015 1:00|1 de Janeiro de 2015 12AM -2AM|
|2|1/1/2015 1 minuto e 54|1 de Janeiro de 2015 12AM -2AM|
|3|1/1/2015 2 minutos e 54|1 de Janeiro de 2015 2AM -4AM|
|4|1/1/2015 3 minutos e 54|1 de Janeiro de 2015 2AM -4AM|
|5|1/1/2015 4:00|1 de Janeiro de 2015 4 AM - 6 da Manhã|
|6|1/1/2015 4 minutos e 54|1 de Janeiro de 2015 4 AM - 6 da Manhã|
|7|1/1/2015 5 minutos e 54|1 de Janeiro de 2015 4 AM - 6 da Manhã|
|8|1/1/2015 6 minutos e 54|1 de Janeiro de 2015 6 da Manhã - 8 AM|
|9|1/1/2015 7:00|1 de Janeiro de 2015 6 da Manhã - 8 AM|

O código acima cria primeiro um construtor para a coluna derivada. Fornece uma matriz de colunas de origem a ter em consideração (`DATE`) e um nome para a nova coluna a adicionar. Como o primeiro exemplo, passar na segunda linha (índice 1) e fornecer um valor esperado para a coluna derivada.

Finalmente, chama `builder.preview()` e pode ver a coluna derivada junto a coluna de origem. O formato parece correto, mas apenas verá valores para a mesma data "1 de Janeiro de 2015".

Agora, passar o número de linhas que quer `skip` da parte superior para ver as linhas mais abaixo.

```
builder.preview(skip=30)
```

||DATA|date_timerange|
|-----|-----|-----|
|30|1/1/2015 22 minutos e 54|1 de Janeiro de 2015 10 PM - 12 AM|
|31|1/1/2015 23 minutos e 54|1 de Janeiro de 2015 10 PM - 12 AM|
|32|1/1/2015 23:59|1 de Janeiro de 2015 10 PM - 12 AM|
|33|1/2/2015 0:54|1 de Fevereiro de 2015 12AM -2AM|
|34|1/2/2015 1:00|1 de Fevereiro de 2015 12AM -2AM|
|35|1/2/2015 1 minuto e 54|1 de Fevereiro de 2015 12AM -2AM|
|36|1/2/2015 2 minutos e 54|1 de Fevereiro de 2015 2AM -4AM|
|37|1/2/2015 3 minutos e 54|1 de Fevereiro de 2015 2AM -4AM|
|38|1/2/2015 4:00|1 de Fevereiro de 2015 4 AM - 6 da Manhã|
|39|1/2/2015 4 minutos e 54|1 de Fevereiro de 2015 4 AM - 6 da Manhã|

Aqui vê um problema com o programa gerado. Com base apenas num dos exemplos fornecidos acima, o programa de derivar optou por analisar a data como "Dia/mês/ano", que é que não o que deseja nesse caso. Para corrigir este problema, indique outro exemplo usando o `add_example()` funcionar no `builder` variável.

```python
builder.add_example(source_data=preview_df.iloc[3], example_value='Jan 2, 2015 12AM-2AM')
builder.preview(skip=30, count=10)
```

||DATA|date_timerange|
|-----|-----|-----|
|30|1/1/2015 22 minutos e 54|1 de Janeiro de 2015 10 PM - 12 AM|
|31|1/1/2015 23 minutos e 54|1 de Janeiro de 2015 10 PM - 12 AM|
|32|1/1/2015 23:59|1 de Janeiro de 2015 10 PM - 12 AM|
|33|1/2/2015 0:54|2 de Janeiro de 2015 12AM -2AM|
|34|1/2/2015 1:00|2 de Janeiro de 2015 12AM -2AM|
|35|1/2/2015 1 minuto e 54|2 de Janeiro de 2015 12AM -2AM|
|36|1/2/2015 2 minutos e 54|2 de Janeiro de 2015 2AM -4AM|
|37|1/2/2015 3 minutos e 54|2 de Janeiro de 2015 2AM -4AM|
|38|1/2/2015 4:00|2 de Janeiro de 2015 4 AM - 6 da Manhã|
|39|1/2/2015 4 minutos e 54|2 de Janeiro de 2015 4 AM - 6 da Manhã|

Agora a manipulação correta de linhas ' 1/2/2015' como "2 de Jan de 2015', mas se examinar mais abaixo da coluna derivada, deverá ver que valores no final tem nada na coluna derivada. Para corrigi-lo, terá de fornecer outro exemplo de linha 66.

```python
builder.add_example(source_data=preview_df.iloc[66], example_value='Jan 29, 2015 8PM-10PM')
builder.preview(count=10)
```

||DATA|date_timerange|
|-----|-----|-----|
|0|1/1/2015 22 minutos e 54|1 de Janeiro de 2015 10 PM - 12 AM|
|1|1/1/2015 23 minutos e 54|1 de Janeiro de 2015 10 PM - 12 AM|
|2|1/1/2015 23:59|1 de Janeiro de 2015 10 PM - 12 AM|
|3|1/2/2015 0:54|2 de Janeiro de 2015 12AM -2AM|
|4|1/2/2015 1:00|2 de Janeiro de 2015 12AM -2AM|
|5|1/2/2015 1 minuto e 54|2 de Janeiro de 2015 12AM -2AM|
|6|1/2/2015 2 minutos e 54|2 de Janeiro de 2015 2AM -4AM|
|7|1/2/2015 3 minutos e 54|2 de Janeiro de 2015 2AM -4AM|
|8|1/2/2015 4:00|2 de Janeiro de 2015 4 AM - 6 da Manhã|
|9|1/2/2015 4 minutos e 54|2 de Janeiro de 2015 4 AM - 6 da Manhã|

Separar a data e hora com "|", adiciona outro exemplo. Desta vez, em vez de passar numa linha da pré-visualização, construir um dicionário de nome de coluna para o valor para o `source_data` parâmetro.

```python
builder.add_example(source_data={'DATE': '11/11/2015 0:54'}, example_value='Nov 11, 2015 | 12AM-2AM')
builder.preview(count=10)
```

||DATA|date_timerange|
|-----|-----|-----|
|0|1/1/2015 22 minutos e 54|Nenhuma|
|1|1/1/2015 23 minutos e 54|Nenhuma|
|2|1/1/2015 23:59|Nenhuma|
|3|1/2/2015 0:54|Nenhuma|
|4|1/2/2015 1:00|Nenhuma|
|5|1/2/2015 1 minuto e 54|Nenhuma|
|6|1/2/2015 2 minutos e 54|Nenhuma|
|7|1/2/2015 3 minutos e 54|Nenhuma|
|8|1/2/2015 4:00|Nenhuma|
|9|1/2/2015 4 minutos e 54|Nenhuma|

Isso tinha claramente os efeitos negativos, como agora as somente as linhas que tenham valores na coluna derivada são aqueles que correspondem exatamente com os exemplos fornecidos a. Chamar `list_examples()` no objeto builder para ver uma lista de atual derivações de exemplo.

```python
examples = builder.list_examples()
```

| |DATA|Exemplo|example_id|
| -------- | -------- | -------- | -------- |
|0|1/1/2015 1:00|1 de Janeiro de 2015 12AM -2AM|-1|
|1|1/2/2015 0:54|2 de Janeiro de 2015 12AM -2AM|-2|
|2|1/29/2015 20 minutos e 54|29 de Janeiro de 2015 8 PM - 10 PM|-3|
|3|11/11/2015 0:54|11 de Novembro de 2015 \| 12AM -2AM|-4|

Neste caso, foram fornecidos exemplos inconsistentes. Para corrigir o problema, substitua os primeiros três exemplos por aqueles que correto (incluindo ' |' entre a data e hora).

Corrigir exemplos inconsistentes, eliminando os exemplos estão incorretos (passando qualquer um dos `example_row` partir os pandas DataFrame ou ao transmitir `example_id` valor) e, em seguida, adicionar novas modificado exemplos novamente.

```python
builder.delete_example(example_id=-1)
builder.delete_example(example_row=examples.iloc[1])
builder.delete_example(example_row=examples.iloc[2])
builder.add_example(examples.iloc[0], 'Jan 1, 2015 | 12AM-2AM')
builder.add_example(examples.iloc[1], 'Jan 2, 2015 | 12AM-2AM')
builder.add_example(examples.iloc[2], 'Jan 29, 2015 | 8PM-10PM')
builder.preview()
```

| | DATA | date_timerange |
| -------- | -------- | -------- |
| 0 | 1/1/2015 0:54 | 1 de Janeiro de 2015 \| 12AM -2AM |
| 1 | 1/1/2015 1:00 | 1 de Janeiro de 2015 \| 12AM -2AM |
| 2 | 1/1/2015 1 minuto e 54 | 1 de Janeiro de 2015 \| 12AM -2AM |
| 3 | 1/1/2015 2 minutos e 54 | 1 de Janeiro de 2015 \| 2AM -4AM |
| 4 | 1/1/2015 3 minutos e 54 | 1 de Janeiro de 2015 \| 2AM -4AM |
| 5 | 1/1/2015 4:00 | 1 de Janeiro de 2015 \| 4 AM - 6 da Manhã|
| 6 | 1/1/2015 4 minutos e 54 | 1 de Janeiro de 2015 \| 4 AM - 6 da Manhã|
| 7 | 1/1/2015 5 minutos e 54 | 1 de Janeiro de 2015 \| 4 AM - 6 da Manhã|
| 8 | 1/1/2015 6 minutos e 54 | 1 de Janeiro de 2015 \| 6 da Manhã - 8 AM|
| 9 | 1/1/2015 7:00 | 1 de Janeiro de 2015 \| 6 da Manhã - 8 AM|

Agora, os dados parecem corretos e que chamar `to_dataflow()` no construtor, que irá devolver um fluxo de dados com colunas derivadas pretendidas adicionadas.

```python
dataflow = builder.to_dataflow()
df = dataflow.to_pandas_dataframe()
```

## <a name="filtering"></a>Filtragem

O SDK inclui os métodos `Dataflow.drop_columns` e `Dataflow.filter` para permitem-lhe filtrar colunas ou linhas.

### <a name="initial-setup"></a>Configuração inicial

```python
import azureml.dataprep as dprep
from datetime import datetime
dataflow = dprep.read_csv(path='https://dprepdata.blob.core.windows.net/demo/green-small/*')
dataflow.head(5)
```

||lpep_pickup_datetime|Lpep_dropoff_datetime|Store_and_fwd_flag|RateCodeID|Pickup_longitude|Pickup_latitude|Dropoff_longitude|Dropoff_latitude|Passenger_count|Trip_distance|Tip_amount|Tolls_amount|Total_amount|
|-----|-----|-----|-----|-----|-----|-----|-----|-----|-----|-----|-----|-----|-----|
|0|Nenhuma|Nenhum|Nenhum|Nenhum|Nenhum|Nenhum|Nenhum|Nenhum|Nenhum|Nenhum|Nenhum|Nenhum|Nenhuma|
|1|2013-08-01 08:14:37|2013-08-01 09:09:06|N|1|0|0|0|0|1|,00|0|0|21.25|
|2|2013-08-01 09:00 13:|2013-08-01 38: UTC+11:00|N|1|0|0|0|0|2|,00|0|0|75|
|3|2013-08-01 09:00 48:|2013-08-01 09:00 49:|N|5|0|0|0|0|1|,00|0|1|2.1|
|4|2013-08-01 10:38:35|2013-08-01 10:38:51|N|1|0|0|0|0|1|,00|0|0|3.25|

### <a name="filtering-columns"></a>Filtragem de colunas

Para filtrar colunas, utilize `Dataflow.drop_columns`. Este método aceita uma lista de colunas para remover ou um argumento mais complexo chamado `ColumnSelector`.

#### <a name="filtering-columns-with-list-of-strings"></a>Filtragem de colunas com a lista de cadeias de caracteres

Neste exemplo, `drop_columns` utiliza uma lista de cadeias de caracteres. Cada cadeia de caracteres deve ser exatamente igual a coluna pretendida para remover.

```python
dataflow = dataflow.drop_columns(['Store_and_fwd_flag', 'RateCodeID'])
dataflow.head(5)
```

||lpep_pickup_datetime|Lpep_dropoff_datetime|Pickup_longitude|Pickup_latitude|Dropoff_longitude|Dropoff_latitude|Passenger_count|Trip_distance|Tip_amount|Tolls_amount|Total_amount|
|-----|-----|-----|-----|-----|-----|-----|-----|-----|-----|-----|-----|
|0|Nenhuma|Nenhum|Nenhum|Nenhum|Nenhum|Nenhum|Nenhum|Nenhum|Nenhum|Nenhum|Nenhuma|
|1|2013-08-01 08:14:37|2013-08-01 09:09:06|0|0|0|0|1|,00|0|0|21.25|
|2|2013-08-01 09:00 13:|2013-08-01 38: UTC+11:00|0|0|0|0|2|,00|0|0|75|
|3|2013-08-01 09:00 48:|2013-08-01 09:00 49:|0|0|0|0|1|,00|0|1|2.1|
|4|2013-08-01 10:38:35|2013-08-01 10:38:51|0|0|0|0|1|,00|0|0|3.25|

#### <a name="filtering-columns-with-regex"></a>Filtragem de colunas com regex

Em alternativa, utilize o `ColumnSelector` expressão remover colunas que correspondem a uma expressão regex. Neste exemplo, que a elimine todas as colunas que correspondem à expressão `Column*|.*longitude|.*latitude`.

```python
dataflow = dataflow.drop_columns(dprep.ColumnSelector('Column*|.*longitud|.*latitude', True, True))
dataflow.head(5)
```

||lpep_pickup_datetime|Lpep_dropoff_datetime|Passenger_count|Trip_distance|Tip_amount|Tolls_amount|Total_amount|
|-----|-----|-----|-----|-----|-----|-----|-----|
|0|Nenhuma|Nenhum|Nenhum|Nenhum|Nenhum|Nenhum|Nenhuma|
|1|2013-08-01 08:14:37|2013-08-01 09:09:06|1|,00|0|0|21.25|
|2|2013-08-01 09:00 13:|2013-08-01 38: UTC+11:00|2|,00|0|0|75|
|3|2013-08-01 09:00 48:|2013-08-01 09:00 49:|1|,00|0|1|2.1|
|4|2013-08-01 10:38:35|2013-08-01 10:38:51|1|,00|0|0|3.25|

## <a name="filtering-rows"></a>Filtragem de linhas

Para filtrar linhas, utilize `DataFlow.filter`. Este método utiliza uma expressão de SDK do Azure Machine Learning Data Prep como um argumento e devolve um novo fluxo de dados com as linhas que a expressão é avaliada como True. As expressões são criadas através dos construtores de expressões (`col`, `f_not`, `f_and`, `f_or`) e operadores regulares (>, <>, =, < =, = =,! =).

### <a name="filtering-rows-with-simple-expressions"></a>Filtrar as linhas com expressões simples

Utilizar o construtor de expressões `col`, especifique o nome da coluna como argumento de cadeia de caracteres `col('column_name')`. Utilize esta expressão em combinação com um dos seguintes operadores padrão >, <>, =, < =, = =,! = para criar uma expressão como `col('Tip_amount') > 0`. Finalmente, passa a expressão incorporada no `Dataflow.filter` função.

Neste exemplo, `dataflow.filter(col('Tip_amount') > 0)` retorna um novo fluxo de dados com as linhas em que o valor de `Tip_amount` for maior que 0.

> [!NOTE] 
> `Tip_amount` em primeiro lugar é convertido em numérico, o que nos permite criar uma expressão compará-lo em relação a outros valores numéricos.

```python
dataflow = dataflow.to_number(['Tip_amount'])
dataflow = dataflow.filter(dprep.col('Tip_amount') > 0)
dataflow.head(5)
```

||lpep_pickup_datetime|Lpep_dropoff_datetime|Passenger_count|Trip_distance|Tip_amount|Tolls_amount|Total_amount|
|-----|-----|-----|-----|-----|-----|-----|-----|
|0|2013-08-01 19:33:28|2013-08-01 19:35:21|5|,00|0,08|0|4.58|
|1|2013-08-05 13:16:38|2013-08-05 13:18:24|1|,00|0,30|0|3.8|
|2|2013-08-05 14:11:42|2013-08-05 14:12:47|1|,00|1,05|0|4.55|
|3|2013-08-05 14:15:56|2013-08-05 14:18:04|5|,00|2.22|0|5.72|
|4|2013-08-05 14:42:14|2013-08-05 14:42:38|1|,00|0.88|0|4.38|

### <a name="filtering-rows-with-complex-expressions"></a>Filtrar as linhas com expressões complexas

Para filtrar usando expressões complexas, combinar uma ou mais expressões simples com dos construtores de expressões `f_not`, `f_and`, ou `f_or`.

Neste exemplo, `Dataflow.filter` retorna um novo fluxo de dados com as linhas em que `'Passenger_count'` é inferior a 5 e `'Tolls_amount'` for maior que 0.

```python
dataflow = dataflow.to_number(['Passenger_count', 'Tolls_amount'])
dataflow = dataflow.filter(dprep.f_and(dprep.col('Passenger_count') < 5, dprep.col('Tolls_amount') > 0))
dataflow.head(5)
```

||lpep_pickup_datetime|Lpep_dropoff_datetime|Passenger_count|Trip_distance|Tip_amount|Tolls_amount|Total_amount|
|-----|-----|-----|-----|-----|-----|-----|-----|
|0|2013-08-08 12:00 16:|2013-08-08 12:00 16:|1.0|,00|2.25|5,00|19.75|
|1|2013-08-12 14:43:53|2013-08-12 15:04:50|1.0|5.28|6.46|5.33|32.29|
|2|2013-08-12 19:48:12|2013-08-12 20:03:42|1.0|5.50|1,00|10.66|30.66|
|3|2013-08-13 06:11:06|2013-08-13 06:30:28|1.0|9.57|7.47|5.33|44.8|
|4|2013-08-16 20:33:50|2013-08-16 20:48:50|1.0|5.63|Digite 3,00|5.33|27.83|

Também é possível filtrar linhas combinar mais do que um construtor de expressões para criar uma expressão aninhada.

> [!NOTE]
> `lpep_pickup_datetime` e `Lpep_dropoff_datetime` primeiro são convertidos em datetime, que nos permite criar uma expressão compará-lo em relação a outros valores de datetime.

```python
dataflow = dataflow.to_datetime(['lpep_pickup_datetime', 'Lpep_dropoff_datetime'], ['%Y-%m-%d %H:%M:%S'])
dataflow = dataflow.to_number(['Total_amount', 'Trip_distance'])
mid_2013 = datetime(2013,7,1)
dataflow = dataflow.filter(
    dprep.f_and(
        dprep.f_or(
            dprep.col('lpep_pickup_datetime') > mid_2013,
            dprep.col('Lpep_dropoff_datetime') > mid_2013),
        dprep.f_and(
            dprep.col('Total_amount') > 40,
            dprep.col('Trip_distance') < 10)))
dataflow.head(5)
```

||lpep_pickup_datetime|Lpep_dropoff_datetime|Passenger_count|Trip_distance|Tip_amount|Tolls_amount|Total_amount|
|-----|-----|-----|-----|-----|-----|-----|-----|
|0|2013-08-13 06:11:06 + 00:00|2013-08-13 GMT+06:30: 28 + 00:00|1.0|9.57|7.47|5.33|44.80|
|1|2013-08-23 12:28: mais de 20 00:00|2013-08-23 12:50:28 + 00:00|2.0|8.22|8.08|5.33|40.41|
|2|2013-08-25 09:12:52 + 00:00|2013-08-25 09:34:34 + 00:00|1.0|8.80|8.33|5.33|41.66|
|3|2013-08-25 16:46:51 + 00:00|2013-08-25 17:13:55 + 00:00|2.0|9.66|7.37|5.33|44.20|
|4|2013-08-25 17:42:11 + 00:00|2013-08-25 18:02:57 + 00:00|1.0|9.60|6.87|5.33|41.20|

## <a name="custom-python-transforms"></a>Transformações de Python personalizadas

Sempre existirão cenários quando a opção mais fácil para fazer uma transformação é escrever seu próprio script. O SDK fornece três pontos de extensão que pode utilizar para scripts personalizados do Python.

- Nova coluna de script
- Novo filtro de script
- Transformar a partição

Cada uma das extensões é suportada o tempo de execução vertical e horizontal. Das principais vantagens de usar esses pontos de extensão é que não é necessário extrair todos os dados para criar um quadro de dados. Seu código será executado apenas de Python personalizado como outras transformações, à escala, pela partição e, normalmente em paralelo.

### <a name="initial-data-preparation"></a>Preparação de dados inicial

Comece por carregar alguns dados do Blob do Azure.

```python
import azureml.dataprep as dprep
col = dprep.col

df = dprep.read_csv(path='https://dpreptestfiles.blob.core.windows.net/testfiles/read_csv_duplicate_headers.csv', skip_rows=1)
df.head(5)
```

| |stnam|fipst|leaid|leanm10|ncessch|MAM_MTH00numvalid_1011|
|-----|-------|---------| -------|------|-----|------|-----|
|0|ALABAMA|1|101710|CONDADO Hale|10171002158| |
|1|ALABAMA|1|101710|CONDADO Hale|10171002162| |
|2|ALABAMA|1|101710|CONDADO Hale|10171002156| |
|3|ALABAMA|1|101710|CONDADO Hale|10171000588|2|
|4|ALABAMA|1|101710|CONDADO Hale|10171000589| |

Seria reduzir o conjunto de dados e fazer algumas transformações básicas.

```python
df = df.keep_columns(['stnam', 'leanm10', 'ncessch', 'MAM_MTH00numvalid_1011'])
df = df.replace_na(columns=['leanm10', 'MAM_MTH00numvalid_1011'], custom_na_list='.')
df = df.to_number(['ncessch', 'MAM_MTH00numvalid_1011'])
df.head(5)
```

| |stnam|leanm10|ncessch|MAM_MTH00numvalid_1011|
|-----|-------|---------| -------|------|-----|
|0|ALABAMA|CONDADO Hale|1.017100e + 10|Nenhuma|
|1|ALABAMA|CONDADO Hale|1.017100e + 10|Nenhuma|
|2|ALABAMA|CONDADO Hale|1.017100e + 10|Nenhuma|
|3|ALABAMA|CONDADO Hale|1.017100e + 10|2|
|4|ALABAMA|CONDADO Hale|1.017100e + 10|Nenhuma|

Procure valores nulos, usando o seguinte filtro.

```python
df.filter(col('MAM_MTH00numvalid_1011').is_null()).head(5)
```

| |stnam|leanm10|ncessch|MAM_MTH00numvalid_1011|
|-----|-------|---------| -------|------|-----|
|0|ALABAMA|CONDADO Hale|1.017100e + 10|Nenhuma|
|1|ALABAMA|CONDADO Hale|1.017100e + 10|Nenhuma|
|2|ALABAMA|CONDADO Hale|1.017100e + 10|Nenhuma|
|3|ALABAMA|CONDADO Hale|1.017100e + 10|Nenhuma|
|4|ALABAMA|CONDADO Hale|1.017100e + 10|Nenhuma|

### <a name="transform-partition"></a>Transformar a partição

Utilize uma função de pandas para substituir todos os valores nulos com um 0. Esse código será executado por partição, não no conjunto completo de dados de uma só vez. Isso significa que num conjunto de dados grandes, esse código pode executar em paralelo conforme o tempo de execução processa os dados, a partição por partição.

O script de Python tem de definir uma função chamada `transform()` que aceita dois argumentos, `df` e `index`. O `df` argumento será um pandas dataframe que contém os dados para a partição e o `index` argumento é um identificador exclusivo da partição. A função de transformação totalmente pode editar o pacote de dados transmitido, mas tem de devolver um dataframe. Quaisquer bibliotecas que importa o script de Python tem de existir no ambiente em que o fluxo de dados é executado.

```python
df = df.transform_partition("""
def transform(df, index):
    df['MAM_MTH00numvalid_1011'].fillna(0,inplace=True)
    return df
""")
df.head(5)
```

||stnam|leanm10|ncessch|MAM_MTH00numvalid_1011|
|-----|-------|---------| -------|------|-----|
|0|ALABAMA|CONDADO Hale|1.017100e + 10|0.0|
|1|ALABAMA|CONDADO Hale|1.017100e + 10|0.0|
|2|ALABAMA|CONDADO Hale|1.017100e + 10|0.0|
|3|ALABAMA|CONDADO Hale|1.017100e + 10|2.0|
|4|ALABAMA|CONDADO Hale|1.017100e + 10|0.0|

### <a name="new-script-column"></a>Nova coluna de script

Pode utilizar o código de Python para criar uma nova coluna que tem o nome do condado e o nome do Estado e também para aproveitar o nome do Estado. Para tal, utilize o `new_script_column()` método no fluxo de dados.

O script de Python tem de definir uma função chamada `newvalue()` que assume um argumento único `row`. O `row` argumento é um dict (`key`: nome da coluna, `val`: valor atual) e serão passados para essa função para cada linha no conjunto de dados. Esta função tem de devolver um valor a ser utilizado na nova coluna. Quaisquer bibliotecas que importa o script de Python tem de existir no ambiente em que o fluxo de dados é executado.

```python
df = df.new_script_column(new_column_name='county_state', insert_after='leanm10', script="""
def newvalue(row):
    return row['leanm10'] + ', ' + row['stnam'].title()
""")
df.head(5)
```

||stnam|leanm10|county_state|ncessch|MAM_MTH00numvalid_1011|
|-----|-------|---------| -------|------|-----|
|0|ALABAMA|CONDADO Hale|CONDADO Hale, Alabama|1.017100e + 10|0.0|
|1|ALABAMA|CONDADO Hale|CONDADO Hale, Alabama|1.017100e + 10|0.0|
|2|ALABAMA|CONDADO Hale|CONDADO Hale, Alabama|1.017100e + 10|0.0|
|3|ALABAMA|CONDADO Hale|CONDADO Hale, Alabama|1.017100e + 10|2.0|
|4|ALABAMA|CONDADO Hale|CONDADO Hale, Alabama|1.017100e + 10|0.0|

### <a name="new-script-filter"></a>Novo filtro de Script

Criar uma expressão de Python para filtrar o conjunto de dados para apenas as linhas onde 'Hale' não está no novo `county_state` coluna. A expressão devolve `True` para manter a linha, e `False` para remover a linha.

```python
df = df.new_script_filter("""
def includerow(row):
    val = row['county_state']
    return 'Hale' not in val
""")
df.head(5)
```

||stnam|leanm10|county_state|ncessch|MAM_MTH00numvalid_1011|
|-----|-------|---------| -------|------|-----|
|0|ALABAMA|CONDADO Jefferson|CONDADO Jefferson, Alabama|1.019200e + 10|1.0|
|1|ALABAMA|CONDADO Jefferson|CONDADO Jefferson, Alabama|1.019200e + 10|0.0|
|2|ALABAMA|CONDADO Jefferson|CONDADO Jefferson, Alabama|1.019200e + 10|0.0|
|3|ALABAMA|CONDADO Jefferson|CONDADO Jefferson, Alabama|1.019200e + 10|0.0|
|4|ALABAMA|CONDADO Jefferson|CONDADO Jefferson, Alabama|1.019200e + 10|0.0|
