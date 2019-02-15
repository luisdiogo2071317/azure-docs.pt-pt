---
title: Mapeamento de transformação de origem do fluxo de dados de fábrica de dados do Azure
description: Mapeamento de transformação de origem do fluxo de dados de fábrica de dados do Azure
author: kromerm
ms.author: makromer
ms.reviewer: douglasl
ms.service: data-factory
ms.topic: conceptual
ms.date: 02/12/2019
ms.openlocfilehash: 35f4e794caf84aba860b98e68eadcdcd88e77952
ms.sourcegitcommit: f715dcc29873aeae40110a1803294a122dfb4c6a
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 02/14/2019
ms.locfileid: "56272120"
---
# <a name="azure-data-factory-mapping-data-flow-source-transformation"></a>Mapeamento de transformação de origem do fluxo de dados de fábrica de dados do Azure

[!INCLUDE [notes](../../includes/data-factory-data-flow-preview.md)]

A transformação de origem configura uma origem de dados que deseja usar para trazer dados para o seu fluxo de dados. Pode ter mais do que 1 origem transformar num único fluxo de dados. Sempre comece criando seus fluxos de dados com uma origem.

> [!NOTE]
> Cada fluxo de dados requer, pelo menos, uma transformação de origem. Adicionar tantas outras fontes que forem necessárias

![Opções de transformação de origem](media/data-flow/source.png "origem")

A origem do fluxo de dados tem de ser associada a exatamente um ADF conjunto de dados, que define a forma e a localização dos seus dados para escrever ou ler a partir de.

## <a name="data-flow-staging-areas"></a>Áreas de preparo de fluxo de dados

O fluxo de dados do ADF tem de linha de visão 5 áreas "staging" no Azure para efetuar as transformações de dados principais: BLOBs do Azure, o ADLS Gen 1, o ADLS Gen 2, BD SQL do Azure e o armazém de dados SQL do Azure. ADF tem acesso a quase 80 conectores nativos diferentes, portanto, para incluir esses outras origens de dados no seu fluxo de dados, primeiro testar esses dados em uma dessas cinco principal fluxo de dados áreas de preparo primeiro através da atividade de cópia:

## <a name="options"></a>Opções

### <a name="allow-schema-drift"></a>Permitir que os descompassos de esquema
Selecione permitir Descompassos de esquema se as colunas de origem serão alterados com freqüência. Esta definição permitirá que todos os campos de entrada da sua origem a ser enviados por meio de transformações para o Sink.

### <a name="fail-if-columns-in-the-dataset-are-not-found"></a>Falhar se não forem encontradas colunas no conjunto de dados
Escolha esta opção para impor uma validação de esquema de origem falhará seu fluxo de dados se não existem colunas esperados da sua origem.

### <a name="sampling"></a>Amostragem
Utilize amostragem para limitar o número de linhas da sua origem.  Isto é útil quando precisa apenas uma amostra dos seus dados de origem para teste e fins de depuração.

### <a name="define-schema"></a>Definir esquema

![Transformação de origem](media/data-flow/source2.png "2 de origem")

### <a name="you-can-modify-the-name-of-the-source-columns-and-their-associated-data-types"></a>Pode modificar o nome de colunas de origem e os tipos de dados associados

Para tipos de ficheiro de origem que não estão com rigidez de tipos (ficheiros simples, ou seja, em vez de ficheiros Parquet), deve definir os tipos de dados para cada campo aqui na transformação de origem, em vez de no conjunto de dados.

Se não vir os nomes das colunas e tipos no seu fluxo de dados, é provável porque não definem-los na secção definem o esquema do coletor. Apenas terá de fazê-lo se não estiver a utilizar o tratamento de desvios de esquema do fluxo de dados.

Aqui na "Schema definir" separador a transformação de origem é onde pode definir os tipos de dados e formatos:

![Transformação de origem](media/data-flow/source003.png "tipos de dados")

### <a name="optimize"></a>Otimizar

![Partições de origem](media/data-flow/sourcepart.png "criação de partições")

No separador de otimização para a transformação de origem, verá um tipo de criação de partições adicional, chamado de "Origem". Isto irá apenas light-up quando tiver selecionado a BD SQL do Azure da sua origem. Isto acontece porque o ADF pretende colocar em paralelo ligações para executar consultas grandes na sua origem de BD SQL do Azure.

A criação de partições de dados na sua origem de BD SQL é opcional, mas é útil para consultas grandes. Tem duas opções:

### <a name="column"></a>Coluna

Selecione uma coluna para a partição da sua tabela de origem. Também tem de definir o número máximo de ligações.

### <a name="query-condition"></a>Condição de consulta

Opcionalmente, pode optar por ligações com base numa consulta de partição. Para esta opção, coloque o conteúdo de um predicado WHERE. Por exemplo, ano > 1980
