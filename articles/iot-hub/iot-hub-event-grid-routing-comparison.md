---
title: Comparar a grelha de evento, encaminhamento para o IoT Hub | Microsoft Docs
description: "IoT Hub oferece o seu próprio serviço de encaminhamento de mensagens, mas também se integra grelha de eventos para publicação de eventos. Compare os dois recursos."
services: iot-hub
documentationcenter: 
author: kgremban
manager: timlt
editor: 
ms.service: iot-hub
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 01/30/2018
ms.author: kgremban
ms.openlocfilehash: 5a0a97ccf033b2981ba13be455482146ba212228
ms.sourcegitcommit: 9d317dabf4a5cca13308c50a10349af0e72e1b7e
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 02/01/2018
---
# <a name="compare-message-routing-and-event-grid-for-iot-hub"></a>Comparar o encaminhamento de mensagens e a grelha de eventos para o IoT Hub

IoT Hub do Azure fornece a capacidade para transmitir dados dos seus dispositivos ligados e integrar os dados as aplicações empresariais. IoT Hub fornece dois métodos para integrar IoT eventos em outros serviços do Azure ou a aplicações empresariais. Este artigo aborda as duas funcionalidades que fornecem esta capacidade, para que possa escolher a opção é melhor para o seu cenário.

* **Encaminhamento de mensagens do IoT Hub**: IoT Hub esta funcionalidade permite aos utilizadores [encaminhar mensagens do dispositivo para nuvem](iot-hub-devguide-messages-read-custom.md) para pontos finais de serviço, como contentores de armazenamento do Azure, Event Hubs, as filas do Service Bus e tópicos do Service Bus. As regras de encaminhamento fornecem flexibilidade para efetuar rotas baseada em consulta. Também ativar [alertas críticos](iot-hub-devguide-messages-d2c.md) que acionar ações através de consultas e pode basear-se nos cabeçalhos de mensagens e corpo. 
* **Integração do IoT Hub com eventos grelha**: grelha de eventos do Azure é um serviço de encaminhamento de eventos totalmente gerido que utiliza um publicar-subscrever o modelo. IoT Hub e a grelha de evento funcionam em conjunto para [integrar eventos do IoT Hub do Azure e os serviços do Azure não](iot-hub-event-grid.md), no quase em tempo real. 

## <a name="similarities-and-differences"></a>Semelhanças e diferenças

Enquanto o encaminhamento de mensagens e eventos grelha ativar a configuração de alerta, existem algumas diferenças fundamentais entre os dois. A tabela seguinte para obter detalhes, consulte:

| Funcionalidade | Encaminhamento de mensagens do IoT Hub | Integração de IoT Hub com a grelha de eventos |
| ------- | --------------- | ---------- |
| **Mensagens do dispositivo** | Sim, o encaminhamento de mensagens pode ser utilizado para dados de telemetria. | Não, o evento grelha só pode ser utilizada para eventos de IoT Hub não telemetria. |
| **Tipo de evento** | Sim, a mensagem encaminhamento pode reportar alterações duplo e eventos de ciclo de vida do dispositivo. | Sim, eventos grelha podem comunicar os dispositivos estão registados a um IoT Hub e a dispositivos são eliminados. |
| **Ordenação** | Sim, é mantida ordenação de eventos.  | Não, a ordem de eventos não é garantida. | 
| **Tamanho da mensagem máximo** | 256 KB, dispositivo-nuvem | 64 KB |
| **Filtragem** | Avançada filtragem através de linguagem como o SQL Server suporta filtragem nos cabeçalhos de mensagens e corpos. Para obter exemplos, consulte [idioma de consulta do IoT Hub](iot-hub-devguide-query-language.md). | Filtragem baseada no sufixo/prefixo de IDs de dispositivo, que funciona bem para hierárquicos serviços como o armazenamento. |
| **Endpoints** | <ul><li>Hub de Eventos</li> <li>Blob de armazenamento</li> <li>Fila do Service Bus</li> <li>Tópicos do Service Bus</li></ul><br>Paga SKUs de Hub IoT (S1, S2 e S3) estão limitados a pontos finais personalizados 10. 100 rotas podem ser criadas por IoT Hub. | <ul><li>Funções do Azure</li> <li>Automatização do Azure</li> <li>Hub de Eventos</li> <li>Aplicações Lógicas</li> <li>Microsoft Flow</li> <li>Serviços de terceiros através de WebHooks</li></ul><br>Para mais atualizadas à sua lista de pontos finais, consulte [processadores de eventos de evento grelha](../event-grid/overview.md#event-handlers). |
| **Cost** | Não há sem qualquer encargo separado para o encaminhamento de mensagens. É-lhe cobrada apenas uma entrada de telemetria para o IoT Hub. Por exemplo, se tiver uma mensagem encaminhada para três diferentes pontos finais, é-lhe faturado apenas uma mensagem. | Não há sem qualquer encargo do IoT Hub. Grelha de evento oferece as operações primeiro 100.000 por mês gratuitamente e, em seguida, $0.60 por milhões operações depois disso. |

Encaminhamento de mensagens do IoT Hub e a grelha de evento têm semelhanças demasiado, algumas das quais estão descritas na tabela seguinte:

| Funcionalidade | Encaminhamento de mensagens do IoT Hub | Integração de IoT Hub com a grelha de eventos |
| ------- | --------------- | ---------- |
| **Fiabilidade** | Elevado: Fornece cada mensagem para o ponto final, pelo menos, uma vez para cada rota. Expirar todas as mensagens que não são entregues dentro de uma hora. | Elevado: Fornece cada mensagem para o webhook, pelo menos, uma vez para cada subscrição. Expirar todos os eventos que não foram fornecidos dentro de 24 horas. | 
| **Escalabilidade** | Elevado: Otimizado para suportar milhões de dispositivos ligados em simultâneo billions de mensagens em fila de envio. | Elevado: Capaz de encaminhamento 10,000,000 eventos por segundo por região. |
| **Latência** | Baixa: Quase em tempo real. | Baixa: Quase em tempo real. |
| **Enviar para vários pontos finais** | Sim, envie uma única mensagem para vários pontos finais. | Sim, envie uma única mensagem para vários pontos finais.  | 
| **Segurança** | IOT Hub fornece identidade por dispositivo e controlo de acesso revocable. Para obter mais informações, consulte o [controlo de acesso do IoT Hub](iot-hub-devguide-security.md). | Grelha de evento fornece a validação em três pontos: evento subscrições, publicação de eventos e entrega de eventos do webhook. Para obter mais informações, consulte [grelha de eventos de segurança e autenticação](../event-grid/security-authentication.md). |

## <a name="how-to-choose"></a>Como escolher

Encaminhamento de mensagens do IoT Hub e a integração do IoT Hub com eventos grelha efetuam diferentes ações para alcançar os resultados semelhantes. Estes tanto demorar informações da sua solução de IoT Hub e transmita-para que possam reagir a outros serviços. Por isso, como a decidir qual delas utilizar? Além dos dados da secção anterior, utilize as perguntas seguintes para ajudar a sua decisão de guia: 

* **Que tipos de dados estão a enviar para os pontos finais?**

   Utilize o IoT Hub mensagem encaminhamento quando tiver de enviar dados de telemetria a outros serviços. Mensagem de encaminhamento, também permite consultar cabeçalhos de mensagens e corpos de mensagens. 

   A integração do IoT Hub com eventos grelha funciona com eventos que ocorrem no serviço de IoT Hub. Estes eventos de IoT Hub incluem a criação de dispositivos e eliminação. 

* **Necessita que os pontos finais receber estas informações?**

   Encaminhamento de mensagens do IoT Hub suporta pontos finais limitados, mas pode criar conectores para reroute os dados e eventos para os pontos finais adicionais. Para obter uma lista completa dos pontos finais suportados, consulte a tabela na secção anterior. 

   A integração do IoT Hub com eventos grelha suporta mais pontos finais. Este exemplo também funciona com webhooks para expandir o encaminhamento fora do ecossistema de serviço do Azure e para aplicações empresariais de terceiros. 

* **, É importante se os dados são recebidos por ordem?**

   Encaminhamento de mensagens do IoT Hub mantém a ordem na qual são enviadas as mensagens, para que estas chegam da mesma forma.

   Grelha de eventos não garante que os pontos finais irão receber eventos pela mesma ordem que ocorreram. No entanto, o esquema de eventos incluem um timestamp que pode ser utilizada para identificar a ordem depois dos eventos de chegar ao ponto final. 

## <a name="next-steps"></a>Passos Seguintes

* Saiba mais sobre [encaminhamento de mensagens do IoT Hub](iot-hub-devguide-messages-d2c.md) e [pontos finais de IoT Hub](iot-hub-devguide-endpoints.md).

* Saiba mais sobre [grelha de eventos do Azure](../event-grid/overview.md)

* Experimentar a integração de grelha de eventos por [enviar notificações de correio eletrónico sobre eventos de IoT Hub do Azure utilizando as Logic Apps](../event-grid/publish-iot-hub-events-to-logic-apps.md)