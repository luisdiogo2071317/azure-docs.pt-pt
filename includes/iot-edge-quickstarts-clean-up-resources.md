---
title: incluir ficheiro
description: incluir ficheiro
services: iot-edge
author: wesmc7777
ms.service: iot-edge
ms.topic: include
ms.date: 06/26/2018
ms.author: wesmc
ms.custom: include file
ms.openlocfilehash: 657006360105ac26091c54fe2e0deb523d1e6dea
ms.sourcegitcommit: e0a678acb0dc928e5c5edde3ca04e6854eb05ea6
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/13/2018
ms.locfileid: "39008506"
---
Se planeia avançar para o próximo artigo recomendado, pode manter os recursos e as configurações que criou e reutilizá-los.

Caso contrário, pode eliminar as configurações locais e os recursos do Azure que criou neste artigo para evitar custos. 

> [!IMPORTANT]
> A eliminação de recursos e grupos de recursos do Azure é irreversível. Quando estes itens são eliminados, o grupo de recursos e todos os recursos nele contidos são eliminados permanentemente. Confirme que não elimina acidentalmente o grupo de recursos ou recursos errados. Se tiver criado o hub IoT no interior de um grupo de recursos existente que tem recursos que pretende manter, elimine apenas o recurso do hub IoT, em vez de eliminar o grupo de recursos.
>

Para eliminar apenas o hub IoT, execute o seguinte comando. Substitua \<YourIoTHub> pelo seu nome de hub IoT e \<TestResources> pelo nome do seu grupo de recursos:

```azurecli-interactive
az iot hub delete --name <YourIoTHub> --resource-group <TestResources>
```


Para eliminar todo o grupo de recursos por nome:

1. Inicie sessão no [Portal do Azure](https://portal.azure.com) e selecione **Grupos de recursos**.

2. Na caixa de texto **Filtrar por nome**, introduza o nome do grupo de recursos que contém o seu hub IoT. 

3. À direita do grupo de recursos na lista de resultados, selecione as reticências (**...**) e, em seguida, selecione **Eliminar grupo de recursos**.

    ![Eliminar grupo de recursos](./media/iot-edge-quickstarts-clean-up-resources/iot-edge-delete-resource-group.png)

4. É-lhe pedido que confirme a eliminação do grupo de recursos. Reintroduza o nome do grupo de recursos para confirmar e, em seguida, selecione **Eliminar**. Após alguns instantes, o grupo de recursos e todos os recursos contidos no mesmo são eliminados.






