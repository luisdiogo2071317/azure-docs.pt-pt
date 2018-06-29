---
title: Implementar, monitorizar módulos do Azure IoT Edge | Microsoft Docs
description: Gerir os módulos que são executadas em dispositivos de limite
keywords: ''
author: kgremban
manager: timlt
ms.author: kgremban
ms.date: 06/07/2018
ms.topic: conceptual
ms.service: iot-edge
services: iot-edge
ms.openlocfilehash: be52a57f10f286bded9a31d84b36a49717b94006
ms.sourcegitcommit: 150a40d8ba2beaf9e22b6feff414f8298a8ef868
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/27/2018
ms.locfileid: "37029762"
---
# <a name="deploy-and-monitor-iot-edge-modules-at-scale-using-the-azure-portal"></a>Implementar e monitorizar os módulos de limite de IoT à escala através do portal do Azure

[!INCLUDE [iot-edge-how-to-deploy-monitor-selector](../../includes/iot-edge-how-to-deploy-monitor-selector.md)]

Limite de IoT do Azure permite-lhe mover analytics para o limite e fornece uma interface de nuvem, para que possa gerir e monitorizar os dispositivos de IoT Edge sem ter de aceder fisicamente cada um deles. A capacidade para gerir remotamente os dispositivos é cada vez mais importante como soluções de Internet das coisas estão a crescer maiores e mais complexas. Limite de IoT do Azure foi concebido para suportar os objetivos de negócio, independentemente do quantos dispositivos a adicionar.

Pode gerir os dispositivos individuais e implementá-las módulos um de cada vez. No entanto, se pretender efetuar alterações nos dispositivos em grande escala, pode criar um **implementação automática de limite de IoT**, que faz parte automática da gestão de dispositivos do IoT Hub. As implementações são dinâmicos processos que permitem-lhe implementar vários módulos em vários dispositivos de uma só vez, controlar o estado e estado de funcionamento dos módulos e efetuar alterações quando for necessário. 

## <a name="identify-devices-using-tags"></a>Identificar dispositivos utilizando as etiquetas

Antes de poder criar uma implementação, tem de ser capazes de especificar quais os dispositivos que pretende afetar. Limite do Azure IoT identifica dispositivos utilizando **etiquetas** no dispositivo duplo. Cada dispositivo pode ter várias etiquetas e pode defini-las qualquer forma que faz sentido para a sua solução. Por exemplo, se gerir um campus dos edifícios inteligentes, poderá adicionar as seguintes etiquetas para um dispositivo:

```json
"tags":{
    "location":{
        "building": "20",
        "floor": "2"
    },
    "roomtype": "conference",
    "environment": "prod"
}
```

Para obter mais informações sobre dispositivos duplos e etiquetas, consulte [compreender e utilizar dispositivos duplos no IoT Hub][lnk-device-twin].

## <a name="create-a-deployment"></a>Criar uma implementação

1. No [portal do Azure][lnk-portal], aceda ao seu IoT hub. 
1. Selecione **IoT Edge**.
1. Selecione **adicionar implementação de limite de IoT**.

Existem cinco passos para criar uma implementação. As secções seguintes guiá-lo através de cada um deles. 

### <a name="step-1-name-and-label"></a>Passo 1: Nome e a etiqueta

1. Dê um nome exclusivo que é até 128 minúsculas da implementação. Evite espaços e os seguintes carateres inválidos: `& ^ [ ] { } \ | " < > /`.
1. Adicione etiquetas para ajudar a monitorizar as implementações. As etiquetas são **nome**, **valor** pares que descrevem a sua implementação. Por exemplo, `HostPlatform, Linux` ou `Version, 3.0.1`.
1. Selecione **seguinte** mover para o passo dois. 

### <a name="step-2-add-modules-optional"></a>Passo 2: Adicionar módulos (opcionais)

Existem dois tipos de módulos que pode adicionar a uma implementação. O primeiro é um módulo baseado num serviço do Azure, como a conta de armazenamento ou o Stream Analytics. O segundo é um módulo baseado nos seu próprio código. Pode adicionar vários módulos de qualquer tipo para uma implementação. 

Se criar uma implementação com nenhuma módulos, remove quaisquer módulos atuais dos dispositivos. 

>[!NOTE]
>O Azure Machine Learning e as funções do Azure não suportam a implementação de serviço do Azure automatizada ainda. Utilize a implementação do módulo personalizado para adicionar manualmente esses serviços à sua implementação. 

Para adicionar um módulo do Azure Stream Analytics, siga estes passos:
1. No **módulos de implementação** secção da página, clique em **adicionar**.
1. Selecione **módulo do Azure Stream Analytics**.
1. Escolha o **subscrição** no menu pendente.
1. Escolha o **tarefa Edge** no menu pendente.
1. Selecione **guardar** para adicionar o módulo para a implementação. 

Para adicionar código personalizado como um módulo ou adicionar manualmente um módulo de serviço do Azure, siga estes passos:
1. No **definições de registo** secção da página, forneça os nomes e as credenciais de quaisquer registos do contentor privada que contêm as imagens do módulo para esta implementação. O agente de limite irá reportar o erro 500 se este não é possível localizar a credencial de registo contrainer para uma imagem de docker.
1. No **módulos de implementação** secção da página, clique em **adicionar**.
1. Selecione **IoT Edge módulo**.
1. Atribua o módulo de um **nome**.
1. Para o **URI de imagem** campo, introduza a imagem do contentor para o módulo. 
1. Especificar qualquer **contentor criar opções** que deve ser transmitido ao contentor. Para obter mais informações, consulte [docker criar][lnk-docker-create].
1. Utilize o menu pendente para selecionar um **reiniciar política**. Escolha uma das seguintes opções: 
   * **Sempre** -o módulo for reiniciado sempre se encerrar por qualquer motivo.
   * **Nunca** -o módulo for reiniciado nunca se encerrar por qualquer motivo.
   * **Falha no** -o módulo for reiniciado se falhar, mas não se encerrar corretamente. 
   * **Num mau** -o módulo for reiniciado caso de falhas ou devolve um Estado de mau estado de funcionamento. Está a funcionar para cada módulo para implementar a função do Estado de funcionamento. 
1. Utilize o menu pendente para selecionar o **estado pretendido** para o módulo. Escolha uma das seguintes opções:
   * **Executar** -esta é a opção predefinida. O módulo iniciará a executar imediatamente depois da implementação.
   * **Parado** -depois da implementação, o módulo permanecerá inativo até chamado após a iniciar por utilizador ou outro módulo.
1. Selecione **ativar** se pretender adicionar quaisquer etiquetas ou propriedades pretendidas para o duplo de módulo. 
1. Introduza **variáveis de ambiente** para este módulo. As variáveis de ambiente fornecem informações de suplemento para um módulo facilitar o início do processo de configuração.
1. Selecione **guardar** para adicionar o módulo para a implementação. 

Depois de ter todos os módulos para uma implementação configurado, selecione **seguinte** mover para o terceiro passo.

### <a name="step-3-specify-routes-optional"></a>Passo 3: Especificar rotas (opcionais)

Rotas definem como módulos comunicam entre si numa implementação. Por predefinição, o assistente fornece-lhe uma rota chamada **rota** e definidos como **FROM /* para $upstream * *, que significa que todas as mensagens de saída por qualquer módulos são enviadas para o seu IoT hub.  

Adicionar ou atualizar as rotas com as informações de [declarar rotas](module-composition.md#declare-routes), em seguida, selecione **seguinte** para continuar para a secção de revisão.


### <a name="step-4-target-devices"></a>Passo 4: Dispositivos de destino

Utilize a propriedade de etiquetas dos seus dispositivos para os dispositivos específicos que devem receber esta implementação de destino. 

Uma vez que as implementações de vários podem ter como destino o mesmo dispositivo, deverá dar-cada implementação de um número de prioridade. Se alguma vez existir um conflito, a implementação com a prioridade mais elevada (valores superiores indicam a prioridade mais alta) wins. Se duas implementações com o mesmo número de prioridade, que foi criada mais recentemente wins. 

1. Introduza um número inteiro positivo para a implementação **prioridade**. No caso de duas ou mais implementações destinam-se no mesmo dispositivo, será aplicada a implementação com o maior valor numérico de prioridade.
1. Introduza um **condição de destino** para determinar quais os dispositivos que irão ser segmentados com esta implementação. A condição é baseada em etiquetas do dispositivo duplo ou dispositivo duplo pretendido propriedades e deve corresponder ao formato de expressão. Por exemplo, `tags.environment='test'` ou `properties.desired.devicemodel='4000x'`. 
1. Selecione **seguinte** para avançar para o passo final.

### <a name="step-5-review-template"></a>Passo 5: Rever modelo

Reveja as informações de implementação, em seguida, selecione **submeter**.

## <a name="monitor-a-deployment"></a>Monitorizar uma implementação

Para ver os detalhes de uma implementação e monitorizar os dispositivos a executá-lo, utilize os seguintes passos:

1. Iniciar sessão para o [portal do Azure] [ lnk-portal] e navegue até ao seu IoT hub. 
1. Selecione **IoT Edge**.
1. Selecione **implementações de limite de IoT**. 

   ![Ver as implementações de limite de IoT][1]

1. Inspecione a lista de implementação. Para cada implementação, pode ver os seguintes detalhes:
   * **ID** -o nome da implementação.
   * **Condição de destino** -tag utilizado para definir os dispositivos visados.
   * **Prioridade** -o número de prioridade atribuído à implementação.
   * **Métricas de sistema** - **direcionadas** Especifica o número de dispositivos duplos no IoT Hub que correspondem à condição filtragem, e **aplicada** Especifica o número de dispositivos que tenham o conteúdo de implementação tinha aplicada aos respetivos duplos módulo no IoT Hub. 
   * **Métricas de dispositivo** -o número de dispositivos de limite na implementação de relatórios êxito ou erros de tempo de execução do cliente do IoT Edge.
   * **Hora de criação** -timestamp de quando a implementação foi criada. Este timestamp é utilizado para dividir ties quando duas implementações têm a mesma prioridade. 
2. Selecione a implementação que pretende monitorizar.  
3. Inspecione os detalhes da implementação. Pode utilizar os separadores para rever os detalhes da implementação.

## <a name="modify-a-deployment"></a>Modificar uma implementação

Quando modifica uma implementação, as alterações imediatamente são replicadas para todos os dispositivos visados. 

Se atualizar a condição de destino, ocorrem as seguintes atualizações:
* Se um dispositivo não cumpre a condição de destino antigo, mas cumpre a condição de destino novo e esta implementação é a prioridade mais elevada para que o dispositivo, esta implementação é aplicada ao dispositivo. 
* Se um dispositivo atualmente em execução nesta implementação já não satisfaz a condição de destino, desinstala esta implementação e demora na implementação de prioridade mais alta seguinte. 
* Se um dispositivo atualmente em execução nesta implementação já não satisfaz a condição de destino e não satisfaz a condição de destino de todas as outras implementações, nenhuma alteração ocorre no dispositivo. O dispositivo continua em execução os respectivos módulos atuais no respetivo estado atual, mas não está a ser gerido como parte desta implementação já. Assim que cumpre a condição de destino de qualquer outra implementação, desinstala esta implementação e demora no novo. 

Para modificar uma implementação, utilize os seguintes passos: 

1. Iniciar sessão para o [portal do Azure] [ lnk-portal] e navegue até ao seu IoT hub. 
1. Selecione **IoT Edge**.
1. Selecione **implementações de limite de IoT**. 

   ![Ver as implementações de limite de IoT][1]

1. Selecione a implementação que pretende modificar. 
1. Efetue atualizações para os seguintes campos: 
   * Condição de destino 
   * Etiquetas 
   * Prioridade 
1. Selecione **Guardar**.
1. Siga os passos no [monitorizar uma implementação] [ anchor-monitor] para ver as alterações a implementar. 

## <a name="delete-a-deployment"></a>Eliminar uma implementação

Quando elimina uma implementação, todos os dispositivos de colocar as suas implementações de maior prioridade seguinte. Se os seus dispositivos não cumprem a condição de destino de qualquer outra implementação, os módulos não são removidos quando a implementação é eliminada. 

1. Iniciar sessão para o [portal do Azure] [ lnk-portal] e navegue até ao seu IoT hub. 
1. Selecione **IoT Edge**.
1. Selecione **implementações de limite de IoT**. 

   ![Ver as implementações de limite de IoT][1]

1. Utilize a caixa de verificação para selecionar a implementação que pretende eliminar. 
1. Selecione **Eliminar**.
1. Uma linha de comandos informar que esta ação irá eliminar esta implementação e reverter o estado anterior para todos os dispositivos.  Isto significa que uma implementação com uma prioridade mais baixa será aplicada.  Não se for aplicada nenhuma outra implementação, sem módulos serão removidos. Se pretender remover todos os módulos do seu dispositivo, uma implementação com zero módulos e implementá-la nos dispositivos. Selecione **Sim** para continuar. 

## <a name="next-steps"></a>Passos Seguintes

Saiba mais sobre [implementar módulos para dispositivos de limite][lnk-deployments].

<!-- Images -->
[1]: ./media/how-to-deploy-monitor/iot-edge-deployments.png

<!-- Links -->
[lnk-device-twin]: ../iot-hub/iot-hub-devguide-device-twins.md
[lnk-portal]: https://portal.azure.com
[lnk-docker-create]: https://docs.docker.com/engine/reference/commandline/create/
[lnk-deployments]: module-deployment-monitoring.md

<!-- Anchor links -->
[anchor-monitor]: #monitor-a-deployment
