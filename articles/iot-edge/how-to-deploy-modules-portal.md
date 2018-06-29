---
title: Implementar módulos de limite de IoT do Azure (portal) | Microsoft Docs
description: Utilizar o portal do Azure para implementar módulos para um dispositivo de limite de IoT
author: kgremban
manager: timlt
ms.author: kgremban
ms.date: 06/06/2018
ms.topic: conceptual
ms.reviewer: menchi
ms.service: iot-edge
services: iot-edge
ms.openlocfilehash: 4082189d451f670c1ae3f76b8ec785d8bd0518b3
ms.sourcegitcommit: 150a40d8ba2beaf9e22b6feff414f8298a8ef868
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/27/2018
ms.locfileid: "37036489"
---
# <a name="deploy-azure-iot-edge-modules-from-the-azure-portal"></a>Implementar os módulos do Azure IoT Edge do portal do Azure

Depois de criar o limite de IoT módulos com a lógica de negócio, pretende implementá-las nos seus dispositivos para funcionar no limite. Se tiver vários módulos que trabalham em conjunto para recolher e processar dados, pode implementá-las, uma vez e declarar as regras de encaminhamento que ligue-os. 

Este artigo mostra como o portal do Azure orienta-o através da criação de um manifesto de implementação e enviar a implementação para um dispositivo de limite de IoT. Para obter informações sobre como criar uma implementação que tenha como destino vários dispositivos com base nas respetivas etiquetas partilhadas, consulte [implementar e monitorizar os módulos de limite de IoT à escala](how-to-deploy-monitor.md)

## <a name="prerequisites"></a>Pré-requisitos

* Um [IoT hub](../iot-hub/iot-hub-create-through-portal.md) na sua subscrição do Azure. 
* Um [dispositivo de limite de IoT](how-to-register-device-portal.md) com o tempo de execução de limite de IoT instalado. 

## <a name="select-your-device"></a>Selecione o seu dispositivo

1. Iniciar sessão para o [portal do Azure](https://portal.azure.com) e navegue até ao seu IoT hub.
2. Selecione **IoT Edge** no menu.
3. Clique no ID do dispositivo de destino na lista de dispositivos. 
4. Selecione **Definir Módulos**.

## <a name="configure-a-deployment-manifest"></a>Configurar um manifesto de implementação

O manifesto de implementação é um documento JSON que descreve quais os módulos que para implementar, como os dados fluem entre os módulos e propriedades pretendidas de duplos o módulo. Para obter mais informações sobre como implementação manifestos trabalho e como criá-los, consulte [compreender como os módulos de IoT limite podem ser utilizados, configurados e reutilizados](module-composition.md).

O portal do Azure tem um assistente que explica como criar o manifesto de implementação, em vez de criar manualmente o documento JSON. Tem três passos: **adicionar módulos**, **especificar rotas**, e **rever implementação**. 

### <a name="add-modules"></a>Adicionar módulos

1. No **definições de registo** secção da página, forneça as credenciais para aceder a quaisquer registos do contentor privada que contêm as imagens de módulo. 
2. No **módulos de implementação** secção da página, selecione **adicionar**. 
3. Selecione o tipo de módulo da lista pendente: 
   * **Módulo de limite de IoT** -a opção predefinida.
   * **Módulo de análise do Azure Stream** -só os módulos gerados a partir de uma carga de trabalho do Azure Stream Analytics. 

4. Forneça um nome para o módulo, em seguida, especifique a imagem de contentor. Por exemplo: 
   * **Nome** -tempSensor
   * **URI de imagem** -mcr.microsoft.com/azureiotedge-simulated-temperature-sensor:1.0
5. Preencha os campos opcionais, se necessário. Para obter mais informações sobre o contentor criar opções de política de reinício, e ver o estado pretendido [EdgeAgent pretendido propriedades](module-edgeagent-edgehub.md#edgeagent-desired-properties). Para obter mais informações sobre o duplo módulo consulte [definir ou atualização pretendido propriedades](module-composition.md#define-or-update-desired-properties).
6. Selecione **Guardar**.
7. Repita os passos 2 a 6 para adicionar módulos adicionais à sua implementação. 
8. Selecione **seguinte** para continuar para a secção de rotas.

### <a name="specify-routes"></a>Especificar rotas

Por predefinição, o assistente fornece-lhe uma rota chamada **rota** e definidos como **FROM /* para $upstream * *, que significa que todas as mensagens de saída por qualquer módulos são enviadas para o seu IoT hub.  

Adicionar ou atualizar as rotas com as informações de [declarar rotas](module-composition.md#declare-routes), em seguida, selecione **seguinte** para continuar para a secção de revisão.

### <a name="review-deployment"></a>Implementação de revisão

A revisão secção mostra que a implementação de JSON manifesto que foi criada com base nas suas seleções nas duas secções anteriores. Tenha em atenção que existem dois módulos declarado que não adicione: **$edgeAgent** e **$edgeHub**. Estes dois módulos constituem o [IoT Edge runtime](iot-edge-runtime.md) e são necessárias predefinições em todas as implementações. 

Reveja as informações de implementação, em seguida, selecione **submeter**. 

## <a name="view-modules-on-your-device"></a>Módulos de ver no seu dispositivo

Assim que tiver implementado módulos no seu dispositivo, pode ver todos eles no **detalhes do dispositivo** página do portal. Esta página apresenta o nome de cada módulo implementado, bem como informações úteis, como o código de saída e de estado de implementação. 

## <a name="next-steps"></a>Passos Seguintes

Saiba como [implementar e monitorizar os módulos de limite de IoT à escala](how-to-deploy-monitor.md)
