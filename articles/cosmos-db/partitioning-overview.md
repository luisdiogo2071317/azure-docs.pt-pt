---
title: Criação de partições no Azure Cosmos DB
description: Descrição geral da criação de partições no Azure Cosmos DB
author: aliuy
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 10/30/2018
ms.author: andrl
ms.openlocfilehash: b89830d566b36b0446836d8f32aee5756e2d0991
ms.sourcegitcommit: 345b96d564256bcd3115910e93220c4e4cf827b3
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 11/28/2018
ms.locfileid: "52498435"
---
# <a name="partitioning-in-azure-cosmos-db"></a>Criação de partições no Azure Cosmos DB

Criação de partições é a técnica usada pelo Cosmos DB Dimensionar contentores individuais num banco de dados para satisfazer as necessidades de desempenho da sua aplicação. Ao utilizar a criação de partições, os itens num contentor estão divididos em subconjuntos distintos, chamados partições lógicas. As partições lógicas são criadas com base no valor de uma propriedade de chave de partição associada a cada item.

Uma partição lógica é um subconjunto distinto de itens num contentor. Os itens numa partição lógica são identificados pelo valor de chave de partição que é partilhado por todos os itens na partição lógica.  Por exemplo, considere um contentor que retém a documentos e cada documento tem um `UserID` propriedade.  Se `UserID` serve como a partição da chave para os itens num contentor e existem 1000 exclusivo `UserID` valores, partições lógicas de 1000 serão criadas para o contentor.

Cada item num contêiner tem um **chave de partição** determina que o item **partição lógica**, e cada item também tem um **id de item** (que é exclusivo dentro de uma lógica partição).  O **índice** de um item identifica de forma exclusiva e ele é formado, combinando a chave de partição e o id do item.

Escolher uma chave de partição é uma decisão importante que irão afetar o desempenho do aplicativo.  Para obter mais informações, consulte [escolher uma chave de partição](partitioning-overview.md#choose-partitionkey) artigo para obter instruções detalhadas.

## <a name="logical-partition-management"></a>Gestão de partição lógica

O cosmos DB forma transparente e automática gerencia o posicionamento de partições lógicas em partições físicas (infraestrutura de servidor) para atender com eficiência as necessidades de escalabilidade e desempenho do contentor. À medida que aumentam os requisitos de armazenamento e débito do aplicativo, o Cosmos DB move partições lógicas para distribuir automaticamente a carga por um maior número de servidores. Para saber mais sobre como o Cosmos DB gere partições, veja [partições lógicas](partition-data.md) artigo. Não é necessário compreender estes detalhes para criar ou executar as suas aplicações.

O cosmos DB utiliza baseada em hash de criação de partições para distribuir partições lógicas entre partições físicas.  O valor de chave de partição de um item é protegido por hash Cosmos DB e o resultado codificado determina a partição física. O cosmos DB aloca o espaço de chave de partição principais hashes uniformemente pelas partições físicas "n".

As consultas que acedem a dados dentro de uma única partição são mais económicas do que as consultas que acessam várias partições. Transações (em procedimentos armazenados ou disparadores) só são permitidas em relação a itens dentro de uma única partição lógica.  

## <a id="choose-partitionkey"></a>Escolher uma chave de partição

Ao escolher uma chave de partição, considere os seguintes detalhes:

* Uma única partição lógica é permitida um limite superior de 10 GB de armazenamento.  

* Contentores particionadas são configurados com débito mínimo de 400 RU/s. Pedidos para a mesma chave de partição não podem exceder a taxa de transferência alocada a uma partição. Se estas excedem o débito alocado, os pedidos será limitado de taxa. Por isso, é importante escolher uma chave de partição não resulta na "pontos de acesso" na sua aplicação.

* Escolha uma chave de partição que propaga uniformemente a carga de trabalho de todas as partições e de forma uniforme ao longo do tempo.  À sua escolha da chave de partição deve equilibrar a necessidade de consultas de partição eficiente e/ou transações face ao objectivo de distribuição de itens em várias partições para alcançar a escalabilidade.

* Escolha uma chave de partição que tenha uma vasta gama de valores e padrões de acesso que são distribuídos uniformemente por partições lógicas. A idéia básica é distribuir os dados e a atividade no seu contentor entre o conjunto de partições lógicas, para que os recursos de armazenamento e débito de dados podem ser distribuídos por partições lógicas.

* Candidatos para as chaves de partição podem incluir as propriedades que são apresentados frequentemente como um filtro em suas consultas. Consultas podem ser encaminhadas com eficiência, incluindo a chave de partição no predicado de filtro.

## <a name="next-steps"></a>Passos Seguintes

* Saiba mais sobre [partições](partition-data.md)
* Saiba mais sobre [débito aprovisionado no Azure Cosmos DB](request-units.md)
* Saiba mais sobre [distribuição global no Azure Cosmos DB](distribute-data-globally.md)
