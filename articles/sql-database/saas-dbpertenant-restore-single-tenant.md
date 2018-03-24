---
title: Restaurar uma base de dados SQL do Azure numa aplicação SaaS multi-inquilino | Microsoft Docs
description: Saiba como restaurar a base de dados SQL de um único inquilino após a eliminação acidental de dados
keywords: tutorial de base de dados sql
services: sql-database
author: stevestein
manager: craigg
ms.service: sql-database
ms.custom: scale out apps
ms.topic: article
ms.date: 05/10/2017
ms.author: sstein
ms.reviewer: billgib
ms.openlocfilehash: 77741c39387dbfc8817b6494f8d79c424e1a498f
ms.sourcegitcommit: 48ab1b6526ce290316b9da4d18de00c77526a541
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 03/23/2018
---
# <a name="restore-a-single-tenant-with-a-database-per-tenant-saas-application"></a>Restaurar um inquilino único com uma aplicação SaaS a base de dados por inquilino

O modelo de base de dados por inquilino torna mais fácil restaurar um inquilino único para um ponto anterior no tempo sem afetar outros inquilinos.

Neste tutorial, saiba dois padrões de recuperação de dados:

> [!div class="checklist"]

> * Restaure uma base de dados para uma base de dados em paralelo (lado a lado).
> * Restaure uma base de dados no local, substituir a base de dados existente.


|||
|:--|:--|
| Restaurar para uma base de dados paralela | Este padrão pode ser utilizado para tarefas como a revisão, auditoria e a conformidade para permitir que um inquilino para inspecionar os dados de um ponto anterior. Base de dados atual do inquilino permanece inalterado e online. |
| Restauro no local | Este padrão é normalmente utilizado para recuperar um inquilino para um ponto anterior, depois de um inquilino elimina acidentalmente ou ficarão corrompidos dados. A base de dados original é colocado offline e substituído com a base de dados restaurada. |
|||

Para concluir este tutorial, confirme que conclui os pré-requisitos seguintes:

* A aplicação Wingtip SaaS é implementada. Para implementar em menos de cinco minutos, consulte [implementar e explorar a aplicação Wingtip SaaS](saas-dbpertenant-get-started-deploy.md).
* O Azure PowerShell está instalado. Para obter mais informações, consulte [começar com o Azure PowerShell](https://docs.microsoft.com/powershell/azure/get-started-azureps).

## <a name="introduction-to-the-saas-tenant-restore-patterns"></a>Introdução aos padrões de restauro de inquilino de SaaS

Existem dois padrões de simples para restaurar dados de um inquilino individual. Uma vez que são isoladas umas das outras bases de dados do inquilino, restaurar um inquilino não tem impacto nos dados de qualquer outro inquilino. A funcionalidade de (PITR) de ponto no tempo-restauro SQL Database do Azure é utilizada em ambos os padrões. PITR sempre cria uma nova base de dados.   

* **Restaurar em paralelo**: padrão de primeiro, uma nova base de dados paralela é criado em conjunto com a base de dados atual do inquilino. O inquilino, em seguida, é atribuído acesso só de leitura para a base de dados restaurada. Os dados restaurados podem ser revistos e potencialmente utilizados para substituir os valores de dados atual. Está a funcionar para o designer de aplicação para determinar a forma como o inquilino acede a base de dados restaurada e que opções de recuperação são fornecidas. Basta permitindo que o inquilino rever os seus dados, um ponto anterior poderá ser tudo o que é necessário em alguns cenários. 

* **Restaurar assegurados**: O segundo padrão é útil se dados foi perdidos ou corrompidos e o inquilino pretende reverter para um ponto anterior. O inquilino é colocado offline enquanto a base de dados é restaurada. A base de dados original é eliminado e a base de dados restaurada é alterado. A cadeia de cópia de segurança da base de dados original permanece acessível após a eliminação, para que possa restaurar a base de dados para um ponto anterior no tempo, se necessário.

Se a base de dados utiliza [georreplicação](sql-database-geo-replication-overview.md) e restaurar em paralelo, recomendamos que copie todos os dados necessários a partir da cópia restaurada na base de dados original. Se substituir a base de dados original com a base de dados restaurada, terá de reconfigurar e ressincronize a georreplicação.

## <a name="get-the-wingtip-tickets-saas-database-per-tenant-application-scripts"></a>Obter os scripts de aplicação de base de dados por inquilino Wingtip bilhetes SaaS

Os scripts de base de dados do Wingtip bilhetes SaaS multi-inquilino e o código fonte da aplicação, estão disponíveis no [WingtipTicketsSaaS DbPerTenant](https://github.com/Microsoft/WingtipTicketsSaaS-DbPerTenant) repositório do GitHub. Para obter os passos transferir e desbloquear os scripts de Wingtip SaaS de pedidos de suporte, consulte o [orientações gerais](saas-tenancy-wingtip-app-guidance-tips.md).

## <a name="before-you-start"></a>Antes de começar

Quando é criada uma base de dados, pode demorar 10 a 15 minutos antes da primeira cópia de segurança completa está disponível para restaurar a partir de. Se acabou de instalar a aplicação, poderá ter de aguardar alguns minutos antes de tentar este cenário.

## <a name="simulate-a-tenant-accidentally-deleting-data"></a>Simular um inquilino eliminação acidental de dados

Para demonstrar estes cenários de recuperação, primeiro "" elimina acidentalmente um evento de uma das bases de dados do inquilino. 

### <a name="open-the-events-app-to-review-the-current-events"></a>Abra a aplicação de eventos para rever os eventos atuais

1. Abra o Hub de eventos (http://events.wtp.&lt; utilizador&gt;. trafficmanager.net) e selecione **Contoso Concert Hall**.

   ![Hub de eventos](media/saas-dbpertenant-restore-single-tenant/events-hub.png)

2. Desloque-se a lista de eventos e tome nota do último evento na lista.

   ![Aparece o último evento](media/saas-dbpertenant-restore-single-tenant/last-event.png)


### <a name="accidentally-delete-the-last-event"></a>Eliminar "Acidentalmente" último evento

1. No ISE do PowerShell, abra... \\Learning módulos\\continuidade do negócio e recuperação após desastre\\RestoreTenant\\*demonstração RestoreTenant.ps1*e defina o valor seguinte:

   * **$DemoScenario** = **1**, *eliminar último evento (com vendas nenhuma permissão)*.
2. Prima F5 para executar o script e elimine o último evento. É apresentada a seguinte mensagem de confirmação:

   ```Console
   Deleting last unsold event from Contoso Concert Hall ...
   Deleted event 'Seriously Strauss' from Contoso Concert Hall venue.
   ```

3. Abre a página de eventos da Contoso. Desloque para baixo e certifique-se de que o evento removido. Se o evento é ainda na lista, selecione **atualizar** e certifique-se de que foi removido.

   ![Último evento removido](media/saas-dbpertenant-restore-single-tenant/last-event-deleted.png)


## <a name="restore-a-tenant-database-in-parallel-with-the-production-database"></a>Restaurar uma base de dados de inquilino em paralelo com a base de dados de produção

Neste exercício restaura a base de dados de Contoso Concert Hall para um ponto no tempo antes do evento foi eliminado. Este cenário pressupõe que pretende rever os dados eliminados numa base de dados em paralelo.

 O *restauro TenantInParallel.ps1* script cria um com o nome da base de dados do inquilino paralelas *ContosoConcertHall\_antigo*, com uma entrada de catálogo paralela. Este padrão de restauro é mais adequada para recuperar a partir de uma perda de dados secundárias. Também pode utilizar este padrão se precisar de rever os dados para fins de auditorias ou de compatibilidade. É a abordagem recomendada quando utiliza [georreplicação](sql-database-geo-replication-overview.md).

1. Concluir o [simular um inquilino acidentalmente eliminar dados](#simulate-a-tenant-accidentally-deleting-data) secção.
2. No ISE do PowerShell, abra... \\Learning módulos\\continuidade do negócio e recuperação após desastre\\RestoreTenant\\_demonstração RestoreTenant.ps1_.
3. Definir **$DemoScenario** = **2**, *inquilino de restauro em paralelo*.
4. Para executar o script, prima F5.

O script restaura a base de dados do inquilino para um ponto no tempo antes de eliminar o evento. A base de dados é restaurada para uma nova base de dados com o nome _ContosoConcertHall\_antigo_. Os metadados de catálogo que existe nesta base de dados restaurada é eliminado e, em seguida, a base de dados é adicionada ao catálogo de utilizando uma chave construída a partir de *ContosoConcertHall\_antigo* nome.

O script de demonstração abre a página de eventos para esta nova base de dados do inquilino no seu browser. Tenha em atenção a partir do URL ```http://events.wingtip-dpt.&lt;user&gt;.trafficmanager.net/contosoconcerthall_old``` que esta página mostra os dados da base de dados restaurada onde *_old* é adicionada ao nome.

Desloque-se os eventos listados no browser para confirmar que o evento eliminado na secção anterior foi restaurado.

Expor o inquilino restaurado como um inquilino adicional, com as suas próprias aplicações de eventos, é pouco provável ser como fornecer um acesso de inquilino para dados restaurados. Serve para ilustrar o padrão de restauro. Normalmente, conceder acesso só de leitura para os dados antigos e manter a base de dados restaurada durante um período de tempo definido. No exemplo, pode eliminar a entrada de inquilino restaurada depois de está a terminar, executando o _remover restaurada inquilino_ cenário.

1. Definir **$DemoScenario** = **4**, *remover restaurada inquilino*.
2. Para executar o script, prima F5.
3. O *ContosoConcertHall\_antigo* entrada agora é eliminada do catálogo. Feche a página de eventos para este inquilino no seu browser.


## <a name="restore-a-tenant-in-place-replacing-the-existing-tenant-database"></a>Restaurar um inquilino no local, substituir a base de dados existente do inquilino

Neste exercício restaura o inquilino Contoso Concert Hall para um ponto antes do evento foi eliminado. O *restauro TenantInPlace* script restaura uma base de dados do inquilino para uma nova base de dados e elimina o original. Neste padrão de restauro é mais adequado para recuperar de danos nos dados grave e o inquilino poderá ter de contemplar perda de dados importantes.

1. No ISE do PowerShell, abra o **demonstração RestoreTenant.ps1** ficheiro.
2. Definir **$DemoScenario** = **5**, *inquilino de restauro no local*.
3. Para executar o script, prima F5.

O script restaura a base de dados do inquilino para um ponto antes do evento foi eliminado. Primeiro demora inquilino Contoso Concert Hall offline para prevenir mais atualizações. Em seguida, é criada uma base de dados paralela restaurando a partir do ponto de restauro. A base de dados restaurada é denominado com um carimbo de hora para se certificar de que o nome de base de dados não entra em conflito com o nome de base de dados existente do inquilino. Em seguida, a base de dados do inquilino antigo é eliminado e a base de dados restaurada é mudado para o nome de base de dados original. Por fim, a Contoso Concert Hall seja colocado online, para permitir o acesso de aplicação para a base de dados restaurada.

Restaurar com êxito a base de dados para um ponto no tempo antes do evento foi eliminado. Quando o **eventos** é aberta a página, confirme que o último evento foi restaurado.

Depois de restaurar a base de dados, que demora a outro 10 a 15 minutos, antes da primeira cópia de segurança completa está disponível para restaurar a partir de novo. 

## <a name="next-steps"></a>Passos Seguintes

Neste tutorial, ficou a saber como:

> [!div class="checklist"]

> * Restaure uma base de dados para uma base de dados em paralelo (lado a lado).
> * Restaure uma base de dados no local.

Repita o [esquema de base de dados do inquilino gerir](saas-tenancy-schema-management.md) tutorial.

## <a name="additional-resources"></a>Recursos adicionais

* [Tutoriais adicionais que criar na aplicação Wingtip SaaS](saas-dbpertenant-wingtip-app-overview.md#sql-database-wingtip-saas-tutorials)
* [Descrição geral da continuidade do negócio com a SQL Database do Azure](sql-database-business-continuity.md)
* [Saiba mais sobre as cópias de segurança de base de dados SQL](sql-database-automated-backups.md)
