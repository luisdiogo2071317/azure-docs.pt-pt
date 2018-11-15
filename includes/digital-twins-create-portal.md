---
title: incluir ficheiro
description: incluir ficheiro
services: digital-twins
author: dsk-2015
ms.service: digital-twins
ms.topic: include
ms.date: 09/17/2018
ms.author: dkshir
ms.custom: include file
ms.openlocfilehash: e5e015c1e12e68f54ae7fd32082faed1abfebe74
ms.sourcegitcommit: b62f138cc477d2bd7e658488aff8e9a5dd24d577
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 11/13/2018
ms.locfileid: "51626403"
---
1. Inicie sessão no [portal do Azure](http://portal.azure.com).

1. No painel esquerdo, selecione **criar um recurso**. Procure **Digital Twins** e selecione **Digital Twins (pré-visualização)**. Selecione **criar** para iniciar o processo de implantação.

   ![Seleções para criar uma nova instância de duplos Digital](./media/create-digital-twins-portal/create-digital-twins.png)

1. No painel **Digital Twins**, introduza as seguintes informações:
   * **Nome do Recurso**: crie um nome exclusivo para a instância do Digital Twins.
   * **Subscrição**: escolha a subscrição que quer utilizar para criar esta instância do Digital Twins. 
   * **Grupo de recursos**: selecione ou crie um [grupo de recursos](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview#resource-groups) para a instância do Digital Twins.
   * **Localização**: selecione a localização mais próxima dos seus dispositivos.

    ![Painel digital de duplos com informações inseridas](./media/create-digital-twins-portal/create-digital-twins-param.png)

1. Reveja as suas informações de duplos Digital e, em seguida, selecione **criar**. A instância de duplos Digital poderá demorar alguns minutos a ser criada. Pode monitorizar o progresso no painel **Notificações**.

1. Abra o painel **Descrição geral** da instância do Digital Twins. Tenha em atenção a ligação em **gestão de API**.

   O **gestão de API** URL é formatado como: _https://yourDigitalTwinsName.yourLocation.azuresmartspaces.net/management/swagger_. Este URL leva-o para a documentação da API REST do Azure Digital Twins que se aplica à sua instância. Leia [Como utilizar o Azure Digital Twins Swagger](../articles/digital-twins/how-to-use-swagger.md) para saber como ler e utilizar esta documentação de API.

    Modificar a **gestão de API** URL nesse formato: _https://yourDigitalTwinsName.yourLocation.azuresmartspaces.net/management/api/v1.0/_. A aplicação irá utilizar o URL modificado como URL base para aceder à sua instância. Copie este URL modificado para um ficheiro temporário. Precisará na próxima seção.

    ![API de Gestão](./media/create-digital-twins-portal/digital-twins-management-api.png)