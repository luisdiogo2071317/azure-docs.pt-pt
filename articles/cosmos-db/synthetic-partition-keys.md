---
title: Crie uma chave de partição sintéticas no Azure Cosmos DB para distribuir uniformemente os dados e a carga de trabalho.
description: Saiba como utilizar as chaves de partição sintéticas no seus contentores do Azure Cosmos DB
author: markjbrown
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 10/30/2018
ms.author: mjbrown
ms.openlocfilehash: 987bfe023e7355a2780af57dc5e5cac3129a5ca1
ms.sourcegitcommit: 698a3d3c7e0cc48f784a7e8f081928888712f34b
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/31/2019
ms.locfileid: "55454153"
---
# <a name="create-a-synthetic-partition-key"></a>Criar uma chave de partição sintético

É recomendável ter uma chave de partição com muitos valores distintos, como centenas ou milhares. O objetivo é distribuir uniformemente os dados e a carga de trabalho entre os itens associados esses valores de chave de partição. Se essa propriedade não existe nos seus dados, pode ser criada uma chave de partição sintético. As secções seguintes descrevem várias técnicas básicas para gerar uma chave de partição sintético para o seu contentor.

## <a name="concatenating-multiple-properties-of-an-item"></a>Concatenar várias propriedades de um item

É possível formar uma chave de partição pela concatenação de vários valores de propriedade num único artificial `partitionKey` propriedade. Estas chaves são conhecidas como chaves sintéticas. Por exemplo, considere o seguinte documento de exemplo:

```JavaScript
{
"deviceId": "abc-123",
"date": 2018
}
```

Para o documento anterior, uma opção é definir /deviceId ou /date como a chave de partição, se pretender que o seu contentor com base no ID de dispositivo ou data de partição. Outra opção é concatenar esses dois valores numa synthetic `partitionKey` propriedade que é utilizada como a chave de partição.

```JavaScript
{
"deviceId": "abc-123",
"date": 2018,
"partitionKey": "abc-123-2018"
}
```

Em cenários em tempo real, pode ter milhares de documentos numa base de dados, portanto, em vez de adicionar manualmente a chave sintética, deve definir a lógica do lado do cliente para concatenar os valores e inserir a chave sintética os documentos.

## <a name="using-a-partition-key-with-random-suffix"></a>Utilizar uma chave de partição com o sufixo aleatório

Outra estratégia possível distribuir a carga de trabalho mais uniformemente é anexar um número aleatório no final do valor da chave de partição. Distribuindo itens nesta permite que a forma parallel efetuar operações de escrita em várias partições.

Por exemplo, se uma chave de partição representa uma data, poderá escolher um número aleatório entre 1 e 400 e concatená-lo como um sufixo para a data. Este método resulta em valores de chave de partição como 2018-08-09.1, 2018-08-09.2 e assim por diante, por meio de 2018-08-09.400. Uma vez que estão a geração aleatória a chave de partição, as operações de escrita no contentor em cada dia são distribuídas uniformemente entre várias partições. Este método resulta em melhor paralelismo e geral maior débito.

## <a name="using-a-partition-key-with-pre-calculated-suffixes"></a>Utilizar uma chave de partição com sufixos previamente calculados 

Embora a geração aleatória estratégia pode melhorar significativamente o débito de escrita, é difícil de ler um item específico, porque não sabe o valor de sufixo que é utilizado ao escrever o item. Para tornar mais fácil de ler itens individuais, pode usar a estratégia de sufixos previamente calculada. Em vez de utilizar um número aleatório para distribuir os itens entre partições, utilize um número que calcule com base em algo que pretende consultar.

Considere o exemplo anterior, onde um contentor utiliza uma data em que a chave de partição. Agora suponha que cada item tem um atributo de VIN (número de identificação de veículos) acessível e se, muitas vezes, executar consultas para localizar itens por VIN, além do mais até à data. Antes da sua aplicação escreve o item no contentor, pode calcular um sufixo de hash com base no VIN e acrescentá-lo para a data de chave de partição. O cálculo pode gerar um número entre 1 e 400, que é distribuída uniformemente, semelhante dos resultados produzidos pelo método estratégia aleatório. O valor da chave de partição, em seguida, será a data concatenada com o resultado calculado.

Com essa estratégia, as gravações são distribuídas uniformemente entre os valores de chave de partição e entre as partições. Pode facilmente ler um item específico e uma data, porque pode calcular o valor da chave de partição para um específico veículo--número de identificação. A vantagem deste método é que pode evitar a criação de uma chave de partição única de acesso frequente (a chave de partição que aproveita todos os a carga de trabalho). 

## <a name="next-steps"></a>Passos Seguintes

Pode saber mais sobre o conceito de criação de partições nos seguintes artigos:

* Saiba mais sobre [partições lógicas](partition-data.md)
* Saiba mais sobre [aprovisionamento débito em bancos de dados e contentores de Cosmos](set-throughput.md)
* Saiba [como aprovisionar o débito num contentor do Cosmos](how-to-provision-container-throughput.md)
* Saiba [como aprovisionar o débito, numa base de dados do Cosmos](how-to-provision-database-throughput.md)
