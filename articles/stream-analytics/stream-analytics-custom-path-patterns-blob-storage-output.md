---
title: Saída de armazenamento (pré-visualização) de blob de padrões de caminho de DateTime personalizadas para o Azure Stream Analytics
description: ''
services: stream-analytics
author: mamccrea
ms.author: mamccrea
ms.reviewer: mamccrea
ms.service: stream-analytics
ms.topic: conceptual
ms.date: 09/24/2018
ms.openlocfilehash: da29c6bd8ddc1e2f62a78fb683df5e1784141722
ms.sourcegitcommit: f31bfb398430ed7d66a85c7ca1f1cc9943656678
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/28/2018
ms.locfileid: "47452569"
---
# <a name="custom-datetime-path-patterns-for-azure-stream-analytics-blob-storage-output-preview"></a>Saída de armazenamento (pré-visualização) de blob de padrões de caminho de DateTime personalizadas para o Azure Stream Analytics

O Azure Stream Analytics suporta datas personalizadas e especificadores de formato de tempo no caminho de ficheiro para saídas de armazenamento de Blobs. Padrões de caminho de DateTime personalizadas permitem-lhe especificar um formato de saída que se alinha com convenções de ramo de registo de transmissão em fluxo, oferecendo a capacidade de enviar dados para o Azure HDInsight e o Azure Databricks para processar downstream de Azure Stream Analytics. Padrões de caminho de DateTime personalizados facilmente são implementados usando o `datetime` palavra-chave no campo de prefixo do caminho do seu blob de saída, juntamente com o especificador de formato. Por exemplo, `{datetime:yyyy}`.

Utilize esta ligação para [Portal do Azure](https://portal.azure.com/?Microsoft_Azure_StreamAnalytics_bloboutputcustomdatetimeformats=true) para ativar/desativar o sinalizador de funcionalidade que permite que os padrões de caminho de DateTime personalizados para a pré-visualização de saída de armazenamento de Blobs. Esta funcionalidade será ativada em breve no portal do principal.

## <a name="supported-tokens"></a>Tokens suportados

Os tokens de especificador de formato seguinte podem ser usados individualmente ou em combinação para alcançar os formatos de DateTime personalizados:

|Especificador de formato   |Descrição   |Os resultados de tempo de exemplo de 2018-01-02T10:06:08|
|----------|-----------|------------|
|{datetime:yyyy}|O ano como um número de quatro dígitos|2018|
|{datetime:MM}|Mês a partir de 01 a 12|01|
|{datetime:M}|Mês a partir de 1 a 12|1|
|{datetime:dd}|Dia a partir de 01 e 31|02|
|{datetime:d}|Dia a partir de 1 a 12|2|
|{datetime:HH}|Usando o formato de 24 horas, de 00 e 23 de hora|10|
|{datetime:mm}|Minutos de 00 a 24|06|
|{datetime:m}|Minutos de 0 a 24|6|
|{datetime:ss}|Segundos a partir de 00 para 60|08|

Se não pretender utilizar padrões de DateTime personalizadas, pode adicionar a {date} e/ou {time} o token para o prefixo do caminho para gerar uma lista suspensa com formatos de DateTime incorporados.

![Formatos de DateTime antigos do Stream Analytics](./media/stream-analytics-custom-path-patterns-blob-storage-output/stream-analytics-old-date-time-formats.png)

## <a name="extensibility-and-restrictions"></a>Extensibilidade e restrições

Pode utilizar os tokens, `{datetime:<specifier>}`, conforme desejar no padrão de caminho, até atingir o limite de carateres de prefixo do caminho. Especificadores de formato não podem ser combinados num único token para além das combinações já listadas pelas listas pendentes de data e hora. 

Para uma partição de caminho de `logs/MM/dd`:

|Expressão válida   |Expressão inválida   |
|----------|-----------|
|`logs/{datetime:MM}/{datetime:dd}`|`logs/{datetime:MM/dd}`|

Pode utilizar o mesmo especificador de formato várias vezes no prefixo do caminho. O token deve ser repetido a cada vez.

## <a name="hive-streaming-conventions"></a>Convenções de transmissão em fluxo do ramo de registo

Padrões de caminho personalizado para o armazenamento de BLOBs podem ser utilizadas com a Convenção de ramo de registo de transmissão em fluxo, que espera pastas para ser rotulados com `column=` no nome da pasta.

Por exemplo, `year={datetime:yyyy}/month={datetime:MM}/day={datetime:dd}/hour={datetime:HH}`.

Saída personalizada elimina a necessidade de alterar tabelas e a adição manual de partições de dados de porta entre o Azure Stream Analytics e o Hive. Em vez disso, muitos pastas podem ser adicionadas automaticamente ao utilizar:

```
MSCK REPAIR TABLE while hive.exec.dynamic.partition true
```

### <a name="example"></a>Exemplo

Criar uma conta de armazenamento, um grupo de recursos, uma tarefa do Stream Analytics e uma origem de entrada de acordo com o [Portal do Azure Stream Analytics do Azure](stream-analytics-quick-create-portal.md) guia de início rápido. Utilize os mesmos dados de exemplo utilizados no guia de início rápido, também está disponível no [GitHub](https://raw.githubusercontent.com/Azure/azure-stream-analytics/master/Samples/GettingStarted/HelloWorldASA-InputStream.json).

Crie um sink de saída do blob com a seguinte configuração:

![Stream Analytics criar sink de saída do blob](./media/stream-analytics-custom-path-patterns-blob-storage-output/stream-analytics-create-output-sink.png)

O padrão de caminho completo é o seguinte:

```
year={datetime:yyyy}/month={datetime:MM}/day={datetime:dd}
```

Quando iniciar a tarefa, uma estrutura de pastas com base no padrão de caminho é criada no contentor de blob. Pode desagregar para o nível do dia.

![Saída de blob do Stream Analytics com o padrão do caminho personalizado](./media/stream-analytics-custom-path-patterns-blob-storage-output/stream-analytics-blob-output-folder-structure.png)

## <a name="next-steps"></a>Passos Seguintes

* [Compreender as saídas do Azure Stream Analytics](stream-analytics-define-outputs.md)
