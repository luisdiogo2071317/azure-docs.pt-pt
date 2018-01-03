---
title: "Começar a utilizar a solução de simulação de dispositivo - Azure | Microsoft Docs"
description: "A solução de simulação de IoT Suite é uma ferramenta que pode ser utilizada para ajudar o desenvolvimento e teste de uma solução de IoT. O serviço de simulação é autónomo oferta que pode ser utilizado em conjunto com outras soluções pré-configuradas ou utilizado com as suas próprias soluções personalizadas."
services: iot device simulation
suite: iot-suite
author: troyhopwood
manager: timlt
ms.author: troyhop
ms.service: iot-suite
ms.date: 12/15/2017
ms.topic: article
ms.devlang: NA
ms.tgt_pltfrm: NA
ms.workload: NA
ms.openlocfilehash: 25b9d1c7debe0d98a87b7d0b47dea6ab6f36c8af
ms.sourcegitcommit: f46cbcff710f590aebe437c6dd459452ddf0af09
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/20/2017
---
# <a name="device-simulation-walkthrough"></a>Instruções de simulação de dispositivo

Simulação de dispositivos IoT do Azure é uma ferramenta que pode ser utilizada para ajudar o desenvolvimento e teste de uma solução de IoT. Dispositivo de simulação é autónomo que pode utilizar em conjunto com outras soluções pré-configuradas ou com as suas próprias soluções personalizadas da oferta.

Este tutorial explica algumas das funcionalidades do dispositivo de simulação. Mostra como funciona e permite-lhe utilizá-la para testar as suas próprias soluções de IoT.

Neste tutorial, ficará a saber como:

>[!div class="checklist"]
> * Configurar uma simulação
> * Iniciar e parar uma simulação
> * Veja as métricas de telemetria

## <a name="prerequisites"></a>Pré-requisitos

Para concluir este tutorial, precisa de uma instância implementada de simulação de dispositivos do IoT do Azure na sua subscrição do Azure.

Se ainda não implementado simulação de dispositivo ainda, deve efetuar o [implementar simulação de dispositivo do Azure IoT](iot-suite-device-simulation-explore.md) tutorial.

## <a name="configuring-device-simulation"></a>Configuração de dispositivo de simulação

Pode configurar e executar o dispositivo simulação completamente no dashboard. Abra o dashboard a partir do IoT Suite [aprovisionado soluções](https://www.azureiotsuite.com/) página. Clique em **iniciar** na sua nova implementação de simulação de dispositivo.

### <a name="target-iot-hub"></a>IoT Hub de destino

Pode utilizar o dispositivo de simulação com um IoT hub pré-aprovisionada ou outros IoT hub:

![IoT Hub de destino](media/iot-suite-device-simulation-explore/targethub.png)

> [!NOTE]
> A opção para utilizar um IoT Hub pré-aprovisionada só está disponível se optar por criar um novo IoT Hub quando implementou o dispositivo de simulação. Se não tiver um IoT hub, pode criar um novo tipo de sempre o [portal do Azure](https://portal.azure.com).

Para um hub IoT específico de destino, forneça o **iothubowner** cadeia de ligação. Pode obter esta cadeia de ligação do [portal do Azure](https://portal.azure.com):

1. Na página de configuração do IoT Hub no portal do Azure, clique em **políticas de acesso partilhado**.
1. Clique em **iothubowner**.
1. Copie ou a chave primária ou secundária.
1. Cole este valor na caixa de texto em destino IoT Hub.

![IoT hub de destino](media/iot-suite-device-simulation-explore/connectionstring.png)

### <a name="device-model"></a>Modelo do dispositivo

O modelo do dispositivo permite-lhe escolher o tipo de dispositivo para simular. Pode escolher um dos modelos dispositivo previamente configurada ou definir uma lista dos sensores para um modelo de dispositivo personalizada:

![Modelo do dispositivo](media/iot-suite-device-simulation-explore/devicemodel.png)

#### <a name="pre-configured-device-models"></a>Modelos de dispositivos pré-configurado

Dispositivo de simulação fornece três modelos de dispositivos pré-configurados. Modelos de dispositivos para Chillers, Elevators e Trucks estão disponíveis.

Modelos de dispositivos pré-configurados incluem vários sensores com uma frequência de telemetria previamente determinada. Não é possível configurar a frequência de telemetria para estes dispositivos.

A tabela seguinte mostra uma lista de configurações para cada modelo de dispositivo previamente configurada:

| Modelo do dispositivo | Sensor de | Unidade | Frequência de telemetria
| -------------| ------ | -----| --------------------|
| Chiller | humidade | % | 5 segundos |
| | pressure | psig | 5 segundos |
| | Temperatura | F | 5 segundos |
| Abordagem | piso | | 5 segundos |
| | vibration | mm | 5 segundos |
| | Temperatura | F | 5 segundos |
| Camião | Latitude | | 3 segundos |
| | Longitude | | 3 segundos |
| | Velocidade | mph | 5 segundos |
| | cargotemperature | F | 5 segundos |

#### <a name="custom-device-model"></a>Modelo de dispositivo personalizada

Modelos de dispositivos personalizados permitem-lhe para sensores de modelo que mais detalhadamente representam os seus próprios dispositivos. Um dispositivo personalizado pode ter até 10 sensores personalizados.

Quando seleciona o tipo de modelo do dispositivo personalizada, pode adicionar novos Sensores clicando **+ adicionar sensor**.

![Adicionar sensores](media/iot-suite-device-simulation-explore/customsensors.png)

Sensores personalizados possuir as seguintes propriedades:

| Campo | Descrição |
| ----- | ----------- |
| Nome do sensor | Um nome amigável para o sensor, tais como **temperatura** ou **velocidade**. |
| Comportamento | Comportamentos ativar dados de telemetria variar a partir de uma mensagem para o seguinte para simular dados reais. **Incremento** aumenta o valor por um em cada mensagem enviada que começa com o valor mínimo. Assim que o valor máximo é cumprido, em seguida, começa através de novamente, o valor mínimo. **Diminuir** comporta-se da mesma forma que **incremento** mas contagens para baixo. O **aleatório** comportamento gera um valor aleatório entre os valores máximos e o valor mínimo. |
| Valor mínimo | O menor número que representa o intervalo aceitável. |
| Valor máximo | O número maior que representa o intervalo aceitável. |
| Unidade | A unidade de medida para o sensor como ° F ou MPH. |

### <a name="number-of-devices"></a>Número de dispositivos

Dispositivo de simulação atualmente permite-lhe simular até 1000 dispositivos.

![Número de dispositivos](media/iot-suite-device-simulation-explore/numberofdevices.png)

### <a name="telemetry-frequency"></a>Frequência de telemetria

Frequência de telemetria permite-lhe especificar com que frequência os seus dispositivos simulados devem enviar dados para o IoT hub. Pode enviar dados como frequentemente como cada 10 segundos ou como raramente cada até 99 horas, 59 minutos e 59 segundos.

![Frequência de telemetria](media/iot-suite-device-simulation-explore/frequency.png)

> [!NOTE]
> Se estiver a utilizar um IoT hub que não seja o IoT hub pré-aprovisionada, deve considerar os limites de mensagem para o seu hub IoT de destino. Por exemplo, se tiver 1000 dispositivos simulados enviar telemetria a cada 10 segundos para um hub de S1 atingirá o limite de telemetria para o hub apenas através de uma hora.

### <a name="simulation-duration"></a>Duração de simulação

Pode escolher para executar a simulação durante um período de tempo específico ou a ser executado até parar explicitamente. Quando seleciona um limite de tempo específico, pode escolher qualquer duração entre 10 minutos até 99 horas, 59 minutos e segundos, 59.

![Duração de simulação](media/iot-suite-device-simulation-explore/duration.png)

### <a name="start-and-stop-the-simulation"></a>Iniciar e parar a simulação

Quando tiver adicionado todos os dados de configuração necessárias para o formulário, o **iniciar simulação** botão está ativado. Para começar a simulação, clicar neste botão.

![Iniciar simulação](media/iot-suite-device-simulation-explore/start.png)

Se tiver especificado uma duração específica para a simulação, em seguida, interrompe automaticamente quando for atingida a hora. Pode sempre parar a simulação antecipadamente clicando **parar a simulação.**

Se optar por executar a simulação indefinidamente, em seguida, é executada até que clique em **parar a simulação**. Pode fechar o browser e volte à página de simulação de dispositivo para parar a simulação em qualquer altura.

![Parar simulação](media/iot-suite-device-simulation-explore/stop.png)

> [!NOTE]
> Apenas um simulação pode ser executada num momento. Tem de parar a simulação em execução antes de iniciar uma nova simulação.
