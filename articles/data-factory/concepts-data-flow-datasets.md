---
title: Mapeamento de conjuntos de dados de fluxo de dados de fábrica de dados do Azure
description: O Azure Data Factory mapeamento de fluxo de dados tem sepecific compatibilidade de conjunto de dados
author: kromerm
ms.author: makromer
ms.reviewer: douglasl
ms.service: data-factory
ms.topic: conceptual
ms.date: 02/14/2019
ms.openlocfilehash: ccf4273489d739bb9b0d802b79944efefcd02ff4
ms.sourcegitcommit: d2329d88f5ecabbe3e6da8a820faba9b26cb8a02
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 02/16/2019
ms.locfileid: "56331290"
---
# <a name="mapping-data-flow-datasets"></a>Conjuntos de dados de fluxo de dados de mapeamento

[!INCLUDE [notes](../../includes/data-factory-data-flow-preview.md)]

Conjuntos de dados são uma construção de fábrica de dados que definem a forma dos dados que está a trabalhar no seu pipeline. No fluxo de dados, os dados de nível de coluna e linha requerem uma definição de muito mais escalados detalhamento que é necessário pelos conjuntos de dados dentro do fluxo de controlo do pipeline.

Utilize conjuntos de dados na origem de fluxo de dados e transformações de Sink para definir o esquema de dados básicos. Se não tiver esquema nos seus dados, pode definir os Descompassos de esquema para a sua origem e Sink. Com o esquema definido do conjunto de dados, terá os tipos de dados relacionados, formatos de dados, localização do ficheiro e informações de ligação do serviço ligado associados.

Atualmente no fluxo de dados, encontrará quatro conjuntos de dados:

* BD SQL do Azure
* Azure SQL DW
* Parquet
* Texto delimitado

Separam os conjuntos de dados de fluxo de dados de origem *tipo* do tipo de ligação de serviço ligado. Normalmente, no Data Factory, selecione o tipo de ligação (Blob, ADLS, etc.) e, em seguida, definir o tipo de ficheiro no conjunto de dados. Dentro do fluxo de dados, escolhe os tipos de origem, o que podem ser associados com diferentes tipos de ligação de serviço ligado.

![Opções de transformação de origem](media/data-flow/dataset1.png "origens")

Ao criar um novo conjunto de dados, há uma caixa de seleção rotulada como "Dados de fluxo compatível" no canto superior direito do painel. Ao clicar nesse botão irá filtrar apenas os conjuntos de dados que podem ser utilizados com fluxos de dados. 

Importar esquemas

Ao importar o esquema de conjuntos de dados de fluxo de dados, verá um botão importar esquema. Ao clicar nesse botão irá apresentar-lhe duas opções: Importar da origem ou importar a partir de um ficheiro local. Na maioria dos casos, irá importar o esquema diretamente da fonte. No entanto, se tiver um ficheiro de esquema existente, pode apontar para esse ficheiro local e o Data Factory irá definir o esquema com base nesse ficheiro de esquema.

