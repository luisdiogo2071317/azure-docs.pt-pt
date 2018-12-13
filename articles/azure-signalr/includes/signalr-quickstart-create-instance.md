---
title: incluir ficheiro
description: incluir ficheiro
author: anthonychu
ms.service: signalr
ms.topic: include
ms.date: 09/14/2018
ms.author: antchu
ms.custom: include file
ms.openlocfilehash: c95f05127d62d366e1ad78b45df23d04ced0164a
ms.sourcegitcommit: 1c1f258c6f32d6280677f899c4bb90b73eac3f2e
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/11/2018
ms.locfileid: "53262804"
---
## <a name="create-an-azure-signalr-service-instance"></a>Criar uma instância do Azure SignalR Service

A aplicação irá ligar a uma instância do SignalR Service no Azure.

1. Selecione o botão Novo, no canto superior esquerdo do portal do Azure. No ecrã Novo, escreva *SignalR Service* na caixa de pesquisa e prima enter.

    ![Procure o SignalR Service](../media/signalr-quickstart-azure-functions-javascript/signalr-quickstart-new.png)

1. Selecione **SignalR Service** nos resultados da pesquisa e, em seguida, selecione **Criar**.

1. Introduza as seguintes definições.

    | Definição      | Valor sugerido  | Descrição                                        |
    | ------------ |  ------- | -------------------------------------------------- |
    | **Nome do recurso** | Nome globalmente exclusivo | Nome que identifica a sua nova instância do SignalR Service. Os carateres válidos são `a-z`, `0-9` e `-`.  | 
    | **Subscrição** | A sua subscrição | A subscrição sob a qual esta nova instância do SignalR Service é criada. | 
    | **[Grupo de Recursos](../../azure-resource-manager/resource-group-overview.md)** |  myResourceGroup | Nome do novo grupo de recursos no qual irá criar a sua instância do SignalR Service. | 
    | **Localização** | EUA Oeste | Escolher uma [região](https://azure.microsoft.com/regions/) próxima de si. |
    | **Escalão de preço** | Gratuito | Experimente gratuitamente o Azure SignalR Service. |
    | **Contagem de unidades** |  Não aplicável | A contagem de unidades especifica quantas ligações a sua instância do SignalR Service pode aceitar. Pode configurar apenas no escalão Standard. |

    ![Criar o Serviço SignalR](../media/signalr-quickstart-azure-functions-javascript/signalr-quickstart-create.png)

1. Selecione **Criar** para começar a implementar a instância do SignalR Service.