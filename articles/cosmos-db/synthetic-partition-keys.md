---
title: Crie uma chave de partição sintéticas no Azure Cosmos DB para distribuir uniformemente os dados e a carga de trabalho.
description: Saiba como utilizar as chaves de partição sintéticas no seus contentores do Azure Cosmos
author: markjbrown
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 10/30/2018
ms.author: mjbrown
ms.openlocfilehash: 8becfe375f2e887348729cf1d76820fc41156d2a
ms.sourcegitcommit: e69fc381852ce8615ee318b5f77ae7c6123a744c
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 02/11/2019
ms.locfileid: "55997126"
---
# <a name="create-a-synthetic-partition-key"></a>Criar uma chave de partição sintética

É recomendável ter uma chave de partição com muitos valores distintos, por exemplo, centenas ou milhares. O objetivo é distribuir uniformemente os dados e a carga de trabalho entre os itens associados esses valores de chave de partição. Se essa propriedade não existe nos seus dados, pode construir uma chave de partição sintético. As secções seguintes descrevem várias técnicas básicas para gerar uma chave de partição sintético para o seu contentor.

## <a name="concatenate-multiple-properties-of-an-item"></a>Concatenar várias propriedades de um item

É possível formar uma chave de partição pela concatenação de vários valores de propriedade num único artificial `partitionKey` propriedade. Estas chaves são conhecidas como chaves sintéticas. Por exemplo, considere o seguinte documento de exemplo:

```JavaScript
{
"deviceId": "abc-123",
"date": 2018
}
```

Para o documento anterior, uma opção é definir /deviceId ou /date como a chave de partição. Utilize esta opção se pretender que o seu contentor com base no ID de dispositivo ou data de partição. Outra opção é concatenar esses dois valores numa synthetic `partitionKey` propriedade que é utilizada como a chave de partição.

```JavaScript
{
"deviceId": "abc-123",
"date": 2018,
"partitionKey": "abc-123-2018"
}
```

Em cenários em tempo real, pode ter milhares de documentos numa base de dados. Em vez de adicionar manualmente a chave sintética, defina a lógica do lado do cliente para concatenar os valores e inserir a chave sintética os documentos.

## <a name="use-a-partition-key-with-a-random-suffix"></a>Utilizar uma chave de partição com um sufixo aleatório

Outra estratégia possível distribuir a carga de trabalho mais uniformemente é anexar um número aleatório no final do valor da chave de partição. Quando distribui itens desta forma, pode efetuar operações de escrita paralelas em várias partições.

Um exemplo é se uma chave de partição representa uma data. Pode escolher um número aleatório entre 1 e 400 e concatená-lo como um sufixo para a data. Este método resulta em valores de chave de partição como 2018-08-09.1, 2018-08-09.2 e assim por diante, por meio de 2018-08-09.400. Uma vez que tornar aleatórios a chave de partição, as operações de escrita no contentor em cada dia são distribuídas uniformemente entre várias partições. Este método resulta em melhor paralelismo e geral maior débito.

## <a name="use-a-partition-key-with-precalculated-suffixes"></a>Utilizar uma chave de partição com sufixos pré-calculadas 

A estratégia aleatório pode melhorar significativamente o débito de escrita, mas é difícil de ler um item específico. Não sabe o valor de sufixo que é utilizado quando escreve o item. Para tornar mais fácil de ler itens individuais, utilize a estratégia de sufixos pré-calculadas. Em vez de utilizar um número aleatório para distribuir os itens entre partições, utilize um número que calcule com base em algo que pretende consultar.

Considere o exemplo anterior, onde um contentor utiliza uma data em que a chave de partição. Agora suponha que cada item tem um atributo de número de identificação de veículos (VIN) acessível. Suponha ainda mais o que, muitas vezes, execute consultas para localizar itens por VIN, além de data. Antes da sua aplicação escreve o item no contentor, pode calcular um sufixo de hash com base no VIN e acrescentá-lo para a data de chave de partição. O cálculo pode gerar um número entre 1 e 400, que é distribuído uniformemente. Este resultado é semelhante dos resultados produzidos pelo método estratégia aleatório. O valor da chave de partição, em seguida, é a data concatenada com o resultado calculado.

Com essa estratégia, as gravações são distribuídas uniformemente entre os valores de chave de partição e entre as partições. Pode ler facilmente um item específico e uma data, porque pode calcular o valor da chave de partição para um específico veículo--número de identificação. A vantagem deste método é que pode evitar a criação de uma chave de partição única de acesso frequente. Uma chave de partição de acesso frequente é a chave de partição que aproveita todos os a carga de trabalho. 

## <a name="next-steps"></a>Passos Seguintes

Pode saber mais sobre o conceito de criação de partições nos seguintes artigos:

* Saiba mais sobre [partições lógicas](partition-data.md).
* Saiba mais sobre como [débito de aprovisionar em bancos de dados e contentores do Azure Cosmos](set-throughput.md).
* Saiba como [débito de aprovisionar num contentor do Azure Cosmos](how-to-provision-container-throughput.md).
* Saiba como [débito de aprovisionar uma base de dados do Azure Cosmos](how-to-provision-database-throughput.md).
