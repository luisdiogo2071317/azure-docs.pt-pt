---
title: Utilizar o portal do Azure para criar um IoT Hub | Documentos da Microsoft
description: Como criar, gerir e eliminar hubs IoT do Azure através do portal do Azure. Inclui informações sobre escalões de preço, o dimensionamento, segurança e configuração de mensagens.
author: dominicbetts
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.date: 04/01/2018
ms.author: dobett
ms.openlocfilehash: 0b03ae434e93dbab45235fe67c499497e1257064
ms.sourcegitcommit: 1aedb52f221fb2a6e7ad0b0930b4c74db354a569
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/17/2018
ms.locfileid: "42055203"
---
# <a name="create-an-iot-hub-using-the-azure-portal"></a>Criar um hub IoT com o portal do Azure

[!INCLUDE [iot-hub-resource-manager-selector](../../includes/iot-hub-resource-manager-selector.md)]

Este artigo descreve:

* Como localizar o serviço IoT Hub no portal do Azure.
* Como criar e gerir IoT hubs.

## <a name="where-to-find-the-iot-hub-service"></a>Onde encontrar o serviço IoT Hub

Pode encontrar o serviço IoT Hub nas seguintes localizações no portal do:

* Escolher **+ novo**, em seguida, escolha **Internet das coisas**.
* No Marketplace, escolha **Internet das coisas**.

## <a name="create-an-iot-hub"></a>Criar um hub IoT

Pode criar um hub IoT com os seguintes métodos:

* O **+ novo** opção abre o painel de mostrado na captura de ecrã seguinte. Os passos para criar o hub IoT através deste método e através do marketplace são idênticos.

* No Marketplace, escolha **criar** para abrir o painel mostrado na captura de ecrã seguinte.

As secções seguintes descrevem os vários passos para criar um hub IoT.

### <a name="choose-the-name-of-the-iot-hub"></a>Escolha o nome do IoT hub

Para criar um hub IoT, tem de nomes do hub IoT. Este nome tem de ser exclusivo em todos os hubs IoT.

[!INCLUDE [iot-hub-pii-note-naming-hub](../../includes/iot-hub-pii-note-naming-hub.md)]

### <a name="choose-the-pricing-tier"></a>Escolha o escalão de preço

Pode escolher entre vários escalões dependendo de quantos recursos pretender e o número de mensagens enviam através da sua solução por dia. O escalão gratuito destina-se para teste e avaliação. Ele permite que os 500 dispositivos de estar ligados ao IoT hub e até 8000 mensagens por dia. Cada subscrição do Azure pode criar um IoT Hub no escalão gratuito. 

Para obter detalhes sobre as outras opções de escalão, consulte [escolher o escalão certo do IoT Hub](iot-hub-scaling.md).

### <a name="iot-hub-units"></a>Unidades do hub IoT

O número de mensagens permitidos por unidade por dia depende do escalão de preço do seu hub. Por exemplo, se pretender que o hub IoT para oferecer suporte a uma entrada de 700.000 mensagens, escolher duas unidades de escalão S1.

### <a name="device-to-cloud-partitions-and-resource-group"></a>Dispositivo para cloud partições e o grupo de recursos

Pode alterar o número de partições para um hub IoT. O número predefinido de partições é 4; Pode escolher um número diferente na lista pendente.

Não é necessário criar explicitamente um grupo de recursos vazio. Quando cria um recurso, pode escolher para criar um novo grupo de recursos ou utilize um grupo de recursos existente.

![Captura de ecrã que mostra a criação de um hub no portal do Azure](./media/iot-hub-create-through-portal/location1.png)

### <a name="choose-subscription"></a>Escolha a subscrição

O IoT Hub do Azure automaticamente apresenta uma lista de subscrições do Azure, que a conta de utilizador está ligada a. Pode escolher a subscrição do Azure para associar o hub IoT para.

### <a name="choose-the-location"></a>Escolha a localização

A opção de localização fornece uma lista das regiões onde o IoT Hub está disponível.

### <a name="create-the-iot-hub"></a>Criar o hub IoT

Quando todos os passos anteriores estiverem concluídos, pode criar o hub IoT. Clique em **criar** para iniciar o processo de back-end para criar e implementar o hub IoT com as opções que escolheu.

Pode demorar alguns minutos para criar o hub IoT como demora algum tempo para a implementação de back-end para execução nos servidores de localização apropriada.

## <a name="change-the-settings-of-the-iot-hub"></a>Alterar as definições do IoT hub
<!--robinsh these screenshots are out of date -->

Pode alterar as definições do hub IoT existente, depois de criada a partir do painel do IoT Hub.

![Captura de ecrã que mostra as definições do IoT hub](./media/iot-hub-create-through-portal/portal-settings.png)

**Partilhado políticas de acesso**: estas políticas definem as permissões para dispositivos e serviços para ligar ao IoT Hub. Pode aceder a estas políticas ao clicar em **políticas de acesso partilhado** sob **geral**. Neste painel, pode modificar as políticas existentes ou adicionar uma nova política.

### <a name="create-a-policy"></a>Criar uma política

* Clique em **adicionar** para abrir um painel. Aqui pode inserir o novo nome de política e as permissões que pretende associar esta política, conforme mostrado na figura a seguir:

    Há várias permissões que podem ser associadas estas políticas partilhadas. O **leitura do registo** e **gravação de Registro** políticas concedem direitos de acesso de escrita e leitura para o registo de identidade. Escolher a opção de escrita automaticamente escolhe a opção de leitura.

    O **ligação de serviço** política concede permissão para aceder a pontos finais de serviço, tal como **receber dispositivo-para-cloud**. O **ligar o dispositivo** política concede permissões para enviar e receber mensagens com os pontos de extremidade do lado do dispositivo do IoT Hub.

* Clique em **criar** adicionar recentemente criada a política para a lista existente.

   ![Captura de ecrã mostra a adição de uma política de acesso partilhado](./media/iot-hub-create-through-portal/shared-access-policies.png)

## <a name="endpoints"></a>Pontos Finais

Clique em **pontos de extremidade** para apresentar uma lista de pontos finais do hub IoT que está a modificar. Existem dois tipos de pontos finais: pontos finais que estão incorporados no IoT hub e pontos de extremidade que adicionar ao IoT hub após a sua criação.

![Captura de ecrã mostra a adição de um ponto final](./media/iot-hub-create-through-portal/messaging-settings.png)

### <a name="built-in-endpoints"></a>Pontos finais incorporados

Existem dois pontos de extremidade internos: **Cloud aos comentários de dispositivo** e **eventos**.

* **Nuvem para comentários de dispositivo** definições: esta definição não tem dois subsettings: **Cloud para dispositivo TTL** (time-to-live) e **período de retenção** (em horas) para as mensagens. Quando o primeiro cria um hub IoT, ambas estas definições têm o valor predefinido de uma hora. Para ajustar estas definições, utilize os controlos de deslize ou escreva os valores.

* **Eventos** definições: esta definição não tem vários subsettings, alguns dos quais são só de leitura. A lista seguinte descreve estas definições:

  * **Partições**: um valor predefinido é definido quando o hub IoT é criado. Pode alterar o número de partições por meio desta definição.

  * **Nome compatível com o Hub de eventos e o ponto final**: hub IoT a quando é criado, um Hub de eventos é criado internamente que poderá ter acesso em determinadas circunstâncias. Não é possível personalizar os valores de nome e o ponto final compatível com o Event Hub, mas pode copiá-los ao clicar em **cópia**.

  * **Período de retenção**: definido como um dia por padrão, mas pode alterá-lo utilizando a lista pendente. Este valor é em dias para a definição de dispositivo para a cloud.

  * **Grupos de consumidores**: grupos de consumidores ativar vários leitores permite ler mensagens de forma independente do IoT hub. Cada hub IoT é criado com um grupo de consumidores predefinido. No entanto, pode adicionar ou eliminar grupos de consumidores para os hubs IoT com esta definição.

  > [!NOTE]
  > O grupo de consumidores predefinido não pode ser editado ou eliminado.

### <a name="custom-endpoints"></a>Pontos finais personalizados

Pode adicionar pontos finais personalizados no seu hub IoT com o portal. Do **pontos de extremidade** painel, clique em **Add** na parte superior para abrir o **adicionar ponto final** painel. Introduza as informações necessárias, em seguida, clique em **OK**. O ponto final personalizado é agora listado no principal **pontos de extremidade** painel.

![Captura de ecrã mostra a criação de um ponto final personalizado](./media/iot-hub-create-through-portal/endpoint-creation.png)

Pode ler mais sobre os pontos finais personalizados na [referência - pontos finais do hub IoT]( iot-hub-devguide-endpoints.md).

## <a name="routes"></a>Rotas

Clique em **rotas** para gerir a forma como o IoT Hub expede suas mensagens do dispositivo para a cloud.

![Captura de ecrã mostra a adição de uma nova rota](./media/iot-hub-create-through-portal/routes-list.png)

Pode adicionar rotas ao seu hub IoT ao clicar **Add** na parte superior a **rotas*** painel, introduzindo as informações necessárias e, clicando em **OK**. O percurso, em seguida, é listado no principal **rotas** painel. Pode editar uma rota ao clicar no mesmo na lista de rotas. Para ativar uma rota, clique no mesmo na lista de rotas e defina a **Enabled** alternar para **desativar**. Para guardar a alteração, clique em **OK** na parte inferior do painel.

![Captura de ecrã que mostra uma nova regra de encaminhamento de edição](./media/iot-hub-create-through-portal/route-edit.png)

## <a name="delete-the-iot-hub"></a>Eliminar o hub IoT

Pode navegar para o hub IoT que pretende eliminar clicando **procurar**e, em seguida, escolhendo o hub apropriado para eliminar. Para eliminar o hub IoT, clique nas **eliminar** botão abaixo do nome do hub IoT.

## <a name="next-steps"></a>Passos Seguintes

Siga estas ligações para saber mais sobre como gerir o IoT Hub do Azure:

* [Gerir dispositivos IoT em massa](iot-hub-bulk-identity-mgmt.md)
* [Métricas do IoT Hub](iot-hub-metrics.md)
* [Monitorização de operações](iot-hub-operations-monitoring.md)

Para explorar ainda mais os recursos do IoT Hub, veja:

* [guia para programadores do IoT Hub](iot-hub-devguide.md)
* [Implementar o AI em dispositivos de ponta com o Azure IoT Edge](../iot-edge/tutorial-simulate-device-linux.md)
* [Proteger a sua solução de IoT desde o backup](../iot-fundamentals/iot-security-ground-up.md)
