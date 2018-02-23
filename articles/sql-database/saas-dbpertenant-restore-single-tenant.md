---
title: "Restaurar uma base de dados SQL do Azure numa aplicação SaaS multi-inquilino | Microsoft Docs"
description: "Saiba como restaurar uma base de dados do SQL Server de inquilinos único após a eliminação acidental de dados"
keywords: tutorial de base de dados sql
services: sql-database
documentationcenter: 
author: stevestein
manager: craigg
editor: 
ms.assetid: 
ms.service: sql-database
ms.custom: scale out apps
ms.workload: Inactive
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/10/2017
ms.author: billgib;sstein
ms.openlocfilehash: 46471073f88247510f45d6c4152afa43be6e1aaa
ms.sourcegitcommit: d1f35f71e6b1cbeee79b06bfc3a7d0914ac57275
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 02/22/2018
---
# <a name="restore-a-single-tenant-with-a-database-per-tenant-saas-application"></a>Restaurar um inquilino único com uma base de dados por inquilino aplicação SaaS

A base de dados por inquilino modelo torna é fácil restaurar um inquilino único para um ponto anterior no tempo sem afetar outros inquilinos.

Neste tutorial aprende dois padrões de recuperação de dados:

> [!div class="checklist"]

> * Restaurar uma base de dados para uma base de dados em paralelo (side lado a lado)
> * Restaurar uma base de dados no local, substituir a base de dados existente


|||
|:--|:--|
| **Restaurar para uma base de dados paralela** | Este padrão pode ser utilizado para análise, a auditoria, de compatibilidade, etc. para permitir que um inquilino para inspecionar os dados de um ponto anterior.  Base de dados atual do inquilino permanece inalterado e online. |
| **Restauro no local** | Este padrão é normalmente utilizado para recuperar um inquilino para um ponto anterior, depois de um inquilino elimina acidentalmente ou ficarão corrompidos dados. A base de dados original é colocado offline e substituído com a base de dados restaurada. |
|||

Para concluir este tutorial, confirme que conclui os pré-requisitos seguintes:

* A aplicação Wingtip SaaS é implementada. Para implementar em menos de cinco minutos, consulte [implementar e explorar a aplicação Wingtip SaaS](saas-dbpertenant-get-started-deploy.md)
* O Azure PowerShell está instalado. Para obter mais detalhes, veja [Introdução ao Azure PowerShell](https://docs.microsoft.com/powershell/azure/get-started-azureps)

## <a name="introduction-to-the-saas-tenant-restore-patterns"></a>Introdução aos padrões de restauro de inquilino de SaaS

Existem dois padrões de simples para restaurar dados de um inquilino individual. Uma vez que são isoladas umas das outras bases de dados do inquilino, restaurar um inquilino não tem impacto nos dados de qualquer outro inquilino.  A funcionalidade de (PITR) de ponto no tempo-restauro de base de dados SQL é utilizada em ambos os padrões.  PITR sempre cria uma nova base de dados.   

No primeiro padrão, **restaurar em paralelo**, é criada uma nova base de dados paralela em conjunto com a base de dados atual do inquilino. O inquilino, em seguida, é atribuído acesso só de leitura para a base de dados restaurada. Os dados restaurados podem ser revistos e potencialmente utilizados para substituir os valores de dados atual. Está a funcionar para o designer de aplicação para determinar a forma como o inquilino acede a base de dados restaurada e que opções de recuperação são fornecidas. Basta permitindo que o inquilino rever os seus dados, um ponto anterior poderá ser tudo o que é necessário em alguns cenários. 

O segundo padrão, **restaurar assegurados**, é útil se os dados foram perdidos ou danificados e o inquilino pretende reverter para um ponto anterior.  O inquilino seja colocado offline enquanto a base de dados é restaurada. A base de dados original é eliminado e a base de dados restaurada é alterado. A cadeia de cópia de segurança da base de dados original permanece acessível após a eliminação, permitindo-lhe restaurar a base de dados para um ponto anterior no tempo, se necessário.

Se a base de dados utiliza [georreplicação](sql-database-geo-replication-overview.md) e restaurar em paralelo, recomenda-se copiar os dados necessários a partir da cópia restaurada na base de dados original. Se substituir a base de dados original com a base de dados restaurada, terá de reconfigurar e ressincronize a georreplicação.

## <a name="get-the-wingtip-tickets-saas-database-per-tenant-application-scripts"></a>Obter os scripts de aplicação Wingtip bilhetes SaaS da base de dados por inquilino

Os scripts de base de dados do Wingtip bilhetes SaaS multi-inquilino e o código fonte da aplicação, estão disponíveis no [WingtipTicketsSaaS DbPerTenant](https://github.com/Microsoft/WingtipTicketsSaaS-DbPerTenant) repositório do GitHub. Veja o [orientações gerais](saas-tenancy-wingtip-app-guidance-tips.md) para obter os passos transferir e os scripts de Wingtip SaaS de pedidos de desbloqueio.

## <a name="before-you-start"></a>Antes de começar

Quando é criada uma base de dados, pode demorar 10 a 15 minutos antes da primeira cópia de segurança completa está disponível para restaurar a partir de.  Se apenas acabou de instalar a aplicação poderá ter de aguardar alguns minutos antes de tentar este cenário.

## <a name="simulate-a-tenant-accidentally-deleting-data"></a>Simular um inquilino eliminação acidental de dados

Para demonstrar nestes cenários de recuperação, primeiro *'acidentalmente'* eliminar um evento de uma das bases de dados do inquilino. 

### <a name="open-the-events-app-to-review-the-current-events"></a>Abra a aplicação de eventos para rever os eventos atuais

1. Abra o *Hub de eventos* (http://events.wtp.&lt; utilizador&gt;. trafficmanager.net) e clique em **Contoso Concert Hall**:

   ![hub de eventos](media/saas-dbpertenant-restore-single-tenant/events-hub.png)

1. Desloque-se a lista de eventos e tome nota do último evento na lista:

   ![último evento](media/saas-dbpertenant-restore-single-tenant/last-event.png)


### <a name="accidentally-delete-the-last-event"></a>'Acidentalmente' Eliminar o último evento

1. Abra... \\Learning módulos\\continuidade do negócio e recuperação após desastre\\RestoreTenant\\*demonstração RestoreTenant.ps1* no *ISE do PowerShell*e defina o valor seguinte:
   * **$DemoScenario** = **1**, eliminar último evento (com vendas nenhuma permissão).
1. Prima **F5** para executar o script e eliminar o último evento. Deverá ver a mensagem de confirmação followng:

   ```Console
   Deleting last unsold event from Contoso Concert Hall ...
   Deleted event 'Seriously Strauss' from Contoso Concert Hall venue.
   ```

1. Abre a página de eventos da Contoso. Desloque para baixo e certifique-se de que o evento é removido. Se o evento é ainda na lista, clique em Atualizar e certifique-se de que é removido.

   ![último evento](media/saas-dbpertenant-restore-single-tenant/last-event-deleted.png)


## <a name="restore-a-tenant-database-in-parallel-with-the-production-database"></a>Restaurar uma base de dados de inquilino em paralelo com a base de dados de produção

Neste exercício restaura a base de dados de Contoso Concert Hall para um ponto no tempo antes do evento foi eliminado. Este cenário pressupõe-se que apenas pretende rever os dados eliminados numa base de dados em paralelo.

 O *restauro TenantInParallel.ps1* script cria um com o nome da base de dados do inquilino paralelas *ContosoConcertHall\_antigo*, com uma entrada de catálogo paralela. Este padrão de restauro é mais adequada para recuperar a partir de uma perda de dados secundária, ou se precisar de rever os dados de conformidade ou a fins de auditoria. Também é a abordagem recomendada quando estiver a utilizar [georreplicação](sql-database-geo-replication-overview.md).

1. Concluir o [simular um inquilino acidentalmente eliminar dados](#simulate-a-tenant-accidentally-deleting-data) secção.
1. No *ISE do PowerShell*, abra... \\Learning módulos\\continuidade do negócio e recuperação após desastre\\RestoreTenant\\_demonstração RestoreTenant.ps1_.
1. Definir **$DemoScenario** = **2**, *inquilino de restauro em paralelo*.
1. Prima **F5** para executar o script.

O script restaura a base de dados do inquilino para um ponto no tempo antes de eliminar o evento. A base de dados é restaurada para a nova base de dados com o nome _ContosoConcertHall\_antigo_. Os metadados de catálogo que existe nesta base de dados restaurada é eliminado e, em seguida, a base de dados é adicionada ao catálogo de utilizar uma chave construída a partir de *ContosoConcertHall\_antigo* nome.

O script de demonstração abre a página de eventos para esta nova base de dados do inquilino no seu browser. Tenha em atenção a partir do URL: ```http://events.wingtip-dpt.&lt;user&gt;.trafficmanager.net/contosoconcerthall_old``` que esta página é mostrar os dados da base de dados restaurada onde *_old* é adicionada ao nome.

Desloque-se os eventos listados no browser para confirmar que o evento eliminado na secção anterior foi restaurado.

Tenha em atenção que o inquilino restaurado a exposição como um inquilino adicional, com as suas próprias aplicações de eventos, é pouco provável ser como iria fornecer um acesso de inquilino para restaurar dados, mas serve para ilustrar o padrão de restauro. Na realidade, provavelmente, seria dar acesso só de leitura para os dados antigos e manter esta base de dados restaurada durante um período de tempo definido. No exemplo, pode eliminar a entrada de inquilino restaurada depois de terminar, executando o _remover restaurada inquilino_ cenário.

1. Definir **$DemoScenario** = **4**, *remover restaurada inquilino*
1. **Executar** **utilizando** **F5**
1. O *ContosoConcertHall\_antigo* entrada agora é eliminada do catálogo. Avançar e fechar a página de eventos para este inquilino no seu browser.


## <a name="restore-a-tenant-in-place-replacing-the-existing-tenant-database"></a>Restaurar um inquilino no local, substituir a base de dados existente do inquilino

Neste exercício restaura o inquilino Contoso Concert Hall para um ponto antes do evento foi eliminado. O *restauro TenantInPlace* script restaura uma base de dados do inquilino para uma nova base de dados e elimina o original.  Neste padrão de restauro é mais adequado para recuperar de danos nos dados grave porque poderá haver perda de dados significativas que o inquilino teria para acomodar.

1. Abra **demonstração RestoreTenant.ps1** ficheiro no ISE do PowerShell
1. Definir **$DemoScenario** = **5**, *inquilino de restauro no local*.
1. Executar utilizando **F5**.

O script restaura a base de dados do inquilino para um ponto antes do evento foi eliminado. Primeiro demora inquilino Contoso Concert Hall offline para impedir que as atualizações adicionais. Em seguida, é criada uma base de dados paralela restaurando a partir do ponto de restauro.  A base de dados restaurada é denominado com um carimbo para garantir que o nome de base de dados não coincide com o nome de base de dados existente do inquilino. Em seguida, a base de dados do inquilino antigo é eliminado e a base de dados restaurada é mudado para o nome de base de dados original. Por fim, a Contoso Concert Hall seja colocado online, para permitir o acesso de aplicação para a base de dados restaurada.

Restaurar com êxito a base de dados para um ponto no tempo antes do evento foi eliminado. Abre a página Eventos, por isso, confirme o último evento foi restaurado.

Tenha em atenção que, depois de restaurar a base de dados demora um mais 10 a 15 minutos antes da primeira cópia de segurança completa está disponível para restaurar a partir de novo. 

## <a name="next-steps"></a>Passos Seguintes

Neste tutorial, ficou a saber como:

> [!div class="checklist"]

> * Restaurar uma base de dados para uma base de dados em paralelo (side lado a lado)
> * Restaurar uma base de dados no local

[Gerir o esquema de base de dados do inquilino](saas-tenancy-schema-management.md)

## <a name="additional-resources"></a>Recursos adicionais

* [Tutoriais adicionais que tirar partido da aplicação Wingtip SaaS](saas-dbpertenant-wingtip-app-overview.md#sql-database-wingtip-saas-tutorials)
* [Descrição geral da continuidade do negócio com a SQL Database do Azure](sql-database-business-continuity.md)
* [Saiba mais sobre as cópias de segurança de base de dados SQL](sql-database-automated-backups.md)
