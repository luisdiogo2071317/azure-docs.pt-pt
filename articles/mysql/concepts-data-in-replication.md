---
title: Replica os dados na base de dados do Azure para MySQL.
description: Este artigo descreve os dados na replicação da base de dados do Azure para MySQL.
services: mysql
author: ajlam
ms.author: andrela
manager: kfile
editor: jasonwhowell
ms.service: mysql
ms.topic: article
ms.date: 06/20/2018
ms.openlocfilehash: 72f8211ecc0534b15402911de8fc0ec3d541a835
ms.sourcegitcommit: 1438b7549c2d9bc2ace6a0a3e460ad4206bad423
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/20/2018
ms.locfileid: "36294909"
---
# <a name="replicate-data-into-azure-database-for-mysql"></a>Replicar os dados na base de dados do Azure para MySQL

Replicação de dados permite-lhe sincronizar os dados de um servidor de MySQL em execução no local, em máquinas virtuais ou serviços de base de dados alojados por outros fornecedores de nuvem para a base de dados do Azure para o serviço de MySQL. Replicação de dados baseia-se o registo binário (binlog) ficheiro posição replicação baseada em MySQL nativa. Para saber mais sobre a replicação de binlog, consulte o [descrição geral de replicação do MySQL binlog](https://dev.mysql.com/doc/refman/5.7/en/binlog-replication-configuration-overview.html). 

## <a name="when-to-use-data-in-replication"></a>Quando utilizar a replicação de dados
Os cenários principais a considerar utilizando a replicação de dados são:

- **Sincronização de dados de híbrida:** com a replicação de dados, pode manter os dados sincronizados entre os servidores no local e a base de dados do Azure para MySQL. Esta sincronização é útil para criar aplicações híbridas. Este método é apelativos para resolver quando existir um servidor de base de dados local, mas pretender mover os dados para uma região próximo aos utilizadores finais.
- **Sincronização de nuvem multi:** para soluções de nuvem complexas, utilize replicação de dados para sincronizar dados entre a base de dados do Azure para o MySQL e os fornecedores de nuvem diferente, incluindo máquinas virtuais e serviços de base de dados alojados nessas nuvens.

## <a name="limitations-and-considerations"></a>Limitações e considerações

### <a name="data-not-replicated"></a>Dados replicados não
O [ *base de dados mysql sistema* ](https://dev.mysql.com/doc/refman/5.7/en/system-database.html) no servidor principal não é replicado. Não são replicadas as alterações às contas e permissões no servidor primário. Se criar uma conta no servidor primário e esta conta tem de aceder ao servidor de réplica, em seguida, crie manualmente a mesma conta no lado do servidor de réplica. Para compreender as tabelas contidas na base de dados de sistema, consulte o [MySQL manual](https://dev.mysql.com/doc/refman/5.7/en/system-database.html).

### <a name="requirements"></a>Requisitos
- A versão do servidor primário tem de ser, pelo menos, o MySQL versão 5.6. 
- As versões de servidor primário e réplica têm de ser iguais. Por exemplo, ambos tem de ser MySQL versão 5.6 ou ambos tem de ser MySQL versão 5.7.
- Cada tabela tem de ter uma chave primária.
- Servidor primário, deve utilizar o motor de MySQL InnoDB.
- Utilizador tem de ter permissões para configurar o registo binário e criar novos utilizadores no servidor primário.

### <a name="other"></a>Outros
- Identificadores de transação global (GTID) não são suportados.

## <a name="next-steps"></a>Passos Seguintes
- Saiba como [configurar a replicação de dados](howto-data-in-replication.md)
