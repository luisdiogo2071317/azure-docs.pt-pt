---
title: "Monitorizar as APIs publicadas na Gestão de API do Azure | Microsoft Docs"
description: "Siga os passos deste tutorial para aprender a monitorizar a sua API na Gestão de API do Azure."
services: api-management
documentationcenter: 
author: juliako
manager: cfowler
editor: 
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.custom: mvc
ms.topic: tutorial
ms.date: 11/19/2017
ms.author: apimpm
ms.openlocfilehash: db1ed08c4d4c9e9abd525ec13f5511da82ee1fe4
ms.sourcegitcommit: 9d317dabf4a5cca13308c50a10349af0e72e1b7e
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 02/01/2018
---
# <a name="monitor-published-apis"></a>Monitorizar as APIs publicadas

O Azure Monitor é um serviço que fornece uma única origem para monitorizar todos os recursos do Azure. Com o Azure Monitor, pode visualizar, consultar, encaminhar, arquivar e tomar medidas relativamente a métricas e registos provenientes de recursos do Azure, como a Gestão de API. 

Neste tutorial, ficará a saber como:

> [!div class="checklist"]
> * Ver registos de atividades
> * Ver registos de diagnóstico
> * Ver métricas da API 
> * Configurar uma regra de alerta quando a API recebe chamadas não autorizadas

O vídeo seguinte mostra como monitorizar a Gestão de API através do Azure Monitor. 

> [!VIDEO https://channel9.msdn.com/Blogs/AzureApiMgmt/Monitor-API-Management-with-Azure-Monitor/player]
>
>

## <a name="prerequisites"></a>Pré-requisitos

+ Conclua o guia de início rápido seguinte: [Criar uma instância da Gestão de API do Azure](get-started-create-service-instance.md).
+ Conclua também o tutorial seguinte: [Importar e publicar a sua primeira API](import-and-publish.md).

[!INCLUDE [api-management-navigate-to-instance.md](../../includes/api-management-navigate-to-instance.md)]

## <a name="diagnostic-logs"></a>Ver registos de atividades

Os registos de atividades fornecem informações aprofundadas sobre as operações executadas nos serviços de Gestão de API. Ao utilizar registos de atividades, pode determinar o "quê, quem e quando" de quaisquer operações de escrita (PUT, POST, DELETE) efetuadas nos seus serviços de Gestão de API. 

> [!NOTE]
> Os registos de atividades não incluem operações de leitura (GET) nem operações executadas no Portal do publicador clássico ou com as APIs de Gestão originais.

Pode aceder aos registos de atividades no serviço de Gestão de API ou aceder aos registos de todos os recursos do Azure no Azure Monitor. 

Para ver registos de atividades:

1. Selecione a instância de serviço APIM.
2. Clique em **Registo de atividades**.

## <a name="view-diagnostic-logs"></a>Ver registos de diagnóstico

Os registos de diagnóstico fornecem informações avançadas sobre operações e erros que são importantes para auditoria, bem como para fins de resolução de problemas. Os registos de diagnóstico diferem dos registos de atividades. Os registos de atividades fornecem informações aprofundadas sobre as operações executadas nos recursos do Azure. Os registos de diagnóstico fornecem informações aprofundadas sobre as operações executadas pelo próprio recurso.

Para aceder a registos de diagnóstico:

1. Selecione a instância de serviço APIM.
2. Clique em **Registo de diagnóstico**.

## <a name="view-metrics-of-your-apis"></a>Ver métricas das APIs

A Gestão de API emite métricas a cada minuto, o que lhe permite ter visibilidade quase em tempo real sobre o estado geral e o estado de funcionamento das suas APIs. Segue-se um resumo de algumas das métricas disponíveis:

* Capacidade (pré-visualização): ajuda-o a tomar decisões sobre a atualização/mudança para uma versão anterior dos serviços APIM. A métrica é emitida por minuto e reflete a capacidade do gateway no momento da criação de relatórios. A métrica varia entre 0 e 100 e é calculada com base nos recursos do gateway, como a utilização da CPU e da memória.
* Total de Pedidos do Gateway: número de pedidos da API num determinado período. 
* Pedidos do Gateway Com Êxito: número de pedidos da API que receberam códigos de resposta HTTP com êxito, incluindo 304, 307 e tudo o que for inferior a 301 (por exemplo, 200). 
* Pedidos do Gateway Falhados: número de pedidos da API que receberam códigos de resposta HTTP erróneos, incluindo 400 e tudo o que for superior a 500.
* Pedidos do Gateway Não Autorizados: número de pedidos da API que receberam códigos de resposta HTTP, incluindo 401, 403 e 429. 
* Outros Pedidos do Gateway: número de pedidos da API que receberam códigos de resposta HTTP que não pertencem a nenhuma das categorias anteriores (por exemplo, 418).

Para aceder a métricas:

1. Selecione **Métricas** no menu junto à parte inferior da página.
2. Na lista pendente, selecione as métricas que lhe interessam (pode adicionar múltiplas métricas). 
    
    Por exemplo, selecione **Total de Pedidos do Gateway** e **Pedidos do Gateway Falhados** na lista de métricas disponíveis.
3. O gráfico mostra o número total de chamadas à API. Mostra também o número de chamadas à API que falharam. 

## <a name="set-up-an-alert-rule-for-unauthorized-request"></a>Configurar uma regra de alerta para um pedido não autorizado

Pode configurar a receção de alertas com base em métricas e registos de atividades. O Azure Monitor permite-lhe configurar um alerta para fazer o seguinte quando é acionado:

* Enviar uma notificação por e-mail
* Chamar um webhook
* Invocar uma Aplicação Lógica do Azure

Para configurar alertas:

1. Selecione **Regras de alerta** na barra de menus junto à parte inferior da página.
2. Selecione **Adicionar alerta de métrica**.
3. Introduza um **Nome** para este alerta.
4. Selecione **Pedidos do Gateway Não Autorizados** como métrica a monitorizar.
5. Selecione **Proprietários, contribuidores e leitores do e-mail**.
6. Prima **OK**.
7. Tente chamar a API da Conferência sem uma chave de API. Como proprietário deste serviço de Gestão de API, receberá um alerta por e-mail. 

    > [!TIP]
    > A regra de alerta também pode chamar um Web Hook ou uma Aplicação Lógica do Azure quando é acionada.

    ![set-up-alert](./media/api-management-azure-monitor/set-up-alert.png)

## <a name="next-steps"></a>Passos seguintes

Neste tutorial, ficou a saber como:

> [!div class="checklist"]
> * Ver registos de atividades
> * Ver registos de diagnóstico
> * Ver métricas da API 
> * Configurar uma regra de alerta quando a API recebe chamadas não autorizadas

Avance para o tutorial seguinte:

> [!div class="nextstepaction"]
> [Rastrear chamadas](api-management-howto-api-inspector.md)
