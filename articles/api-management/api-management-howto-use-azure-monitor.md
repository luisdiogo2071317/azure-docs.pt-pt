---
title: Monitor publicados APIs na API Management do Azure | Microsoft Docs
description: Siga os passos deste tutorial para saber como monitorizar a sua API na API Management do Azure.
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
ms.openlocfilehash: bdca9d4968e9e68314f350787907f15e417821f7
ms.sourcegitcommit: b854df4fc66c73ba1dd141740a2b348de3e1e028
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/04/2017
---
# <a name="monitor-published-apis"></a>Monitorizar APIs publicadas

Monitor do Azure é um serviço Azure que fornece uma única origem para a monitorização de todos os seus recursos do Azure. Com a monitorização do Azure, pode visualizar, consultar, encaminhar, arquivar e executar ações no métricas e registos proveniente de recursos do Azure, tais como a API Management. 

Neste tutorial, ficará a saber como:

> [!div class="checklist"]
> * Ver registos de atividades
> * Ver registos de diagnóstico
> * Veja as métricas da sua API 
> * Configurar uma regra de alerta quando a API obtém chamadas não autorizadas

O vídeo seguinte mostra como monitorizar a gestão de API utilizando o Monitor do Azure. 

> [!VIDEO https://channel9.msdn.com/Blogs/AzureApiMgmt/Monitor-API-Management-with-Azure-Monitor/player]
>
>

## <a name="prerequisites"></a>Pré-requisitos

+ Concluir o guia de introdução seguinte: [criar uma instância de API Management do Azure](get-started-create-service-instance.md).
+ Além disso, concluir o tutorial seguinte: [importação e publicar a sua primeira API](import-and-publish.md).

[!INCLUDE [api-management-navigate-to-instance.md](../../includes/api-management-navigate-to-instance.md)]

## <a name="diagnostic-logs"></a>Ver registos de atividade

Registos de atividade fornecem informações aprofundadas as operações que foram executadas no seu serviços de gestão de API. Utilizar registos de atividade, poderá determinar o "o que, quem e quando" para quaisquer operações (PUT, POST, DELETE) efetuadas nos seus serviços de gestão de API de escrita. 

> [!NOTE]
> Registos de atividade inclui as operações de leitura (GET) ou operações executadas no Portal clássico do publicador ou com as APIs de gestão original.

Pode aceder a registos de atividade no seu serviço de API Management ou aceder a registos de todos os seus recursos do Azure no Monitor do Azure. 

Para ver registos de atividade:

1. Do seu **API Management** instância, clique em **registo de atividade**.

## <a name="view-diagnostic-logs"></a>Ver registos de diagnóstico

Os registos de diagnóstico fornecem informações avançadas sobre operações e erros que são importantes para auditoria, bem como para fins de resolução de problemas. Registos de diagnóstico diferem dos registos de atividade. Registos de atividade fornecem informações sobre as operações que foram executadas no seus recursos do Azure. Registos de diagnóstico fornecem informações aprofundadas operations que o seu recurso efetuadas em si.

Para aceder a registos de diagnóstico:

1. Do seu **API Management** instância, clique em **registo de diagnóstico**.

## <a name="view-metrics-of-your-apis"></a>Veja as métricas das suas APIs

API de gestão emite a métricas cada minuto, dando-lhe quase em tempo real visibilidade sobre o estado e estado de funcionamento das suas APIs. Segue-se um resumo de algumas das métricas disponíveis:

* Capacidade (pré-visualização): ajuda-o a tomar decisões sobre a atualização/mudança os serviços APIM. A métrica é emitida por minuto e reflete a capacidade de gateway no momento de criação de relatórios. A métrica intervalos entre 0-100 e é calculada com base no recourses de gateway, tais como a utilização da CPU e memória.
* Total de pedidos de Gateway: o número de API pedidos no período. 
* Pedidos de Gateway com êxito: o número de pedidos de API que recebeu com êxito códigos de resposta HTTP, incluindo 304, 307 e algo mais pequeno do que 301 (por exemplo, 200). 
* Pedidos falhados do Gateway: o número de pedidos de API que receberam errados códigos de resposta HTTP, incluindo 400 e algo maior do que 500.
* Pedidos de Gateway não autorizados: o número de pedidos de API que foi recebido, incluindo 401, 403 e 429 códigos de resposta HTTP. 
* Outros pedidos de Gateway: o número de pedidos de API que receberam códigos de resposta HTTP que não pertencem a qualquer uma das categorias anteriores (por exemplo, 418).

Para aceder à métricas:

1. Selecione **métricas** no menu de perto da parte inferior da página.
2. Na lista pendente, selecione métricas que está interessado (pode adicionar várias métricas). 
    
    Por exemplo, seleccione **Total de pedidos de Gateway** e **pedidos falhados de Gateway** da lista de métricas disponíveis.
3. O gráfico mostra o número total de chamadas à API. Mostra o número de chamadas API que falhou. 

## <a name="set-up-an-alert-rule-for-unauthorized-request"></a>Configurar uma regra de alerta para o pedido não autorizado

Pode configurar para receber alertas com base nos registos de métricas e atividade. Monitor do Azure permite-lhe configurar um alerta para fazer o seguinte quando aciona:

* Enviar uma notificação por e-mail
* Chamar um webhook
* Invocar uma aplicação de lógica do Azure

Para configurar alertas:

1. Selecione **regras de alerta** na barra de menu perto da parte inferior da página.
2. Selecione **Adicionar alerta métrica**.
3. Introduza um **nome** para este alerta.
4. Selecione **pedidos não autorizados de Gateway** como a métrica para monitorizar.
5. Selecione **proprietários, contribuintes e leitores de E-Mail**.
6. Prima **OK**.
7. Tente chamar nossa API conferência sem uma chave de API. Como o proprietário deste serviço de API Management, receberá um alerta por e-mail. 

    > [!TIP]
    > A regra de alerta também pode chamar um Web Hook ou a uma aplicação de lógica do Azure quando é acionada.

    ![set up alerta](./media/api-management-azure-monitor/set-up-alert.png)

## <a name="next-steps"></a>Passos seguintes

Neste tutorial, ficou a saber como:

> [!div class="checklist"]
> * Ver registos de atividades
> * Ver registos de diagnóstico
> * Veja as métricas da sua API 
> * Configurar uma regra de alerta quando a API obtém chamadas não autorizadas

Avançar para o próximo tutorial:

> [!div class="nextstepaction"]
> [Chamadas de rastreio](api-management-howto-api-inspector.md)