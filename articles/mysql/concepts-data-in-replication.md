---
title: Replicar os dados na base de dados do Azure para MySQL.
description: Este artigo descreve os dados na replicação da base de dados do Azure para MySQL.
services: mysql
author: ajlam
ms.author: andrela
manager: kfile
editor: jasonwhowell
ms.service: mysql
ms.topic: article
ms.date: 08/31/2018
ms.openlocfilehash: 6135e4a0182f3af7db54eab974e4c307402185ab
ms.sourcegitcommit: 31241b7ef35c37749b4261644adf1f5a029b2b8e
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/04/2018
ms.locfileid: "43666081"
---
# <a name="replicate-data-into-azure-database-for-mysql"></a>Replicar os dados na base de dados do Azure para MySQL

Replicação de dados-in permite-lhe sincronizar dados de um servidor MySQL em execução no local, nas máquinas virtuais ou serviços de base de dados hospedados por outros fornecedores de cloud para a base de dados do Azure para o serviço MySQL. Replicação de dados baseia-se o registo binário (binlog) com base na posição replicação de ficheiros nativa ao MySQL. Para saber mais sobre a replicação de binlog, consulte a [descrição geral de replicação do MySQL binlog](https://dev.mysql.com/doc/refman/5.7/en/binlog-replication-configuration-overview.html). 

## <a name="when-to-use-data-in-replication"></a>Quando utilizar a replicação de dados
Os principais cenários para considerar a utilização de replicação de dados são:

- **Sincronização de dados híbrida:** com a replicação de dados, pode manter os dados sincronizados entre os servidores no local e a base de dados do Azure para MySQL. Esta sincronização é útil para criar aplicações híbridas. Este método é atraente quando existir um servidor da base de dados local, mas quiser mover os dados para uma região mais perto dos utilizadores finais.
- **Sincronização de várias Cloud:** para soluções de cloud complexas, utilize replicação de dados para sincronizar dados entre a base de dados do Azure para MySQL e fornecedores de cloud diferentes, incluindo máquinas virtuais e serviços de base de dados hospedados nessas nuvens.

## <a name="limitations-and-considerations"></a>Limitações e considerações

### <a name="data-not-replicated"></a>Dados replicados não
O [ *base de dados de sistema de mysql* ](https://dev.mysql.com/doc/refman/5.7/en/system-database.html) no servidor principal não é replicado. Alterações às contas e permissões no servidor principal não são replicadas. Se criar uma conta no servidor principal e esta conta tem de aceder ao servidor de réplica, em seguida, crie manualmente a mesma conta no lado do servidor de réplica. Para compreender quais tabelas estão contidas na base de dados do sistema, consulte a [MySQL manual](https://dev.mysql.com/doc/refman/5.7/en/system-database.html).

### <a name="requirements"></a>Requisitos
- A versão do servidor principal tem de ser, pelo menos, versão 5.6 do MySQL. 
- As versões de servidor mestre e de réplica tem de ser o mesmo. Por exemplo, ambos têm de ser MySQL versão 5.6 ou ambos têm de ser MySQL versão 5.7.
- Cada tabela tem de ter uma chave primária.
- Servidor mestre deve utilizar o motor MySQL InnoDB.
- Utilizador tem de ter permissões para configurar o registo binário e criar novos utilizadores no servidor principal.

### <a name="other"></a>Outros
- A replicação de dados-in é apenas suportado, em geral, objetivo e com a otimização de memória escalões de preço
- Identificadores de transação global (GTID) não são suportadas.

## <a name="next-steps"></a>Passos Seguintes
- Saiba como [configurar a replicação de dados-in](howto-data-in-replication.md)
