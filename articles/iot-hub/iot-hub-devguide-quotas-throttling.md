---
title: Compreender as quotas do IoT Hub do Azure e limitação | Microsoft Docs
description: Guia para programadores - descrição das quotas que se aplicam ao IoT Hub e o comportamento de limitação esperado.
services: iot-hub
documentationcenter: .net
author: dominicbetts
manager: timlt
editor: ''
ms.assetid: 425e1b08-8789-4377-85f7-c13131fae4ce
ms.service: iot-hub
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 04/01/2018
ms.author: dobett
ms.openlocfilehash: 37d1397b0601e09b12c0c05ff0adc6a916d66d70
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/28/2018
---
# <a name="reference---iot-hub-quotas-and-throttling"></a>Referência - quotas do IoT Hub e a limitação

## <a name="quotas-and-throttling"></a>Quotas e limitação
Cada subscrição do Azure pode ter no máximo 10 os hubs IoT e, no máximo, 1 hub gratuito.

Cada IoT hub é aprovisionado com um determinado número de unidades de um escalão específico. Para obter mais informações, consulte [preços do Azure IoT Hub][lnk-pricing]. A camada e o número de unidades de determinam a quota máxima de diária de mensagens em fila que possa ser enviado.

A camada também determina os limites de limitação do IoT Hub impõe em todas as operações.

## <a name="operation-throttles"></a>Limitações de operação
Limitações de operação tem limitações de velocidade a que são aplicadas em intervalos de minutos e destinam-se para evitar abuso. IoT Hub tenta evitar a devolver erros sempre que possível, mas inicia exceções a devolver se a limitação é violada há demasiado tempo.

Em qualquer momento, pode aumentar quotas ou limites de limitação de aumento do número de unidades aprovisionados num IoT hub.

A tabela seguinte mostra as limitações impostas. Consulte os valores para um hub individuais.

| Limitação | Libertar, B1 e S1 | B2 e S2 | B3 e S3 | 
| -------- | ------- | ------- | ------- |
| Operações de registo de identidade (criar, obter, listar, atualizar, eliminar) | 1.67/SEC/Unit (min/100/unidade) | 1.67/SEC/Unit (min/100/unidade) | 83.33/SEC/Unit (5000/mínimo/unidade) |
| Ligações do dispositivo | Mais de 100 por segundo ou seg/12/unidade <br/> Por exemplo, duas unidades de S1: 2\*12 = 24/seg, mas tem de ter, pelo menos, 100/seg nas suas unidades. Com unidades de S1 nove, tiver 108/seg (9\*12) entre as unidades. | 120/seg/unidade | 6000/seg/unidade |
| Envios do dispositivo para a cloud | Mais de 100 por segundo ou seg/12/unidade <br/> Por exemplo, duas unidades de S1: 2\*12 = 24/seg, mas tem de ter, pelo menos, 100/seg nas suas unidades. Com unidades de S1 nove, tiver 108/seg (9\*12) entre as unidades. | 120/seg/unidade | 6000/seg/unidade |
| Nuvem para o dispositivo envia<sup>1</sup> | 1.67/SEC/Unit (min/100/unidade) | 1.67/SEC/Unit (min/100/unidade) | 83.33/SEC/Unit (5000/mínimo/unidade) |
| Nuvem para o dispositivo recebe<sup>1</sup> <br/> (apenas ao dispositivo utiliza HTTPS)| 16.67/SEC/Unit (1000/mínimo/unidade) | 16.67/SEC/Unit (1000/mínimo/unidade) | 833.33/SEC/Unit (50000/mínimo/unidade) |
| Carregamento de ficheiros | ficheiro 1.67 carregamento notificações/seg/unidade (min/100/unidade) | ficheiro 1.67 carregamento notificações/seg/unidade (min/100/unidade) | ficheiro 83.33 carregamento notificações/seg/unidade (5000/mínimo/unidade) |
| Direcionar métodos<sup>1</sup> | 160KB/seg/unidade<sup>2</sup> | 480KB/seg/unidade<sup>2</sup> | 24MB/seg/unidade<sup>2</sup> | 
| Duplo (dispositivo e módulo) leituras<sup>1</sup> | 10/seg | Mais de 10 por segundo ou seg/1/unidade | 50/seg/unidade |
| Duplo atualizações (dispositivo e módulo)<sup>1</sup> | 10/seg | Mais de 10 por segundo ou seg/1/unidade | 50/seg/unidade |
| As tarefas operações<sup>1</sup> <br/> (criar, atualizar, listar, eliminar) | 1.67/SEC/Unit (min/100/unidade) | 1.67/SEC/Unit (min/100/unidade) | 83.33/SEC/Unit (5000/mínimo/unidade) |
| Débito de operação de por dispositivo tarefas<sup>1</sup> | 10/seg | Mais de 10 por segundo ou seg/1/unidade | 50/seg/unidade |

<sup>1</sup>esta funcionalidade não está disponível na camada básica do IoT Hub. Para obter mais informações, consulte [como escolher o IoT Hub direita](iot-hub-scaling.md). <br/><sup>2</sup>limitação de tamanho do medidor é de 8 KB.

O *ligações de dispositivos* limitação regulam a velocidade a que seja possível estabelecer novas ligações de dispositivo com um IoT hub. O *ligações de dispositivos* limitação não governar o número máximo de dispositivos ligados em simultâneo. A limitação depende o número de unidades que sejam aprovisionados para o IoT hub.

Por exemplo, se comprar uma única unidade S1, receberá uma limitação de ligações de 100 por segundo. Por conseguinte, para estabelecer a ligação de 100 000 dispositivos, demora menos de 1000 segundos (cerca de 16 minutos). No entanto, pode ter como vários dispositivos ligados em simultâneo que tenham dispositivos registados no registo de identidade.

Para um debate aprofundado do IoT Hub limitação comportamento, consulte a mensagem de blogue [limitação do IoT Hub e][lnk-throttle-blog].

> [!IMPORTANT]
> Operações de registo de identidade destinam-se a utilização de tempo de execução em cenários de aprovisionamento e gestão de dispositivos. Ler ou atualizar um grande número de identidades de dispositivo é suportada através de [importar e exportar tarefas][lnk-importexport].
> 
> 

## <a name="other-limits"></a>Outros limites

IoT Hub impõe outros limites operacionais:

| Operação | Limite |
| --------- | ----- |
| Os URIs de carregamento de ficheiros | 10000 SAS URIs podem ser enviados para uma conta do storage em simultâneo. <br/> 10 SAS URIs/dispositivo podem ficar fora de uma só vez. |
| As tarefas<sup>1</sup> | Histórico da tarefa é retido cópias de segurança para 30 dias <br/> Tarefas simultâneas máximas é 1 (para gratuito) e S1, 5 (para S2), 10 (para S3). |
| Pontos finais adicionais | Paga SKU hubs podem ter pontos finais adicionais 10. Hubs SKU livres podem ter um ponto final adicional. |
| As regras de encaminhamento de mensagens | Paga SKU hubs podem ter 100 regras de encaminhamento. Hubs SKU livres podem ter regras de encaminhamento cinco. |
| Mensagens do dispositivo-nuvem | 256 KB de tamanho da mensagem máxima |
| Mensagens da nuvem para dispositivo<sup>1</sup> | 64 KB de tamanho da mensagem máxima. Máximo de mensagens para a entrega pendentes é 50. |
| Método direto<sup>1</sup> | Tamanho do payload de método direto máximo é 128 KB. |

<sup>1</sup>esta funcionalidade não está disponível na camada básica do IoT Hub. Para obter mais informações, consulte [como escolher o IoT Hub direita](iot-hub-scaling.md).

> [!NOTE]
> Atualmente, o número máximo de dispositivos que pode ligar a um IoT hub único é 500 000. Se pretende aumentar este limite, contacte [Microsoft Support](https://azure.microsoft.com/support/options/).

## <a name="latency"></a>Latência
IoT Hub strives fornecer a latência baixa para todas as operações. No entanto, devido a condições de rede e outros fatores imprevisíveis, não pode garantir uma latência máxima. Ao conceber a sua solução, deve:

* Evite efetuar qualquer pressupostos sobre a latência máxima de todas as operações de IoT Hub.
* Aprovisione o seu IoT hub na região do Azure mais próxima para os seus dispositivos.
* Considere a utilização do Azure IoT Edge para efetuar operações de sensíveis à latência no dispositivo ou num gateway perto do dispositivo.

Várias unidades de IoT Hub afetam a limitação, tal como descrito anteriormente, mas não fornece quaisquer benefícios de latência adicionais ou garantias.
Se vir inesperados aumentos de latência da operação, contacte [Microsoft Support](https://azure.microsoft.com/support/options/).

## <a name="next-steps"></a>Passos Seguintes
Outros tópicos de referência neste guia para programadores do IoT Hub incluem:

* [Pontos finais de IoT Hub][lnk-devguide-endpoints]
* [Idioma de consulta do IoT Hub para dispositivos duplos, tarefas e o encaminhamento de mensagens][lnk-devguide-query]
* [Suporte de MQTT do IoT Hub][lnk-devguide-mqtt]

[lnk-pricing]: https://azure.microsoft.com/pricing/details/iot-hub
[lnk-throttle-blog]: https://azure.microsoft.com/blog/iot-hub-throttling-and-you/
[lnk-importexport]: iot-hub-devguide-identity-registry.md#import-and-export-device-identities

[lnk-devguide-endpoints]: iot-hub-devguide-endpoints.md
[lnk-devguide-query]: iot-hub-devguide-query-language.md
[lnk-devguide-mqtt]: iot-hub-mqtt-support.md
