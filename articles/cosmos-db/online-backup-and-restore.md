---
title: Online backup e restauração com o Azure Cosmos DB | Documentos da Microsoft
description: Saiba como fazer cópia de segurança automática e restaurar uma base de dados do Azure Cosmos DB.
keywords: cópia de segurança e restauro, a cópia de segurança online
services: cosmos-db
author: SnehaGunda
manager: kfile
ms.service: cosmos-db
ms.devlang: na
ms.topic: conceptual
ms.date: 11/15/2017
ms.author: sngun
ms.openlocfilehash: 66b4f63e75773aa0c1857dfcc19e22b48a0c3537
ms.sourcegitcommit: 30fd606162804fe8ceaccbca057a6d3f8c4dd56d
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/30/2018
ms.locfileid: "39343164"
---
# <a name="automatic-online-backup-and-restore-with-azure-cosmos-db"></a>Cópia de segurança online automática e restauro com o Azure Cosmos DB
O Azure Cosmos DB tira automaticamente cópias de segurança de todos os seus dados em intervalos regulares. As cópias de segurança automáticas são executadas sem afetar o desempenho ou a disponibilidade das suas operações de base de dados. Todas as suas cópias de segurança são armazenadas separadamente em outro serviço de armazenamento, e essas cópias de segurança são replicadas globalmente para resiliência contra desastres regionais. As cópias de segurança automáticas destinam-se a cenários quando acidentalmente eliminar o contentor do Cosmos DB e mais tarde necessitam de recuperação de dados ou uma solução de recuperação após desastre.  

Este artigo começa com uma recapitulação rápida da redundância de dados e a disponibilidade no Cosmos DB e, em seguida, aborda as cópias de segurança. 

## <a name="high-availability-with-cosmos-db---a-recap"></a>Elevada disponibilidade com o Cosmos DB - uma recapitulação
O cosmos DB foi concebido para ser [globalmente distribuída](distribute-data-globally.md) – permite-lhe dimensionar o débito em várias regiões do Azure, juntamente com a política controlado por ativação pós-falha e as APIs multi-homing transparentes. Azure Cosmos DB oferece [SLAs de disponibilidade de 99,99%](https://azure.microsoft.com/support/legal/sla/cosmos-db) para todas as contas de região única e todas as contas de várias regiões com consistência flexível e 99,999% de disponibilidade em todas as contas de base de dados de várias regiões de leitura. Todas as gravações no Azure Cosmos DB são consolidadas de forma para discos locais por um quórum de réplicas dentro de um centro de dados local antes de ter consciência de para o cliente. A elevada disponibilidade do Cosmos DB baseia-se no armazenamento local e não depende de qualquer tecnologia de armazenamento externo. Além disso, se a sua conta de base de dados é associada a mais do que uma região do Azure, as escritas são replicadas em outras regiões também. Para reduzir os dados de débito e o acesso às latências baixas, pode ter como muitas regiões associadas à sua conta de base de dados gosta de leitura. Em cada região de leitura, os dados (replicados) maneira duradoura são mantidos num conjunto de réplicas.  

Conforme ilustrado no diagrama seguinte, um único contentor do Cosmos DB é [particionada horizontalmente](partition-data.md). Uma "partição" está em falta por um círculo no diagrama seguinte, e cada partição é feita a elevada disponibilidade através de um conjunto de réplicas. Esta é a distribuição local numa única região do Azure (representado pelo eixo X). Além disso, cada partição (com o respetivo conjunto de réplica correspondente), em seguida, é distribuída globalmente em várias regiões associados à conta de base de dados (por exemplo, em regiões este ilustração os três – E.U.A. leste, E.U.A. oeste e Índia Central). O "conjunto de partição" é uma distribuição global que inclui entidades de várias cópias dos seus dados em cada região (representado pelo eixo Y). Pode atribuir prioridade para as regiões associadas à sua conta de base de dados e o Cosmos DB será failover transparente para a região seguinte em caso de desastre. Também manualmente, pode simular ativação pós-falha para testar a disponibilidade de ponto a ponto da sua aplicação.  

A imagem seguinte ilustra o alto grau de redundância com o Cosmos DB.

![Alto grau de redundância com o Cosmos DB](./media/online-backup-and-restore/redundancy.png)

![Alto grau de redundância com o Cosmos DB](./media/online-backup-and-restore/global-distribution.png)

## <a name="full-automatic-online-backups"></a>Cópias de segurança completas, automáticas e online
UPS, eu eliminado meu contentor ou a base de dados! Com o Cosmos DB, não apenas seus dados, mas as cópias de segurança dos seus dados também são feitas altamente redundantes e resiliente desastres regionais. Estas cópias de segurança automáticas atualmente direcionadas aproximadamente a cada quatro horas e mais recente duas cópias de segurança são armazenadas em todos os momentos. Se os dados acidentalmente são removidos ou danificados, contacte [suporte do Azure](https://azure.microsoft.com/support/options/) no espaço de oito horas. 

As cópias de segurança são efetuadas sem afetar o desempenho ou a disponibilidade das suas operações de base de dados. O cosmos DB tira a cópia de segurança em segundo plano sem consumir os RUs aprovisionadas ou que afetam o desempenho e sem afetar a disponibilidade da base de dados. 

Ao contrário dos seus dados são armazenados dentro do Cosmos DB, as cópias de segurança automáticas são armazenadas no serviço de armazenamento de Blobs do Azure. Para garantir o carregamento de latência baixa/eficiente, o instantâneo da cópia de segurança é carregado para uma instância do armazenamento de Blobs do Azure na mesma região que a região de escrita atual da sua conta de base de dados do Cosmos DB. Para resiliência contra desastre regional, a cada instantâneo dos seus dados de cópia de segurança no armazenamento de Blobs do Azure novamente é replicado através do armazenamento georredundante (GRS) para outra região. O diagrama seguinte mostra que o contêiner inteiro do Cosmos DB (com todos os três principais partições na região E.U.A. oeste, neste exemplo) é uma cópia de segurança numa conta de armazenamento de Blobs do Azure remota na região E.U.A. oeste e, em seguida, GRS são replicados para E.U.A. Leste. 

A imagem seguinte ilustra as cópias de segurança completas periódicas de todas as entidades do Cosmos DB GRS do armazenamento do Azure.

![Cópias de segurança completas periódicas de todas as entidades do Cosmos DB GRS do armazenamento do Azure](./media/online-backup-and-restore/automatic-backup.png)

## <a name="backup-retention-period"></a>Período de retenção de cópia de segurança
Conforme descrito acima, Azure Cosmos DB tira instantâneos dos seus dados a cada quatro horas no nível da partição. Em qualquer momento, apenas os últimos dois instantâneos são retidos. No entanto, se o contentor/base de dados é eliminada, o Azure Cosmos DB retém os instantâneos existentes para todas as partições eliminadas dentro de determinado contentor/base de dados durante 30 dias.

Para a API de SQL, se pretender manter o seu próprio instantâneos, pode usar a opção Exportar para JSON no Azure Cosmos DB [ferramenta de migração de dados](import-data.md#export-to-json-file) para agendar cópias de segurança adicionais.

> [!NOTE]
> Se "Aprovisionar débito para um conjunto de contentores ao nível da base de dados" – não se esqueça do restauro ocorre ao nível de conta de base de dados completo. Terá também de Certifique-se em contacto dentro de 8 horas para a equipa de suporte se acidentalmente eliminou seu contentor. Não não possível restaurar dados se não contactar a equipa de suporte dentro de 8 horas. 


## <a name="restoring-a-database-from-an-online-backup"></a>Restaurar uma base de dados a partir de uma cópia de segurança online

Se eliminar acidentalmente a base de dados ou o contentor, pode [enviar um pedido de suporte](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) ou [contactar o suporte do Azure](https://azure.microsoft.com/support/options/) para restaurar os dados da última cópia de segurança automática. Suporte do Azure está disponível para planos selecionados apenas como padrão, o desenvolvedor, o suporte não está disponível com o plano básico. Para saber mais sobre os planos de suporte a diferentes, veja [planos de suporte do Azure](https://azure.microsoft.com/support/plans/) página. 

Se tiver de restaurar a base de dados devido a problema de Corrupção de dados (inclui casos em que são eliminados os documentos dentro de um contêiner), consulte [lidar com danos nos dados](#handling-data-corruption) que precisar de efetuar passos adicionais para impedir que os dados danificados substitua as cópias de segurança existentes. Para um instantâneo específico da cópia de segurança seja restaurado, o Cosmos DB requer que os dados estavam disponíveis durante o ciclo de cópia de segurança para que o instantâneo.

## <a name="handling-data-corruption"></a>Manipulação de Corrupção de dados

O Azure Cosmos DB mantém as últimas duas cópias de segurança de cada partição na conta de base de dados. Esse modelo funciona bem quando um contentor (coleção de documentos, gráfico, tabela) ou uma base de dados sejam eliminado acidentalmente, uma vez que uma das versões do último pode ser restaurada. No entanto, no caso de quando os utilizadores podem introduzir um problema de Corrupção de dados, do Azure Cosmos DB pode não terem conhecimento da Corrupção de dados e é possível que a existência de danos pode ter substituído as cópias de segurança existentes. 

Assim que for detetada corrupção, contacte o suporte ao cliente com informações de conta e contentor de base de dados com o tempo aproximado de corrupção. Outra ação que o utilizador pode fazer no caso de danificado (eliminação/atualização de dados) o utilizador deve eliminar o contentor danificado (coleção/gráfico/tabela) para que as cópias de segurança são protegidas contra a ser substituído pelos dados danificados.  

A imagem seguinte ilustra a criação do pedido de suporte para o restauro de container(collection/graph/table) através do portal do Azure para a eliminação acidental ou de atualização de dados dentro de um contêiner

![Restaurar um contentor para atualização errada ou eliminar de dados no Cosmos DB](./media/online-backup-and-restore/backup-restore-support.png)

Quando é efetuar um restauro para este tipo de cenários - os dados são restaurados para outra conta (com o sufixo do "-restaurado") e um contentor. Este restauro não é feito no local para fornecer uma chance ao cliente para fazer a validação de dados e mover os dados conforme necessário. O contentor de restaurada está na mesma região com o mesmo RUs e políticas de indexação. 

## <a name="next-steps"></a>Passos Seguintes

Para replicar a base de dados em vários datacenters, veja [distribuir dados globalmente com o Cosmos DB](distribute-data-globally.md). 

Suporte do Azure, de contacto do ficheiro [enviar um pedido do portal do Azure](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade).

