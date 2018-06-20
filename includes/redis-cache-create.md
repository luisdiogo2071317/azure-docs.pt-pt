---
title: incluir ficheiro
description: incluir ficheiro
services: redis-cache
author: wesmc7777
ms.service: cache
ms.topic: include
ms.date: 03/28/2018
ms.author: wesmc
ms.custom: include file
ms.openlocfilehash: 6a96bd7c3d7d02f181a7d7513edb3bb39881274f
ms.sourcegitcommit: 7de1432648c4ff3bcd09530c079418477d9f4d00
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/18/2018
ms.locfileid: "35719435"
---
1. Para criar uma cache, primeiro, inicie sessão no [portal do Azure](https://portal.azure.com). Em seguida, selecione **Criar um recurso** > **Bases de dados** > **Cache de Redis**.

    ![Nova cache](media/redis-cache-create/redis-cache-new-cache-menu.png)

2. Em **Nova Cache de Redis**, configure as definições para a nova cache.

    | Definição      | Valor sugerido  | Descrição |
    | ------------ |  ------- | -------------------------------------------------- |
    | **Nome DNS** | Nome globalmente exclusivo | O nome da cache. Tem de ser uma cadeia entre 1 e 63 carateres e conter apenas números, letras e o caráter `-`. O nome da cache não pode começar nem terminar com o caráter `-` e os carateres `-` consecutivos não são válidos.  | 
    | **Subscrição** | A sua subscrição | A subscrição no âmbito da qual esta nova instância da Cache de Redis do Azure é criada. | 
    | **Grupo de recursos** |  *TestResources* | Nome do grupo de recursos novo no qual a cache será criada. Ao colocar todos os recursos de uma aplicação num grupo, pode geri-los em conjunto. Por exemplo, eliminar o grupo de recursos elimina todos os recursos associados à aplicação. | 
    | **Localização** | EUA Leste | Selecione uma [região](https://azure.microsoft.com/regions/) próxima de outros serviços que irão utilizar a cache. |
    | **[Escalão de preço](https://azure.microsoft.com/pricing/details/cache/)** |  C0 básico (Cache de 250 MB) |  O escalão de preço determina o tamanho, o desempenho e as funcionalidades que estão disponíveis para a cache. Para obter mais informações, veja [Azure Redis Cache Overview](../articles/redis-cache/cache-overview.md) (Descrição Geral da Cache de Redis do Azure). |
    | **Afixar ao dashboard** |  Selecionado | Afixe a nova cache ao seu dashboard para que esta seja fácil de localizar. |

    ![Criar cache](media/redis-cache-create/redis-cache-cache-create.png) 

3. Depois de configurar as definições da nova cache, selecione **Criar**. 

    A criação da cache pode demorar alguns minutos. Para verificar o estado, pode monitorizar o progresso no dashboard. Uma vez criada, a nova cache apresenta o estado **Em Execução** e está pronta para ser utilizada.

    ![Cache criada](media/redis-cache-create/redis-cache-cache-created.png)

