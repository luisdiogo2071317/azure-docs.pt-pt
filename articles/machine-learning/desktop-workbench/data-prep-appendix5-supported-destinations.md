---
title: Destinos de dados e saídas disponíveis com a preparação de dados do Azure Machine Learning suportados | Documentos da Microsoft
description: Este documento fornece uma lista completa dos destinos suportados e saídas disponível para a preparação de dados do Azure Machine Learning
services: machine-learning
author: euangMS
ms.author: euang
manager: lanceo
ms.reviewer: jmartens, jasonwhowell, mldocs
ms.service: machine-learning
ms.component: core
ms.workload: data-services
ms.custom: ''
ms.devlang: ''
ms.topic: article
ms.date: 02/01/2018
ms.openlocfilehash: 123328010758eea6e7eadce29440e204f91dcef6
ms.sourcegitcommit: e8f443ac09eaa6ef1d56a60cd6ac7d351d9271b9
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/12/2018
ms.locfileid: "35647127"
---
# <a name="supported-data-exports-for-this-preview"></a>Suportado exportações de dados para esta pré-visualização 
É possível exportar para diversos formatos diferentes. Pode usar esses formatos para manter os resultados intermediários de preparação de dados antes dos resultados integrar o resto do fluxo de trabalho do Machine Learning.

## <a name="types"></a>Tipos 
### <a name="csv-file"></a>Ficheiro CSV 
Gravar um arquivo de separados por vírgulas para o armazenamento.

#### <a name="options"></a>Opções
- Produzir de linha
- Substituir valores NULL com
- Substituir erros com 
- Separador


### <a name="parquet"></a>Parquet 
Escreva um conjunto de dados no armazenamento como Parquet.

Parquet como um formato pode ter várias formas no armazenamento. Para conjuntos de dados mais pequenos, um ficheiro único .parquet, às vezes, é utilizado. Várias bibliotecas de Python suportam a leitura e escrita em ficheiros .parquet único. 

Atualmente, o Azure Machine Learning Workbench baseia-se da biblioteca de Python de PyArrow para gravação Parquet durante o uso interativo local. Isso significa que o único ficheiro Parquet atualmente é o único formato de saída de Parquet suportado durante o uso interativo local.

Durante execuções de escalamento horizontal (no Spark), Azure Machine Learning Workbench depende Parquet do Spark leitura e gravação de capacidades. Formato de saída predefinido do Spark para o Parquet (atualmente o único suportado) é semelhante na estrutura de um conjunto de dados do Hive. Isso significa que uma pasta contém muitos arquivos de .parquet todas a uma partição menor de um conjunto de dados maior. 

#### <a name="caveats"></a>Limitações 
Parquet como um formato é relativamente nova e tenha algumas inconsistências de implementação entre as bibliotecas diferentes. Por exemplo, o Spark coloca restrições em que carateres são válidos em nomes de coluna quando o gravar para Parquet. PyArrow não faz isso. Os seguintes carateres não podem ser um nome de coluna: 
- , 
- ;
- {}
- ()
- \\n
- \\t
- =

>[!NOTE]
>- Para garantir a compatibilidade com o Spark, sempre que escreve dados no Parquet, ocorrências destes carateres nos nomes de coluna são substituídas por em caráter de sublinhado (_).
>- Para assegurar a consistência entre execuções locais e de escalamento horizontal, todos os dados gravados para Parquet, através da aplicação, Python ou Spark, tem seus nomes de coluna corrigido para garantir a compatibilidade do Spark. Para garantir nomes de colunas esperado ao escrever a caracteres Parquet no Spark, remova o conjunto de colunas antes de escrevê-las de inválido.



## <a name="locations"></a>Localizações 
### <a name="local"></a>Local 
Disco rígido local ou no local de armazenamento de rede mapeadas.

### <a name="azure-blob-storage"></a>Armazenamento de Blobs do Azure
Armazenamento de Blobs do Azure requer uma subscrição do Azure.

