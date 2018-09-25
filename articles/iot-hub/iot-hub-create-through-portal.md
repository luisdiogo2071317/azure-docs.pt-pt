---
title: Utilizar o portal do Azure para criar um IoT Hub | Documentos da Microsoft
description: Como criar, gerir e eliminar hubs IoT do Azure através do portal do Azure. Inclui informações sobre escalões de preço, o dimensionamento, segurança e configuração de mensagens.
author: robinsh
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.date: 09/06/2018
ms.author: robinsh
ms.openlocfilehash: 8f08141f5c14a734f89ba91045767e2a36a44fd2
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/24/2018
ms.locfileid: "46985610"
---
# <a name="create-an-iot-hub-using-the-azure-portal"></a>Criar um hub IoT com o portal do Azure

[!INCLUDE [iot-hub-resource-manager-selector](../../includes/iot-hub-resource-manager-selector.md)]

Este artigo descreve como criar e gerir hubs IoT com o [portal do Azure](https://portal.azure.com).

Para utilizar os passos neste tutorial, precisa de uma subscrição do Azure. Se não tiver uma subscrição do Azure, crie uma [conta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de começar.

## <a name="create-an-iot-hub"></a>Criar um hub IoT

1. Inicie sessão no [Portal do Azure](https://portal.azure.com). 

2. Escolher +**criar um recurso**, em seguida, escolha **Internet das coisas**.

3. Clique em **Iot Hub** da lista à direita. Veja a primeira tela para criar um hub IoT.

   ![Captura de ecrã que mostra a criação de um hub no portal do Azure](./media/iot-hub-create-through-portal/iot-hub-create-screen-basics.png)

   Preencha os campos.

   **Subscrição**: selecione a subscrição a utilizar para o seu hub IoT.

   **Grupo de recursos**: pode criar um novo grupo de recursos ou utilize um já existente. Para criar um novo, clique em **criar novo** e preencha o nome que pretende utilizar. Para utilizar um grupo de recursos existente, clique em **utilizar existente** e selecione o grupo de recursos na lista pendente.

   **Região**: selecione a região na qual pretende que o seu hub a ser localizado na lista pendente.

   **Nome do Hub IoT**: colocar o nome do hub IoT. Este nome tem de ser globalmente exclusivo. 

   [!INCLUDE [iot-hub-pii-note-naming-hub](../../includes/iot-hub-pii-note-naming-hub.md)]

4. Clique em **seguinte: tamanho e a escala** para ir para o ecrã seguinte.

   ![Captura de ecrã que mostra o tamanho de definição e a escala para um novo hub IoT no portal do Azure](./media/iot-hub-create-through-portal/iot-hub-create-screen-size-scale.png)

   Neste ecrã, pode obter os padrões e basta clicar **rever + criar** na parte inferior. Ou pode preencher os campos conforme necessário.

   **Escalão de preço e escala**: pode escolher entre vários escalões dependendo de quantos recursos pretender e o número de mensagens enviam através da sua solução por dia. O escalão gratuito destina-se para teste e avaliação. Ele permite que os 500 dispositivos de estar ligados ao IoT hub e até 8000 mensagens por dia. Cada subscrição do Azure pode criar um IoT Hub no escalão gratuito. 

   **Unidades do IoT Hub**: O número de mensagens permitidos por unidade por dia depende do escalão de preço do seu hub. Por exemplo, se pretender que o hub IoT para oferecer suporte a uma entrada de 700.000 mensagens, escolher duas unidades de escalão S1.

   Para obter detalhes sobre as outras opções de escalão, consulte [escolher o escalão certo do IoT Hub](iot-hub-scaling.md).

   **Partições avançadas / dispositivo-para-cloud**: Esta propriedade está relacionada com as mensagens do dispositivo para a cloud para o número de leitores simultâneos das mensagens. A maioria dos hubs de IoT precisam apenas quatro partições. 

5. Clique em **rever + criar** para rever as opções. Verá algo semelhante a este ecrã.

   ![Captura de ecrã rever informações para a criação de novo hub IoT](./media/iot-hub-create-through-portal/iot-hub-create-review.png)

5. Clique em **criar** para criar o seu hub IoT novo. A criar o hub demora alguns minutos.

## <a name="change-the-settings-of-the-iot-hub"></a>Alterar as definições do IoT hub

Pode alterar as definições do hub IoT existente, depois de criada a partir do painel do IoT Hub.

![Captura de ecrã que mostra as definições do IoT hub](./media/iot-hub-create-through-portal/iot-hub-settings-panel.png)

Aqui estão algumas das propriedades que pode ser definido para um hub IoT:

**Preço e escala**: pode utilizar esta propriedade para migrar para um escalão diferente ou definir o número do IoT Hub unidades. 

**Monitorização de operações**: Ativar as diferentes categorias de monitorização ou, como o registo de eventos relacionados com mensagens do dispositivo para a cloud ou mensagens na cloud para o dispositivo.

**Filtro de IP**: especificar um intervalo de endereços IP que serão aceites ou rejeitados pelo IoT hub.

**Propriedades**: fornece a lista de propriedades que pode copiar e utilizar noutro local, como o ID de recurso, grupo de recursos, localização e assim por diante.

### <a name="shared-access-policies"></a>Políticas de acesso partilhado

Também pode ver ou modificar a lista de políticas de acesso partilhado ao clicar em **políticas de acesso partilhado** no **definições** secção. Estas políticas definem as permissões para dispositivos e serviços para ligar ao IoT Hub. 

Clique em **Add** para abrir o **adicionar uma política de acesso partilhado** painel.  Pode introduzir o novo nome de política e as permissões que pretende associar esta política, conforme mostrado na figura a seguir:

![Captura de ecrã mostra a adição de uma política de acesso partilhado](./media/iot-hub-create-through-portal/iot-hub-add-shared-access-policy.png)

* O **leitura do registo** e **gravação de Registro** políticas concedem direitos de acesso de escrita e leitura para o registo de identidade. Escolher a opção de escrita automaticamente escolhe a opção de leitura.

* O **ligação de serviço** política concede permissão para aceder a pontos finais de serviço, tal como **receber dispositivo-para-cloud**. 

* O **ligar o dispositivo** política concede permissões para enviar e receber mensagens com os pontos de extremidade do lado do dispositivo do IoT Hub.

Clique em **criar** adicionar recentemente criada a política para a lista existente.

## <a name="message-routing-for-an-iot-hub"></a>Encaminhamento de mensagens para um hub IoT

Clique em **roteamento de mensagens** sob **Messaging** para ver o painel de encaminhamento de mensagens, onde define as rotas e os pontos finais personalizados para o hub. [Roteamento de mensagens](iot-hub-devguide-messages-d2c.md) permite-lhe gerir a forma como os dados são enviados dos seus dispositivos para os pontos finais. A primeira etapa é adicionar uma nova rota. Em seguida, pode adicionar um ponto final existente para a rota ou criar um novo dos tipos suportados, como o armazenamento de Blobs. 

![Painel de roteamento de mensagem](./media/iot-hub-create-through-portal/iot-hub-message-routing.png)

### <a name="routes"></a>Rotas

Rotas é o primeiro separador no painel de encaminhamento de mensagens. Para adicionar uma nova rota, clique em +**adicionar**. Veja o ecrã seguinte. 

![Captura de ecrã mostra a adição de uma nova rota](./media/iot-hub-create-through-portal/iot-hub-add-route-storage-endpoint.png)

Nome do seu hub. O nome tem de ser exclusivo dentro da lista de rotas para esse hub. 

Para **ponto final**, pode selecionar um na lista pendente ou adicionar um novo. Neste exemplo, uma conta de armazenamento e um contentor já estão disponíveis. Para adicioná-los como um ponto de extremidade, clique em +**Add** junto a lista pendente de ponto final e selecione **armazenamento de BLOBs**. A tela seguinte mostra onde a conta de armazenamento e o contentor estão especificados.

![Captura de ecrã mostra a adição de um ponto de final de armazenamento para a regra de encaminhamento](./media/iot-hub-create-through-portal/iot-hub-routing-add-storage-endpoint.png)

Clique em **escolher um contentor** para selecionar a conta de armazenamento e o contentor. Quando tiver selecionado esses campos, ele retorna para o painel de ponto final. Utilize as predefinições para o resto dos campos e **criar** para criar o ponto final para a conta de armazenamento e adicioná-lo para as regras de encaminhamento.

Para **origem de dados**, selecione as mensagens de telemetria do dispositivo. 

Em seguida, adicione uma consulta de encaminhamento. Neste exemplo, as mensagens que têm uma propriedade de aplicativo chamada `level` com um valor igual a `critical` são encaminhados para a conta de armazenamento.

![Captura de ecrã que mostra a guardar uma nova regra de encaminhamento](./media/iot-hub-create-through-portal/iot-hub-add-route.png)

Clique em **guardar** para guardar a regra de encaminhamento. Regressar ao painel de encaminhamento de mensagens e sua nova regra de encaminhamento é apresentada.

### <a name="custom-endpoints"></a>Pontos finais personalizados

Clique nas **pontos finais personalizados** separador. Ver todos os pontos finais personalizados já criados. A partir daqui, pode adicionar novos pontos de extremidade ou eliminar pontos finais existentes. 

> [!NOTE]
> Se eliminar uma rota, não elimina os pontos de extremidade atribuídos a essa rota. Para eliminar um ponto de extremidade, clique no separador de pontos de extremidade personalizado, selecione o ponto final que pretende eliminar e clique em eliminar.
>

Pode ler mais sobre os pontos finais personalizados na [referência - pontos finais do hub IoT](iot-hub-devguide-endpoints.md).

Pode definir até 10 pontos finais personalizados para um hub IoT. 

Para ver um exemplo completo de como utilizar os pontos finais personalizados com o encaminhamento, consulte [encaminhamento com o IoT Hub de mensagens](tutorial-routing.md).

## <a name="find-a-specific-iot-hub"></a>Encontrar um hub de IoT específico

Eis duas formas de localizar um hub de IoT específico na sua subscrição:

1. Se conhecer o recurso ao qual pertence o hub IoT, clique em **grupos de recursos**, em seguida, selecione o grupo de recursos da lista. O ecrã de grupo de recursos mostra todos os recursos nesse grupo, incluindo os hubs IoT. Clique no hub para o qual está à procura.

2. Clique em **todos os recursos**. Sobre o **todos os recursos** painel, há uma lista suspensa que, por padrão `All types`. Clique na lista pendente, desmarque `Select all`. Encontrar `IoT Hub` e verificá-lo. Clique na caixa de lista suspensa para fechá-lo e serão filtradas as entradas, que mostra apenas os hubs IoT.

## <a name="delete-the-iot-hub"></a>Eliminar o hub IoT

Para eliminar um hub Iot, encontrar o hub IoT que pretende eliminar, em seguida, clique nas **eliminar** botão abaixo do nome do hub IoT.

## <a name="next-steps"></a>Passos Seguintes

Siga estas ligações para saber mais sobre como gerir o IoT Hub do Azure:

* [Roteamento de mensagens com o IoT Hub](tutorial-routing.md)
* [Métricas do IoT Hub](iot-hub-metrics.md)
* [Monitorização de operações](iot-hub-operations-monitoring.md)