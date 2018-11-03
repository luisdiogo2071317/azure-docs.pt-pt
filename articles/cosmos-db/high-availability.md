---
title: Elevada disponibilidade no Azure Cosmos DB
description: Este artigo descreve como o Azure Cosmos DB fornece elevada disponibilidade
services: cosmos-db
author: markjbrown
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 10/15/2018
ms.author: mjbrown
ms.reviewer: sngun
ms.openlocfilehash: fb7de4a913fa1edb3329e25276f83be2fb6cecd8
ms.sourcegitcommit: ada7419db9d03de550fbadf2f2bb2670c95cdb21
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 11/02/2018
ms.locfileid: "50961811"
---
# <a name="high-availability-in-azure-cosmos-db"></a>Elevada disponibilidade no Azure Cosmos DB

O Azure Cosmos DB replica de forma transparente os dados em todas as regiões do Azure à conta do Cosmos. O cosmos DB emprega várias camadas de redundância para os seus dados, conforme mostrado na imagem seguinte:

![A criação de partições de recursos](./media/high-availability/figure1.png) 

- Os dados dentro de contentores de Cosmos é particionados horizontalmente.

- Em cada região, cada partição é protegida por um conjunto de réplicas com todas as gravações replicadas e consolidada de maneira duradoura, a maioria das réplicas. As réplicas são distribuídas por domínios de falha de até 10 a 20.

- Cada partição em todas as regiões é replicada. Cada região contém todas as partições de dados de um contentor do Cosmos e pode aceitar escritas e servir leituras.  

Se a sua conta do Cosmos é distribuída em várias regiões do N Azure, haverá, pelo menos, N x 4 cópias de todos os seus dados. Além de fornecer acesso de dados de baixa latência e dimensionar o débito de leitura/escrita em todas as regiões à conta do Cosmos, a presença também a mais regiões (superior N) melhora a disponibilidade.  

## <a name="slas-for-availability"></a>SLAs de disponibilidade   

Como uma base de dados globalmente distribuída, o Cosmos DB oferece SLAs abrangentes que abranger o débito, latência no percentil de 99, a consistência e a elevada disponibilidade. A tabela a seguir, descrevemos as garantias relativas a elevada disponibilidade fornecida pelo Cosmos DB para contas únicas e várias regiões. Para elevada disponibilidade, configure as contas do Cosmos para ter várias regiões de escrita.

|Tipo de operação  | Região única |Várias regiões (gravações de região única)|Várias regiões (gravações de várias regiões) |
|---------|---------|---------|-------|
|Escritas    | 99,99    |99,99   |99,999|
|Leituras     | 99,99    |99,999  |99,999|

> [!NOTE]
> Na prática, a disponibilidade de gravação real para estagnação limitada, sessão, modelos de prefixo consistente e eventual consistência é significativamente maior do que os SLAs publicados. A disponibilidade de leitura real para todos os níveis de consistência é significativamente maior do que os SLAs publicados.

## <a name="regional-outages"></a>Falhas regionais

Falhas regionais não são invulgares e Azure Cosmos DB torna-se de que a sua base de dados está sempre disponível. Os seguintes detalhes de capturar o comportamento do Cosmos DB durante um período de inatividade, dependendo da configuração da sua conta de Cosmos: 

- Contas de várias regiões configuradas com a escrita de várias regiões permanecerá elevada disponibilidade para escritas e leituras. As ativações pós-falha regionais são instantâneas e não necessitam de quaisquer alterações da aplicação.

- Contas de várias regiões com uma região de escrita única: durante uma falha de região de escrita, estas contas serão permanecem altamente disponíveis para leitura. No entanto, para escritas "ative a ativação pós-falha automática" na sua conta do Cosmos para ativação pós-falha a região afetada para outra região associado. A ativação pós-falha irá ocorrer por ordem de prioridade da região que especificou. Eventualmente, quando a região afetada esteja novamente online, os dados unreplicated presentes na região de escrita afetados durante o período de inatividade são disponibilizados por meio de conflitos de feed. Aplicações podem ler os conflitos de feed, resolver conflitos com base na lógica específica de aplicativo e escrever os dados atualizados de volta para o contentor do Cosmos conforme apropriado. Assim que a região de escrita anteriormente afetado recupera, esta fica automaticamente disponível como uma região de leitura. Pode invocar uma ativação pós-falha manual e trazer de volta a região afetada como a região de escrita. Pode fazer uma ativação pós-falha manual usando [CLI do Azure ou o portal do Azure](how-to-manage-database-account.md#enable-manual-failover-for-your-cosmos-account).  

- Contas de várias regiões com uma região de escrita única: durante uma falha de região de leitura, estas contas irão continuar altamente disponíveis para leituras e gravações. A região afetada é automaticamente desligada a partir da região de escrita e será marcada como offline. Os SDKs do Cosmos DB irá redirecionar as chamadas de leitura para a próxima região disponível na lista de região preferencial. Se nenhuma das regiões na lista de região preferencial estiver disponível, chamadas automaticamente revertam para a região de escrita atual. Sem alterações são necessárias no código da aplicação para lidar com indisponibilidade da região de leitura. Eventualmente, quando a região afetada esteja novamente online, a região de leitura anteriormente afetado serão automaticamente sincronizados com a região de escrita atual e vai estar disponível novamente para atender a solicitações de leitura. Leituras subseqüentes são redirecionadas para a região recuperada sem exigir alterações ao código da aplicação. Durante a ativação pós-falha tanto rejoining de uma região anteriormente falhada, garantias de consistência de leitura continuam a ser cumpridas do Cosmos DB.

- Contas de região única poderão perder a disponibilidade em caso de falha regional. É recomendado para configurar, pelo menos, duas regiões (de preferência, regiões de escrita, pelo menos, dois) com a sua conta do Cosmos para garantir a elevada disponibilidade em todos os momentos.

## <a name="building-highly-available-applications"></a>Criar aplicações de elevada disponibilidade

- Certifique-se de escrita elevada e disponibilidade de leitura, configure a conta do Cosmos para abranger, pelo menos, duas regiões com a escrita de várias regiões. Esta configuração irá garantir a disponibilidade, latência mais baixa e leituras e escritas isto suportado por SLAs de escalabilidade para ambos. Para obter mais informações, consulte como [configure a conta do Cosmos com várias regiões de escrita](tutorial-global-distribution-sql-api.md).

- Para contas do Cosmos de várias regiões que estão configuradas com uma região de escrita única [ativar a ativação pós-falha automática utilizando a CLI do Azure ou o portal do Azure](how-to-manage-database-account.md#enable-automatic-failover-for-your-cosmos-account). Depois de ativar a ativação pós-falha automática, sempre que houver um desastre regional, Cosmos DB será automaticamente ativação pós-falha sua conta.  

- Mesmo que a sua conta do Cosmos está altamente disponível, seu aplicativo pode não ser criado corretamente para continuar altamente disponível. Para garantir a disponibilidade elevada de ponto-a-ponto para a sua aplicação, invocar periodicamente o [ativação pós-falha manual ao utilizar a CLI do Azure ou o portal do Azure](how-to-manage-database-account.md#enable-manual-failover-for-your-cosmos-account), como parte do seu teste de aplicativos ou a recuperação após desastre (DR) explorações. 

## <a name="next-steps"></a>Passos Seguintes

Em seguida pode aprender sobre dimensionar o débito no seguinte artigo:

* [Dimensionar o débito](scaling-throughput.md)

* [Desvantagens de desempenho e disponibilidade para vários níveis de consistência](consistency-levels-tradeoffs.md)

* [Dimensionar o débito global](scaling-throughput.md)

* [Distribuição global - sob definições avançadas](global-dist-under-the-hood.md)

* [Níveis de consistência no Azure Cosmos DB](consistency-levels.md)


