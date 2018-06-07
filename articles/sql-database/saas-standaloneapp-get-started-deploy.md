---
title: Tutorial de SaaS multi-inquilino - SQL Database do Azure | Microsoft Docs
description: Implementar e explorar uma aplicação SaaS autónomo único inquilino, que utiliza a SQL Database do Azure.
keywords: tutorial de base de dados sql
services: sql-database
author: stevestein
manager: craigg
ms.service: sql-database
ms.custom: scale out apps
ms.topic: conceptual
ms.date: 04/01/2018
ms.author: genemi
ms.openlocfilehash: 32cfa2e9bd48dd4e27da5c4010391c032d67d96b
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/01/2018
ms.locfileid: "34644727"
---
# <a name="deploy-and-explore-a-standalone-single-tenant-application-that-uses-azure-sql-database"></a>Implementar e explorar uma aplicação de inquilino único autónoma que utiliza a SQL Database do Azure

Neste tutorial, implementar e explorar a aplicação de exemplo Wingtip bilhetes SaaS desenvolvida utilizando o padrão de aplicação autónoma ou a aplicação por inquilino.  A aplicação foi concebida para demonstram as funcionalidades da SQL Database do Azure que simplificam a ativar cenários de SaaS multi-inquilino.

A aplicação autónoma ou o padrão de aplicação por inquilino implementa uma instância de aplicação para cada inquilino.  Cada aplicação está configurada para um inquilino específico e implementada num grupo de recursos do Azure separado. Várias instâncias da aplicação são aprovisionadas para fornecer uma solução multi-inquilino. Este padrão é mais adequado a um número mais pequeno, de inquilinos onde o isolamento de inquilino é uma prioridade superior. O Azure tem programas de parceiro que permitem que os recursos implementados numa subscrição de um inquilino e geridos por um fornecedor de serviço em nome do inquilino. 

Neste tutorial, irá implementar três aplicações de autónomo para três inquilinos na sua subscrição do Azure.  Tem acesso total a explorar e trabalhar com os componentes de aplicações individuais.

Os scripts de código e gestão da origem de aplicação estão disponíveis no [WingtipTicketsSaaS StandaloneApp](https://github.com/Microsoft/WingtipTicketsSaaS-StandaloneApp) repositório do GitHub.


Neste tutorial, ficará a saber:

> [!div class="checklist"]
> * Como implementar a aplicação autónoma do Wingtip pedidos de suporte de SaaS.
> * Onde obter o código fonte da aplicação e os scripts de gestão.
> * Sobre os servidores e bases de dados que constituem a aplicação.

Tutoriais adicionais serão lançadas. Irá permitir-lhe explorar uma gama de cenários de gestão com base num padrão aplicação.   

## <a name="deploy-the-wingtip-tickets-saas-standalone-application"></a>Implementar a aplicação de SaaS autónomo Wingtip bilhetes

Implemente a aplicação para os inquilinos fornecidos três:

1. Clique em cada blue **implementar no Azure** botão para abrir o modelo de implementação no [portal do Azure](https://portal.azure.com). Cada modelo requer dois valores de parâmetro; um nome para um novo grupo de recursos e um nome de utilizador que distingue esta implementação de outras implementações da aplicação. O passo seguinte fornece detalhes para definir estes valores.<br><br>
    <a href="http://aka.ms/deploywingtipsa-contoso" target="_blank"><img style="vertical-align:middle" src="media/saas-standaloneapp-get-started-deploy/deploy.png"/></a> &nbsp; **Contoso Concert Hall**
<br><br>
    <a href="http://aka.ms/deploywingtipsa-dogwood" target="_blank"><img style="vertical-align:middle" src="media/saas-standaloneapp-get-started-deploy/deploy.png"/></a> &nbsp; **Dogwood Dojo**
<br><br>
    <a href="http://aka.ms/deploywingtipsa-fabrikam" target="_blank"><img style="vertical-align:middle" src="media/saas-standaloneapp-get-started-deploy/deploy.png"/></a> &nbsp; **Fabrikam Jazz Club**

2. Introduza os valores de parâmetros necessários para cada implementação.

    > [!IMPORTANT]
    > Alguns firewalls de autenticação e o servidor são intencionalmente protegidas para fins de demonstração. **Criar um novo grupo de recursos** para cada implementação de aplicação.  Não utilize um grupo de recursos existente. Não utilize a aplicação ou a quaisquer recursos cria, para produção. Elimine todos os grupos de recursos quando tiver terminado com as aplicações para parar a faturação relacionada.

    É melhor utilizar apenas letras minúsculas, números e hífenes nos nomes de recursos.
    * Para **grupo de recursos**, selecione a criar novo e, em seguida, forneça um nome para o grupo de recursos em minúsculas. **Wingtip-sa -\<venueName\>-\<utilizador\>**  é o padrão recomendado.  Para \<venueName\>, substitua o nome de venue sem espaços. Para \<utilizador\>, substitua o valor de utilizador a partir do abaixo.  Neste padrão, os nomes de grupo de recursos poderão dever *wingtip-sa contosoconcerthall af1*, *wingtip sa-dogwooddojo af1*, *wingtip-sa fabrikamjazzclub af1*.
    * Selecione um **localização** na lista pendente.

    * Para **utilizador** -recomendamos um valor de utilizador curto, tais como as suas iniciais e um dígito: por exemplo, *af1*.


3. **Implemente a aplicação**.

    * Clique para aceitar os termos e condições.
    * Clique em **Comprar**.

4. Monitorizar o estado de todas as implementações de três clicando **notificações** (no ícone de campainha para a direita da caixa de pesquisa). Implementar as aplicações demora cerca de cinco minutos.


## <a name="run-the-applications"></a>Executar as aplicações

A aplicação showcases venues que alojam os eventos.  Os venues são os inquilinos da aplicação. Cada venue obtém um web site personalizado para listar os respetivos eventos e propor pedidos de suporte. Os tipos de venue incluem concert halls, jazz clubs e clubs desporto. No exemplo, o tipo de venue determina a fotografia de fundo apresentada no web site do venue.   No modelo de aplicação autónoma, cada venue tem uma instância de aplicacionais separados com a sua própria base de dados de SQL Server autónomo.

1. Abra a página de eventos para cada um dos três inquilinos no separadores de browser separados:

    - http://events.contosoconcerthall.&lt; utilizador&gt;. trafficmanager.net
    - http://events.dogwooddojo.&lt; utilizador&gt;. trafficmanager.net
    - http://events.fabrikamjazzclub.&lt; utilizador&gt;. trafficmanager.net

    (No cada URL, substitua &lt;utilizador&gt; com o valor de utilizador da sua implementação.)

   ![Eventos](./media/saas-standaloneapp-get-started-deploy/fabrikam.png)

Para controlar a distribuição de pedidos recebidos, as utilizações de aplicação [ *Traffic Manager do Azure*](../traffic-manager/traffic-manager-overview.md). Cada instância da aplicação do inquilino específico inclui o nome de inquilino como parte do nome de domínio no URL. Todos os inquilinos URLs incluem específicos da sua **utilizador** valor. Os URLs siga o seguinte formato:
- http://events.&lt; venuename&gt;.&lt; utilizador&gt;. trafficmanager.net

Base de dados de cada inquilino **localização** está incluído nas definições da aplicação da aplicação implementada correspondente.

Num ambiente de produção, normalmente, cria um registo CNAME DNS para [ *apontar um domínio de internet da empresa* ](../traffic-manager/traffic-manager-point-internet-domain.md) para o URL do perfil do Gestor de tráfego.


## <a name="explore-the-servers-and-tenant-databases"></a>Explorar os servidores e bases de dados de inquilino

Vamos ver alguns dos recursos que foram implementados:

1. No [portal do Azure](http://portal.azure.com), navegue para a lista de grupos de recursos.
2. Deverá ver os grupos de recursos de três inquilino.
3. Abra o **wingtip-sa-fabrikam -&lt;utilizador&gt;**  grupo de recursos, que contém os recursos para a implementação da Fabrikam Jazz Club.  O **fabrikamjazzclub -&lt;utilizador&gt;**  servidor contém o **fabrikamjazzclub** base de dados.

Cada base de dados do inquilino é um DTU 50 *autónomo* base de dados.

## <a name="additional-resources"></a>Recursos adicionais

<!--
* Additional [tutorials that build on the Wingtip SaaS application](sql-database-wtp-overview.md#sql-database-wingtip-saas-tutorials)
* To learn about elastic pools, see [*What is an Azure SQL elastic pool*](https://docs.microsoft.com/azure/sql-database/sql-database-elastic-pool)
* To learn about elastic jobs, see [*Managing scaled-out cloud databases*](https://docs.microsoft.com/azure/sql-database/sql-database-elastic-jobs-overview)
-->

- Para saber mais sobre as aplicações de SaaS multi-inquilino, consulte [padrões para aplicações de SaaS multi-inquilino de conceção](saas-tenancy-app-design-patterns.md).

 
## <a name="delete-resource-groups-to-stop-billing"></a>Eliminar grupos de recursos para parar a faturação ##

Quando tiver concluído a utilizar o exemplo, elimine todos os grupos de recursos que criou para parar a faturação associada.

## <a name="next-steps"></a>Passos Seguintes

Neste tutorial, ficou a saber:

> [!div class="checklist"]
> * Como implementar a aplicação autónoma do Wingtip pedidos de suporte de SaaS.
> * Sobre os servidores e bases de dados que constituem a aplicação.
> * Como eliminar os recursos de exemplo para parar a faturação relacionada.

Em seguida, tente o [aprovisionar e catálogo](saas-standaloneapp-provision-and-catalog.md) tutorial em que será a explorar a utilização de um catálogo de inquilinos que permite uma gama de cenários de inquilino em vários locais, tais como a análise de gestão e de inquilino do esquema.
 

