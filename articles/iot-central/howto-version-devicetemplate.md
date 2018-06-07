---
title: Noções sobre o controlo de versões de modelo de dispositivo para as suas aplicações do Azure IoT Central | Microsoft Docs
description: Iterar sobre os modelos de dispositivos através da criação de novas versões e sem afetar os dispositivos ligados em direto
author: sandeeppujar
ms.author: sandeepu
ms.date: 01/19/2018
ms.topic: conceptual
ms.service: iot-central
services: iot-central
manager: peterpr
ms.openlocfilehash: b125d822596675b138560c14c76f9a3120ce3424
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/01/2018
ms.locfileid: "34628832"
---
# <a name="create-a-new-device-template-version"></a>Criar uma nova versão de modelo do dispositivo

Central do Microsoft Azure IoT permite um desenvolvimento rápido de aplicações IoT. Pode iterar rapidamente sobre as estruturas de modelo do dispositivo ao adicionar, editar ou eliminar medidas, definições ou propriedades. Algumas destas alterações poderia ser intrusivas para os dispositivos atualmente ligados. Centro de IoT do Azure identifica estas alterações e fornece uma forma de implementar em segurança estas atualizações para os dispositivos.

Um modelo de dispositivo tem um número de versão aquando da respetiva criação. Por predefinição, o número de versão é 1.0.0. Se as editar um modelo de dispositivo e, se essa alteração pode afetar em direto dispositivos ligados, Central de IoT do Azure pede-lhe criar uma nova versão de modelo do dispositivo.

> [!NOTE]
> Para obter mais informações sobre como criar um modelo de dispositivo, consulte [configurar um modelo de dispositivo](howto-set-up-template.md)

## <a name="changes-that-prompt-a-version-change"></a>Alterações que uma alteração de versão de linha de comandos

Em geral alterações a definições ou propriedades do seu modelo de dispositivo solicitar uma alteração de versão.

> [!NOTE]
> As alterações efetuadas ao modelo de dispositivo não pedir para a criação de uma nova versão, quando nenhum dispositivo ou em mais de um dispositivo está ligado.

A lista seguinte descreve as ações de utilizador que poderão exigir uma nova versão:

* Propriedades (obrigatório)
    * Adicionar ou eliminar uma propriedade necessária
    * Alterar o nome do campo de uma propriedade, o nome do campo que é utilizado pelos seus dispositivos para enviar mensagens.
*  Propriedades (opcionais)
    * Eliminar uma propriedade opcional
    * Alterar o nome do campo de uma propriedade, o nome do campo que é utilizado pelos seus dispositivos para enviar mensagens.
    * Alterar uma propriedade opcional para uma propriedade necessária
*  Definições
    * Adicionar ou eliminar uma definição
    * Alterar o nome do campo de uma definição, o nome do campo que é utilizado pelos seus dispositivos para enviar e receber mensagens.

## <a name="what-happens-on-version-change"></a>O que acontece em alteração da versão?

O que acontece a regras e dashboards do dispositivo quando existe uma alteração de versão?

**Regras** podem conter condições que dependem de propriedades. Se tiver removido um ou mais destas propriedades, estas regras foi dividir na sua nova versão de modelo do dispositivo. Pode aceder a estas regras específicas e as condições para corrigir as regras de atualização. Regras para a sua versão anterior deverão funcionar sem afetar.

**Dashboards de dispositivo** pode conter vários tipos de mosaicos. Algumas dos mosaicos podem conter propriedades e definições. Quando uma propriedade ou definição utilizada num mosaico é removida, o mosaico é interrompido totais ou parciais. Pode ir para o mosaico e resolva o problema ou ao remover o mosaico ou atualizar o conteúdo do mosaico.

## <a name="migrate-a-device-across-device-template-versions"></a>Migrar de um dispositivo nas versões de modelo do dispositivo

Pode criar várias versões do modelo de dispositivo. Ao longo do tempo, terá de vários dispositivos ligados utilizando estes modelos de dispositivos. Pode migrar dispositivos de uma versão do seu modelo do dispositivo para outra. Os passos seguintes descrevem como migrar um dispositivo:

1. Vá para o **Explorer** página.
1. Selecione o dispositivo que terá de migrar para outra versão.
1. Escolha **migrar dispositivo**.
1. Selecione o número de versão que pretende migrar o dispositivo e escolher **migrar**.

![Como migrar um dispositivo](media\howto-version-devicetemplate\pick-version.png)

## <a name="next-steps"></a>Passos Seguintes

Agora que aprendeu a utilizar versões do modelo de dispositivo na sua aplicação do Azure IoT Central, eis o passo seguinte sugerido:

> [!div class="nextstepaction"]
> [Como criar regras de telemetria](howto-create-telemetry-rules.md)