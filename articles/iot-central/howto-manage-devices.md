---
title: Gerir os dispositivos na sua aplicação do Azure IoT Central | Microsoft Docs
description: Como um operador, saiba como gerir dispositivos na sua aplicação do Azure IoT Central.
services: iot-central
author: ellenfosborne
ms.author: elfarber
ms.date: 01/21/2018
ms.topic: article
ms.prod: microsoft-iot-central
manager: timlt
ms.openlocfilehash: b56e352a1f39dbd536347f6c7e83a6aabfe32ecc
ms.sourcegitcommit: eb75f177fc59d90b1b667afcfe64ac51936e2638
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/16/2018
---
# <a name="manage-devices-in-your-azure-iot-central-application"></a>Gerir dispositivos na sua aplicação do Azure IoT Central

Este artigo descreve como, como um operador, para gerir dispositivos na sua aplicação do Microsoft Azure IoT Central. Como um operador, pode:

- Utilize o **Explorer** página para ver, adicionar e eliminar os dispositivos ligados à sua aplicação do Azure IoT Central.
- Manter um inventário dos seus dispositivos atualizado.
- Manter os seus metadados do dispositivo atualizados alterando os valores armazenados nas propriedades do dispositivo.
- Controlar o comportamento dos seus dispositivos através da atualização de uma definição de um dispositivo específico do **definições** página.

## <a name="view-your-devices"></a>Ver os seus dispositivos

Para ver um dispositivo individual:

1. Escolha **Explorer** no menu de navegação esquerdo. Aqui pode ver uma lista das suas [modelos de dispositivo](howto-set-up-template.md).

1. Escolha um **modelo de dispositivo** no painel da esquerda.

1. No painel da direita, verá uma lista de dispositivos criada a partir desse modelo do dispositivo. Escolha um dispositivo individual para ver o **detalhes do dispositivo** página desse dispositivo:

    ![Página de detalhes do dispositivo](./media/howto-manage-devices/image1.png)

## <a name="add-a-device"></a>Adicionar um dispositivo

Para adicionar um dispositivo à sua aplicação do Azure IoT Central:

1. Escolha **Explorer** no menu de navegação esquerdo.

1. Escolha o modelo de dispositivo a partir do qual pretende criar um dispositivo.

1. Escolha + **novo**.

1. Escolha **Real** ou **Simulated**. Um dispositivo real é para um dispositivo físico se ligar à sua aplicação do Azure IoT Central. Um dispositivo simulado tem dados de exemplo gerados pelo Azure IoT Central. Este exemplo utiliza um dispositivo real. Escolha **Real** para navegar para o **detalhes do dispositivo** página para o novo dispositivo.

## <a name="delete-a-device"></a>Eliminar um dispositivo

Para eliminar a um dispositivo simulado ou real da sua aplicação do Azure IoT Central:

1. Escolha **Explorer** no menu de navegação.

1. Escolha o modelo de dispositivo do dispositivo que pretende eliminar.

1. Selecione a caixa junto ao dispositivo para eliminar.

1. Escolha **eliminar**.

## <a name="change-a-device-setting"></a>Alterar uma definição de dispositivo

Definições controlam o comportamento de um dispositivo. Por outras palavras, elas permitem forneça entradas para o seu dispositivo. Pode ver e atualizar as definições do dispositivo no **detalhes do dispositivo** página.

1. Escolha **Explorer** no menu de navegação.

1. Escolha o modelo de dispositivo do dispositivo cujas definições pretende alterar.

1. Escolha o **definições** separador. Aqui pode ver todas as definições de que tem o seu dispositivo e os respetivos valores atuais. Para cada definição, pode ver se o dispositivo ainda está a sincronizar.

1. Modificar as definições para os valores pretendidos. Pode modificar várias definições de uma só vez e atualizá-las todas ao mesmo tempo.

1. Escolha **atualização**. Os valores são enviados para o seu dispositivo. Quando o dispositivo reconhece a alteração da definição, o estado da definição anterior entra em **sincronizado**.

## <a name="change-a-property"></a>Alterar uma propriedade

As propriedades são os metadados associados ao dispositivo, por exemplo, cidade e número de série do dispositivo. Pode ver e atualizar as propriedades no **detalhes do dispositivo** página.

1. Escolha **Explorer** no menu de navegação.

1. Escolha o modelo de dispositivo do dispositivo cujas propriedades pretende alterar.

1. Escolha o **propriedades** separador, onde pode ver todas as propriedades.

1. Modificar as propriedades para os valores pretendidos. Pode modificar várias propriedades de uma só vez e atualizá-las todas ao mesmo tempo.

1. Escolha **atualização**.

> [!NOTE]
> Não é possível alterar o valor de _propriedades do dispositivo_. Propriedades do dispositivo são definidas pelo dispositivo e são só de leitura na aplicação do Azure IoT Central.

## <a name="next-steps"></a>Passos Seguintes

Agora que aprendeu como gerir dispositivos na sua aplicação do Azure IoT Central, eis o passo seguinte sugerido:

> [!div class="nextstepaction"]
> [Como utilizar os conjuntos de dispositivo](howto-use-device-sets.md)

<!-- Next how-tos in the sequence -->