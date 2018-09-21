---
title: Criar uma aplicação do Azure IoT Central | Microsoft Docs
description: Crie uma nova aplicação do Azure IoT Central para gerir dispositivos de venda refrigerados. Veja os dados telemétricos gerados a partir dos seus dispositivos simulados.
author: tbhagwat3
ms.author: tanmayb
ms.date: 04/15/2018
ms.topic: quickstart
ms.service: iot-central
services: iot-central
ms.custom: mvc
manager: peterpr
ms.openlocfilehash: af06766d89804b2f3d0aaf061494fb836f6ec262
ms.sourcegitcommit: 06724c499837ba342c81f4d349ec0ce4f2dfd6d6
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/19/2018
ms.locfileid: "46465610"
---
# <a name="create-an-azure-iot-central-application"></a>Criar uma aplicação do Azure IoT Central

Como _construtor_, utilize a IU do Azure IoT Central para definir a sua aplicação do Microsoft Azure IoT Central. Este manual de início rápido mostra-lhe como:

- Crie uma aplicação do Azure IoT Central que contenha um exemplo do _modelo de dispositivo_ e _dispositivos_ simulados.
- Veja as funcionalidades do modelo de dispositivo da **Máquina de Venda Refrigerada** na sua aplicação.
- Veja a telemetria e a análise a partir dos seus dispositivos do **Refrigerador**.

Neste início rápido, verá um dispositivo do **Refrigerador** simulado a partir de um modelo de dispositivo. O dispositivo simulado:

* Envia telemetria, como a temperatura e a pressão, para a sua aplicação.
* Comunica os valores de propriedade do dispositivo, como um alerta de movimento para a aplicação.
* Tem definições de dispositivo, como a velocidade da ventoinha, que pode definir na aplicação.

Quando cria um dispositivo simulado a partir de um modelo de dispositivo numa aplicação do Azure IoT Central, o dispositivo simulado permite-lhe testar a aplicação antes de ligar a um dispositivo real.

## <a name="create-the-application"></a>Criar a aplicação

Para concluir este início rápido, tem de criar uma aplicação do Azure IoT Central a partir do modelo da aplicação de **Exemplo do Contoso**.

Navegue para a página do [Gestor de Aplicações](https://aka.ms/iotcentral) do Azure IoT Central. Em seguida, introduza o endereço de e-mail e a palavra-passe que utiliza para aceder à sua subscrição do Azure:

![Introduza a sua conta da organização](media/quick-deploy-iot-central/sign-in.png)

Para começar a criar uma nova aplicação do Azure IoT Central, escolha **Nova Aplicação**:

![Página do Gestor de Aplicações do Azure IoT Central](media/quick-deploy-iot-central/iotcentralhome.png)

Para criar uma nova aplicação do Azure IoT Central:

1. Escolha o plano de pagamento **Aplicação de Avaliação Gratuita**.
1. Escolha um nome simpático para a aplicação, como **Contoso IoT**. O Azure IoT Central gera um prefixo de URL exclusivo para si. Pode alterar este prefixo de URL para algo mais memorável.
1. Escolha o modelo da aplicação de **Exemplo do Contoso** .
1. Em seguida, escolha **Criar**.

![Página da Aplicação do Azure IoT Central](media/quick-deploy-iot-central/iotcentralcreate.png)

## <a name="next-steps"></a>Passos seguintes

Neste início rápido criou uma aplicação do Azure IoT Central pré-preenchida que contém um modelo de dispositivo e dispositivos simulados da **Máquina de Venda Refrigerada**. Veja [Definir um novo modelo de dispositivo na sua aplicação](tutorial-define-device-type.md) para saber mais, enquanto construtor, sobre como definir os seus próprios modelos de dispositivos.
