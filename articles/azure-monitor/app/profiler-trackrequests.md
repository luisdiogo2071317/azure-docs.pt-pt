---
title: Escrever código para monitorizar os pedidos com o Azure Application Insights | Documentos da Microsoft
description: Escrever código para monitorizar os pedidos com o Application Insights para que possa obter os perfis para seus reqeusts
services: application-insights
documentationcenter: ''
author: mrbullwinkle
manager: carmonm
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.topic: conceptual
ms.reviewer: cawa
ms.date: 08/06/2018
ms.author: mbullwin
ms.openlocfilehash: 20f408d9dd32c3fd7a0e319e4051483e3aa54dd9
ms.sourcegitcommit: fbf0124ae39fa526fc7e7768952efe32093e3591
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/08/2019
ms.locfileid: "54083167"
---
# <a name="write-code-to-track-requests-with-application-insights"></a>Escrever código para monitorizar os pedidos com o Application Insights

Para ver perfis para a sua aplicação na página de desempenho, o Application Insights tem de estar controlo pedidos para a sua aplicação. O Application Insights automaticamente pode monitorizar os pedidos para as aplicações que são baseiam-se em arquiteturas que já estão instrumentadas, como ASP.net e ASP.Net Core. Mas para outras aplicações, como funções de trabalho do serviço Cloud do Azure e APIs sem monitoração de estado do Service Fabric, a necessidade de escrever código para dizer ao Application Insights, onde os pedidos de início e fim. Uma vez que escreveu esse código, telemetria de pedidos será enviada para o Application Insights e verá a telemetria na página de desempenho e perfis serão recolhidos para essas solicitações. 

Aqui estão as etapas que necessárias para monitorizar os pedidos manualmente:


  1. No início do tempo de vida do aplicativo, adicione o seguinte código:  

        ```csharp
        using Microsoft.ApplicationInsights.Extensibility;
        ...
        // Replace with your own Application Insights instrumentation key.
        TelemetryConfiguration.Active.InstrumentationKey = "00000000-0000-0000-0000-000000000000";
        ```
      Para obter mais informações sobre esta configuração de chave de instrumentação globais, consulte [utilizar o Service Fabric com o Application Insights](https://github.com/Azure-Samples/service-fabric-dotnet-getting-started/blob/dev/appinsights/ApplicationInsights.md).  

  1. Para qualquer parte do código que deseja instrumentar, adicione uma `StartOperation<RequestTelemetry>` **USING** instrução em torno dele, conforme mostrado no exemplo a seguir:

        ```csharp
        using Microsoft.ApplicationInsights;
        using Microsoft.ApplicationInsights.DataContracts;
        ...
        var client = new TelemetryClient();
        ...
        using (var operation = client.StartOperation<RequestTelemetry>("Insert_Your_Custom_Event_Unique_Name"))
        {
          // ... Code I want to profile.
        }
        ```

        A invocar `StartOperation<RequestTelemetry>` dentro de outra `StartOperation<RequestTelemetry>` âmbito não é suportado. Pode usar `StartOperation<DependencyTelemetry>` aninhada no escopo em vez disso. Por exemplo:  
        
        ```csharp
        using (var getDetailsOperation = client.StartOperation<RequestTelemetry>("GetProductDetails"))
        {
        try
        {
          ProductDetail details = new ProductDetail() { Id = productId };
          getDetailsOperation.Telemetry.Properties["ProductId"] = productId.ToString();
        
          // By using DependencyTelemetry, 'GetProductPrice' is correctly linked as part of the 'GetProductDetails' request.
          using (var getPriceOperation = client.StartOperation<DependencyTelemetry>("GetProductPrice"))
          {
              double price = await _priceDataBase.GetAsync(productId);
              if (IsTooCheap(price))
              {
                  throw new PriceTooLowException(productId);
              }
              details.Price = price;
          }
        
          // Similarly, note how 'GetProductReviews' doesn't establish another RequestTelemetry.
          using (var getReviewsOperation = client.StartOperation<DependencyTelemetry>("GetProductReviews"))
          {
              details.Reviews = await _reviewDataBase.GetAsync(productId);
          }
        
          getDetailsOperation.Telemetry.Success = true;
          return details;
        }
        catch(Exception ex)
        {
          getDetailsOperation.Telemetry.Success = false;
        
          // This exception gets linked to the 'GetProductDetails' request telemetry.
          client.TrackException(ex);
          throw;
        }
        }
        ```
