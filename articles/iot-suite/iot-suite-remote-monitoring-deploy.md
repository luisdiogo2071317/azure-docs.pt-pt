---
title: Implementar a solução de monitorização remota - Azure | Microsoft Docs
description: Este tutorial mostra como aprovisionar o acelerador solução monitorização remota do azureiotsuite.com.
services: iot-suite
suite: iot-suite
author: dominicbetts
manager: timlt
ms.author: dobett
ms.service: iot-suite
ms.date: 05/01/2018
ms.topic: article
ms.devlang: NA
ms.tgt_pltfrm: NA
ms.workload: NA
ms.openlocfilehash: eb78ce91755c43f1c6fedf62a70238df911b940f
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/07/2018
ms.locfileid: "33773637"
---
# <a name="deploy-the-remote-monitoring-solution-accelerator"></a>Implementar o acelerador de solução de monitorização remota

Este tutorial mostra como aprovisionar o acelerador de solução de monitorização remota. Implementar a solução de azureiotsuite.com. Também pode implementar a solução utilizando a CLI do AZURE, para saber mais sobre esta opção, consulte [implementar um acelerador solução na linha de comandos](iot-suite-remote-monitoring-deploy-cli.md).

Neste tutorial, ficará a saber como:

> [!div class="checklist"]
> * Configurar o acelerador de solução
> * Implementar o acelerador de solução
> * Iniciar sessão para o acelerador de solução

## <a name="prerequisites"></a>Pré-requisitos

Para concluir este tutorial, precisa de uma subscrição ativa do Azure.

Se não tiver uma conta, pode criar uma de avaliação gratuita em apenas alguns minutos. Para obter mais detalhes, consulte [Avaliação gratuita do Azure](http://azure.microsoft.com/pricing/free-trial/).

## <a name="deploy-the-solution-accelerator"></a>Implementar o acelerador de solução

Antes de implementar o acelerador solução à sua subscrição do Azure, tem de escolher algumas opções de configuração:

1. Inicie sessão no [azureiotsolutions.com](https://www.azureiotsolutions.com/Accelerators) com as suas credenciais de conta do Azure.

1. Clique em **Tente agora** no **monitorização remota** mosaico.

    ![Escolha a monitorização remota](media/iot-suite-remote-monitoring-deploy/remotemonitoring.png)

1. No **solução de monitorização remota criar** página, introduza um **nome da solução** para o acelerador de solução de monitorização remota.

1. Selecione um **básico** ou **padrão** implementação. Se estiver a implementar a solução para saber como funciona ou para executar uma demonstração, escolha o **básico** opção para minimizar os custos.

1. Escolha o **Java** ou **.NET** como o idioma. Todos os micro-serviços estão disponíveis como implementações de Java ou .NET.

1. Reveja o **detalhes de solução** painel para obter mais informações sobre as opções de configuração.

1. Selecione a **Subscrição** e a **Região** que quer utilizar para aprovisionar a solução.

1. Clique em **Criar Solução** para iniciar o processo de aprovisionamento. Este processo normalmente demora vários minutos a executar:

    ![Detalhes de solução de monitorização remotos](media/iot-suite-remote-monitoring-deploy/createform.png)

Para informações de resolução de problemas, consulte [o que fazer quando ocorre uma falha de uma implementação](https://github.com/Azure/azure-iot-pcs-remote-monitoring-dotnet/wiki/Developer-Troubleshooting-Guide#what-to-do-when-a-deployment-fails) no repositório GitHub.

## <a name="sign-in-to-the-solution-accelerator"></a>Iniciar sessão para o acelerador de solução

Quando o processo de aprovisionamento estiver concluído, pode iniciar sessão sua acelerador de solução de monitorização remota.

1. No **aprovisionado soluções** página, escolha a sua nova solução de monitorização remota:

    ![Escolha a nova solução](media/iot-suite-remote-monitoring-deploy/choosenew.png)

1. Pode ver informações sobre a sua solução de monitorização remota, no painel que aparece. Escolha **dashboard da solução** para ligar à sua solução de monitorização remota.

    > [!NOTE]
    > Pode eliminar a solução de monitorização remota neste painel quando tiver terminado com o mesmo.

    ![Painel de solução](media/iot-suite-remote-monitoring-deploy/solutionpanel.png)

1. Apresenta o dashboard de solução de monitorização remota no seu browser.

## <a name="next-steps"></a>Passos Seguintes

Neste tutorial, ficou a saber como:

> [!div class="checklist"]
> * Configurar o acelerador de solução
> * Implementar o acelerador de solução
> * Iniciar sessão para o acelerador de solução

Agora que implementou a solução de monitorização remota, o passo seguinte consiste em [explorar as funcionalidades do dashboard de solução](./iot-suite-remote-monitoring-explore.md).

<!-- Next tutorials in the sequence -->