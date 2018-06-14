---
title: Gerir um plano de serviço de aplicações no Azure | Microsoft Docs
description: Saiba como efetuar tarefas diferentes para gerir um plano de serviço de aplicações.
keywords: do serviço de aplicações, o serviço de aplicações do azure, o dimensionamento, o plano do app service, alterar, criar, gerir, gestão
services: app-service
documentationcenter: ''
author: cephalin
manager: cfowler
editor: ''
ms.assetid: 4859d0d5-3e3c-40cc-96eb-f318b2c51a3d
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/09/2017
ms.author: cephalin
ms.openlocfilehash: 1dfe8a903e19ff524a1c4a0228e6aefcbe9ff183
ms.sourcegitcommit: 059dae3d8a0e716adc95ad2296843a45745a415d
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 02/09/2018
ms.locfileid: "29117685"
---
# <a name="manage-an-app-service-plan-in-azure"></a>Gerir um plano de serviço de aplicações no Azure

Um [plano do App Service do Azure](azure-web-sites-web-hosting-plans-in-depth-overview.md) fornece os recursos que tem de executar uma aplicação de serviço de aplicações. Este guia mostra como gerir um plano de serviço de aplicações.

## <a name="create-an-app-service-plan"></a>Crie um plano do Serviço de Aplicações

> [!TIP]
> Se tiver um ambiente de serviço de aplicações, consulte [criar um plano de serviço de aplicações num ambiente de serviço de aplicações](environment/app-service-web-how-to-create-a-web-app-in-an-ase.md#createplan).

Pode criar um plano de serviço de aplicações vazio ou pode criar um plano como parte da criação da aplicação.

1. No [portal do Azure](https://portal.azure.com), selecione **novo** > **Web + móvel**e, em seguida, selecione **aplicação Web** ou outro tipo de aplicação do app Service.

2. Selecione um plano de serviço aplicacional existente ou criar um plano para a nova aplicação.

   ![Crie uma aplicação no portal do Azure.][createWebApp]

   Para criar um plano de:

   a. Selecione **[+] criar novos**.

      ![Crie um plano de serviço de aplicações.][createASP] 

   b. Para **plano do App Service**, introduza o nome do plano.

   c. Para **localização**, selecione uma localização adequada.

   d. Para **escalão de preço**, selecione um escalão de preço adequado para o serviço. Selecione **ver todos os** para ver mais preços opções, tais como **livres** e **partilhados**. Depois de selecionar o escalão de preço, clique em de **selecione** botão.

<a name="move"></a>

## <a name="move-an-app-to-another-app-service-plan"></a>Mover uma aplicação para outro plano do App Service

Pode mover uma aplicação para outro plano do App Service, desde que o plano de origem e o plano de destino estiverem no _mesmo grupo de recursos e a região geográfica_.

1. No [portal do Azure](https://portal.azure.com), navegue para a aplicação que pretende mover.

2. No menu, procure o **plano do App Service** secção.

3. Selecione **plano de serviço de aplicações de alteração** para abrir o **plano do App Service** Seletor.

   ![Seletor do plano de serviço de aplicações.][change] 

4. No **plano do App Service** Seletor, selecione um existente planear mover esta aplicação para.   

> [!IMPORTANT]
> O **plano do App Service/selecionar** página é filtrada pelos seguintes critérios: 
> - Existe no mesmo grupo de recursos 
> - Existe na mesma região geográfica 
> - Existe o mesmo espaço de Web  
> 
> A _espaço Web_ é uma construção lógica no App Service, que define um agrupamento de recursos do servidor. Uma região geográfica (por exemplo, EUA oeste) contém muitas webspaces para alocar os clientes que utilizam o serviço de aplicações. Atualmente, não é possível mover recursos do serviço de aplicações entre webspaces. 
> 

[!INCLUDE [app-service-dev-test-note](../../includes/app-service-dev-test-note.md)]

Cada plano tem o seu próprio escalão de preço. Por exemplo, mover um site a partir um **livres** camada para um **padrão** camada permite que todas as aplicações atribuídas para utilizar as funcionalidades e recursos do **padrão** camada. No entanto, a mover uma aplicação de um plano superior camadas para um plano de camadas inferiores significa que já não tem acesso a algumas funcionalidades. Se a sua aplicação utiliza uma funcionalidade que não está disponível no plano de destino, receberá um erro que indica qual o recurso está em utilização que não está disponível. 

Por exemplo, se uma das suas aplicações utiliza certificados SSL, poderá ver esta mensagem de erro:

`Cannot update the site with hostname '<app_name>' because its current SSL configuration 'SNI based SSL enabled' is not allowed in the target compute mode. Allowed SSL configuration is 'Disabled'.`

Neste caso, antes de pode mover a aplicação para o plano de destino, terá uma:
- Dimensionar o escalão de preço do plano de destino para **básico** ou superior.
- Remova todas as ligações de SSL para a sua aplicação.

## <a name="move-an-app-to-a-different-region"></a>Mover uma aplicação numa região diferente

A região na qual a aplicação é executada é a região do plano de serviço de aplicações está a ser. No entanto, não é possível alterar a região de um plano de serviço de aplicações. Se pretender executar a aplicação numa região diferente, uma alternativa é aplicação clonagem. A clonagem efetua uma cópia da sua aplicação num plano de serviço aplicacional novo ou existente em qualquer região.

Pode encontrar **Clone aplicação** no **ferramentas de desenvolvimento** secção do menu.

> [!IMPORTANT]
> A clonagem tem algumas limitações. Pode ler sobre os mesmos em [clonagem de aplicação do serviço de aplicações do Azure](app-service-web-app-cloning.md).

## <a name="scale-an-app-service-plan"></a>Dimensionar um plano de serviço de aplicações

Para aumentar verticalmente a um serviço de aplicações plano do escalão de preço, consulte [aumentar verticalmente a uma aplicação no Azure](web-sites-scale.md).

Para ampliar a contagem de instâncias de uma aplicação, consulte [dimensionar a contagem de instâncias manual ou automaticamente](../monitoring-and-diagnostics/insights-how-to-scale.md).

<a name="delete"></a>

## <a name="delete-an-app-service-plan"></a>Eliminar um plano de serviço de aplicações

Para evitar encargos inesperados, ao eliminar a última aplicação num plano do serviço de aplicações, o serviço de aplicações também elimina o plano por predefinição. Se optar por manter o plano em vez disso, deve alterar o plano para **livres** camada pelo que não está a cobrados.

> [!IMPORTANT]
> Planos de serviço de aplicações que não tem aplicações associadas ainda pagar porque podem continuar a reserva de instâncias VM configuradas.

## <a name="next-steps"></a>Passos Seguintes

> [!div class="nextstepaction"]
> [Aumentar verticalmente a uma aplicação no Azure](web-sites-scale.md)

[change]: ./media/azure-web-sites-web-hosting-plans-in-depth-overview/change-appserviceplan.png
[createASP]: ./media/azure-web-sites-web-hosting-plans-in-depth-overview/create-appserviceplan.png
[createWebApp]: ./media/azure-web-sites-web-hosting-plans-in-depth-overview/create-web-app.png
