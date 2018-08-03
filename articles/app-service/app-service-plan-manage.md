---
title: Gerir um plano do serviço de aplicações no Azure | Documentos da Microsoft
description: Saiba como executar tarefas diferentes para gerir um plano do serviço de aplicações.
keywords: serviço de aplicações, serviço de aplicações do azure, dimensionável, plano de serviço de aplicações, alterar, criar, gerir, gestão
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
ms.openlocfilehash: 2c08522df598bd5c6313c3f026efe48e1c4a2c56
ms.sourcegitcommit: 1d850f6cae47261eacdb7604a9f17edc6626ae4b
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/02/2018
ms.locfileid: "39449364"
---
# <a name="manage-an-app-service-plan-in-azure"></a>Gerir um plano do serviço de aplicações no Azure

Uma [plano do serviço de aplicações do Azure](azure-web-sites-web-hosting-plans-in-depth-overview.md) fornece os recursos que tem de executar uma aplicação de serviço de aplicações. Este guia mostra como gerir um plano do serviço de aplicações.

## <a name="create-an-app-service-plan"></a>Crie um plano do Serviço de Aplicações

> [!TIP]
> Se tiver um ambiente de serviço de aplicações, veja [criar um plano do serviço de aplicações num ambiente de serviço de aplicações](environment/app-service-web-how-to-create-a-web-app-in-an-ase.md#createplan).

Pode criar um plano do serviço de aplicações vazio ou pode criar um plano como parte da criação de aplicações.

1. Na [portal do Azure](https://portal.azure.com), selecione **New** > **Web + móvel**e, em seguida, selecione **aplicação Web** ou outro tipo de aplicação de serviço de aplicações.

1. Selecione um plano de serviço de aplicações existente ou crie um plano para a nova aplicação.

   ![Crie uma aplicação no portal do Azure.][createWebApp]

   Para criar um plano:

   a. Selecione **[+] Criar um novo**.

      ![Crie um plano de serviço de aplicações.][createASP] 

   b. Para **plano do App Service**, introduza o nome do plano.

   c. Para **localização**, selecione uma localização adequada.

   d. Para **escalão de preço**, selecione um escalão de preço adequado para o serviço. Selecione **ver tudo** para ver mais opções, de preços, tal como **gratuito** e **partilhado**. Depois de selecionar o escalão de preço, clique nas **selecione** botão.

<a name="move"></a>

## <a name="move-an-app-to-another-app-service-plan"></a>Mover uma aplicação para outro plano do serviço de aplicações

Pode mover uma aplicação para outro plano do serviço de aplicações, enquanto o plano de origem e o plano de destino estão na _mesmo grupo de recursos e região geográfica_.

1. Na [portal do Azure](https://portal.azure.com), navegar para a aplicação que pretende mover.

1. No menu, procure o **plano do serviço de aplicações** secção.

1. Selecione **plano do serviço de aplicações de alteração** para abrir o **plano do App Service** Seletor.

   ![Seletor de plano de serviço de aplicações.][change] 

1. Na **plano do App Service** Seletor, selecione um existente planear mover esta aplicação em.   

> [!IMPORTANT]
> O **plano do serviço de aplicações selecione** página é filtrada pelos seguintes critérios: 
> - Existe no mesmo grupo de recursos 
> - Existe na mesma região geográfica 
> - Existe o mesmo espaço de Web  
> 
> R _espaço Web_ é uma construção lógica no serviço de aplicações que define um agrupamento de recursos do servidor. Uma região geográfica (por exemplo, E.U.A. oeste) contém espaços Web muitas para alocar os clientes que utilizam o serviço de aplicações. Atualmente, não é possível mover recursos do serviço de aplicações entre espaços Web. 
> 

[!INCLUDE [app-service-dev-test-note](../../includes/app-service-dev-test-note.md)]

Cada plano tem seu próprio para o escalão de preço. Por exemplo, mover um site a partir um **gratuito** escalão para um **padrão** escalão permite que todas as aplicações atribuídas ao mesmo a usar os recursos e recursos do **padrão** escalão. No entanto, a mover uma aplicação de um plano superior em camadas para um plano em camadas inferiores significa que já não tem acesso a determinados recursos. Se a sua aplicação utiliza uma funcionalidade que não está disponível no plano de destino, obterá um erro que mostra a qual o recurso está em utilização que não está disponível. 

Por exemplo, se uma das suas aplicações utiliza certificados SSL, poderá ver esta mensagem de erro:

`Cannot update the site with hostname '<app_name>' because its current SSL configuration 'SNI based SSL enabled' is not allowed in the target compute mode. Allowed SSL configuration is 'Disabled'.`

Neste caso, antes de prosseguir a aplicação para o plano de destino, precisa:
- Aumentar verticalmente o escalão de preço do plano de destino no **básica** ou superior.
- Remova todas as ligações de SSL para a sua aplicação.

## <a name="move-an-app-to-a-different-region"></a>Mover uma aplicação para uma região diferente

A região em que a aplicação for executada é a região do plano do serviço de aplicações está a ser. No entanto, não é possível alterar a região de um plano de serviço de aplicações. Se quiser executar a sua aplicação numa região diferente, uma alternativa é a aplicação de clonagem. A clonagem faz uma cópia da sua aplicação num plano do serviço de aplicações novo ou existente em qualquer região.

Pode encontrar **clonar aplicação** no **ferramentas de desenvolvimento** secção do menu.

> [!IMPORTANT]
> A clonagem tem algumas limitações. Pode ler sobre os mesmos em [clonagem de aplicação do serviço de aplicações do Azure](app-service-web-app-cloning.md).

## <a name="scale-an-app-service-plan"></a>Dimensionar um plano do serviço de aplicações

Para aumentar verticalmente o serviço de aplicações do plano do escalão de preço, consulte [aumentar verticalmente uma aplicação no Azure](web-sites-scale.md).

Para aumentar o número de instâncias de uma aplicação, consulte [dimensionar a contagem de instâncias manual ou automaticamente](../monitoring-and-diagnostics/insights-how-to-scale.md).

<a name="delete"></a>

## <a name="delete-an-app-service-plan"></a>Eliminar um plano do serviço de aplicações

Para evitar cobranças inesperadas, ao eliminar a última aplicação num plano do serviço de aplicações, serviço de aplicações também elimina o plano por predefinição. Se optar por manter o plano em vez disso, deve alterar o plano para **gratuito** camada para que não será cobrado.

> [!IMPORTANT]
> Planos de serviço de aplicações que não tem aplicações associadas a eles continuam a incorrer em custos porque eles continuam a reservar instâncias de VM configuradas.

## <a name="next-steps"></a>Passos Seguintes

> [!div class="nextstepaction"]
> [Aumentar verticalmente uma aplicação no Azure](web-sites-scale.md)

[change]: ./media/azure-web-sites-web-hosting-plans-in-depth-overview/change-appserviceplan.png
[createASP]: ./media/azure-web-sites-web-hosting-plans-in-depth-overview/create-appserviceplan.png
[createWebApp]: ./media/azure-web-sites-web-hosting-plans-in-depth-overview/create-web-app.png
