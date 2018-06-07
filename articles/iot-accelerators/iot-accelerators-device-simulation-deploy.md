---
title: Implementar a solução de simulação de dispositivo - Azure | Microsoft Docs
description: Este tutorial mostra como aprovisionar a solução de dispositivo de simulação de azureiotsuite.com.
author: troyhopwood
manager: ''
ms.author: troyhop
ms.service: iot-accelerators
services: iot-accelerators
ms.date: 12/18/2017
ms.topic: conceptual
ms.openlocfilehash: e96c0f16ca4b69d103674b7f895c52d197280ca5
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/01/2018
ms.locfileid: "34627011"
---
# <a name="deploy-the-azure-iot-device-simulation-solution"></a>Implementar a solução de simulação de dispositivos do IoT do Azure

Este tutorial mostra como aprovisionar uma solução de simulação de dispositivo. Implementar a solução de azureiotsuite.com.

Neste tutorial, ficará a saber como:

> [!div class="checklist"]
> * Configurar a solução de simulação de dispositivo
> * Implementar a solução de simulação de dispositivo
> * Iniciar sessão para a solução de simulação de dispositivo

## <a name="prerequisites"></a>Pré-requisitos

Para concluir este tutorial, precisa de uma subscrição ativa do Azure.

Se não tiver uma conta, pode criar uma de avaliação gratuita em apenas alguns minutos. Para obter mais detalhes, consulte [Avaliação gratuita do Azure](http://azure.microsoft.com/pricing/free-trial/).

## <a name="deploy-the-solution"></a>Implementar a solução

Antes de implementar a solução para a sua subscrição do Azure, tem de escolher algumas opções de configuração:

1. Inicie sessão no [azureiotsuite.com](https://www.azureiotsolutions.com) utilizando o Azure credenciais de contas e clique em **+** para criar uma nova solução:

    ![Criar uma nova solução](./media/iot-accelerators-device-simulation-deploy/createnewsolution.png)

1. Clique em **selecione** no **simulação de dispositivo** mosaico:

    ![Escolha a simulação de dispositivo](./media/iot-accelerators-device-simulation-deploy/select.png)

1. No **solução de simulação de dispositivo criar** página, introduza um **nome da solução** para a sua solução de simulação de dispositivo.

1. Selecione a **Subscrição** e a **Região** que quer utilizar para aprovisionar a solução.

1. Especifique se pretende que implementar um novo IoT Hub com a sua solução de simulação de dispositivo. Se selecionar esta caixa, um novo IoT Hub é implementado na sua subscrição. Independentemente da opção, pode apontar sempre a simulação em qualquer IoT Hub.

1. Clique em **Criar Solução** para iniciar o processo de aprovisionamento. Este processo normalmente demora vários minutos a executar:

    ![Detalhes de solução de simulação de dispositivo](./media/iot-accelerators-device-simulation-deploy/createsolution.png)

## <a name="sign-in-to-the-solution"></a>Iniciar sessão para a solução

Quando o processo de aprovisionamento estiver concluído, pode iniciar sessão sua solução de simulação de dispositivo.

1. No **aprovisionado soluções** página, clique em **iniciar** na sua nova solução de simulação de dispositivo:

    ![Escolha a nova solução](./media/iot-accelerators-device-simulation-deploy/newsolution.png)

1. Pode ver informações sobre a sua solução de simulação de dispositivo no painel que aparece. Escolha **dashboard da solução** para ligar à sua solução de simulação de dispositivo.

    > [!NOTE]
    > Pode eliminar a solução de simulação de dispositivo neste painel quando tiver terminado com o mesmo.

    ![Painel de solução](./media/iot-accelerators-device-simulation-deploy/properties.png)

1. Apresenta o dashboard de solução de simulação de dispositivo no seu browser.

## <a name="next-steps"></a>Passos Seguintes

Neste tutorial, ficou a saber como:

> [!div class="checklist"]
> * Configurar a solução
> * Implementar a solução
> * Iniciar sessão para a solução

Agora que implementou a solução de simulação de dispositivo, o passo seguinte consiste em [explorar as funcionalidades da solução de simulação de dispositivo](iot-accelerators-device-simulation-explore.md).

<!-- Next tutorials in the sequence -->
