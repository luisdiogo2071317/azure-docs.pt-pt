---
title: Experimentar e executar uma solução de simulação de dispositivo no Azure | Microsoft Docs
description: Neste início rápido, vai implementar o acelerador de soluções de IoT de simulação de dispositivo do Azure. Inicie sessão no dashboard da solução para criar uma simulação.
author: dominicbetts
manager: timlt
ms.service: iot-accelerators
services: iot-accelerators
ms.topic: quickstart
ms.custom: mvc
ms.date: 07/05/2018
ms.author: dobett
ms.openlocfilehash: 549a1b867ad35c6de42969722ba5a2bd28c8f99a
ms.sourcegitcommit: 248c2a76b0ab8c3b883326422e33c61bd2735c6c
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/23/2018
ms.locfileid: "39213161"
---
# <a name="quickstart-deploy-and-run-a-cloud-based-device-simulation-solution"></a>Início Rápido: implementar e executar uma solução de simulação de dispositivo com base na cloud

Este início rápido mostra como implementar o acelerador de soluções de Simulação de Dispositivo de IoT do Azure para testar a sua solução de IoT. Depois de implementar o acelerador de soluções, utilize a página **Simulação** para criar e executar uma simulação.

Para concluir este início rápido, precisa de uma subscrição ativa do Azure.

Se não tiver uma subscrição do Azure, crie uma [conta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de começar.

## <a name="deploy-the-solution"></a>Implementar a solução

Ao implementar o acelerador de soluções na sua subscrição do Azure, tem de definir algumas opções de configuração.

Inicie sessão em [azureiotsolutions.com](https://www.azureiotsolutions.com/Accelerators) com as credenciais da conta do Azure.

Clique em **Experimente Agora** no mosaico **Simulação de Dispositivo**.

![Escolher a Simulação de Dispositivo](./media/quickstart-device-simulation-deploy/devicesimulation.png)

Na página **Criar solução de Simulação de Dispositivo**, introduza um **Nome de solução** exclusivo. Anote o nome da solução, pois é o nome do grupo de recursos do Azure que contém todos os recursos da solução.

Selecione a **Subscrição** e a **Região** que quer utilizar para implementar o acelerador de soluções. Normalmente, pode escolher a região mais próxima para si. Tem de ser um [administrador global ou de utilizador](iot-accelerators-permissions.md) na subscrição.

Selecione a caixa para implementar um hub IoT para utilizar com a sua solução de simulação de dispositivo. Pode sempre alterar o hub IoT que a simulação utilizará mais tarde.

Clique em **Criar Solução** para começar a aprovisionar a solução. Este processo demora, pelo menos, cinco minutos a ser executado:

![Detalhes da solução de Simulação de Dispositivos](./media/quickstart-device-simulation-deploy/createform.png)

## <a name="sign-in-to-the-solution"></a>Iniciar sessão na solução

Quando o processo de aprovisionamento estiver concluído, pode iniciar sessão no dashboard do acelerador de soluções de simulação de dispositivo.

Na página **Soluções aprovisionadas**, clique no novo acelerador de soluções de Simulação de Dispositivo:

![Escolher a nova solução](./media/quickstart-device-simulation-deploy/choosenew.png)

Pode ver informações sobre o acelerador de soluções de Simulação de Dispositivo no painel apresentado. Selecione **Dashboard de soluções** para ver o acelerador de soluções de Simulação de Dispositivo:

![Painel de soluções](./media/quickstart-device-simulation-deploy/solutionpanel.png)

Clique em **Aceitar** para aceitar o pedido de permissões. O dashboard da solução de Simulação de Dispositivo é apresentado no browser:

[![Dashboard de soluções](./media/quickstart-device-simulation-deploy/solutiondashboard-inline.png)](./media/quickstart-device-simulation-deploy/solutiondashboard-expanded.png#lightbox)

## <a name="configure-the-simulation"></a>Configurar a simulação

Configurou e executou uma simulação a partir do dashboard. Utilize os valores na tabela seguinte para configurar a simulação:

| Definição             | Valor                       |
| ------------------- | --------------------------- |
| Hub IoT de destino      | Utilizar Hub IoT pré-aprovisionado |
| Modelo do dispositivo        | Sistema de arrefecimento                     |
| Número de dispositivos   | 10                          |
| Frequência da telemetria | 10 segundos                  |
| Duração da simulação | 5 minutos                   |

[![Configuração de simulação](./media/quickstart-device-simulation-deploy/simulationconfig-inline.png)](./media/quickstart-device-simulation-deploy/simulationconfig-expanded.png#lightbox)

## <a name="run-the-simulation"></a>Executar a simulação

Clique em **Iniciar Simulação**. A simulação é executada durante o período que escolheu. Pode parar a simulação em qualquer altura ao clicar em **Parar Simulação**. A simulação mostra as estatísticas da execução atual. Clique em **Ver as métricas do Hub IoT no portal do Azure** para ver as métricas comunicadas pelo IoT hub:

[![Execução de simulação](./media/quickstart-device-simulation-deploy/simulationrun-inline.png)](./media/quickstart-device-simulation-deploy/simulationrun-expanded.png#lightbox)

Só pode executar uma simulação de cada vez a partir de uma instância aprovisionada do acelerador de soluções.

## <a name="clean-up-resources"></a>Limpar recursos

Se quiser explorar ainda mais, deixe o acelerador de soluções de Simulação de Dispositivo implementado.

Se já não precisar do acelerador de soluções, elimine-o na página [Soluções aprovisionadas](https://www.azureiotsolutions.com/Accelerators#dashboard) ao selecioná-lo e, em seguida, clique em **Eliminar Solução**:

![Eliminar solução](media/quickstart-device-simulation-deploy/deletesolution.png)

## <a name="next-steps"></a>Passos seguintes

Neste início rápido, implementou o acelerador de soluções de Simulação de Dispositivo e executou uma simulação de dispositivo IoT.

Para saber como utilizar o Hub IoT existente numa simulação, siga o seguinte guia de procedimentos:

> [!div class="nextstepaction"]
> [Utilizar um hub IoT existente com o acelerador de soluções de Simulação de Dispositivo](iot-accelerators-device-simulation-choose-hub.md)
