---
title: "Resolução de problemas para eventos de entrada com formato incorreto no Azure Stream Analytics | Microsoft Docs"
description: "Como saber qual o evento na minha entrada dados está a causar o problema de uma tarefa de Stream Analytics"
keywords: 
documentationcenter: 
services: stream-analytics
author: SnehaGunda
manager: Kfile
ms.assetid: 
ms.service: stream-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: data-services
ms.date: 02/19/2018
ms.author: sngun
ms.openlocfilehash: 2b4f82bae20c3cb398848a89715bfdc1316e1ba1
ms.sourcegitcommit: 12fa5f8018d4f34077d5bab323ce7c919e51ce47
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 02/23/2018
---
# <a name="troubleshoot-for-malformed-input-events"></a>Resolução de problemas para eventos de entrada com formato incorreto

Quando o fluxo de entrada da tarefa do Stream Analytics contém mensagens mal formadas, causar problemas de serialização. Mensagens mal formadas incluem serialização incorreto, tais como parênteses em falta num objeto JSON ou formato de carimbo de data / hora incorretos. Quando uma tarefa de Stream Analytics recebe uma mensagem incorretamente formada, ignora a mensagem e notifica o utilizador com um aviso. Um símbolo de aviso é apresentado no **entradas** mosaico da tarefa do Stream Analytics:

![Entradas do mosaico](media/stream-analytics-malformed-events/inputs_tile.png)

## <a name="troubleshooting-steps"></a>Passos de resolução de problemas

1. Navegue para o mosaico de entrada e clique para ver os avisos.
2. O mosaico de entrada de detalhes apresenta um conjunto de avisos com detalhes sobre o problema. Segue-se uma mensagem de aviso de exemplo, a mensagem de aviso mostra a partição, o desvio e números de sequência em que contém dados JSON com formato incorreto. 

   ![Mensagem de aviso com desvio igual a](media/stream-analytics-malformed-events/warning_message_with_offset.png)

3. Para obter os dados JSON tem um formato incorreto, execute o seguinte fragmento de código. Este bloco de código lê a Id de partição de deslocamento e imprime os dados. Pode obter o exemplo completo do [repositório do GitHub exemplos](https://github.com/Azure/azure-stream-analytics/tree/master/Samples/CheckMalformedEventsEH). Depois de ler os dados, pode analisar e corrija o formato de serialização.

```csharp
static void PrintMessages(string partitionId, long offset, int numberOfEvents)
        {
            EventHubReceiver receiver;
            
            try
            {
                foreach (var e in receiver.Receive(numberOfEvents, TimeSpan.FromMinutes(1)))
                {
                    Console.WriteLine(Encoding.UTF8.GetString(e.GetBytes()));
                    Console.WriteLine("----");
                }
            }
            catch (Exception ex)
            {
                Console.WriteLine(ex.ToString());
                return;
            }

            receiver.Close();
        }
```
## <a name="next-steps"></a>Passos Seguintes

* [Referência do idioma de consulta do Azure Stream Analytics](https://msdn.microsoft.com/library/azure/dn834998.aspx)
* [Referência de API do REST de gestão do Azure Stream Analytics](https://msdn.microsoft.com/library/azure/dn835031.aspx)
