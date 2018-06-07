---
title: Configurar e monitorizar dispositivos de IoT de escala com o Azure IoT Hub | Microsoft Docs
description: Utilizar configurações de automático de dispositivos do IoT Hub do Azure para atribuir uma configuração em vários dispositivos
author: ChrisGMsft
manager: bruz
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.date: 04/13/2018
ms.author: chrisgre
ms.openlocfilehash: fe5ce960663f39d4f2c87a7bbffa091d327e9559
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/01/2018
ms.locfileid: "34632453"
---
# <a name="configure-and-monitor-iot-devices-at-scale---preview"></a>Configurar e monitorizar dispositivos de IoT à escala – pré-visualização

Gestão de dispositivos automática no IoT Hub do Azure automatiza muitas das tarefas repetitivas e complexas de gerir dispositivos grande fleets através da totalidade da respetiva ciclos de vida. Com a gestão de dispositivos automático, pode um conjunto de dispositivos com base nas respetivas propriedades de destino, definir uma configuração desejada e permitir que o IoT Hub, atualize-os sempre que entram em âmbito.  Este procedimento é efetuado através de uma configuração de dispositivos automático, que também permitirá resumir conclusão e conformidade, identificador de intercalação e entra em conflito e implementar configurações numa abordagem faseada.

[!INCLUDE [iot-hub-basic](../../includes/iot-hub-basic-whole.md)]

Trabalho de configurações automático de dispositivos através da atualização de um conjunto de dispositivos duplos com propriedades pretendidas e reporta um resumo com base no dispositivo duplo comunicadas propriedades.  Introduz uma nova classe e o documento JSON chamado um _configuração_ que tem três partes:

* O **condição de destino** define o âmbito dos dispositivos duplos ser atualizados. A condição de destino é especificada como uma consulta em tags de duplo de dispositivo e/ou registadas propriedades.

* O **destino conteúdo** define as propriedades pretendidas para ser adicionados ou atualizados nos dispositivos visados duplos. O conteúdo inclui um caminho para a secção de propriedades pretendidas para ser alterada.

* O **métricas** definir o número de resumo de vários Estados de configuração, tais como **êxito**, **em curso**, e **erro**. Métricas personalizadas são especificadas como consultas no dispositivo duplo comunicadas propriedades.  Métricas de sistema são nas métricas predefinidas que medem o estado de atualização de duplo, tais como o número de dispositivos duplos destinadas e o número de duplos foram atualizadas com êxito. 

> [!Note]
> Durante a pré-visualização, esta funcionalidade não está disponível para os Hubs IoT em regiões EUA leste, EUA oeste, Europa do Norte e Europa Ocidental.

## <a name="implement-device-twins-to-configure-devices"></a>Implementar dispositivos duplos para configurar dispositivos

Configurações de dispositivos automático requerem a utilização de dispositivos duplos sincronizar estado entre a nuvem e de dispositivos.  Consulte [compreender e utilizar dispositivos duplos no IoT Hub] [ lnk-device-twin] para obter orientações sobre como utilizar dispositivos duplos.

## <a name="identify-devices-using-tags"></a>Identificar dispositivos utilizando as etiquetas

Antes de poder criar uma configuração, tem de especificar quais os dispositivos que pretende afetar. IoT Hub do Azure identifica dispositivos utilizando as etiquetas no dispositivo duplo. Cada dispositivo pode ter várias etiquetas e pode defini-las qualquer forma que faz sentido para a sua solução. Por exemplo, se gerir dispositivos em diferentes localizações, pode adicionar as seguintes etiquetas para um dispositivo duplo:

```json
"tags": {
    "location": {
        "state": "Washington",
        "city": "Tacoma"
    }
},
```

## <a name="create-a-configuration"></a>Criar uma configuração

1. No [portal do Azure][lnk-portal], aceda ao seu IoT hub. 
1. Selecione **configuração de dispositivos de IoT (pré-visualização)**.
1. Selecione **Adicionar configuração**.

Existem cinco passos para criar uma configuração. As secções seguintes guiá-lo através de cada um deles. 

### <a name="step-1-name-and-label"></a>Passo 1: Nome e a etiqueta

1. Dê um nome exclusivo que é até 128 letras minúsculas da configuração. Evite espaços e os seguintes carateres inválidos: `& ^ [ ] { } \ | " < > /`.
1. Adicione etiquetas para ajudar a controlar as configurações. As etiquetas são **nome**, **valor** pares que descrevem a configuração. Por exemplo, `HostPlatform, Linux` ou `Version, 3.0.1`.
1. Selecione **seguinte** mover para o passo dois. 

### <a name="step-2-specify-settings"></a>Passo 2: Especificar as definições

Esta secção especifica o conteúdo de destino ser definida no destino dispositivos duplos. Existem duas entradas para cada conjunto de definições. O primeiro é o caminho de duplo de dispositivo, o que é o caminho para a secção JSON dentro as propriedades de duplo assim o desejar que serão definidas.  A segundo é o conteúdo JSON a inserir na secção. Por exemplo, defina o caminho do dispositivo duplo e o conteúdo para o seguinte:

![Definir o caminho do dispositivo duplo e o conteúdo](./media/iot-hub-auto-device-config/create-configuration-full-browser.png)

Também pode definir definições individuais especificando o caminho completo no dispositivo duplo caminho e o valor no conteúdo com sem parênteses Retos. Por exemplo, definir o caminho do dispositivo duplo `properties.desired.chiller-water.temperature` e definir o conteúdo: `66`

Se duas ou mais configurações de visam o mesmo caminho duplo de dispositivo, o conteúdo a partir da configuração de prioridade mais alta aplicará (prioridade é definida no passo 4).

Se pretender remover uma propriedade, especifique o valor da propriedade para `null`.

Pode adicionar definições adicionais ao selecionar **Adicionar definição do dispositivo duplo**

### <a name="step-3-specify-metrics-optional"></a>Passo 3: Especificar as métricas (opcionais)

Métricas fornecem contagens de resumidas dos Estados de vários que um dispositivo pode informar como resultado de aplicar o conteúdo de configuração. Por exemplo, pode criar uma métrica para pendentes alterações de definições, uma métrica de erros e uma métrica para as alterações de definições com êxito.

1. Introduza um nome para **nome da métrica**
1. Introduza uma consulta para **métrica critérios**.  A consulta é baseada no dispositivo duplo comunicadas propriedades.  A métrica representa o número de linhas devolvidas pela consulta.

Por exemplo: `SELECT deviceId FROM devices WHERE properties.reported.chillerWaterSettings.status='pending'`

Pode incluir uma cláusula que a configuração foi aplicada, por exemplo: `SELECT deviceId FROM devices WHERE configurations.[[yourconfigname]].status='Applied'` incluindo Retos duplos.


### <a name="step-4-target-devices"></a>Passo 4: Dispositivos de destino

Utilize a propriedade de etiquetas dos dispositivos duplos para os dispositivos específicos que devem receber esta configuração de destino.  Também pode visar dispositivos por dispositivo duplo comunicadas propriedades.

Uma vez que as configurações com várias podem ter como destino o mesmo dispositivo, deverá dar-cada configuração um número de prioridade. Se alguma vez existir um conflito, a configuração com a prioridade mais alta wins. 

1. Introduza um número inteiro positivo para a configuração **prioridade**. Maior valor numérico é considerado a prioridade mais elevada. Se duas configurações de tem o mesmo número de prioridade, que foi criada mais recentemente wins. 
1. Introduza um **condição de destino** para determinar quais os dispositivos visados com esta configuração. A condição é baseada em etiquetas do dispositivo duplo ou dispositivo duplo reportados propriedades e deve corresponder ao formato de expressão. Por exemplo, `tags.environment='test'` ou `properties.reported.chillerProperties.model='4000x'`. 
1. Selecione **seguinte** para avançar para o passo final.

### <a name="step-5-review-configuration"></a>Passo 5: Configuração de revisão

Reveja as informações de configuração, em seguida, selecione **submeter**.

## <a name="monitor-a-configuration"></a>Monitorizar uma configuração

Para ver os detalhes de uma configuração e monitorizar os dispositivos a executá-lo, utilize os seguintes passos:

1. No [portal do Azure][lnk-portal], aceda ao seu IoT hub. 
1. Selecione **configuração de dispositivos de IoT (pré-visualização)**.
1. Inspecione a lista de configuração. Para cada configuração, pode ver os seguintes detalhes:
   * **ID** -o nome da configuração.
   * **Condição de destino** -a consulta utilizada para definir os dispositivos visados.
   * **Prioridade** -o número de prioridade atribuído à configuração.
   * **Hora de criação** -timestamp de quando a configuração foi criada. Este timestamp é utilizado para dividir ties quando duas configurações de tem a mesma prioridade. 
   * **Métricas de sistema** -as métricas que são calculadas pelo IoT Hub e não não possível personalizar pelos programadores. Direcionados Especifica o número de dispositivos duplos que correspondem à condição de destino. Aplica-se especificado o número de dispositivos duplos que foram modificados na configuração, o que pode incluir parciais modificações no caso de uma configuração de prioridade superior, separado também efetuadas alterações. 
   * **Métricas personalizadas** -as métricas que foram especificadas pelo programador como consultas no dispositivo duplo comunicadas propriedades.  Até cinco métricas personalizadas podem ser definidas por configuração. 
   
1. Selecione a configuração que pretende monitorizar.  
1. Inspecione os detalhes da configuração. Pode utilizar os separadores para ver detalhes específicos sobre os dispositivos que receberam a configuração: 
   * **Condição de destino** -os dispositivos que correspondem à condição de destino. 
   * **Métricas** -uma lista de métricas de sistema e métricas personalizadas.  Pode ver uma lista de dispositivos que são contados para cada métrica, selecionando a métrica no menu pendente e, em seguida, selecionar **ver dispositivos**.
   * **Definições do dispositivo duplo** -as definições do dispositivo duplo que estão definidas na configuração. 
   * **Configuração etiquetas** -pares chave-valor utilizados para descrever uma configuração.  As etiquetas têm sem afetar a funcionalidade. 

## <a name="modify-a-configuration"></a>Modificar uma configuração

Quando modifica uma configuração, as alterações imediatamente são replicadas para todos os dispositivos visados. 

Se atualizar a condição de destino, ocorrem as seguintes atualizações:
* Se um dispositivo duplo não satisfaz a condição de destino antigo, mas se cumpre a condição de destino novo e esta configuração é a prioridade mais elevada para esse dispositivo duplo, em seguida, esta configuração é aplicada ao dispositivo duplo. 
* Se um dispositivo duplo já não satisfaz a condição de destino, as definições da configuração serão removidas e o dispositivo duplo irá ser modificado pela seguinte configuração de prioridade mais elevada. 
* Se um dispositivo duplo atualmente em execução esta configuração já não satisfaz a condição de destino e não satisfaz a condição de destino de quaisquer outras configurações, em seguida, as definições da configuração serão removidas e outras alterações não serão efetuadas no duplo. 

Para modificar uma configuração, utilize os seguintes passos: 

1. No [portal do Azure][lnk-portal], aceda ao seu IoT hub. 
1. Selecione **configuração de dispositivos de IoT (pré-visualização)**. 
1. Selecione a configuração que pretende modificar. 
1. Efetue atualizações para os seguintes campos: 
   * Condição de destino 
   * Etiquetas 
   * Prioridade 
   * Métricas
1. Selecione **Guardar**.
1. Siga os passos no [uma configuração de Monitor] [âncora-monitor] para ver as alterações a implementar. 

## <a name="delete-a-configuration"></a>Eliminar uma configuração

Quando elimina uma configuração, quaisquer dispositivos duplos execute a seguinte configuração de prioridade mais elevada. Se os dispositivos duplos não satisfazem a condição de destino de qualquer outra configuração, não existem outras definições são aplicadas. 

1. No [portal do Azure][lnk-portal], aceda ao seu IoT hub. 
1. Selecione **configuração de dispositivos de IoT (pré-visualização)**. 
1. Utilize a caixa de verificação para selecionar a configuração que pretende eliminar. 
1. Selecione **Eliminar**.
1. Uma linha de comandos irá pedir-lhe para confirmar.

## <a name="next-steps"></a>Passos Seguintes
Neste artigo, aprendeu como configurar e monitorizar dispositivos de IoT à escala. Siga estas ligações para saber mais sobre a gestão do Azure IoT Hub:

* [Gerir as identidades de dispositivo do IoT Hub em massa][lnk-bulkIDs]
* [Métricas de IoT Hub][lnk-metrics]
* [Operações de monitorização][lnk-monitor]

Para explorar ainda mais as capacidades do IoT Hub, consulte:

* [Guia para programadores do IoT Hub][lnk-devguide]
* [Implementar o AI em dispositivos de ponta com o Azure IoT Edge][lnk-iotedge]

Para explorar, utilizando o serviço de aprovisionamento de dispositivos do IoT Hub para ativar o aprovisionamento de zero touch, just-in-time, consulte: 

* [Serviço de aprovisionamento de dispositivos de IoT Hub do Azure][lnk-dps]

[lnk-device-twin]: iot-hub-devguide-device-twins.md
[lnk-bulkIDs]: iot-hub-bulk-identity-mgmt.md
[lnk-metrics]: iot-hub-metrics.md
[lnk-monitor]: iot-hub-operations-monitoring.md

[lnk-devguide]: iot-hub-devguide.md
[lnk-iotedge]: ../iot-edge/tutorial-simulate-device-linux.md
[lnk-dps]: https://azure.microsoft.com/documentation/services/iot-dps
[lnk-portal]: https://portal.azure.com
