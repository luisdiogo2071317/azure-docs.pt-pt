---
title: "Simulações de recuperação de desastres do SQL Server da base de dados | Microsoft Docs"
description: "Saiba orientações e melhores práticas para utilizar a SQL Database do Azure para efetue simulações de recuperação após desastre."
services: sql-database
documentationcenter: 
author: anosov1960
manager: jhubbard
editor: monicar
ms.assetid: b44a269c-fe2a-404f-b013-290030860bd1
ms.service: sql-database
ms.custom: business continuity
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.date: 10/20/2016
ms.workload: Inactive
ms.author: sashan
ms.reviewer: carlrab
ms.openlocfilehash: 73c2cbe978c980cbe1269b34cdb9f5ff86113e61
ms.sourcegitcommit: 6f33adc568931edf91bfa96abbccf3719aa32041
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/22/2017
---
# <a name="performing-disaster-recovery-drill"></a>Efetuar exercício de recuperação de desastre
Recomenda-se que a validação da preparação da aplicação para o fluxo de trabalho de recuperação é efetuada periodicamente. Verificar o comportamento da aplicação e as implicações de perda de dados e/ou a interrupção envolve a que a ativação pós-falha é uma boa prática de engenharia. Também é um requisito pela maioria das normas da indústria como parte da certificação de continuidade do negócio.

Efetuar um exercício de recuperação de desastre é composta por:

* Falha de camada de dados simulando
* Recuperar
* Validar a recuperação de post de integridade de aplicações

Dependendo de como pode [concebido a aplicação para a continuidade do negócio](sql-database-business-continuity.md), o fluxo de trabalho a executar o exercício pode variar. Este artigo descreve as melhores práticas para realizar um exercício de recuperação após desastre no contexto da SQL Database do Azure.

## <a name="geo-restore"></a>Georrestauro
Para evitar a potencial perda de dados quando realizar um exercício de recuperação após desastre, execute o exercício utilizando um ambiente de teste ao criar uma cópia de ambiente de produção e utilizá-la para verificar o fluxo de trabalho de ativação pós-falha da aplicação.

#### <a name="outage-simulation"></a>Simulação de falha
Para simular a interrupção, pode mudar o nome da base de dados de origem. Isto faz com que as falhas de conectividade de aplicação.

#### <a name="recovery"></a>Recuperação
* Efetuar o restauro georreplicação da base de dados para um servidor diferente, tal como descrito [aqui](sql-database-disaster-recovery.md).
* Alterar a configuração da aplicação para ligar à base de dados recuperada e siga o [configurar uma base de dados após a recuperação](sql-database-disaster-recovery.md) guia para concluir a recuperação.

#### <a name="validation"></a>Validação
* Conclua a desagregação ao verificar a recuperação de post de integridade de aplicação (incluindo cadeias de ligação, inícios de sessão, testar a funcionalidade básica ou outra validações parte dos procedimentos de signoffs padrão de aplicação).

## <a name="failover-groups"></a>Grupos de ativação pós-falha
Para uma base de dados que está protegida com grupos de ativação pós-falha, o exercício desagregação envolve a ativação pós-falha planeada para o servidor secundário. A ativação pós-falha planeada assegura que o primário e as bases de dados secundárias no grupo de ativação pós-falha permanecem sincronizadas quando as funções são mudadas. Ao contrário a ativação pós-falha não planeada, esta operação resulta numa perda de dados, pelo que a desagregação pode ser executada no ambiente de produção.

#### <a name="outage-simulation"></a>Simulação de falha
Para simular a interrupção, pode desativar a aplicação web ou a máquina virtual ligada à base de dados. Isto resulta em falhas de conectividade para os clientes web.

#### <a name="recovery"></a>Recuperação
* Certifique-se a configuração da aplicação em pontos de região DR para anterior secundário, que torna-se a nova principal totalmente acessível.
* Iniciar [a ativação pós-falha planeada](scripts/sql-database-setup-geodr-and-failover-database-powershell.md) do grupo de ativação pós-falha do servidor secundário.
* Siga o [configurar uma base de dados após a recuperação](sql-database-disaster-recovery.md) guia para concluir a recuperação.

#### <a name="validation"></a>Validação
Conclua a desagregação ao verificar a recuperação de post de integridade de aplicação (incluindo conectividade, testar a funcionalidade básica ou outras validações necessárias para o signoffs desagregação).

## <a name="next-steps"></a>Passos Seguintes
* Para saber mais sobre cenários de continuidade do negócio, consulte [cenários de continuidade](sql-database-business-continuity.md).
* Para saber mais sobre o SQL do Azure, base de dados automática de cópias de segurança, consulte [cópias de segurança automatizadas de base de dados SQL](sql-database-automated-backups.md)
* Para saber mais sobre a utilização de cópias de segurança automatizadas para recuperação, consulte [restaurar uma base de dados de cópias de segurança iniciou o serviço](sql-database-recovery-using-backups.md).
* Para saber mais sobre as opções de recuperação mais rápidas, consulte [grupos de replicação geográfica e ativação pós-falha Active Directory](sql-database-geo-replication-overview.md).  
