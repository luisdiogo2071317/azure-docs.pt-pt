---
title: Como integrar o API Management do Azure com o Azure Application Insights | Microsoft Docs
description: Saiba como iniciar sessão e ver eventos de API Management do Azure no Azure Application Insights.
services: api-management
documentationcenter: ''
author: mikebudzynski
manager: erikre
editor: ''
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/20/2018
ms.author: apimpm
ms.openlocfilehash: 7740da505f7635944536252d60ec2c2039295975
ms.sourcegitcommit: 65b399eb756acde21e4da85862d92d98bf9eba86
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/22/2018
ms.locfileid: "36320584"
---
# <a name="how-to-integrate-azure-api-management-with-azure-application-insights"></a>Como integrar o API Management do Azure com o Azure Application Insights

Gestão de API do Azure permite a integração fácil com o Azure Application Insights - um serviço extensível para programadores de web criar e gerir aplicações em várias plataformas. Este guia explica cada passo de tal integração e descreve as estratégias para reduzir o impacto do desempenho na sua instância do serviço de API Management.

## <a name="prerequisites"></a>Pré-requisitos

Para seguir este guia, tem de ter uma instância de API Management do Azure. Se não tiver uma, conclua o [tutorial](get-started-create-service-instance.md) primeiro.

## <a name="create-an-azure-application-insights-instance"></a>Criar uma instância do Azure Application Insights

Antes de poder utilizar o Azure Application Insights, terá primeiro de criar uma instância do serviço.

1. Abra o **portal do Azure** e navegue para **Application Insights**.  
    ![Criar App Insights](media/api-management-howto-app-insights/apim-app-insights-instance-1.png)  
2. Clique em **+ Adicionar**.  
    ![Criar App Insights](media/api-management-howto-app-insights/apim-app-insights-instance-2.png)  
3. Preencha o formulário. Selecione **geral** como o **tipo de aplicação**.
4. Clique em **Criar**.

## <a name="create-a-connection-between-azure-application-insights-and-azure-api-management-service-instance"></a>Criar uma ligação entre a instância de serviço de API Management do Azure e Azure Application Insights

1. Navegue até à sua **instância de serviço de API Management do Azure** no **portal do Azure**.
2. Selecione **Application Insights** no menu à esquerda.
3. Clique em **+ Adicionar**.  
    ![Registo de informações de aplicação](media/api-management-howto-app-insights/apim-app-insights-logger-1.png)  
4. Selecione criado anteriormente **Application Insights** instância e forneça uma descrição breve.
5. Clique em **Criar**.
6. Acabou de criar um registo de Azure Application Insights com uma chave de instrumentação. Deverá agora aparecer na lista.  
    ![Registo de informações de aplicação](media/api-management-howto-app-insights/apim-app-insights-logger-2.png)  

## <a name="enable-application-insights-logging-for-your-api"></a>Ativar o registo do Application Insights para a API

1. Navegue até à sua **instância de serviço de API Management do Azure** no **portal do Azure**.
2. Selecione **APIs** no menu à esquerda.
3. Clique na sua API, neste caso **demonstração conferência API**.
4. Vá para o **definições** separador da barra superior.
5. Desloque para baixo até o **registos de diagnóstico** secção.  
    ![Registo de informações de aplicação](media/api-management-howto-app-insights/apim-app-insights-api-1.png)  
6. Verifique o **ativar** caixa.
7. Selecione o seu registo anexado no **destino** pendente.
8. Entrada **100** como **amostragem (%)** e escala a **sempre registar erros** caixa de verificação.
9. Entrada **1024** no **primeiros bytes do corpo** campo.
10. Clique em **Guardar**.

| Nome da definição                        | Tipo de valor                        | Descrição                                                                                                                                                                                                                                                                                                                                      |
|-------------------------------------|-----------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Ativar                              | boolean                           | Especifica se o registo desta API está ativado.                                                                                                                                                                                                                                                                                                |
| Destino                         | Registo do Azure do Application Insights | Especifica o registo de Azure Application Insights para ser utilizado                                                                                                                                                                                                                                                                                           |
| Amostragem (%)                        | Decimal                           | Valores entre 0 e 100 (por cento). <br/> Especifica que percentagem de pedidos será registada no Azure Application Insights. a amostragem de % de 0 significa zero pedidos com sessão iniciados, enquanto a amostragem de 100% significa que todos os pedidos com sessão iniciados. <br/> Esta definição é utilizada para reduzir as implicações de desempenho de pedidos de registo para o Azure Application Insights (consulte a secção abaixo). |
| Sempre os erros do registo                   | boolean                           | Se esta definição está selecionada, todas as falhas serão registadas no Azure Application Insights, independentemente do **amostragem** definição.                                                                                                                                                                                                                  |
| Opções básicas: cabeçalhos              | lista                              | Especifica os cabeçalhos que serão registados no Azure Application Insights para pedidos e respostas.  Predefinição: nenhuma cabeçalhos são registados.                                                                                                                                                                                                             |
| Opções básicas: Primeiros bytes do corpo  | inteiro                           | Especifica quantas primeiros bytes do corpo são registados para o Azure Application Insights para pedidos e respostas.  Predefinição: corpo não é registado.                                                                                                                                                                                              |
| Opções avançadas: Pedido de front-end  |                                   | Especifica se e como *pedidos de front-end* será registado no Azure Application Insights. *Pedido de front-end* é um pedido de entrada para o serviço de API Management do Azure.                                                                                                                                                                        |
| Opções avançadas: Resposta de front-end |                                   | Especifica se e como *respostas de front-end* será registado no Azure Application Insights. *Resposta de front-end* uma resposta a enviar do serviço de API Management do Azure.                                                                                                                                                                   |
| Opções avançadas: Pedido de back-end   |                                   | Especifica se e como *pedidos de back-end* será registado no Azure Application Insights. *Pedido de back-end* um pedido a enviar do serviço de API Management do Azure.                                                                                                                                                                        |
| Opções avançadas: Resposta de back-end  |                                   | Especifica se e como *respostas de back-end* será registado no Azure Application Insights. *Resposta de back-end* é uma resposta de entrada para o serviço de API Management do Azure.                                                                                                                                                                       |

> [!NOTE]
> Pode especificar registadores em diferentes níveis - registador de API único ou um registo para todos os APIs.
>  
> Se especificar ambas:
> + Se forem registadores diferentes, ambos os parâmetros serão utilizado (registos de multiplexação),
> + Se forem registadores os mesmos, mas tem definições diferentes, em seguida, a uma única API (nível mais granular) irá substituir um para todos os APIs.

## <a name="what-data-is-added-to-azure-application-insights"></a>Os dados que são adicionados ao Azure Application Insights

Recebe o Azure Application Insights:

+ *Pedido* itens de telemetria, para cada pedido recebido (*pedido de front-end*, *resposta de front-end*),
+ *Dependência* itens de telemetria, para cada pedido reencaminhado para um serviço de back-end (*pedido de back-end*, *resposta de back-end*),
+ *Exceção* item de telemetria, para todos os pedidos falhados.

Um pedido falhado é um pedido, que:

+ falhou devido a uma ligação de cliente fechada, ou
+ acionado um *no erro* secção das políticas de API, ou
+ tem um 4xx de código de estado HTTP do resposta correspondente ou 5xx.

## <a name="performance-implications-and-log-sampling"></a>As implicações de desempenho e registo amostragem

> [!WARNING]
> Registo de eventos de todos os pode ter implicações de desempenho grave, dependendo da taxa de pedidos recebidos.

Com base nos testes de carga interno, a ativação desta funcionalidade causou a falha uma redução de % de 40 a 50% no débito quando a taxa de pedidos foi excedido 1000 pedidos por segundo. Azure Application Insights foi concebido para utilizar a análise estatística para avaliar os desempenhos de aplicação. Este não se destina a ser um sistema de auditoria e não é adequada para cada pedido individual para APIs de elevado volume de registo.

Pode manipular o número de pedidos a ser registado ao ajustar o **amostragem** definição (consulte os passos acima). Valor meios de 100% todos os pedidos são registados, enquanto 0% reflete sem o registo de todo. **Amostragem** ajuda a reduzir o volume de telemetria, impedir eficazmente degredation significativas de desempenho, enquanto ainda as devidas as vantagens de registo.

A ignorar o registo de cabeçalhos e um corpo de pedidos e respostas também irá ter um impacto positivo no alleviating problemas de desempenho.

## <a name="next-steps"></a>Passos Seguintes

+ Saiba mais sobre [Azure Application Insights](https://docs.microsoft.com/en-us/azure/application-insights/).
+ Considere [registo com o Event Hubs do Azure](api-management-howto-log-event-hubs.md).
